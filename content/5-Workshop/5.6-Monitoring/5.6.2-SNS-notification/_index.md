---
title: "Configure SNS Notification for Alerts"
date: 2026-08-04
weight: 2
chapter: false
pre: " <b> 5.6.2 </b> "
---

---

In the **Automatic Image Optimization System on AWS**, Amazon SNS is used to send alert notifications to administrators when errors occur during the image processing workflow.

SNS enables the system to send automatic notifications instead of requiring administrators to continuously check logs on CloudWatch.

Cases that require sending alerts:

- AWS Lambda image processing fails.
- Unable to read images from S3.
- Unable to write metadata to DynamoDB.
- Errors occur during image optimization.

---

### 1. Overview of Notification Flow

SNS operation flow:

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
Email Notification
      |
      |
Administrator
```

When the system detects an error:

1. Lambda records the error during processing.
2. CloudWatch monitors the execution status.
3. SNS sends alerts to administrators.

---

### 2. Access Amazon SNS Console

Open:

```
AWS Management Console
```

![AWSConsole](../../../images/5-Workshop/5.6-Monitoring/sns-notification/aws_console.jpg)

Search for the service:

```
Simple Notification Service (SNS)
```

![AWSConsole](../../../images/5-Workshop/5.6-Monitoring/sns-notification/search_sns.jpg)

In the left navigation panel, select:

```
Topics
```

![SNSConsole](../../../images/5-Workshop/5.6-Monitoring/sns-notification/sns_step1.jpg)

Then select:

```
Create topic
```

to create a new SNS Topic.

![SNSConsole](../../../images/5-Workshop/5.6-Monitoring/sns-notification/sns_step2.jpg)

---

### 3. Create SNS Topic

In the interface:

```
Create topic
```

Select the type:

```
Standard
```

Configuration:

#### Name

Enter:

```
image-processing-alerts
```

This Topic is used to manage system alert notifications.

---

#### Display name

Enter:

```
Image Processing Alert
```

This helps easily identify the purpose of the Topic.

![create-topic](../../../images/5-Workshop/5.6-Monitoring/sns-notification/create_topic.jpg)

---

### 4. Create Subscription to Receive Notifications

After creating the Topic, select:

```
Create subscription
```

Configuration:

## Protocol

Select:

```
Email
```

---

## Endpoint

Enter the administrator's email address that receives alerts.

Example:

```
admin@example.com
```

Then select:

```
Create subscription
```

![create-subscription](../../../images/5-Workshop/5.6-Monitoring/sns-notification/create_sub.jpg)

---

### 5. Confirm Email Subscription

AWS SNS sends a confirmation email to the registered email address.

Open the email and select:

```
Confirm subscription
```

![Mail](../../../images/5-Workshop/5.6-Monitoring/sns-notification/mail_confirm.jpg)

After confirmation, the Subscription status changes to:

```
Confirmed
```

![confirm-subscription](../../../images/5-Workshop/5.6-Monitoring/sns-notification/sub_confirm.jpg)

---

### 6. Check SNS Topic

After completing the configuration, the SNS Topic displays:

```
Topic name:

image-processing-alerts
```

Subscription:

```
Protocol:

Email

Status:

Confirmed
```

![topic-created](../../../images/5-Workshop/5.6-Monitoring/sns-notification/sub_status.jpg)

---

### 7. Configure Alert Notifications from CloudWatch

To automatically send notifications when the Lambda Function encounters errors, create a **CloudWatch Alarm** based on the Lambda **Errors** metric and configure it to send notifications to **Amazon SNS**.

#### Step 1. Access CloudWatch

Access:

```text
Amazon CloudWatch
```

![loudWatch](../../../images/5-Workshop/5.6-Monitoring/sns-notification/cw_console.jpg)

Select:

```text
Alarms
    └── Create alarm
```

![CloudWatch](../../../images/5-Workshop/5.6-Monitoring/sns-notification/create_alarm.jpg)

---

#### Step 2. Select Metric

Select the Metric using the following path:

```text
Lambda
    └── By Function Name
            └── image-optimizer-lambda
                    └── Errors
```

![CloudWatch](../../../images/5-Workshop/5.6-Monitoring/sns-notification/metric_lambda.jpg)

---

#### Step 3. Configure Conditions

Set the alert condition:

```text
Metric

Errors

Condition

Errors >= 1
```

![CloudWatch](../../../images/5-Workshop/5.6-Monitoring/sns-notification/alarm_condition.jpg)

---

#### Step 4. Select SNS Topic

In the **Notifications** section, select the previously created SNS Topic:

```text
image-processing-alerts
```

![CloudWatch](../../../images/5-Workshop/5.6-Monitoring/sns-notification/cw_sns_select.jpg)

---

#### Step 5. Complete Alarm Creation

Set the Alarm name, review the configuration, and select **Create alarm**.

After successful creation, the Alarm status will be:

```text
OK
```

![CloudWatch](../../../images/5-Workshop/5.6-Monitoring/sns-notification/alarm_name.jpg)

When the Lambda Function generates an error, the processing flow is:

```text
Lambda Error
      │
      ▼
CloudWatch Metric (Errors)
      │
      ▼
CloudWatch Alarm
      │
      ▼
Amazon SNS
      │
      ▼
Administrator Email
```

---

### 8. Test Notification Sending

After completing the CloudWatch Alarm and Amazon SNS configuration, test the system's notification capability.

#### Step 1. Create an Error

An error can be created using one of the following methods:

- Upload an unsupported image format.
- Remove Lambda Function permission to access Amazon S3.
- Intentionally create an Exception in the Lambda source code for testing.

---

#### Step 2. Check CloudWatch Logs

After Lambda encounters an error, access:

```text
Amazon CloudWatch

Logs

Log groups

/aws/lambda/image-optimizer-lambda
```

![CloudWatch](../../../images/5-Workshop/5.6-Monitoring/sns-notification/cw_log.jpg)

CloudWatch records error information, for example:

```text
ERROR

Image processing failed
```

![CloudWatch](../../../images/5-Workshop/5.6-Monitoring/sns-notification/cw_log_error.jpg)

---

#### Step 3. Check Alarm Status

After CloudWatch collects the Metric, the Alarm changes from:

```text
OK
```

to:

```text
In alarm
```

![CloudWatch](images/5-Workshop/5.6-Monitoring/sns-notification/alarm_trigger.jpg)

---

#### Step 4. Check Email Notification

Amazon SNS automatically sends an email notification to the registered address.

Example:

```text
Subject

AWS Notification
```

```text
Message

Image processing failed.

Function:
image-optimizer-lambda

Status:
FAILED
```

![SNS](../../../images/5-Workshop/5.6-Monitoring/sns-email.jpg)

The result shows that the system can automatically detect Lambda Function errors and successfully send notifications through Amazon SNS.

---

### 9. Result

After completing the Amazon SNS configuration, the system can automatically send alerts when errors occur.

Achieved results:

- SNS Topic is created successfully.
- Administrator email is registered to receive notifications.
- CloudWatch can be connected to send alerts.
- Supports faster incident detection and troubleshooting.

Complete Monitoring Flow:

```
                AWS Lambda
                     |
                     |
              Image Processing
                     |
          +----------+----------+
          |                     |
          v                     v
   CloudWatch Logs        DynamoDB
          |
          |
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

After this step, the **Automatic Image Optimization System on AWS** has completed the monitoring and automatic alerting capability during the image processing workflow.
