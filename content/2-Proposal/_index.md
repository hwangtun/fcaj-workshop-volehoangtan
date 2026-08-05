---
title: "Proposal"
date: 2026-08-04
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

---

# Automatic Image Optimization System on AWS

## An Automatic Image Optimization Solution Using AWS Serverless Architecture

### 1. Executive Summary

The Automatic Image Optimization System is a platform that automatically optimizes images after they are uploaded by users. The system is designed for individuals and organizations that need to store large numbers of images while maintaining display quality and reducing storage consumption.

Users simply upload images through a web interface. AWS Lambda then automatically processes, compresses, and optimizes the images, generates thumbnails, and stores the results in Amazon S3. Metadata generated during the processing workflow is stored in Amazon DynamoDB for history tracking and retrieval. The system uses AWS KMS to encrypt stored data, Amazon CloudWatch for monitoring, and Amazon SNS to send notifications when processing errors occur.

The serverless architecture enables the system to scale automatically, reduce operational costs, and eliminate the need to manage application servers.

---

### 2. Problem Statement

#### Current Challenges

Many individuals and organizations need to optimize images before storing them or publishing them on websites to reduce file size while maintaining image quality. However, this process is often performed manually using image editing software or online services, making it time-consuming and difficult to manage when processing large numbers of images.

In addition, many existing solutions focus only on image compression without providing features for tracking processing history, managing metadata, or monitoring processing activities.

#### Proposed Solution

The team proposes an automatic image optimization system built on the AWS platform.

Users upload images through a web application. The Spring Boot backend stores the uploaded images in the Amazon S3 Input Bucket and creates the initial metadata record. When a new image is uploaded, Amazon S3 triggers AWS Lambda to optimize the image according to the user-selected configuration.

After processing, the optimized image and its thumbnail are stored in the Amazon S3 Output Bucket. The processing metadata is updated in Amazon DynamoDB to support processing history queries and status tracking. Amazon CloudWatch collects logs and monitors system activities, while Amazon SNS sends notifications whenever image processing fails. AWS KMS encrypts data stored in Amazon S3 to enhance security.

The system provides a web interface that allows users to:

- Register and sign in.
- Upload multiple images simultaneously.
- Track processing status.
- View processing history.
- Download processed images.

In addition, administrators can monitor system usage statistics and manage user accounts.

#### Benefits and Return on Investment (ROI)

The system significantly reduces the time required for manual image optimization while decreasing storage usage and network bandwidth consumption. Automating the entire workflow minimizes administrative effort, improves operational efficiency, and enables the system to scale easily as the number of users grows.

By leveraging AWS serverless services, operating costs are incurred only when image processing requests are executed, making the solution cost-effective for small and medium-sized applications.

---

### 3. Solution Architecture

The system adopts an AWS serverless architecture integrated with a Spring Boot backend and a React frontend.

Main processing workflow:

1. Users upload images through the web application.
2. The backend stores the images in the Amazon S3 Input Bucket.
3. Amazon S3 generates an `ObjectCreated` event.
4. AWS Lambda is triggered to process the image.
5. Lambda optimizes the image and generates a thumbnail.
6. The processed image is stored in the Amazon S3 Output Bucket.
7. Metadata is updated in Amazon DynamoDB.
8. Amazon CloudWatch records logs and monitors system activities.
9. Amazon SNS sends notifications if processing errors occur.
10. Users view their processing history and download the optimized images.

_(Insert the team's architecture diagram here.)_

#### AWS Services Used

- Amazon S3: stores original and optimized images.
- AWS Lambda: processes and optimizes images.
- Amazon DynamoDB: stores metadata and processing history.
- AWS KMS: encrypts data stored in Amazon S3.
- Amazon CloudWatch: monitors the system and collects logs.
- Amazon SNS: sends error notifications.
- AWS IAM: manages access permissions for AWS services.

#### Component Design

- **Frontend (React):** user interface.
- **Backend (Spring Boot):** authentication, image upload, and history retrieval.
- **Amazon S3:** stores input and output images.
- **AWS Lambda:** processes images using Pillow.
- **Amazon DynamoDB:** stores processing status and metadata.
- **AWS KMS:** encrypts stored data.
- **Amazon CloudWatch & Amazon SNS:** monitor the system and deliver notifications.

---

### 4. Technical Implementation

#### Implementation Phases

1. Analyze requirements and design the system architecture.
2. Develop the Spring Boot backend and REST APIs.
3. Develop the React frontend.
4. Deploy Amazon S3, AWS Lambda, Amazon DynamoDB, and AWS IAM.
5. Integrate AWS services with the backend.
6. Perform functional testing and finalize the deployment.

#### Technical Requirements

**Frontend**

- React
- TypeScript
- Tailwind CSS
- Ant Design

**Backend**

- Spring Boot
- Spring Security
- JWT Authentication
- AWS SDK for Java

**AWS**

- Amazon S3
- AWS Lambda (Python + Pillow)
- Amazon DynamoDB
- AWS IAM
- AWS KMS
- Amazon CloudWatch
- Amazon SNS

---

### 5. Implementation Timeline and Milestones

- **Week 1:** Requirements analysis and architecture design.
- **Week 2:** Backend and frontend development.
- **Week 3:** Deployment of AWS services.
- **Week 4:** System integration, testing, and final report completion.

---

### 6. Estimated Budget

The primary costs are associated with AWS services.

**Planned Infrastructure**

- Amazon S3
- AWS Lambda
- Amazon DynamoDB
- AWS KMS
- Amazon CloudWatch
- Amazon SNS

For educational and testing purposes, the estimated monthly cost is only a few US dollars by taking advantage of the AWS Free Tier and the pay-as-you-go pricing model.

---

### 7. Risk Assessment

#### Risk Matrix

- AWS Lambda processing failures.
- Image upload failures.
- Exceeding the AWS Free Tier limits.
- Users uploading unsupported image files.
- Connectivity issues with AWS services.

#### Risk Mitigation Strategies

- Use Amazon CloudWatch Logs to monitor AWS Lambda execution and assist with troubleshooting image processing failures.
- Use Amazon SNS to notify administrators whenever image optimization fails.
- Validate image formats and file sizes before processing to reduce errors caused by invalid input.
- Apply IAM roles following the Principle of Least Privilege to improve security.
- Use AWS KMS to encrypt images stored in Amazon S3.

#### Contingency Plan

- Store metadata and processing status in Amazon DynamoDB to help administrators track processing history and identify failures.
- Maintain processing logs in Amazon CloudWatch to support troubleshooting and recovery.
- Back up critical AWS configuration information to facilitate system recovery when necessary.

---

### 8. Expected Outcomes

#### Technical Improvements

- Fully automated image optimization workflow.
- Reduced storage consumption while maintaining image quality.
- Real-time processing status tracking.
- Centralized management of processing history.

#### Long-Term Value

The system provides a foundation for developing cloud-based multimedia processing applications. In the future, it can be extended with additional capabilities such as image format conversion, watermarking, AI-based image enhancement, content recognition using Amazon Rekognition, or CDN integration to optimize image delivery.