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




Here’s a **quick starter guide** for working with **AWS CloudFormation**:

---

# 📦 **Basic CloudFormation Templates**

### 1. **EC2 Instance Template**

```yaml
AWSTemplateFormatVersion: "2010-09-09"
Description: "Basic EC2 Instance with Security Group"

Resources:
  MyEC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t3.micro
      ImageId: ami-00ca32bbc84273381   # Amazon Linux 2 (update for your region)
      KeyName: key
      SecurityGroups:
        - !Ref InstanceSecurityGroup

  InstanceSecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Enable SSH and HTTP
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: 22
          ToPort: 22
          CidrIp: 0.0.0.0/0
        - IpProtocol: tcp
          FromPort: 80
          ToPort: 80
          CidrIp: 0.0.0.0/0
```

---

### 2. **S3 Bucket Template**

```yaml
AWSTemplateFormatVersion: "2010-09-09"
Description: "Basic S3 Bucket"

Resources:
  MyS3Bucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: my-basic-cloudformation-bucket
      VersioningConfiguration:
        Status: Enabled
```

---

### 3. **VPC + Subnet Template**

```yaml
AWSTemplateFormatVersion: "2010-09-09"
Description: "Basic VPC with Subnet"

Resources:
  MyVPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: 10.0.0.0/16
      EnableDnsSupport: true
      EnableDnsHostnames: true
      Tags:
        - Key: Name
          Value: MyVPC

  MySubnet:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId: !Ref MyVPC
      CidrBlock: 10.0.1.0/24
      AvailabilityZone: !Select [0, !GetAZs ""]
      MapPublicIpOnLaunch: true
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
