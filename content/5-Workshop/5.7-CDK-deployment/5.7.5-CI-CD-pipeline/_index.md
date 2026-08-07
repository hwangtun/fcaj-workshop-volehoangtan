---
title: "CI/CD Deployment with GitHub Actions"
date: 2026-08-07
weight: 5
chapter: false
pre: " <b> 5.7.5 </b> "
---

---

After completing the manual deployment process, the team uses **GitHub Actions** to automate the **Continuous Integration and Continuous Deployment (CI/CD)** process.

The workflow is defined in the following file:

```text
.github/workflows/deploy.yml
```

Setup deploy.yml

```
name: Deploy CDK

on:
  workflow_dispatch:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  check:
    if: github.event_name == 'pull_request'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with: { python-version: "3.13" }
      - uses: actions/setup-node@v4
        with: { node-version: "24" }
      - run: npm install -g aws-cdk
      - run: python -m pip install -r requirements.txt
      - uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-region: us-east-1
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-session-token: ${{ secrets.AWS_SESSION_TOKEN }}

      - name: Create Backend .env File
        run: |
          echo "GOOGLE_CLIENT_ID=${{ secrets.VITE_GOOGLE_CLIENT_ID }}" > backend/image-optimizer/.env

      - run: cdk diff --all

  deploy:
    if: github.event_name != 'pull_request'
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-python@v5
        with:
          python-version: "3.13"

      - uses: actions/setup-node@v4
        with:
          node-version: "24"

      - name: Install AWS CDK
        run: npm install -g aws-cdk

      - name: Install Python dependencies
        run: |
          python -m pip install -r requirements.txt

      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-region: us-east-1
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-session-token: ${{ secrets.AWS_SESSION_TOKEN }}

      - name: Create Backend .env File
        run: |
          echo "GOOGLE_CLIENT_ID=${{ secrets.VITE_GOOGLE_CLIENT_ID }}" > backend/image-optimizer/.env

      - name: Sync Amplify Secrets
        env:
          VITE_GOOGLE_CLIENT_ID: ${{ secrets.VITE_GOOGLE_CLIENT_ID }}
        run: |
          if [ -z "$VITE_GOOGLE_CLIENT_ID" ]; then
            echo "Missing GitHub secret: VITE_GOOGLE_CLIENT_ID"
            exit 1
          fi

          aws secretsmanager describe-secret --secret-id amplify/google-client-id >/dev/null 2>&1 \
            && aws secretsmanager put-secret-value --secret-id amplify/google-client-id --secret-string "$VITE_GOOGLE_CLIENT_ID" >/dev/null \
            || aws secretsmanager create-secret --name amplify/google-client-id --secret-string "$VITE_GOOGLE_CLIENT_ID" >/dev/null

      - name: CDK Synth
        run: cdk synth

      - name: Deploy CDK Stacks
        run: |
          cdk deploy \
            StorageStack \
            ProcessingStack \
            BackendStack \
            ApiStack \
            AmplifyHostingStack \
            --require-approval never \
            --outputs-file cdk-outputs.json

      - name: Build React Frontend
        env:
          VITE_GOOGLE_CLIENT_ID: ${{ secrets.VITE_GOOGLE_CLIENT_ID }}
        run: |
          API_GATEWAY_URL=$(python -c "import json; print(json.load(open('cdk-outputs.json'))['ApiStack']['ApiGatewayUrl'].rstrip('/'))")

          cd frontend/image-optimization-system/image-optimization-frontend
          echo "VITE_API_GATEWAY_URL=$API_GATEWAY_URL" > .env
          echo "VITE_GOOGLE_CLIENT_ID=$VITE_GOOGLE_CLIENT_ID" >> .env
          npm ci
          npm run build
          cd dist
          zip -r "$GITHUB_WORKSPACE/frontend-dist.zip" .
          unzip -l "$GITHUB_WORKSPACE/frontend-dist.zip"

      - name: Deploy Frontend to Amplify
        run: |
          AMPLIFY_APP_ID=$(python -c "import json; print(json.load(open('cdk-outputs.json'))['AmplifyHostingStack']['AmplifyAppId'])")
          DEPLOYMENT=$(aws amplify create-deployment --app-id "$AMPLIFY_APP_ID" --branch-name main)
          JOB_ID=$(python -c "import json, sys; print(json.loads(sys.argv[1])['jobId'])" "$DEPLOYMENT")
          ZIP_UPLOAD_URL=$(python -c "import json, sys; print(json.loads(sys.argv[1])['zipUploadUrl'])" "$DEPLOYMENT")

          curl --fail --show-error --location \
            -H "Content-Type: application/zip" \
            --upload-file frontend-dist.zip \
            "$ZIP_UPLOAD_URL"

          aws amplify start-deployment --app-id "$AMPLIFY_APP_ID" --branch-name main --job-id "$JOB_ID"

          for attempt in {1..60}; do
            STATUS=$(aws amplify get-job --app-id "$AMPLIFY_APP_ID" --branch-name main --job-id "$JOB_ID" --query 'job.summary.status' --output text)
            echo "Amplify deployment status: $STATUS"

            if [ "$STATUS" = "SUCCEED" ]; then
              exit 0
            fi

            if [ "$STATUS" = "FAILED" ] || [ "$STATUS" = "CANCELLED" ]; then
              exit 1
            fi

            sleep 10
          done

          echo "Timed out waiting for Amplify deployment"
          exit 1

      - name: Upload CDK Outputs
        uses: actions/upload-artifact@v4
        with:
          name: cdk-outputs
          path: cdk-outputs.json

```

