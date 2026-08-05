---
title: "Monitoring System with CloudWatch and SNS"
date: 2026-08-04
weight: 6
chapter: false
pre: " <b> 5.6 </b> "
---

---

In the **Automatic Image Optimization System on AWS**, Monitoring is implemented to monitor system activities, verify the image processing workflow, and send alerts when errors occur.

The system uses two main AWS services:

- **Amazon CloudWatch Logs**: Stores and monitors AWS Lambda execution logs.
- **Amazon SNS**: Sends alert notifications to administrators when errors are detected.

Implementing Monitoring helps the system easily detect issues, support debugging, and ensure that the image processing workflow operates reliably.

---

### Monitoring Architecture Overview

System monitoring workflow:

```
User Upload Image
        |
        v
Amazon S3 Input Bucket
        |
        v
S3 Event Trigger
        |
        v
AWS Lambda
        |
        +----------------+
        |                |
        v                v
Amazon S3 Output    DynamoDB
        |
        |
        v
CloudWatch Logs
        |
        |
        v
CloudWatch Alarm
        |
        |
        v
Amazon SNS
        |
        |
        v
Administrator
```

---

### Amazon CloudWatch Logs

Amazon CloudWatch Logs is used to record the entire execution process of AWS Lambda.

Whenever Lambda is triggered to process an image, the system automatically creates logs containing information such as:

- Processing start and completion time.
- Lambda Request ID.
- Information about the image file being processed.
- Processing results.
- Error information if an issue occurs.

Example log:

```
START RequestId: xxxx

Processing image:
sample-image.jpg

Optimizing image...

Uploading result to S3...

Saving metadata to DynamoDB...

END RequestId: xxxx
```

CloudWatch Logs helps:

- Verify whether Lambda has been triggered.
- Monitor the image processing workflow.
- Analyze errors when the system encounters issues.

---

### Amazon SNS Notification

Amazon SNS is used to automatically send notifications to administrators when the system encounters errors.

Alert workflow:

```
AWS Lambda
      |
      |
CloudWatch Logs
      |
      |
CloudWatch Alarm
      |
      |
Amazon SNS Topic
      |
      |
Administrator
```

Cases where alerts can be sent:

- Lambda execution fails.
- Image processing encounters errors.
- Unable to access AWS resources.
- The number of errors exceeds the allowed threshold.

Example notification content:

```
Alert:

Image processing failed.

Function:
image-optimizer-lambda

Status:
FAILED
```

---

### Monitoring Deployment Steps

The Monitoring deployment process consists of two steps:

#### 5.6.1. Monitoring Lambda Logs with CloudWatch

Configure CloudWatch Logs to:

- Check Lambda Function logs.
- Monitor image processing status.
- View detailed errors when Lambda fails.

Expected results:

- Lambda automatically records logs after each execution.
- Processing history can be retrieved.

---

#### 5.6.2. Configure SNS Notification

Configure Amazon SNS to:

- Create an SNS Topic.
- Register an email address to receive notifications.
- Connect alerts with the Monitoring system.
- Test notification delivery.

Expected results:

- Administrators receive alerts when Lambda encounters errors.

---

### Role of Monitoring in the System

Implementing CloudWatch and SNS helps the system:

- Monitor AWS Lambda activities.
- Track the image processing workflow.
- Detect errors quickly.
- Support maintenance and troubleshooting.
- Improve system reliability.

After completing Monitoring, the **Automatic Image Optimization System on AWS** is capable of automatically processing images, storing metadata, and monitoring the entire operational workflow.

---

#### Contents

- [Monitoring Lambda Logs with Amazon CloudWatch](5.6.1-cloudwatch-logs/)
- [Configure SNS Notification for Alerts](5.6.2-sns-notification/)
