# ☁️ AWS CloudFormation – EC2, S3, VPC Deployment Guide

This document explains **step-by-step how to deploy AWS infrastructure using CloudFormation** (EC2 Linux, EC2 Windows, S3, and VPC) using the **AWS CLI** on **Amazon Web Services**.
![Image](https://d2908q01vomqb2.cloudfront.net/7719a1c782a1ba91c031a682a0a2f8658209adbf/2022/06/09/standard-eip.png)

![Image](https://miro.medium.com/v2/resize%3Afit%3A1400/1%2AP1FeZnLoY6jaeqvC8Ffz2w.png)

![Image](https://docs.aws.amazon.com/images/AWSCloudFormation/latest/UserGuide/images/update-stack-diagram.png)

---

## 🔐 0️⃣ Prerequisites (IAM & CLI)

### IAM Setup

* **IAM User:** `atul`
* **IAM Group:** `admin`
* **Policy Attached:** `AdministratorAccess` (FullAdminAccess)

> ⚠️ For production, use **least-privilege IAM policies** instead of full admin access.

### AWS CLI

```bash
aws --version
aws configure
```

Provide:

* AWS Access Key
* AWS Secret Key
* Region (e.g., `us-east-1`)
* Output format (`json`)

---

## 📂 1️⃣ Clone CloudFormation Templates

```bash
git clone https://github.com/atulkamble/cloudformation-templates.git
cd cloudformation-templates
```

**Expected structure**

```
cloudformation-templates/
├── ec2-linux.yaml
├── ec2-windows.yaml
├── s3.yaml
├── vpc.yaml
└── README.md
```

---

## 🧪 2️⃣ Validate CloudFormation Templates

### EC2 – Linux

```bash
aws cloudformation validate-template \
  --template-body file://ec2-linux.yaml
```

### EC2 – Windows

```bash
aws cloudformation validate-template \
  --template-body file://ec2-windows.yaml
```

> ✔️ Validation ensures **syntax + basic resource correctness** before deployment.

---

## 🔑 3️⃣ Create Key Pair (Required for EC2)

Create a **key pair** from the AWS Console:

```
EC2 → Key Pairs → Create key pair → Download key.pem
```

* Keep `key.pem` secure
* Ensure **same key name** is referenced in the EC2 template

---

## 🐧 4️⃣ Launch EC2 Linux Using CloudFormation

```bash
aws cloudformation create-stack \
  --stack-name ec2-linux \
  --template-body file://ec2-linux.yaml \
  --capabilities CAPABILITY_NAMED_IAM
```

### Verify

```bash
aws cloudformation describe-stacks --stack-name ec2-linux
```

---

## 🪟 5️⃣ Launch EC2 Windows Server

### Validate

```bash
aws cloudformation validate-template \
  --template-body file://ec2-windows.yaml
```

### Create Stack

```bash
aws cloudformation create-stack \
  --stack-name ec2-windows \
  --template-body file://ec2-windows.yaml \
  --capabilities CAPABILITY_NAMED_IAM
```

> 📝 Retrieve **Windows Administrator password** using:
> EC2 → Instance → Get Windows Password → Upload `key.pem`

---

## 🪣 6️⃣ Create S3 Bucket Using CloudFormation

> ⚠️ **S3 bucket names must be globally unique**

```bash
aws cloudformation create-stack \
  --stack-name s3 \
  --template-body file://s3.yaml
```

---

## 🌐 7️⃣ Create VPC Using CloudFormation

### Create VPC

```bash
aws cloudformation create-stack \
  --stack-name vpc \
  --template-body file://vpc.yaml
```

### Update VPC

```bash
aws cloudformation update-stack \
  --stack-name vpc \
  --template-body file://vpc.yaml \
  --capabilities CAPABILITY_NAMED_IAM
```

### Delete VPC

```bash
aws cloudformation delete-stack --stack-name vpc
```

---

## ⚙️ Core CloudFormation CLI Commands (Quick Reference)

### Validate Template

```bash
aws cloudformation validate-template --template-body file://template.yaml
```

### Create Stack

```bash
aws cloudformation create-stack \
  --stack-name my-stack \
  --template-body file://template.yaml \
  --capabilities CAPABILITY_NAMED_IAM
```

### Update Stack

```bash
aws cloudformation update-stack \
  --stack-name my-stack \
  --template-body file://template.yaml \
  --capabilities CAPABILITY_NAMED_IAM
```

### Delete Stack

```bash
aws cloudformation delete-stack --stack-name my-stack
```

### Describe Stack

```bash
aws cloudformation describe-stacks --stack-name my-stack
```

### List Stacks

```bash
aws cloudformation list-stacks
```

---

## 🧠 Best Practices

* Always **validate templates** before deployment
* Use **Parameters** for AMI IDs, instance types, CIDRs
* Enable **stack termination protection** for critical resources
* Use **separate stacks** for VPC, EC2, and databases
* Store templates in **GitHub** for version control

---
