---
title: "Worklog Week 2"
date: 2026-06-29
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Week 2 Objectives

* Build an image storage environment using Amazon S3.
* Deploy AWS Lambda for automatic image processing.
* Study the Pillow library for image optimization.
* Test the automated image processing workflow on AWS.

### Tasks Performed This Week

| Day | Tasks | Start Date | Completion Date | References |
| --- | --- | --- | --- | --- |
| Mon | - Created Amazon S3 Input Bucket and Output Bucket.<br>- Configured bucket permissions and access policies.<br>- Performed image upload and download tests. | 29/06/2026 | 29/06/2026 | AWS S3 Documentation |
| Tue | - Created an AWS Lambda function using Python.<br>- Performed basic Lambda testing.<br>- Learned to use Amazon CloudWatch Logs for monitoring and debugging. | 30/06/2026 | 30/06/2026 | AWS Lambda Documentation |
| Wed | - Studied the Pillow image processing library.<br>- Learned how to package dependencies using Lambda Layers.<br>- Prepared the image processing environment for Lambda. | 01/07/2026 | 01/07/2026 | Pillow Documentation |
| Thu | - Implemented image resizing functionality.<br>- Implemented JPEG image compression.<br>- Configured the output image size according to project requirements. | 02/07/2026 | 03/07/2026 | AWS Lambda Developer Guide |
| Fri | - Configured Amazon S3 Event Trigger.<br>- Connected Amazon S3 with AWS Lambda.<br>- Tested the complete workflow: Upload → Lambda → Output Bucket. | 04/07/2026 | 05/07/2026 | AWS Documentation |

### Week 2 Achievements

* Successfully deployed Amazon S3 Input Bucket and Output Bucket.
* Successfully created an AWS Lambda function using Python.
* Learned how to monitor and debug Lambda executions using Amazon CloudWatch Logs.
* Integrated the Pillow library into AWS Lambda for image processing.
* Implemented core image processing features:
  * Image resizing.
  * JPEG image compression.
  * Prepared the foundation for thumbnail generation in later stages.
* Successfully configured Amazon S3 Event Trigger to automatically invoke AWS Lambda after image uploads.
* Successfully tested the complete image processing workflow from the Input Bucket to the Output Bucket.