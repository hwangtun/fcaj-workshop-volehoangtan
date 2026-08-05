---
title: "Worklog Week 3"
date: 2026-06-29
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Objectives

* Complete the image processing features on AWS Lambda.
* Resolve deployment issues related to the Pillow library.
* Test the automated image processing workflow.
* Prepare metadata for future system enhancements.

### Tasks

| Day | Task | Start Date | Finish Date | Reference |
| --- | --- | --- | --- | --- |
| Mon | - Test AWS Lambda with different image formats.<br>- Verify image resizing and compression functions.<br>- Evaluate output image quality. | 29/06/2026 | 29/06/2026 | AWS Lambda Documentation |
| Tue | - Troubleshoot Lambda Layer issues when integrating Pillow.<br>- Study the differences between the local development environment and the AWS Lambda runtime.<br>- Experiment with different library packaging methods. | 30/06/2026 | 30/06/2026 | Pillow Documentation |
| Wed | - Use Docker to build a Lambda Layer compatible with the required Python runtime.<br>- Verify successful Pillow imports in AWS Lambda. | 01/07/2026 | 01/07/2026 | AWS Lambda Layers |
| Thu | - Complete the thumbnail generation feature.<br>- Store processed images in the Output Bucket.<br>- Verify the folder structure in Amazon S3. | 02/07/2026 | 03/07/2026 | Amazon S3 Documentation |
| Fri | - Design metadata for image processing records.<br>- Define fields such as Batch ID, Processing ID, processing status and image size.<br>- Summarize system testing results. | 04/07/2026 | 05/07/2026 | AWS Best Practices |

### Results

* Successfully resolved the deployment issues related to the Pillow library on AWS Lambda.
* Built a Lambda Layer compatible with the AWS Python runtime.
* Completed the core image processing features:
  * Image resizing.
  * Image compression.
  * Thumbnail generation.
* Successfully stored processed images in the Amazon S3 Output Bucket.
* Verified the complete automated processing workflow from Amazon S3 to AWS Lambda.
* Designed a metadata structure to support image processing history and future database integration.