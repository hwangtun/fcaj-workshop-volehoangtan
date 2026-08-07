---
title: "Verification After Deployment"
date: 2026-08-07
weight: 6
chapter: false
pre: " <b> 5.7.6 </b> "
---

---

After the **CI/CD Pipeline** has completed successfully, the entire system should be verified to ensure that the Frontend, Backend, and image processing workflow are working correctly in the AWS environment.

The verification process follows this workflow:

```text
Frontend
   │
   ▼
Login
   │
   ▼
Upload Image
   │
   ▼
S3 Original Bucket
   │
   ▼
Lambda Image Processing
   │
   ├──► S3 Optimized Bucket
   │
   └──► CloudWatch Logs
```

### Step 1: Verify the Frontend on AWS Amplify

Access the **AWS Management Console** and open the **AWS Amplify** service.

Select the deployed application and the `main` branch.

Get the application's **public URL** and open it in a web browser.

#### Expected Result

The **Automatic Image Optimization System** interface is displayed correctly and can be accessed from the Internet.

---

### Step 2: Verify the Login Functionality

On the application interface, log in using **Google**.

Verify that the authentication process is completed successfully and that the user can access features that require authentication.

#### Expected Result

The user logs in successfully and can access the main interface of the system.

---

### Step 3: Verify the Upload Functionality

Access the **Upload** feature of the system.

Upload an image larger than **1 MB** to test the image processing workflow.

After uploading, check the image processing status on the interface if the system supports status display.

#### Expected Result

The image is uploaded successfully and the system starts the image processing process.

---

### Step 4: Verify the S3 Original Bucket

Access **Amazon S3** and open the bucket used to store the original images (**Original Bucket**).

Check whether the uploaded image has been stored in the bucket.

#### Expected Result

The original image appears in the Original Bucket with information and a path corresponding to the uploaded file.

---

### Step 5: Verify the S3 Optimized Bucket

After Lambda completes the image processing process, access the bucket used to store optimized images (**Optimized Bucket**).

Check the image file generated after processing.

The following information can be verified:

- The image has been generated successfully.
- The file size after processing is smaller than the original image.
- The image format is converted to **WebP** if required by the optimization configuration.
- The file is stored in the correct location in the Output Bucket.

#### Expected Result

The optimized image appears in the Optimized Bucket and complies with the configured image processing settings.

---

### Step 6: Verify Lambda Logs in CloudWatch

To inspect the image processing process in detail, access **Amazon CloudWatch** and open the Log Group of the image processing Lambda function.

Find the log corresponding to the upload operation just performed and check the processing details.

You can refer to the [CloudWatch Logs](../../5.6-Monitoring/5.6.1-CloudWatch-logs/) section for instructions on accessing and monitoring Lambda logs.

Check whether Lambda encountered any errors during the processing and confirm that the image processing completed successfully.

#### Expected Result

CloudWatch records the Lambda execution logs without any errors during the processing. Lambda successfully completes the image optimization process.

---

### Verification Results

After completing the steps above, the entire system workflow can be verified as follows:

| Component           | Expected Result                             |
| ------------------- | ------------------------------------------- |
| AWS Amplify         | Frontend is accessible                      |
| Google Login        | Login is successful                         |
| S3 Original Bucket  | Original image is received                  |
| AWS Lambda          | Lambda is triggered and processes the image |
| S3 Optimized Bucket | Optimized image is stored successfully      |
| CloudWatch          | Processing logs are recorded successfully   |

If all the steps above meet the expected results, the system has been successfully deployed and is ready for actual use.
