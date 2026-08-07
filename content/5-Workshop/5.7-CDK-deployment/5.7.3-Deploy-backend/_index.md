---
title: "Deploy Backend Infrastructure with AWS CDK"
date: 2026-08-07
weight: 3
chapter: false
pre: " <b> 5.7.3 </b> "
---

---

After completing the environment preparation, the next step is to deploy the Backend infrastructure using **AWS CDK**.

The entire Backend infrastructure is defined as **Infrastructure as Code (IaC)** and organized into 5 CDK Stacks:

- `StorageStack`: Deploys the system's storage resources.

```
from aws_cdk import (
    Stack,
    RemovalPolicy,
    aws_s3 as s3,
    aws_dynamodb as _dynamodb,
)
from constructs import Construct


class StorageStack(Stack):
    def __init__(self, scope: Construct, construct_id: str, **kwargs) -> None:
        super().__init__(scope, construct_id, **kwargs)

        self.original_bucket = s3.Bucket(
            self,
            "OriginalImageBucket",
            bucket_name="aws-img-opt-original-bucket",
            versioned=True,
            encryption=s3.BucketEncryption.S3_MANAGED,
            block_public_access=s3.BlockPublicAccess.BLOCK_ALL,
            removal_policy=RemovalPolicy.DESTROY,
            auto_delete_objects=True,
            event_bridge_enabled=True,  # bắt buộc để ProcessingStack's EventBridge Rule nhận được sự kiện
            cors=[
                s3.CorsRule(
                    allowed_methods=[s3.HttpMethods.GET, s3.HttpMethods.PUT, s3.HttpMethods.POST, s3.HttpMethods.HEAD],
                    allowed_origins=["*"],
                    allowed_headers=["*"],
                    max_age=3000
                )
            ]
        )

        self.optimized_bucket = s3.Bucket(
            self,
            "OptimizedImageBucket",
            bucket_name="aws-img-opt-optimized-bucket",
            versioned=True,
            encryption=s3.BucketEncryption.S3_MANAGED,
            block_public_access=s3.BlockPublicAccess.BLOCK_ALL,
            removal_policy=RemovalPolicy.DESTROY,
            auto_delete_objects=True,
        )


        self.table = _dynamodb.Table(
            self,
            "ImageMetadata",
            table_name="aws-img-opt-metadata-table",
            partition_key=_dynamodb.Attribute(
                name="batchId",
                type=_dynamodb.AttributeType.STRING,
            ),
            sort_key=_dynamodb.Attribute(
                name="processingId",
                type=_dynamodb.AttributeType.STRING,
            ),
            billing_mode=_dynamodb.BillingMode.PAY_PER_REQUEST,
            removal_policy=RemovalPolicy.DESTROY,
        )


        # Bảng UserMetadata: Lưu thông tin người dùng
        self.user_table = _dynamodb.Table(
            self,
            "UserMetadata",
            table_name="aws-img-opt-user-metadata-table",
            partition_key=_dynamodb.Attribute(
                name="userId",
                type=_dynamodb.AttributeType.STRING,
            ),
            billing_mode=_dynamodb.BillingMode.PAY_PER_REQUEST,
            removal_policy=RemovalPolicy.DESTROY,
        )
        self.table.add_global_secondary_index(
            index_name="UserImagesIndex",
            partition_key=_dynamodb.Attribute(
                name="userId",
                type=_dynamodb.AttributeType.STRING,
            ),
            sort_key=_dynamodb.Attribute(
                name="processedAt",
                type=_dynamodb.AttributeType.STRING,
            ),
            projection_type=_dynamodb.ProjectionType.ALL,
        )

        from aws_cdk import CfnOutput

        CfnOutput(
            self,
            "OriginalImageBucketName",
            value=self.original_bucket.bucket_name,
            description="Name of the original image bucket"
        )

        CfnOutput(
            self,
            "OptimizedImageBucketName",
            value=self.optimized_bucket.bucket_name,
            description="Name of the optimized image bucket"
        )

        CfnOutput(
            self,
            "ImageMetadataTableName",
            value=self.table.table_name,
            description="Name of the DynamoDB table for image metadata"
        )

        CfnOutput(
            self,
            "UserMetadataTableName",
            value=self.user_table.table_name,
            description="Name of the DynamoDB table for user metadata"
        )
```

- `ProcessingStack`: Deploys components required for the image processing workflow.

