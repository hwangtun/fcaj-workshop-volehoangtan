---
title: "Configure Amazon S3"
date: 2026-08-04
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

#### Configuring Amazon S3

In this section, the team configures **Amazon** S3 to store the images used by the system. Two S3 Buckets are used: the **Input Bucket** and the **Output Bucket**.

- The **Input Bucket** is used to store images uploaded by users. When a new image is uploaded, Amazon S3 generates an **ObjectCreated** event to trigger AWS Lambda and initiate the image processing workflow.
- The **Output Bucket** is used to store the optimized images and the thumbnails generated during the processing workflow.

Using Amazon S3 enables the system to store data with high durability, flexible scalability, and seamless integration with other AWS services.

#### Contents

- [Create S3 Buckets for Image Storage](5.3.1-create-buckets/)
- [Configure an S3 Trigger for AWS Lambda](5.3.2-configure-trigger/)
