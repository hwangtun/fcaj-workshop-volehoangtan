---
title: "Introduction"
date: 2026-08-04
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

#### Introduction to the Automatic Image Optimization System on AWS

The **Automatic Image Optimization System** is designed to automate the image optimization process on the AWS cloud platform. The system allows users to upload images, which are then automatically processed to reduce file size, optimize dimensions, and store the optimized results.

The solution leverages AWS's serverless architecture to minimize infrastructure management, improve scalability, and ensure reliable processing of large volumes of images.

The main AWS services used in the system include:

- **Amazon S3:** Stores both the original input images and the optimized output images.
- **AWS Lambda:** Automatically executes image processing tasks whenever a new image is uploaded to Amazon S3.
- **Amazon DynamoDB:** Stores processing metadata such as file information, processing status, execution time, and error details.
- **Amazon CloudWatch:** Monitors logs, tracks Lambda activities, and helps detect errors within the system.
- **Amazon SNS:** Sends alert notifications to administrators when errors or important events occur.
- **AWS IAM:** Manages access permissions between AWS services based on the principle of least privilege.
- **AWS KMS:** Protects data through encryption and secure key management.

### System Architecture Overview

In this system, the image processing workflow is implemented using an event-driven architecture.

- **Input S3 Bucket:** Receives images uploaded by users. When a new object is created, an Amazon S3 event trigger invokes AWS Lambda.

- **AWS Lambda Image Processing:** The Lambda function performs image optimization tasks such as resizing, compressing, and converting image formats according to the selected configuration.

- **Output S3 Bucket:** Stores the processed images, making them available for users to download.

- **Amazon DynamoDB:** Stores metadata for each processing batch, enabling the system to retrieve and track image processing history.

- **Amazon CloudWatch and Amazon SNS:** Monitor the operational status of the system and send alert notifications to administrators whenever errors occur.

- **AWS IAM and AWS KMS:** Ensure that AWS services have only the permissions they require and that data remains protected during storage.

![overview](../../images/5-Workshop/5.1-Workshop-overview/kientruc.jpg)
