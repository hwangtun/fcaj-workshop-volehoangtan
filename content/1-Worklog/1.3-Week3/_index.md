---
title: "Worklog Week 3"
date: 2026-07-06
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Week 3 Objectives

* Complete the image processing functions on AWS Lambda.
* Resolve issues related to deploying the Pillow library.
* Test the automated image processing workflow.
* Prepare output metadata for future system enhancements.

### Tasks Performed This Week

| Day | Tasks | Start Date | Completion Date | References |
| --- | --- | --- | --- | --- |
| Mon | - Tested AWS Lambda with different image formats.<br>- Verified image resizing and compression features.<br>- Evaluated the quality of processed images. | 06/07/2026 | 06/07/2026 | AWS Lambda Documentation |
| Tue | - Resolved Lambda Layer issues when integrating the Pillow library.<br>- Studied the differences between the local development environment and the AWS Lambda runtime.<br>- Tested multiple dependency packaging approaches. | 07/07/2026 | 07/07/2026 | Pillow Documentation |
| Wed | - Used Docker to build a Lambda Layer compatible with the Python runtime.<br>- Verified successful Pillow import on AWS Lambda. | 08/07/2026 | 08/07/2026 | AWS Lambda Layers Documentation |
| Thu | - Completed the thumbnail generation feature.<br>- Saved processed images to the Amazon S3 Output Bucket.<br>- Verified the storage structure of processed images in Amazon S3. | 09/07/2026 | 10/07/2026 | Amazon S3 Documentation |
| Fri | - Designed the metadata structure for image processing records.<br>- Defined metadata fields such as Batch ID, Processing ID, processing status, and image size.<br>- Summarized system testing results. | 11/07/2026 | 12/07/2026 | AWS Well-Architected Framework |

### Week 3 Achievements

* Successfully resolved deployment issues related to the Pillow library on AWS Lambda.
* Built a Lambda Layer compatible with the AWS Python runtime.
* Completed the core image processing features:
  * Image resizing.
  * Image compression.
  * Thumbnail generation.
* Completed the workflow for storing processed images in the Output Bucket.
* Successfully tested the automated processing workflow from Amazon S3 to AWS Lambda.
* Designed a metadata structure to support image processing history and future integration with Amazon DynamoDB.