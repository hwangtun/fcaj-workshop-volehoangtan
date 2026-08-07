---
title: "Cleaning Up AWS Resources After Deployment"
date: 2026-08-04
weight: 8
chapter: false
pre: " <b> 5.8 </b> "
---

After completing the deployment of the **Automatic Image Optimization System on AWS**, it is recommended to clean up any AWS resources that are no longer needed. This helps prevent unexpected charges and ensures that the AWS environment remains well organized and efficiently managed.

Performing resource cleanup provides the following benefits:

- Removes resources created during testing.
- Reduces AWS operating costs.
- Prevents unnecessary background services from running.
- Keeps the AWS account organized and secure.

---

### AWS Resources to Review Before Deletion

The following AWS resources were deployed as part of this system:

```text
AWS Resources

├── Amazon S3
│   ├── Input Bucket
│   └── Output Bucket
│
├── AWS Lambda
│   └── image-optimizer-lambda
│
├── Amazon DynamoDB
│   └── ImageMetadata
│
├── Amazon CloudWatch
│   └── Lambda Logs
│
├── Amazon SNS
│   └── Notification Topic
│
└── IAM
    └── Lambda Execution Role
```

---

### 1. Delete Data from the Amazon S3 Buckets

Before deleting an Amazon S3 bucket, all objects stored in the bucket must be removed.

The buckets to review are:

```text
auto-images-input-bucket

auto-images-output-bucket
```

Follow these steps:

1. Open the **Amazon S3 Console**.

![s3](../../images/5-Workshop/5.8-Cleanup/s3_console.jpg)

2. Select the bucket to delete.

![s3](../../images/5-Workshop/5.8-Cleanup/s3_bucket_select.jpg)

3. Select all objects in the bucket.

![s3](../../images/5-Workshop/5.8-Cleanup/s3_object.jpg)

4. Select:

```text
Delete
```

5. Confirm the deletion of all objects.

![s3](../../images/5-Workshop/5.8-Cleanup/s3_delete_confirm.jpg)

{{% notice warning %}}
Carefully review the data before deletion. Once objects are deleted from Amazon S3, they may not be recoverable.
{{% /notice %}}

---

### 2. Delete the Amazon S3 Buckets

After all objects have been removed, delete the buckets.

Select:

```text
Delete bucket
```

![s3](../../images/5-Workshop/5.8-Cleanup/s3_delete_bucket.jpg)

Enter the bucket name to confirm the deletion.

Example:

```text
auto-images-input-bucket

auto-images-output-bucket
```

---

### 3. Delete the AWS Lambda Function

Open:

```text
AWS Lambda Console
```

![Lambda](../../images/5-Workshop/5.8-Cleanup/s3_lambda_console.jpg)

Select the function:

```text
image-optimizer-lambda
```

![Lambda](../../images/5-Workshop/5.8-Cleanup/s3_lambda_select.jpg)

Then choose:

```text
Actions
    └── Delete function
```

![Lambda](../../images/5-Workshop/5.8-Cleanup/s3_lambda_delete_confirm.jpg)

After confirmation, the Lambda function will be removed from the system.

---

### 4. Delete the Amazon DynamoDB Table

Open:

```text
Amazon DynamoDB
```

![DynamoDB](../../images/5-Workshop/5.8-Cleanup/db_console.jpg)

Select the table:

```text
ImageMetadata
```

![DynamoDB](../../images/5-Workshop/5.8-Cleanup/db_select.jpg)

Choose:

```text
Delete table
```

Confirm the operation:

![DynamoDB](../../images/5-Workshop/5.8-Cleanup/db_delete_confirm.jpg)

```text
Delete
```

After the table is deleted, all metadata stored in it will be permanently removed.

---

### 5. Delete CloudWatch Logs

Even after the Lambda function has been deleted, log groups in Amazon CloudWatch may still remain and continue to incur storage charges.

Open:

```text
Amazon CloudWatch
```

![CloudWatch](../../images/5-Workshop/5.8-Cleanup/cw_console.jpg)

Select:

```text
Logs
    └── Log groups
```

![CloudWatch](../../images/5-Workshop/5.8-Cleanup/cw_select.jpg)

Delete the log group:

![CloudWatch](../../images/5-Workshop/5.8-Cleanup/cw_log_delete.jpg)

```text
/aws/lambda/image-optimizer-lambda
```

---

### 6. Delete the Amazon SNS Topic

Open:

```text
Amazon SNS
```

![SNS](../../images/5-Workshop/5.8-Cleanup/sns_console.jpg)

Select the topic:

```text
image-processing-alerts
```

![SNS](../../images/5-Workshop/5.8-Cleanup/sns_select.jpg)

Choose:

```text
Delete topic
```

![SNS](../../images/5-Workshop/5.8-Cleanup/sns_delete_confirm.jpg)

After the topic is deleted, all subscriptions associated with it will also be removed.

---

### 7. Review the IAM Role

After deleting the Lambda function, review the IAM role:

```text
image-optimizer-lambda-role
```

If the role is no longer used by any AWS Lambda function or other AWS service, it can be deleted to eliminate unnecessary permissions.

{{% notice note %}}
Do not delete an IAM role if it is still being used by another AWS Lambda function or AWS service.
{{% /notice %}}

---

### 8. Review AWS Billing

After completing the cleanup process, review your AWS charges.

Open:

```text
AWS Billing Console
```

![Billing](../../images/5-Workshop/5.8-Cleanup/aws_bill.jpg)

Review the following services:

- Amazon S3 Storage
- AWS Lambda Requests
- Amazon DynamoDB Usage
- Amazon CloudWatch Logs
- Amazon SNS Requests

Ensure that no unnecessary AWS resources remain that could continue to generate charges.

---

### Result

After completing the resource cleanup process:

- Resources created during testing have been removed.
- No unnecessary AWS services remain running.
- The risk of unexpected AWS charges has been reduced.
- The AWS environment has been returned to a clean state and is ready for future deployments.

The complete deployment workflow of the system is as follows:

```text
5.3 S3 Deployment
        │
        ▼
5.4 Lambda Deployment
        │
        ▼
5.5 DynamoDB Deployment
        │
        ▼
5.6 Monitoring
        │
        ▼
5.7 Cleanup
```

At this point, the **Automatic Image Optimization System on AWS** has completed the entire lifecycle, from deployment and testing to monitoring and resource cleanup on the AWS platform.
