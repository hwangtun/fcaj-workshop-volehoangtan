---
title: "Deploying AWS Lambda for Image Processing"
date: 2026-08-04
weight: 4
chapter: false
pre: " <b> 5.4 </b> "
---

In the **Automatic Image Optimization System on AWS**, AWS Lambda serves as the main processing component, responsible for automatically optimizing images after users upload them to the system.

Lambda is used following the **serverless** model, allowing the system to process images without managing servers or application infrastructure.

Lambda workflow:

```
User Upload Image
        |
        v
Amazon S3 Input Bucket
        |
        v
S3 Trigger Event
        |
        v
AWS Lambda
        |
        +----------------+
        |                |
        v                v
S3 Output Bucket    DynamoDB
(Optimized Image)   (Metadata)
```

AWS Lambda performs the following tasks:

- Receives the image upload event from Amazon S3.
- Reads the image file from the Input Bucket.
- Checks the image format and information.
- Optimizes the image size and file size.
- Converts the image format based on the configuration.
- Generates a thumbnail for faster display.
- Uploads the processed image to the Output Bucket.
- Stores processing metadata in DynamoDB.

---

### Lambda Deployment Steps

The AWS Lambda deployment process consists of the following steps:

#### 5.4.1. Create a Lambda Function

Create a new Lambda Function in the AWS Console, select the appropriate runtime, and configure the execution permissions for the Lambda function.

Lambda uses an IAM Role to access the required AWS services:

- Amazon S3: read input images and write processed images.
- Amazon DynamoDB: store processing metadata.
- CloudWatch Logs: record logs and monitor execution.

---

#### 5.4.2. Configure the Lambda Function

After creating the Lambda function, configure the following:

- Upload the image processing source code.
- Configure environment variables.
- Set appropriate timeout and memory values.
- Configure permissions for AWS resources.

The following environment variables are used:

```
INPUT_BUCKET
OUTPUT_BUCKET
METADATA_TABLE
MAX_WIDTH
JPEG_QUALITY
THUMB_SIZE
```

These settings allow the Lambda function to operate flexibly without modifying the source code whenever the system configuration changes.

---

#### 5.4.3. Test the Image Processing Workflow

After completing the configuration, test the entire workflow:

1. Upload an image to the Input Bucket.
2. Verify that Lambda is triggered.
3. Check the logs in CloudWatch.
4. Verify the optimized image in the Output Bucket.
5. Verify the metadata in DynamoDB.

Expected results:

- Lambda processes the image successfully.
- The output image is generated with the correct structure.
- Metadata is stored completely.
- The processing status is updated:

```
status = SUCCESS
```

---

### The Role of Lambda in the System

Using AWS Lambda enables the system to:

- Automate the image processing workflow.
- Scale as the number of images increases.
- Eliminate the need to maintain a dedicated processing server.
- Integrate seamlessly with Amazon S3 and DynamoDB.
- Reduce operational costs through a pay-per-execution pricing model.

After the Lambda deployment is completed, the system is capable of automatically receiving user-uploaded images and performing the entire image optimization process on the AWS platform.

#### Contents

- [Create an AWS Lambda Function](5.4.1-create-lambda/)
- [Configure the AWS Lambda Function](5.4.2-configure-lambda/)
- [Test the Image Processing Workflow with AWS Lambda](5.4.3-test-upload/)