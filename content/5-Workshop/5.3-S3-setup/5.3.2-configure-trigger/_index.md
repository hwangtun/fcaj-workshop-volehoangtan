---
title: "Configure S3 Trigger for AWS Lambda"
date: 2026-08-04
weight: 2
chapter: false
pre: " <b> 5.3.2 </b> "
---

#### In the **Automatic Image Optimization System on AWS**, AWS Lambda is used to automatically process images after users upload them to the system.

To invoke Lambda automatically, the system uses **Amazon S3 Event Notification**. Whenever a new image is uploaded to the Input Bucket, Amazon S3 sends an event to AWS Lambda to initiate the image optimization workflow.

Processing Workflow:

1. The user uploads an image to the **Input Bucket**.
2. Amazon S3 generates an **Object Created** event.
3. The S3 Trigger invokes AWS Lambda.
4. AWS Lambda performs the following tasks:
   - Reads the image from the Input Bucket.
   - Optimizes the image size and file size.
   - Converts the image format according to the configured settings.
   - Generates a thumbnail.
   - Stores the processed image in the Output Bucket.
   - Stores the processing information in DynamoDB.

---

### 1. Open the AWS Lambda Console

Access [AWS Lambda Console](https://console.aws.amazon.com/lambda/)

From the list of Lambda functions, select the image processing function:

![lambda-function](../../../images/5-Workshop/5.3-S3-setup/lambda-function.png)

---

### 2. Add a Trigger to the Lambda Function

In the Lambda interface, select Function overview, then click Add trigger.

![add-trigger](../../../images/5-Workshop/5.3-S3-setup/add-trigger.png)

---

### 3. Select Amazon S3 as the Trigger

In the **Trigger configuration**, select Amazon S3 as the trigger source:

![select-s3-trigger](../../../images/5-Workshop/5.3-S3-setup/select-s3-trigger.png)

---

### 4. Configure the S3 Trigger

In the **Bucket** field, select the Input Bucket created in the previous step:

```
auto-images-input-bucket
```

In the **Event type** field, select:

```
All object create events
```

This event is triggered whenever a new object is created in the bucket.

![s3-trigger-config](../../../images/5-Workshop/5.3-S3-setup/s3-trigger-config.png)

---

### 5. Configure File Filters

To limit the files processed by AWS Lambda, you can configure filters based on the file path or file format.

Example:

#### Prefix

```
uploads/
```

Only files located in the `uploads` folder will be processed.

Example:

```
uploads/user001/image01.jpg
```

#### Suffix

```
.jpg
```

The filter can also be applied to other supported image formats:

```
.jpg
.jpeg
.png
.webp
```

{{% notice note %}}
In addition to using S3 Trigger filters, AWS Lambda also validates the image format and file size before processing to ensure that the input data is valid.
{{% /notice %}}

---

### 6. Grant Permission for Amazon S3 to Invoke AWS Lambda

When an S3 Trigger is created, AWS automatically creates the required permission allowing Amazon S3 to invoke the Lambda function.

Verify it at:

```
Lambda
 → Configuration
 → Permissions
 → Resource-based policy statements
```

The policy will include the following permission:

```
s3.amazonaws.com
    lambda:InvokeFunction
```

![lambda-permission](../../../images/5-Workshop/5.3-S3-setup/lambda-permission.png)

---

### 7. Verify That the Trigger Has Been Created

After the configuration is completed successfully, the connection will be displayed in the **Function overview** section of the Lambda function:

![trigger-complete](../../../images/5-Workshop/5.3-S3-setup/trigger-complete.png)

---

### 8. Verify the Processing Workflow

Upload a test image to the Input Bucket.

Example:

```
uploads/user001/test-image.jpg
```

After the upload, the system performs the following steps:

1. Amazon S3 generates an Object Created event.
2. AWS Lambda is invoked.
3. AWS Lambda downloads the image from the Input Bucket.
4. The image is optimized.
5. The processed image is stored in the Output Bucket.
6. The processing metadata is stored in DynamoDB.

Data structure after processing:

```
auto-images-output-bucket

optimized/
 └── user001/
      └── batchId/
            └── test-image.webp

thumbnails/
 └── user001/
      └── batchId/
            └── test-image.webp
```

The processing information is stored in DynamoDB:

```
ImageMetadata

├── batchId
├── processingId
├── originalName
├── processedSize
├── compressionRatio
├── processingTimeMs
└── status = SUCCESS
```

Therefore, the S3 Trigger enables the system to automatically invoke AWS Lambda immediately after users complete the image upload process, ensuring that the image optimization workflow is fully automated and requires no manual intervention.