```
import aws_cdk as cdk
from aws_cdk import (
    Stack,
    Duration,
    aws_lambda as _lambda,
    aws_events as events,
    aws_events_targets as targets,
    aws_s3 as s3,
    aws_dynamodb as _dynamodb,
)
from constructs import Construct


class ProcessingStack(Stack):
    def __init__(
        self, scope: Construct, construct_id: str,
        original_bucket: s3.Bucket,
        optimized_bucket: s3.Bucket,
        metadata_table: _dynamodb.Table,
        **kwargs
    ) -> None:
        super().__init__(scope, construct_id, **kwargs)

        self.processor = _lambda.Function(
            self,
            "ImageProcessor",
            runtime=_lambda.Runtime.PYTHON_3_13,
            handler="handler.lambda_handler",
            code=_lambda.Code.from_asset("lambda",
                bundling=cdk.BundlingOptions(
                    image=_lambda.Runtime.PYTHON_3_13.bundling_image,
                    command=["bash", "-c", "pip install -r requirements.txt -t /asset-output && cp -r . /asset-output"],
                ),
            ),
            timeout=Duration.seconds(30),
            memory_size=512,
            environment={
                "METADATA_TABLE": metadata_table.table_name,
                "OUTPUT_BUCKET": optimized_bucket.bucket_name
            }
        )

        # Grant permissions
        original_bucket.grant_read(self.processor)
        optimized_bucket.grant_write(self.processor)
        metadata_table.grant_read_write_data(self.processor)


        rule = events.Rule(
            self, "S3ObjectCreatedRule",
            event_pattern=events.EventPattern(
                source=["aws.s3"],
                detail_type=["Object Created"],
                detail={
                    "bucket": {
                        "name": [original_bucket.bucket_name]
                    }
                }
            )
        )
        rule.add_target(targets.LambdaFunction(self.processor))
```

- `BackendStack`: Deploys the application's Backend components.

```
from aws_cdk import (
    Stack,
    Duration,
    aws_lambda as _lambda,
    aws_s3 as s3,
    aws_dynamodb as _dynamodb,
)
from constructs import Construct
import os

class BackendStack(Stack):
    def __init__(
        self, scope: Construct, construct_id: str,
        original_bucket: s3.Bucket,
        optimized_bucket: s3.Bucket,
        metadata_table: _dynamodb.Table,
        user_table: _dynamodb.Table,
        **kwargs
    ) -> None:
        super().__init__(scope, construct_id, **kwargs)

        # Deploy Spring Boot as a Lambda function using AWS Lambda Web Adapter
        # Đọc file .env để lấy Secret (trong thực tế CI/CD nên dùng AWS Secrets Manager)
        env_file_path = os.path.join("backend", "image-optimizer", ".env")
        google_client_id = "default_client_id"
        jwt_secret = "default_jwt_secret_must_be_at_least_32_characters_long_for_hmac_sha256"

        if os.path.exists(env_file_path):
            with open(env_file_path, "r") as f:
                for line in f:
                    if line.startswith("GOOGLE_CLIENT_ID="):
                        google_client_id = line.strip().split("=", 1)[1]
                    elif line.startswith("JWT_SECRET="):
                        jwt_secret = line.strip().split("=", 1)[1]

        self.spring_boot_lambda = _lambda.DockerImageFunction(
            self,
            "SpringBootApiLambda",
            code=_lambda.DockerImageCode.from_image_asset(
                directory="backend/image-optimizer"
            ),
            memory_size=512,  # Set to 512 to satisfy AWS Lab constraints
            timeout=Duration.seconds(30), # API Gateway has 29s timeout anyway
            environment={
                "S3_INPUT_BUCKET": original_bucket.bucket_name,
                "S3_OUTPUT_BUCKET": optimized_bucket.bucket_name,
                "DYNAMODB_IMAGE_TABLE": metadata_table.table_name,
                "DYNAMODB_USER_TABLE": user_table.table_name,
                "GOOGLE_CLIENT_ID": google_client_id,
                "JWT_SECRET": jwt_secret
            }
        )

        # Grant necessary permissions
        original_bucket.grant_read_write(self.spring_boot_lambda)
        optimized_bucket.grant_read_write(self.spring_boot_lambda)
        metadata_table.grant_read_write_data(self.spring_boot_lambda)
        user_table.grant_read_write_data(self.spring_boot_lambda)

```

