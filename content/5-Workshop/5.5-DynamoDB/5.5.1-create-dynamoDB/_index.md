---
title: "Create DynamoDB Tables for Metadata Storage"
date: 2026-08-04
weight: 1
chapter: false
pre: " <b> 5.5.1 </b> "
---

In the **Automatic Image Optimization System on AWS**, Amazon DynamoDB is used to store the system's metadata.

DynamoDB includes two main tables:

- **UserMetadata**: stores user information for user management and links image data to user accounts.
- **ImageMetadata**: stores information about image uploads, processing, and optimization performed by AWS Lambda.

The **ImageMetadata** table is used to track:

- Image file name.
- Image owner.
- Source bucket and output bucket information.
- Paths to the original image, optimized image, and thumbnail.
- Processing status.
- Image size before and after optimization.
- Processing time.
- Error information if image processing fails.

---

### 1. DynamoDB Overview in the System

Metadata storage architecture:

```
User
|
|
v
UserMetadata
|
|
userId
|
|
v
Image Upload
|
|
v
ImageMetadata
|
|
AWS Lambda Processing
```

The system uses two DynamoDB tables:

| Table         | Purpose                          |
| ------------- | -------------------------------- |
| UserMetadata  | Stores user account information  |
| ImageMetadata | Stores image processing metadata |

---

### 2. Create the ImageMetadata Table

#### Access the Amazon DynamoDB Console

Open:

```
AWS Management Console
```

Search for the service:

```
DynamoDB
```

In the left navigation pane, select:

```
Tables
```

Then select:

```
Create table
```

to create a new DynamoDB table.

![dynamodb-console](../../../images/5-Workshop/5.5-DynamoDB/dynamodb-console.png)

---

### 3. Configure the Table

On the:

```
Create table
```

page, configure the following:

#### Table name

Enter:

```
ImageMetadata
```

This table stores metadata for processed images in the system.

---

#### Partition Key

Set:

```
batchId
```

Data type:

```
String
```

The `batchId` is used to group images uploaded in the same batch.

Example:

```
batchId:
8f7c9e12-xxxx-xxxx
```

A single batch can contain multiple images.

---

#### Sort Key

Enable:

```
Add sort key
```

Enter:

```
processingId
```

Data type:

```
String
```

The `processingId` uniquely identifies each image within the same batch.

Example:

```
batchId:
batch-001

processingId:
image-001
```

Key structure:

```
ImageMetadata

Partition Key:
batchId

Sort Key:
processingId
```

![table-key](../../../images/5-Workshop/5.5-DynamoDB/table-key.png)

---

### 4. Create the DynamoDB Table

After completing the configuration, click:

```
Create table
```

AWS creates the table.

![create-table](../../../images/5-Workshop/5.5-DynamoDB/create-table.png)

---

### 5. Verify the Data Structure

Select:

```
ImageMetadata
```

Then open:

```
Explore table items
```

Example item:

```json
{
  "batchId": "batch-001",
  "processingId": "processing-001",
  "userId": "user-001",
  "originalName": "image.jpg",
  "status": "SUCCESS",
  "format": "WEBP",
  "originalSize": 2048000,
  "processedSize": 512000
}
```

In this table:

```
userId
```

is used to associate image data with the:

```
UserMetadata
```

table.

---

### 6. UserMetadata Table

In addition to ImageMetadata, the system also uses the:

```
UserMetadata
```

table.

This table stores user information for:

- User account management.
- Access control.
- Linking users to their uploaded images.

Key structure:

```
UserMetadata

Partition Key:

userId
```

Example:

```json
{
  "userId": "user-001",
  "email": "user@gmail.com",
  "role": "USER",
  "createdAt": "2026-08-04T10:00:00Z"
}
```

Relationship between the two tables:

```
UserMetadata

userId
   |
   |
   v

ImageMetadata

userId
```

---

### 7. Result

At this stage, the system has all the required DynamoDB tables:

```
DynamoDB

|
|-- UserMetadata
|
|-- ImageMetadata
```

ImageMetadata table information:

```
Table:

ImageMetadata


Primary Key:

batchId


Sort Key:

processingId


Capacity Mode:

On-demand


Encryption:

Enabled
```

Metadata processing workflow:

```
User
 |
 |
Upload Image
 |
 v
Amazon S3
 |
 |
AWS Lambda
 |
 |
Process Image
 |
 |
 v

ImageMetadata
```

DynamoDB is now ready to store user information and metadata generated during the image optimization process.

The next step is to verify the metadata written by AWS Lambda to DynamoDB in [5.5.2. Verify Metadata in DynamoDB](5.5.2-verify-metadata/).
