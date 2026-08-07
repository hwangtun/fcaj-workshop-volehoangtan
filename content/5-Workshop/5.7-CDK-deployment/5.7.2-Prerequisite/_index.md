---
title: "Prepare Deployment Environment"
date: 2026-08-07
weight: 2
chapter: false
pre: " <b> 5.7.2 </b> "
---

---

Before starting the system deployment using **AWS CDK**, the environment needs to be prepared on a local machine or an **AWS Cloud9** environment. The following tools are required to deploy the Backend Infrastructure, build the Frontend application, and perform the deployment process.

### 1. AWS CLI

**AWS CLI (Command Line Interface)** is used to interact with AWS services through the command line.

Check whether AWS CLI is installed:

```bash id="y7i8a3"
aws --version
```

If AWS credentials have not been configured, execute:

```bash id="u2n5kd"
aws configure
```

Then enter the required information:

```text id="z9k4q1"
AWS Access Key ID:
AWS Secret Access Key:
Default region name:
Default output format:
```

The AWS account used for deployment must have sufficient permissions to create and configure the required resources for the system, such as **IAM, S3, Lambda, DynamoDB, API Gateway**, and related services.

> **Note:** Do not share or commit the Access Key and Secret Access Key to the repository.

---

### 2. Node.js

**Node.js version 18 or higher** is used to install the AWS CDK CLI and build the React/Vite Frontend application.

Check the Node.js version:

```bash id="2f8v6n"
node --version
```

Check npm:

```bash id="w6m3h8"
npm --version
```

If the output shows Node.js version 18 or higher, the environment meets the requirements.

---

### 3. AWS CDK CLI

AWS CDK CLI is used to perform operations such as bootstrap, synthesize, and deploy CDK Stacks.

Install AWS CDK CLI using npm:

```bash id="p5x9bc"
npm install -g aws-cdk
```

After installation, check the version:

```bash id="m8z2qw"
cdk --version
```

If the command returns the AWS CDK version, the installation is completed successfully.

---

### 4. Python

The project uses **Python 3.13** for CDK Stacks and related Python components.

Check the Python version:

```bash id="n3k7yv"
python --version
```

Expected output:

```text id="v5s8cx"
Python 3.13.x
```

Python is used in the following steps to create a virtual environment and install the required project dependencies.

---

### 5. Docker

**Docker** is used during the Backend deployment process. Some Lambda components of the system are packaged as Docker Images through the `DockerImageFunction` configuration in AWS CDK.

Therefore, Docker must be installed and in a **running** state before executing `cdk deploy`.

Check Docker installation:

```bash id="q6r4zt"
docker --version
```

Docker status can be checked using:

```bash id="e1x7kp"
docker ps
```

If Docker is running, the command returns a list of running containers or an empty list if no containers are currently running.

> **Note:** During deployment, AWS CDK may use Docker to build Docker Images for Lambda functions defined in the CDK Stacks. Therefore, Docker Desktop must be started before performing the deployment process.

---

### 6. Environment Verification

After completing the steps above, the required tools can be quickly verified using the following commands:

```bash id="k9w2fd"
aws --version
node --version
npm --version
cdk --version
python --version
docker --version
```

If all commands return the corresponding version information and AWS CLI has been configured successfully, the environment is ready to perform the system deployment process using AWS CDK.
