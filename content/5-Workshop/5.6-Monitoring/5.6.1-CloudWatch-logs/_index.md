---
title: "Monitoring Lambda Logs with Amazon CloudWatch"
date: 2026-08-04
weight: 1
chapter: false
pre: " <b> 5.6.1 </b> "
---

---

In the **Automatic Image Optimization System on AWS**, Amazon CloudWatch is used to monitor the execution process of AWS Lambda and store logs generated during image processing.

CloudWatch Logs helps administrators:

- Verify whether Lambda has been triggered successfully.
- Monitor each step during the image processing process.
- Detect and analyze errors when issues occur.
- Evaluate Lambda processing performance.

---

### 1. Access CloudWatch Console

Open:

```
AWS Management Console
```

![AWSConsole](../../../images/5-Workshop/5.6-Monitoring/sns-notification/aws_console.jpg)

Search for the service:

```
CloudWatch
```

![AWSConsole](../../../images/5-Workshop/5.6-Monitoring/cloudwatch-logs/aws_search_cw.jpg)

In the left navigation panel, select:

```
Logs
    |
    +--> Log groups
```

![cloudwatch-console](../../../images/5-Workshop/5.6-Monitoring/cloudwatch-logs/aws_console.jpg)

---

### 2. Check Lambda Log Group

When an AWS Lambda function is created, AWS automatically creates a corresponding Log Group in CloudWatch.

Select the Log Group:

```
/aws/lambda/image-optimizer-lambda
```

This Log Group contains all logs generated during Lambda execution.

![log-group](../../../images/5-Workshop/5.6-Monitoring/sns-notification/cw_log.jpg)

---

### 3. Check Log Stream

Inside the Log Group:

Select:

```
Log streams
```

Each time Lambda is invoked, a new Log Stream is created.

The Log Stream contains information about each execution.

Example:

```
2026/08/04/[$LATEST]xxxxxxxx
```

![log-stream](../../../images/5-Workshop/5.6-Monitoring/cloudwatch-logs/log_stream.jpg)

---

### 4. Check Lambda Execution Logs

Open a Log Stream to view details of the processing workflow.

Example log:

```
START RequestId: xxxx

Received S3 Event

Processing image:
sample-image.jpg

Downloading image from S3...

Optimizing image...

Generating thumbnail...

Uploading result to S3...

Saving metadata to DynamoDB...

END RequestId: xxxx
```

Important information:

| Log              | Description                  |
| ---------------- | ---------------------------- |
| START            | Lambda execution starts      |
| RequestId        | ID of the Lambda invocation  |
| Processing image | Image file being processed   |
| Uploading result | Upload processed image to S3 |
| Saving metadata  | Save metadata to DynamoDB    |
| END              | Lambda execution completed   |

![execution-log](../../../images/5-Workshop/5.6-Monitoring/cloudwatch-logs/log_event.jpg)

---

### 5. Check Errors During Processing

When Lambda encounters an error, CloudWatch records the error information.

Example:

```
ERROR

Image processing failed

Exception:
Unsupported image format
```

![Log-Error](../../../images/5-Workshop/5.6-Monitoring/cloudwatch-logs/cw_log_error.jpg)

Errors that can be detected:

- Invalid image files.
- No permission to access S3.
- Unable to write data to DynamoDB.
- Errors during image processing using Pillow.

CloudWatch helps identify the cause of problems for troubleshooting.

---

### 6. Check Lambda Metrics

Besides Logs, CloudWatch also provides Metrics to monitor Lambda.

Important metrics:

```
Invocations
Errors
Duration
Throttles
```

Meaning:

| Metric      | Description                        |
| ----------- | ---------------------------------- |
| Invocations | Number of times Lambda is invoked  |
| Errors      | Number of failed Lambda executions |
| Duration    | Execution time                     |
| Throttles   | Number of requests being limited   |

These metrics help evaluate system performance and scalability.

---

### 7. Verify Monitoring Results

After uploading an image and Lambda successfully processes it:

CloudWatch displays logs:

```
START RequestId

Processing image

Image optimization completed

Saving metadata to DynamoDB

END RequestId
```

![Log](../../../images/5-Workshop/5.6-Monitoring/cloudwatch-logs/log_success.jpg)

This confirms:

- S3 Trigger successfully invokes Lambda.
- Lambda performs image processing.
- Metadata is stored in DynamoDB.
- The processing workflow is completed successfully.

---

### 8. Result

After this step, the system has been configured and verified for monitoring using Amazon CloudWatch.

Achieved results:

- Lambda automatically creates logs after each execution.
- The image processing workflow can be monitored.
- Errors can be detected and analyzed.
- Monitoring data is available for system supervision.

Monitoring workflow:

```
Amazon S3
     |
     |
S3 Event Trigger
     |
     v
AWS Lambda
     |
     |
CloudWatch Logs
     |
     |
Administrator
```

The next step is to configure **Amazon SNS Notification** to send automatic alerts when system errors occur.