- `ApiStack`: Deploys API Gateway and configures the connection to the Backend.

```
from aws_cdk import (
    Stack,
    Duration,
    CfnOutput,
    aws_lambda as _lambda,
    aws_apigateway as apigw,
    aws_s3 as s3,
    aws_dynamodb as _dynamodb,
)
from constructs import Construct

class ApiStack(Stack):
    def __init__(
        self, scope: Construct, construct_id: str,
        original_bucket: s3.Bucket,
        metadata_table: _dynamodb.Table,
        backend_lambda: _lambda.Function,
        **kwargs
    ) -> None:
        super().__init__(scope, construct_id, **kwargs)

        # Upload Lambda for generating pre-signed URLs
        self.upload_api_lambda = _lambda.Function(
            self,
            "UploadApiLambda",
            runtime=_lambda.Runtime.PYTHON_3_13,
            handler="api_handler.lambda_handler",
            code=_lambda.Code.from_asset("lambda"),
            timeout=Duration.seconds(10),
            memory_size=256,
            environment={
                "ORIGINAL_BUCKET": original_bucket.bucket_name,
                "METADATA_TABLE": metadata_table.table_name
            }
        )

        # Grant permissions
        original_bucket.grant_put(self.upload_api_lambda)
        metadata_table.grant_read_write_data(self.upload_api_lambda)

        # API Gateway REST API
        self.api = apigw.RestApi(
            self, "ImageUploadApi",
            rest_api_name="Image Upload Service",
            description="This service handles uploading images to S3."
        )

        # Integration
        upload_integration = apigw.LambdaIntegration(
        self.upload_api_lambda
        )

        # Resource and Method for Upload Lambda (Python)
        upload_resource = self.api.root.add_resource("upload")
        upload_resource.add_method("POST", upload_integration)
        upload_resource.add_method("OPTIONS", upload_integration)

        # Integration for Spring Boot Lambda (Java)
        spring_boot_integration = apigw.LambdaIntegration(backend_lambda)

        # Resource and Method for Spring Boot APIs: /api/{proxy+}
        api_resource = self.api.root.add_resource("api")
        api_resource.add_proxy(
            default_integration=spring_boot_integration,
            any_method=True
        )

        # Export API Gateway URL so sync_backend_env.py can read it from cdk-outputs.json
        CfnOutput(
            self,
            "ApiGatewayUrl",
            value=self.api.url,
            description="API Gateway endpoint URL (e.g. https://xxx.execute-api.region.amazonaws.com/prod/)"
        )


```

- `AmplifyHostingStack`: Configures resources for hosting the Frontend application on AWS Amplify.

```
from aws_cdk import (
    Stack,
    CfnOutput,
    aws_amplify as amplify,
)
from constructs import Construct


class AmplifyHostingStack(Stack):
    def __init__(
        self,
        scope: Construct,
        construct_id: str,
        api_url: str,
        **kwargs
    ) -> None:
        super().__init__(scope, construct_id, **kwargs)

        app = amplify.CfnApp(
            self,
            "FrontendAmplifyApp",
            name="image-optimization-frontend",
            platform="WEB",
            enable_branch_auto_deletion=False,
            environment_variables=[
                amplify.CfnApp.EnvironmentVariableProperty(
                    name="VITE_API_GATEWAY_URL",
                    value=api_url.rstrip("/")
                ),
                amplify.CfnApp.EnvironmentVariableProperty(
                    name="VITE_GOOGLE_CLIENT_ID",
                    value="{{resolve:secretsmanager:amplify/google-client-id:SecretString}}"
                ),
            ],
            build_spec="""version: 1
applications:
  - appRoot: frontend/image-optimization-system/image-optimization-frontend
    frontend:
      phases:
        preBuild:
          commands:
            - npm ci
        build:
          commands:
            - npm run build
      artifacts:
        baseDirectory: dist
        files:
          - '**/*'
      cache:
        paths:
          - node_modules/**/*
""",
            custom_rules=[
                amplify.CfnApp.CustomRuleProperty(
                    source="</^[^.]+$|\\.(?!(css|gif|ico|jpg|jpeg|js|png|svg|txt|webp|woff|woff2)$)([^.]+$)/>",
                    target="/index.html",
                    status="200",
                )
            ],
        )

        branch = amplify.CfnBranch(
            self,
            "MainBranch",
            app_id=app.attr_app_id,
            branch_name="main",
            stage="PRODUCTION",
            enable_auto_build=False,
        )

        CfnOutput(
            self,
            "AmplifyAppId",
            value=app.attr_app_id,
            description="Amplify app ID for the frontend",
        )

        CfnOutput(
            self,
            "FrontendUrl",
            value=f"https://{branch.branch_name}.{app.attr_default_domain}",
            description="Amplify HTTPS URL for the frontend",
        )

```

