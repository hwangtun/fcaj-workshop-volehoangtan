---
title: "Testing the Image Upload and Processing Workflow"
date: 2026-08-04
weight: 3
chapter: false
pre: " <b> 5.4.3 </b> "
---

After creating the S3 buckets and configuring the S3 Trigger for AWS Lambda, the next step is to test the entire image upload and processing workflow to ensure that all system components operate as designed.

The testing process includes:

1. Upload an image through the Backend.
2. Verify that the image is stored in the Input S3 Bucket.
3. Verify that the Backend creates the initial metadata in DynamoDB with the `PROCESSING` status.
4. Verify that the S3 Event Notification triggers AWS Lambda.
5. Verify that Lambda processes the image and stores the result in the Output S3 Bucket.
6. Verify that Lambda updates the metadata in DynamoDB with the `SUCCESS` or `FAILED` status.

Testing workflow:

```text
Client
   |
   | POST /api/image/upload
   v
Spring Boot Backend
   |
   +-------------------------------+
   |                               |
   | Upload image to S3            | Create metadata
   |                               | status = PROCESSING
   v                               v
Input S3 Bucket               DynamoDB
      |
      v
S3 Event Notification
      |
      v
AWS Lambda Image Processing
      |
      +-------------------------------+
      |                               |
      | Save optimized image          | Update metadata
      v                               |
Output S3 Bucket                      |
                                      |
                                      v
                               DynamoDB
                         status = SUCCESS/FAILED
```

---

### 1. Upload an Image to the System

The user uploads images through the Backend API:

```
POST /api/image/upload
```

The request includes:

- A list of images to upload.
- `userId`.
- Image optimization configuration (`configReq`).
- Output image format (`format`).

Example response after a successful upload:

```json
{
  "batchId": "11320000-0919-4d79-9a3c-94d15318ce87",
  "status": "PROCESSING"
}
```

This indicates that the Backend has received the request and started the image processing workflow.

![upload-image](../../../images/5-Workshop/5.4-Lambda-deployment/upload-image.png)

---

### 2. Verify the Data After Upload

After the Backend receives the upload request, the system performs two operations simultaneously:

- Uploads the image to the **Input S3 Bucket**.
- Creates the initial metadata record in **DynamoDB**.

The image is stored in the Input Bucket using the following structure:

```
uploads/
 └── user001/
      └── test-image.jpg
```

![test-upload-image](../../../images/5-Workshop/5.4-Lambda-deployment/test-upload-image.png)

---

### 3. Verify the Initial Metadata in DynamoDB

The Backend creates a record in the:

```
ImageMetadata
```

table with the following status:

```
PROCESSING
```

Example:

```json
{
  "batchId": "11320000-0919-4d79-9a3c-94d15318ce87",
  "processingId": "uuid-processing-id",
  "userId": "user001",
  "originalName": "test-image.jpg",
  "status": "PROCESSING",
  "uploadedAt": "2026-08-04T10:30:00Z"
}
```

This record allows the system to track the processing progress of each image immediately after it is uploaded.

---

### 4. Verify the S3 Trigger

When the image is stored in the Input Bucket, Amazon S3 generates the following event:

```
Object Created
```

This event triggers the Lambda function:

```
autoImageProcessing
```

You can verify the execution by navigating to:

```
AWS Lambda
 → Monitor
 → View CloudWatch Logs
```

![lambda-log](../../../images/5-Workshop/5.4-Lambda-deployment/lambda-log.png)

If the Lambda function executes successfully, CloudWatch Logs will display the processing workflow, and the final log entry will indicate successful execution.

![success-log](../../../images/5-Workshop/5.4-Lambda-deployment/success-log.png)

---

### 5. Verify the Processed Images

After Lambda completes image processing, open the Output Bucket:

```
auto-images-output-bucket
```

Expected result:

```
optimized/
 └── user001/
      └── batchId/
            └── test-image.webp

thumbnails/
 └── user001/
      └── batchId/
            └── test-image.webp
```

![output-image-1](../../../images/5-Workshop/5.4-Lambda-deployment/output-image-1.png)
![output-image-2](../../../images/5-Workshop/5.4-Lambda-deployment/output-image-2.png)

Verify that:

- The image has been optimized according to the configuration.
- The processed image size is smaller than the original image.
- The thumbnail has been generated successfully.
- The files are stored under the correct `userId` and `batchId`.

---

### 6. Verify the Metadata After Processing

After Lambda completes the processing, the corresponding record in DynamoDB is updated.

Navigate to:

```
Amazon DynamoDB
 → Tables
 → ImageMetadata
```

Example:

```json
{
  "batchId": "11320000-0919-4d79-9a3c-94d15318ce87",
  "processingId": "uuid-processing-id",
  "userId": "user001",
  "originalName": "test-image.jpg",
  "status": "SUCCESS",
  "inputKey": "uploads/user001/test-image.jpg",
  "outputKey": "optimized/user001/11320000-0919-4d79-9a3c-94d15318ce87/test-image.webp",
  "thumbnailKey": "thumbnails/user001/11320000-0919-4d79-9a3c-94d15318ce87/test-image.webp",
  "originalSize": 5242880,
  "processedSize": 1048576,
  "compressionRatio": 80,
  "processingTimeMs": 2500,
  "processedAt": "2026-08-04T10:30:03Z"
}
```

Lambda updates the following fields:

- `status`
- `outputKey`
- `thumbnailKey`
- `processedSize`
- `compressionRatio`
- `processingTimeMs`
- `processedAt`

to reflect the final processing result of the image.

---

### 7. Verify Error Handling

If an error occurs during processing, Lambda updates the metadata in DynamoDB with the following status:

```
FAILED
```

and stores the error details in the:

```
errorMessage
```

field.

Example:

```json
{
  "status": "FAILED",
  "errorMessage": "Unsupported image format"
}
```

Possible error scenarios include:

- The uploaded file is not in a supported format.
- The image file is corrupted or cannot be read.
- Lambda does not have permission to access Amazon S3.
- Lambda cannot write to or update data in DynamoDB.

---

### 8. Test Results

After completing the testing process, the system produces the following results:

| Component                                           | Result  |
| --------------------------------------------------- | ------- |
| Upload image through the Backend                    | Success |
| Upload image to the Input S3 Bucket                 | Success |
| Backend creates the initial metadata (`PROCESSING`) | Success |
| S3 Event Notification triggers Lambda               | Success |
| Lambda processes the image                          | Success |
| Optimized image is stored in the Output S3 Bucket   | Success |
| Thumbnail is generated                              | Success |
| Lambda updates the metadata in DynamoDB             | Success |
| Processing logs are stored in CloudWatch            | Success |

The test results demonstrate that the system operates according to the designed architecture. After a user uploads an image, the Backend stores the image in the Input S3 Bucket and creates the initial metadata in DynamoDB. The S3 Event Notification triggers AWS Lambda to process the image, save the processed output to the Output S3 Bucket, and update the metadata in DynamoDB. This workflow ensures that image processing is fully automated while allowing the processing status of each image to be tracked through the database.
