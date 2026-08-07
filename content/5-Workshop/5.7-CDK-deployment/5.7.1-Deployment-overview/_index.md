---
title: "System Deployment Overview"
date: 2026-08-07
weight: 1
chapter: false
pre: " <b> 5.7.1 </b> "
---

---

In the previous sections, the **Automatic Image Optimization System** has been developed and the necessary components have been configured on the AWS platform. This section presents the process of deploying the entire system to the actual AWS environment using **AWS CDK**, **AWS Amplify**, and **GitHub Actions**.

The deployment process follows an **Infrastructure as Code (IaC)** approach combined with **CI/CD**, which helps minimize manual configuration tasks, ensure consistency, and simplify future system redeployment.

The deployment system consists of three main components:

### 1. Backend Infrastructure

The Backend infrastructure is defined as **Infrastructure as Code (IaC)** using **AWS CDK with Python**. CDK allows AWS resources to be described through source code and automatically creates and configures the required resources during deployment.

The main services used in the Backend Infrastructure include:

- **Amazon API Gateway:** Provides APIs for Frontend communication with the Backend.
- **AWS Lambda:** Executes the system's processing functions.
- **Amazon S3:** Stores input images and processed results.
- **Amazon DynamoDB:** Stores metadata and image processing status information.
- **Amazon EventBridge:** Supports event processing and system event orchestration.
- **AWS IAM:** Manages access permissions between AWS services.

These resources are defined in the project's CDK Stacks and can be automatically deployed through AWS CDK.

---

### 2. Frontend Application

The system's Frontend is developed using **React and Vite**. After the source code is built into static files, the application is deployed and hosted on **AWS Amplify**.

AWS Amplify provides a hosting environment that allows users to access the application through the Internet. The Frontend communicates with the Backend through APIs provided by API Gateway.

The Frontend deployment process includes:

```text id="h4x8qw"
React/Vite Source Code
        │
        ▼
     npm build
        │
        ▼
    Build Output
        │
        ▼
  AWS Amplify Hosting
```

---

### 3. CI/CD Pipeline

The deployment process is automated using **GitHub Actions**. The CI/CD workflow is configured in the project repository and performs the necessary steps to update the Backend and Frontend.

The general workflow is:

```text id="k2m7pv"
Developer
    │
    ▼
GitHub Repository
    │
    ▼
GitHub Actions
    │
    ├──────────────► AWS CDK
    │                    │
    │                    ▼
    │             Backend Infrastructure
    │
    └──────────────► Build Frontend
                         │
                         ▼
                    AWS Amplify
```

When new source code is pushed to the branch configured in the workflow, GitHub Actions automatically performs the deployment process according to the project configuration. This reduces manual operations and ensures that the system is updated with the latest source code version.

---

### Summary

By combining **AWS CDK**, **AWS Amplify**, and **GitHub Actions**, the system can be deployed through an automated and consistent process.

AWS CDK is responsible for defining and deploying the Backend infrastructure, AWS Amplify provides the hosting environment for the Frontend, while GitHub Actions manages the CI/CD automation workflow.

The detailed deployment steps are presented in the following sections.