These Stacks are deployed through AWS CDK, which enables automatic creation and configuration of AWS resources instead of manually performing these tasks through the AWS Management Console.

---

### Step 1: Clone Source Code and Create Virtual Environment

Clone the project source code from the repository:

```bash id="f1a6mh"
git clone https://github.com/MyPhungg/aws-automatic-image-optimization-system.git
```

After successfully cloning the repository, navigate to the project directory:

```bash id="gr3v1q"
cd aws-automatic-image-optimization-system
```

Create a Python Virtual Environment:

```bash id="y8sx0f"
python -m venv .venv
```

Activate the Virtual Environment.

On **Windows**:

```bash id="1g4q2t"
.venv\Scripts\activate
```

On **Linux/macOS**:

```bash id="5uvq4d"
source .venv/bin/activate
```

After activating the virtual environment, install the required dependencies:

```bash id="7x6c3v"
pip install -r requirements.txt
```

These libraries include the required dependencies for AWS CDK and other related components of the project.

---

### Step 2: Configure Environment Variables

Some configuration values are required during the Backend deployment process. Create a `.env` file at:

```text id="d4t6v7"
backend/image-optimizer/.env
```

Sample content:

```env id="b1t2n4"
GOOGLE_CLIENT_ID=your-google-oauth-client-id
JWT_SECRET=your-secret-key-at-least-32-chars-long
```

Where:

- `GOOGLE_CLIENT_ID`: Google OAuth Client ID used for Google login functionality.
- `JWT_SECRET`: Secret key used for JWT creation and authentication.

Replace the sample values with the actual configuration values of the system.

> **Note:** The `.env` file may contain sensitive information. Do not commit this file to GitHub or share actual secret values.

---

### Step 3: Bootstrap AWS CDK

Before deployment, AWS CDK needs to be **bootstrapped** on the AWS account and region being used.

If the AWS account has not been bootstrapped, execute:

```bash id="q4d9fz"
cdk bootstrap aws://<ACCOUNT_ID>/<REGION>
```

Where:

- `<ACCOUNT_ID>` is the AWS Account ID.
- `<REGION>` is the AWS Region used for deployment.

Example:

```bash id="1s5x3c"
cdk bootstrap aws://123456789012/ap-southeast-1
```

The bootstrap process creates the required resources that allow AWS CDK to deploy applications into the AWS environment.

> **Note:** Normally, each AWS account and region only needs to be bootstrapped once. If the environment has already been bootstrapped, proceed to the deployment step.

---

### Step 4: Deploy Backend Infrastructure

After completing the bootstrap process, deploy all CDK Stacks using the following command:

```bash id="8yq2qf"
cdk deploy --all --require-approval never --outputs-file cdk-outputs.json
```

Where:

- `--all`: Deploys all CDK Stacks in the project.
- `--require-approval never`: Skips manual approval for changes that normally require confirmation during deployment.
- `--outputs-file cdk-outputs.json`: Stores Stack output values in the `cdk-outputs.json` file.

AWS CDK reads the configuration from the source code, identifies the required resources, and creates or updates them in the AWS environment.

After the deployment process is completed, the `cdk-outputs.json` file contains output information generated from the Stacks. These values can be used in the Frontend deployment process and system verification steps.

---

### Step 5: Build Docker Image for Backend

During the deployment process, some Backend Lambda functions are packaged as **Docker Images** through the `DockerImageFunction` configuration.

AWS CDK uses Docker on the local machine to build the corresponding images.

The general workflow is:

```text id="r7m3k2"
AWS CDK
   │
   ▼
Docker Build
   │
   ▼
Docker Image
   │
   ▼
Amazon ECR
   │
   ▼
AWS Lambda
```

Therefore, ensure that **Docker Desktop is running** before executing the `cdk deploy` command.

After the Docker image is successfully built, the image is pushed to **Amazon ECR** and used by Lambda during execution.

> **Note:** The Docker image build process may take additional time depending on the source code size, dependencies, and computer performance.
