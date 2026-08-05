---
title: "Storing Metadata with Amazon DynamoDB"
date: 2026-08-04
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

In the **Automatic Image Optimization System on AWS**, Amazon DynamoDB is used to store metadata for images throughout the processing workflow.

After AWS Lambda completes the image optimization process, the processing information is stored in DynamoDB to support:

- Tracking image processing status.
- Storing processing history.
- Retrieving information about optimized images.
- Displaying image data in the user interface.
- Managing processing results for each upload batch.

---

### 1. DynamoDB Design Overview

The system uses the following DynamoDB table:

```
ImageMetadata
```

This table stores detailed information for each image after it has been processed by AWS Lambda.

Primary key structure:

```
ImageMetadata

Partition Key:
batchId

Sort Key:
processingId
```

Where:

| Attribute | Description |
| ---------------- | ------------------------------------------------- |
| batchId | ID used to group images uploaded in the same batch |
| processingId | Unique identifier for each image processing operation |
| userId | User who uploaded the image |
| originalName | Original image file name |
| inputBucket | Amazon S3 bucket containing the original image |
| outputBucket | Amazon S3 bucket containing the optimized image |
| inputKey | Path to the original image in Amazon S3 |
| outputKey | Path to the processed image |
| thumbnailKey | Path to the thumbnail image |
| format | Output image format after optimization |
| uploadedAt | Image upload timestamp |
| processedAt | Processing completion timestamp |
| status | Processing status |
| errorMessage | Error message if processing fails |
| lambdaRequestId | Lambda execution request ID |
| originalSize | Original image size |
| processedSize | Optimized image size |
| compressionRatio | Compression ratio |
| processingTimeMs | Image processing time |

---

### 2. Metadata Storage Workflow

Metadata is stored in DynamoDB through the following workflow:

```
User
 |
 |
Upload Image
 |
 v
Amazon S3 Input Bucket
 |
 |
Object Created Event
 |
 v
AWS Lambda
 |
 |
Image Optimization
 |
 +----------------+
 |
 v
DynamoDB ImageMetadata
```

After Lambda completes image processing:

- The optimized image is stored in the Output Bucket.
- A thumbnail is generated.
- The image metadata is written to DynamoDB.

---

### 3. DynamoDB Deployment

The DynamoDB deployment consists of two steps:

```
5.5-DynamoDB
│
├── 5.5.1-create-dynamodb
│
└── 5.5.2-verify-metadata
```

---

#### 5.5.1-create-dynamodb

This step includes:

- Creating the DynamoDB table.
- Configuring the Partition Key.
- Configuring the Sort Key.
- Configuring the Capacity Mode.
- Verifying encryption.

The table created:

```
Table name:

ImageMetadata
```

Primary key:

```
Partition Key:

batchId


Sort Key:

processingId
```

---

#### 5.5.2-verify-metadata

After AWS Lambda successfully processes an image, verify that the metadata has been written to DynamoDB.

Verify the following:

- An item is created in the table.
- The processing status.
- Image file information.
- Image size before and after optimization.
- Image paths in Amazon S3.

Example item:

```json
{
  "batchId": "batch-001",
  "processingId": "image-001",
  "originalName": "sample-image.jpg",
  "status": "SUCCESS",
  "format": "WEBP",
  "originalSize": 204800,
  "processedSize": 51200,
  "compressionRatio": 75
}
```

---

### 4. Result

After completing the DynamoDB deployment, the system is capable of storing and managing metadata for processed images.

The achieved results include:

- The `ImageMetadata` DynamoDB table is created successfully.
- AWS Lambda can write image processing data to DynamoDB.
- Metadata is associated with images stored in Amazon S3.
- The system supports querying image processing history.

Metadata storage architecture:

```
                 AWS Lambda
                      |
                      |
              Process Image Result
                      |
                      v
              Amazon DynamoDB
                      |
                      |
              ImageMetadata Table
```

At this stage, the metadata storage component for the automatic image optimization workflow has been fully implemented.

#### Contents

- [Create the ImageMetadata DynamoDB Table](5.5.1-create-dynamodb/)
- [Verify Metadata in DynamoDB](5.5.2-verify-metadata/)