Whenever new changes are pushed to the `main` branch, GitHub Actions automatically performs the necessary steps to update the Backend and Frontend in the AWS environment.

### Step 1: Configure GitHub Secrets

To allow GitHub Actions to access and deploy resources on AWS, authentication information must be configured as **GitHub Secrets**.

Access the project's repository on GitHub, then select:

**Settings → Secrets and variables → Actions**

Add the following Secrets:

| Secret                  | Description                                                                                        |
| ----------------------- | -------------------------------------------------------------------------------------------------- |
| `AWS_ACCESS_KEY_ID`     | Access Key of the IAM User used to deploy AWS resources                                            |
| `AWS_SECRET_ACCESS_KEY` | Corresponding Secret Access Key                                                                    |
| `AWS_SESSION_TOKEN`     | Session Token, used when AWS credentials are temporary credentials such as AWS Academy credentials |
| `VITE_GOOGLE_CLIENT_ID` | Google OAuth Client ID used for Google login                                                       |

> **Note:** `AWS_SESSION_TOKEN` is only required when using temporary credentials. When using standard Access Key/Secret Key credentials, this Secret may not be required.

The authentication information is stored as GitHub Secrets to prevent credentials from being directly exposed in the source code.

---

### Step 2: Trigger the CI/CD Pipeline

The workflow is configured to run whenever new code is pushed to the `main` branch.

The general process is as follows:

```text
Developer
    │
    │ git push
    ▼
GitHub Repository
    │
    ▼
GitHub Actions
    │
    ├──► Setup Python
    │
    ├──► Setup Node.js
    │
    ├──► Install AWS CDK
    │
    ├──► Deploy Backend
    │
    ├──► Get CDK Outputs
    │
    ├──► Build Frontend
    │
    └──► Deploy to AWS Amplify
```

This allows developers to avoid manually executing the entire deployment process after every source code update.

---

### Step 3: Deploy the Backend

After the workflow is triggered, GitHub Actions prepares the required environment for the project, including Python, Node.js, and AWS CDK.

The workflow then deploys the CDK Stacks using the following command:

```bash
cdk deploy --all
```

Changes in the CDK source code are checked and deployed to the AWS environment.

If the Backend deployment is successful, the CDK output values are used in the subsequent steps of the pipeline.

---

### Step 4: Retrieve Information from CDK Outputs

After the Backend deployment is completed, the workflow retrieves the required values from the CDK outputs, such as:

- `ApiGatewayUrl`: The API Gateway URL used by the Frontend.
- `AmplifyAppId`: The ID of the AWS Amplify application.

These values allow the pipeline to configure and deploy the Frontend using the Backend resources that have just been updated.

---

### Step 5: Build the Frontend

After retrieving the API Gateway URL, the workflow builds the React/Vite application.

The API environment variable is configured so that the Frontend uses the latest API Gateway URL:

```text
VITE_API_GATEWAY_URL
```

The application is then built into static files for deployment:

```bash
npm install
npm run build
```

The build output is generated in the `dist` directory.

---

### Step 6: Deploy the Frontend to AWS Amplify

After the Frontend is successfully built, the workflow packages the files in the `dist` directory and deploys them to **AWS Amplify**.

This process updates the latest Frontend version in the Live environment without requiring manual deployment through the **AWS Management Console**.

Once the deployment is completed, users can access the latest version of the application through the URL provided by AWS Amplify.

---

### Verify the CI/CD Pipeline

After pushing code to the `main` branch, the workflow execution can be monitored by following these steps:

1. Access the project's repository on GitHub.
2. Select the **Actions** tab.
3. Select the deployment workflow.
4. Monitor the status of each step.
5. Verify that the workflow completes successfully.

If all steps are completed successfully, the Backend and Frontend have been successfully updated in the AWS environment.

#### Result

After the CI/CD Pipeline is configured, the system deployment process is automated as follows:

```text
Push code to main
        │
        ▼
GitHub Actions
        │
        ├──► Deploy AWS CDK
        │
        ├──► Retrieve API Gateway URL
        │
        ├──► Build React Frontend
        │
        └──► Deploy to AWS Amplify
```

Implementing CI/CD helps reduce manual deployment tasks, minimize errors during system updates, and shorten the time required to deliver new versions to the AWS environment.
