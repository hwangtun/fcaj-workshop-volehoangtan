---
title: "Checking Metadata in DynamoDB"
date: 2026-08-04
weight: 2
chapter: false
pre: " <b> 5.5.2 </b> "
---

---

After creating the DynamoDB Table and configuring AWS Lambda with permission to write data, this step verifies the metadata stored after the image processing workflow is completed.

Verification objectives:

- Confirm that Lambda successfully writes data to DynamoDB.
- Check image information after processing.
- Ensure data is correctly linked with the S3 Input Bucket and Output Bucket.
- Verify the image processing status.

---

### 1. Prepare the Verification Process

To create metadata data in DynamoDB, upload a sample image to the Input Bucket.

Example:

```
sample-image.jpg
```

Processing workflow:

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
 v
DynamoDB ImageMetadata
```

After Lambda completes the processing, a new item will be created in DynamoDB.

---

### 2. Access DynamoDB Table

Open:

```
AWS Management Console
```

Select:

```
DynamoDB
```

From the Tables list, select:

```
ImageMetadata
```

![dynamodb-table](../../../images/5-Workshop/5.5-DynamoDB/dynamodb-table.png)

---

### 3. Access Table Data

In the DynamoDB Table interface:

Select:

```
Explore table items
```

or:

```
Scan
```

to view stored items.

![explore-items](../../../images/5-Workshop/5.5-DynamoDB/press-explore.png)
![explore-items](../../../images/5-Workshop/5.5-DynamoDB/explore-items.png)

---

### 4. Check Metadata Item

After Lambda successfully processes the image, DynamoDB contains an item corresponding to the uploaded image.

Example:

```json
{
  "batchId": "batch-001",
  "processingId": "processing-001",
  "userId": "user001",
  "originalName": "sample-image.jpg",
  "inputBucket": "auto-images-input-bucket",
  "outputBucket": "auto-images-output-bucket",
  "status": "SUCCESS",
  "format": "WEBP",
  "originalSize": 204800,
  "processedSize": 51200,
  "compressionRatio": 75
}
```

---

### 5. Check Important Data Fields

#### Batch Information

Check:

```
batchId
processingId
```

Purpose:

- Identify the group of uploaded images.
- Distinguish each image processing execution.

Example:

```
batchId:
8f7c9e12-xxxx


processingId:
11320000-xxxx
```

---

#### File Information

Check:

```
originalName
format
```

Example:

```
originalName:
sample-image.jpg


format:
WEBP
```

Confirms that the image has been converted to an optimized format.

---

#### Processing Status

Check:

```
status
```

Available statuses:

```
PROCESSING
SUCCESS
FAILED
```

Example:

```
status:

SUCCESS
```

indicates that the processing workflow completed successfully.

---

#### Storage Information

Check:

```
inputBucket
outputBucket
inputKey
outputKey
thumbnailKey
```

Example:

```
inputBucket:

auto-images-input-bucket


outputBucket:

auto-images-output-bucket
```

These fields help associate metadata with actual files stored in S3.

---

### 6. Check Image Optimization Information

DynamoDB stores optimization parameters before and after processing:

```
originalSize

processedSize

compressionRatio

processingTimeMs
```

Example:

```
Original size:

204800 bytes


Processed size:

51200 bytes


Compression ratio:

75%
```

This helps evaluate the effectiveness of the image optimization process.

---

### 7. Check Error Cases

If Lambda processing fails, DynamoDB stores the status:

```
status:

FAILED
```

and error information:

```
errorMessage
```

Example:

```json
{
  "status": "FAILED",
  "errorMessage": "Unsupported image format"
}
```

Error data helps the system easily monitor and troubleshoot issues.

---

### 8. Check Data Using AWS CLI (Optional)

DynamoDB can also be checked using AWS CLI:

```bash
aws dynamodb scan \
--table-name ImageMetadata
```

Returned result contains the metadata list:

```json
{
  "Items": [
    {
      "batchId": {
        "S": "batch-001"
      },
      "status": {
        "S": "SUCCESS"
      }
    }
  ]
}
```

---

### 9. Result

After verification, DynamoDB has successfully stored metadata of the processed images.

Achieved results:

- Lambda successfully writes data to DynamoDB.
- Metadata is stored completely.
- Processing status is updated accurately.
- Image processing history can be queried.

Complete workflow:

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
       S3 Object Created
              |
              v
        AWS Lambda
              |
      +-------+-------+
      |               |
      v               v
 Output S3       DynamoDB
                     |
                     |
             ImageMetadata
```

After this step, the metadata storage component of the **Automatic Image Optimization System on AWS** has been verified and is operating successfully.
