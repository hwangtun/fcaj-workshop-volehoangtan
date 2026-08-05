---
title: "Worklog Week 2"
date: 2026-06-22
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---


### Objectives

* Build the image storage environment using Amazon S3.
* Develop an AWS Lambda function for automatic image processing.
* Study the Pillow library for image optimization.
* Test the automated image processing workflow on AWS.

### Tasks

| Day | Task | Start Date | Finish Date | Reference |
| --- | --- | --- | --- | --- |
| Mon | - Create Amazon S3 Input and Output Buckets.<br>- Configure bucket permissions.<br>- Test image upload and download. | 22/06/2026 | 22/06/2026 | https://docs.aws.amazon.com/AmazonS3 |
| Tue | - Create an AWS Lambda function using Python.<br>- Perform basic Lambda tests.<br>- Learn how to use CloudWatch Logs for monitoring. | 23/06/2026 | 23/06/2026 | https://docs.aws.amazon.com/lambda |
| Wed | - Study the Pillow image processing library.<br>- Learn how to package external libraries using Lambda Layers.<br>- Prepare the image processing environment. | 24/06/2026 | 24/06/2026 | https://pillow.readthedocs.io |
| Thu | - Implement image resizing.<br>- Apply JPEG compression.<br>- Configure appropriate output image dimensions. | 25/06/2026 | 26/06/2026 | AWS Lambda Developer Guide |
| Fri | - Configure Amazon S3 Event Trigger.<br>- Connect Amazon S3 with AWS Lambda.<br>- Test the Upload → Lambda → Output Bucket workflow. | 27/06/2026 | 28/06/2026 | AWS Documentation |

### Results

* Successfully deployed Amazon S3 Input and Output Buckets.
* Developed an AWS Lambda function using Python.
* Learned how to monitor Lambda execution through Amazon CloudWatch Logs.
* Integrated the Pillow library into AWS Lambda for image processing.
* Implemented the core image optimization features:
  * Image resizing.
  * JPEG compression.
  * Prepared the system for thumbnail generation in the next phase.
* Successfully configured Amazon S3 Event Triggers to invoke AWS Lambda automatically after image uploads.
* Verified the complete workflow from image upload to processed image storage in the Output Bucket.