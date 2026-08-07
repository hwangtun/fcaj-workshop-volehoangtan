---
title: "Create S3 Buckets for Image Storage"
date: 2026-08-04
weight: 1
chapter: false
pre: " <b> 5.3.1 </b> "
---

In the **Automatic Image Optimization System on AWS**, Amazon S3 is used to store images throughout the entire processing workflow.

The system uses two separate S3 Buckets:

- **Input Bucket**: Stores the original images uploaded by users.
- **Output Bucket**: Stores the images after they have been optimized by AWS Lambda, along with the generated thumbnail images.

Separating the two buckets helps organize data more effectively, manage access permissions through IAM, and ensure that data is protected throughout the processing workflow.

Data Processing Workflow:

```
                 User
                  |
                  v
          Upload Image
                  |
                  v
        Amazon S3 Input Bucket
                  |
                  |
        Create Metadata Record
                  |
                  v
          DynamoDB ImageMetadata
             (PENDING)
                  |
                  |
        S3 Object Created Event
                  |
                  v
             AWS Lambda
                  |
        +---------+---------+
        |                   |
        v                   v
 Output S3 Bucket     Update DynamoDB
(Optimized Image)     (SUCCESS/FAILED)
```

---

### 1. Access the Amazon S3 Console

Open [Amazon S3 Console](https://s3.console.aws.amazon.com/s3/)

In the left navigation pane, select:

```
Buckets
```

Then select:

```
Create bucket
```

to start creating the bucket for image storage.

![s3-console](../../../images/5-Workshop/5.3-S3-setup/s3-dashboard.png)

---

### 2. Create the Input Bucket

In the **Create bucket** interface, configure the following settings:

#### Bucket name

Enter the bucket name:

```
auto-images-input-bucket
```

{{% notice warning %}}
An Amazon S3 Bucket name must be globally unique across AWS. If the bucket name already exists, you must choose a different name.
{{% /notice %}}

![s3-create-name-bucket](../../../images/5-Workshop/5.3-S3-setup/s3-create-name-bucket.png)

---

### 3. Configure Input Bucket Access

In the **Object Ownership** section, select:

```
ACLs disabled (recommended)
```

The system uses IAM Policies to manage access permissions instead of ACLs.

In the **Block Public Access** section, keep the default configuration:

```
Block all public access
```

{{% notice note %}}
User-uploaded images are not publicly accessible. Access to the images is controlled through the Backend API and IAM permissions.
{{% /notice %}}

![input-bucket-access](../../../images/5-Workshop/5.3-S3-setup/s3-access.png)

---

### 4. Configure Encryption for the Input Bucket

In the **Default encryption** section, enable data encryption:

Select:

```
Server-side encryption with AWS KMS keys (SSE-KMS)
```

Then select the KMS Key created for the system.

Using AWS KMS helps protect the original images while they are stored in Amazon S3.

![s3-kms-encryption](../../../images/5-Workshop/5.3-S3-setup/open-kms-dashboard.png)
![s3-kms-encryption](../../../images/5-Workshop/5.3-S3-setup/kms-2.png)
Enter the alias name:

```
image-optimization-kms
```

![s3-kms-encryption](../../../images/5-Workshop/5.3-S3-setup/kms-3.png)

Select the IAM User that manages the system:
![s3-kms-encryption](../../../images/5-Workshop/5.3-S3-setup/kms-4.png)

Add the Lambda IAM Role:
![s3-kms-encryption](../../../images/5-Workshop/5.3-S3-setup/kms-5.png)
![s3-kms-encryption](../../../images/5-Workshop/5.3-S3-setup/kms-6.png)
![s3-kms-encryption](../../../images/5-Workshop/5.3-S3-setup/kms-7.png)

Select the KMS Key that was just created, then click Create bucket:
![s3-kms-encryption](../../../images/5-Workshop/5.3-S3-setup/kms-8.png)

The Input Bucket has now been created successfully:
![create-input-bucket](../../../images/5-Workshop/5.3-S3-setup/create-input-bucket.png)

---

### 5. Create the Output Bucket

Repeat the same steps used to create the Input Bucket.

Set the bucket name to:

```
auto-images-output-bucket
```

The Output Bucket is used to store:

- Optimized images.
- Thumbnail images for fast display in the user interface.

Storage structure:

```
auto-images-output-bucket

optimized/
 └── {userId}/
      └── {batchId}/
            └── image.webp

thumbnails/
 └── {userId}/
      └── {batchId}/
            └── image.webp
```

The Output Bucket has now been created successfully:
![create-output-bucket](../../../images/5-Workshop/5.3-S3-setup/create-output-bucket.png)

---

### 6. Verify the Bucket List

After completing the setup, the Amazon S3 Bucket list will display:

| Bucket                    | Purpose                                      |
| ------------------------- | -------------------------------------------- |
| auto-images-input-bucket  | Stores the original images uploaded by users |
| auto-images-output-bucket | Stores the optimized images and thumbnails   |

![bucket-list](../../../images/5-Workshop/5.3-S3-setup/s3-dashboard.png)

---

# 7. Verify the S3 Bucket Workflow

After the buckets have been created successfully, the system will use them in the image processing workflow:

1. The backend receives images from users.
2. The images are uploaded to the **Input Bucket**.
3. An S3 Trigger invokes AWS Lambda.
4. AWS Lambda performs the image optimization process.
5. The processed images are stored in the **Output Bucket**.
6. The processing information is stored in DynamoDB.

The metadata stored in DynamoDB includes:

```
ImageMetadata

├── batchId
├── processingId
├── userId
├── originalName
├── inputBucket
├── outputBucket
├── inputKey
├── outputKey
├── uploadedAt
├── processedAt
├── status
├── originalSize
├── processedSize
├── compressionRatio
└── processingTimeMs
```

Using Amazon S3 enables the system to store large volumes of images while integrating with AWS Lambda to build an automated and scalable image processing workflow.
