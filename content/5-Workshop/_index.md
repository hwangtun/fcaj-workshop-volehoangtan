---
title: "Workshop"
date: 2026-08-04
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

### Deploying the Automatic Image Optimization System on AWS

#### Overview

The **Automatic Image Optimization System** is deployed on the AWS platform to automate image processing and optimization workflows. The solution leverages a serverless architecture, reducing infrastructure management overhead, enhancing scalability, and ensuring stable processing capabilities as image volume grows.

In this system, users can upload images directly. Uploaded images are stored in **Amazon S3**, which triggers an automated processing workflow powered by **AWS Lambda**. Lambda performs optimization tasks such as file compression, resizing, and generating processed image variants.

The processed outputs are stored in an S3 Output Bucket, while execution metadata is logged to **Amazon DynamoDB** to support history lookups, status tracking, and data management.

Following the construction and testing of individual system components, **AWS CDK (Cloud Development Kit)** is used to automate infrastructure deployment as **Infrastructure as Code (IaC)**. Furthermore, the deployment workflow integrates with a **CI/CD Pipeline** to automate updates and system deployments, ensuring consistency and minimizing manual configuration.

The system also incorporates monitoring and security mechanisms:

- **CloudWatch:** Monitors logs, operational status, and Lambda performance metrics.
- **SNS:** Dispatches alert notifications to administrators when processing errors occur.
- **IAM:** Manages cross-service access permissions following the principle of Least Privilege.
- **KMS:** Encrypts data to strengthen security during storage.

#### Contents

1. [System Architecture Overview](5.1-Workshop-overview/)
2. [AWS Environment Setup](5.2-Prerequiste/)
3. [Deploying Amazon S3 for Image Storage](5.3-S3-setup/)
4. [Deploying AWS Lambda for Image Processing](5.4-Lambda-deployment/)
5. [Deploying Amazon DynamoDB for Metadata Storage](5.5-DynamoDB/)
6. [System Monitoring with CloudWatch and SNS](5.6-Monitoring/)
7. [Automated Deployment via AWS CDK and CI/CD](5.7-CDK-deployment/)
8. [Cleaning Up AWS Resources Post-Deployment](5.8-Cleanup/)
