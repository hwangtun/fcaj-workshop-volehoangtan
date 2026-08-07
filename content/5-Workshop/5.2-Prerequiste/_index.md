---
title: "Prerequiste"
date: 2026-08-04
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

#### IAM Permissions

Before deploying the **Automatic Image Optimization System**, it is necessary to prepare an AWS account or an IAM User with sufficient permissions to create and manage the services used in the project.

During the development process, the team used an IAM User with permissions for the following services:

- Amazon S3
- AWS Lambda
- Amazon DynamoDB
- Amazon CloudWatch
- Amazon SNS
- AWS IAM
- AWS KMS

For the Production environment, it is recommended to apply the **Principle of Least Privilege**, granting only the permissions required for each service to enhance the security of the system.

![iam](../../images/5-Workshop/5.2-Prerequisite/iam_policy.png)

<!-- <center>
Figure 5.2.1: Custom IAM Policy configured with the necessary access permissions based on the Principle of Least Privilege
</center> -->

---

#### Preparing AWS Resources

In this workshop, the team uses **AWS Region N. Virginia (us-east-1)** to deploy the entire system.

Before getting started, create the following resources:

- Input S3 Bucket
- Output S3 Bucket
- DynamoDB Table
- Lambda Function
- IAM Role for Lambda
- SNS Topic
- KMS Key

---

#### Creating S3 Buckets

Create two Amazon S3 Buckets for the image processing workflow:

- **Input Bucket:** stores images uploaded by users.
- **Output Bucket:** stores images after they have been optimized.

After completing the creation process, verify the list of Buckets in the Amazon S3 Console.

![s3](../../images/5-Workshop/5.2-Prerequisite/s3-buckets.png)

---

#### Creating the DynamoDB Table

Next, create the **ImageMetadata** table to store information about the image processing workflow.

The table includes:

- Partition Key: `batchId`
- Sort Key: `processingId`

In addition, the table stores the following information:

- userId
- originalName
- status
- originalSize
- processedSize
- compressionRatio
- uploadedAt
- processedAt

![dynamodb](../../images/5-Workshop/5.2-Prerequisite/dynamodb.png)

---

#### Creating the IAM Role for Lambda

Create an IAM Role for AWS Lambda and grant the necessary permissions so that the Lambda function can interact with AWS services during the image processing workflow. The IAM Role includes permissions to read from and write to Amazon S3, write metadata to Amazon DynamoDB, and send logs to Amazon CloudWatch. If AWS KMS encryption or direct notifications through Amazon SNS are used, the corresponding permissions should also be added to the Role. After the Role has been created, assign it as the Execution Role for the Lambda Function.

![iam-role](../../images/5-Workshop/5.2-Prerequisite/iam-role-for-lambda.png)

---

#### Verifying the Environment

After completing the preparation steps, verify that all resources have been created successfully.

The deployment environment should include:

- 02 Amazon S3 Buckets.
- 01 AWS Lambda Function.
- 01 Amazon DynamoDB Table.
- 01 IAM Role.
- 01 SNS Topic.
- 01 AWS KMS Key.

After completing the preparation steps, the AWS environment is ready to deploy the Automatic Image Optimization System in the following sections.
