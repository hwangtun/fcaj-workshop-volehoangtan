---
title: "Create an AWS Lambda Function"
date: 2026-08-04
weight: 1
chapter: false
pre: " <b> 5.4.1 </b> "
---

In the **Automatic Image Optimization System on AWS**, AWS Lambda is used to perform automatic image processing after users upload images to Amazon S3.

The Lambda function is responsible for:

- Receiving events from Amazon S3.
- Reading images from the Input Bucket.
- Optimizing images using an image processing library.
- Generating thumbnail images.
- Saving the processed images to the Output Bucket.
- Storing processing metadata in DynamoDB.

---

### 1. Access the AWS Lambda Console

Open the [AWS Lambda Console](https://console.aws.amazon.com/lambda/)

In the left navigation pane, select:

```
Functions
```

Then select:

```
Create function
```

to create a new Lambda function.

![lambda-console](../../../images/5-Workshop/5.4-Lambda-deployment/lambda-console.png)

---

### 2. Choose the Lambda Function Creation Method

On the **Create function** page, select:

```
Author from scratch
```

This option creates a Lambda function using source code developed specifically for the system.

![author-from-scratch](../../../images/5-Workshop/5.4-Lambda-deployment/author-from-scratch.png)

---

### 3. Configure the Lambda Function

In the **Basic information** section, configure the following:

#### Function name

Enter the Lambda function name:

```
autoImageProcessing
```

This name is used to identify the image processing function in the system.

![function-name](../../../images/5-Workshop/5.4-Lambda-deployment/function-name.png)

---

#### Runtime

Select the runtime:

```
Python 3.14
```

The Lambda function uses Python for image processing with the following libraries:

- boto3: interacts with AWS services.
- Pillow (PIL): processes and optimizes images.

![lambda-runtime](../../../images/5-Workshop/5.4-Lambda-deployment/lambda-runtime.png)

![create-function](../../../images/5-Workshop/5.4-Lambda-deployment/create-function.png)

---

### 4. Configure the Lambda Execution Role

After the Lambda function has been created successfully, navigate to:

```
Configuration
→ Permissions
```

In the:

```
Execution role
```

section, select:

```
Edit
```

Then choose:

```
Use existing role
```

Select the IAM role that was created previously:

```
LambdaExecutionRole
```

This role includes the permissions required for image processing:

- Amazon S3:
  - Read images from the Input Bucket.
  - Write optimized images and thumbnails to the Output Bucket.

- Amazon DynamoDB:
  - Store processing metadata.

- AWS KMS:
  - Access resources encrypted with KMS.

- CloudWatch Logs:
  - Record Lambda execution logs.

{{% notice note %}}
Lambda uses an Execution Role to access other AWS services during processing. Permissions are granted according to the Principle of Least Privilege, providing only the permissions required for the image processing functionality.
{{% /notice %}}

---

### 5. Verify the Lambda Function

After creating the Lambda function and configuring the Execution Role successfully, the Lambda console displays:

```
Function name:

autoImageProcessing
```

Key information:

```
Runtime:
Python 3.14

Architecture:
x86_64

Execution role:
LambdaExecutionRole
```

---

### 6. Result

At this point, the system has a Lambda function ready to perform image processing.

Deployment structure:

```
Input S3 Bucket
|
| Object Created Event
|
v
autoImageProcessing Lambda
|
+--> Download Original Image
|
+--> Optimize Image
|
+--> Generate Thumbnail
|
+--> Upload Optimized Image
|
+--> Upload Thumbnail
|
+--> Save Metadata To DynamoDB
|
+--> Write Logs To CloudWatch
```

The Lambda function will be further configured with the source code, environment variables, and the S3 Event Trigger in the following steps.
