---
title: "Workshop"
date: 2026-08-04
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Deploying the Automatic Image Optimization System on AWS

#### Overview

The **Automatic Image Optimization System** is deployed on the AWS platform to automate image processing and optimization. The solution is built using a serverless architecture, which minimizes infrastructure management, improves scalability, and ensures reliable processing performance as the number of uploaded images increases.

In this system, users can upload images through the application. The uploaded images are stored in **Amazon S3**, which automatically triggers an **AWS Lambda** function for image processing. AWS Lambda performs optimization tasks such as image compression, resizing, and generating optimized versions of the uploaded images.

The processed images are stored in the S3 Output Bucket, while metadata generated during the processing workflow is stored in **Amazon DynamoDB**. This metadata supports history tracking, processing status monitoring, and image management.

The system also integrates monitoring and security services:

- **Amazon CloudWatch:** Monitors Lambda logs, execution status, and performance.
- **Amazon SNS:** Sends alert notifications to administrators when processing errors occur.
- **AWS IAM:** Manages permissions between AWS services according to the Principle of Least Privilege.
- **AWS KMS:** Encrypts stored data to enhance security.

#### Contents

1. [System Architecture Overview](5.1-Workshop-overview/)
2. [Preparing the AWS Environment](5.2-Prerequiste/)
3. [Deploying Amazon S3 for Image Storage](5.3-S3-setup/)
4. [Deploying AWS Lambda for Image Processing](5.4-Lambda-deployment/)
5. [Deploying Amazon DynamoDB for Metadata Storage](5.5-DynamoDB/)
6. [Monitoring the System with CloudWatch and SNS](5.6-Monitoring/)
7. [Cleaning Up AWS Resources After Deployment](5.7-Cleanup/)