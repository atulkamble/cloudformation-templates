// cloudformation-ec2 

0. IAM >> user (atul) >> admin (group) >> FullAdminAccess Policy  

1. 
```
git clone https://github.com/atulkamble/cloudformation-templates.git
cd cloudformation-templates
```

2. Vaildate Template
```
aws cloudformation validate-template --template-body file://ec2-linux.yaml
```

3. create key.pem from aws console (keypair)

4. 

```
aws cloudformation create-stack \
  --stack-name ec2-linux \
  --template-body file://ec2-linux.yaml \
  --capabilities CAPABILITY_NAMED_IAM
```

5.
```
aws cloudformation validate-template --template-body file://ec2-windows.yaml
```
6. launch windows server 

```
```
aws cloudformation create-stack \
  --stack-name ec2-windows \
  --template-body file://ec2-windows.yaml \
  --capabilities CAPABILITY_NAMED_IAM
```
// s3 

tip: have a unique bucket name 

```
aws cloudformation create-stack \
  --stack-name s3 \
  --template-body file://s3.yaml 
```
// vpc 

```
aws cloudformation create-stack \
  --stack-name vpc \
  --template-body file://vpc.yaml 
```
```
aws cloudformation delete-stack --stack-name vpc
```
```
aws cloudformation update-stack \
  --stack-name vpc \
  --template-body file://vpc.yaml \
  --capabilities CAPABILITY_NAMED_IAM
```

---

# ⚙️ **CloudFormation Commands**

### 1. **Validate Template**

```bash
aws cloudformation validate-template --template-body file://template.yaml
```

### 2. **Create Stack**

```bash
aws cloudformation create-stack \
  --stack-name my-stack \
  --template-body file://template.yaml \
  --capabilities CAPABILITY_NAMED_IAM
```

### 3. **Update Stack**

```bash
aws cloudformation update-stack \
  --stack-name my-stack \
  --template-body file://template.yaml \
  --capabilities CAPABILITY_NAMED_IAM
```

### 4. **Delete Stack**

```bash
aws cloudformation delete-stack --stack-name my-stack
```

### 5. **Describe Stacks**

```bash
aws cloudformation describe-stacks --stack-name my-stack
```

### 6. **List Stacks**

```bash
aws cloudformation list-stacks
```

---

👉 Do you want me to also give you a **GitHub-ready project** with multiple CloudFormation templates (EC2, S3, VPC, RDS) plus a `README.md` with commands and steps to deploy?
