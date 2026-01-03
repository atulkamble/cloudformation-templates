## 🌩️ AWS CloudFormation — Basic Coding Samples

![Image](https://d2908q01vomqb2.cloudfront.net/fc074d501302eb2b93e2554793fcaf50b3bf7291/2024/05/15/fig1-comfyui-stable-diffusion-1024x580.png)

![Image](https://docs.aws.amazon.com/images/AWSCloudFormation/latest/UserGuide/images/create-stack-diagram.png)

![Image](https://docs.aws.amazon.com/images/AWSCloudFormation/latest/UserGuide/images/update-stack-diagram.png)

---

## 1️⃣ Hello World – Create an S3 Bucket

```yaml
AWSTemplateFormatVersion: "2010-09-09"
Description: Basic CloudFormation template to create an S3 bucket

Resources:
  MyS3Bucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: my-basic-cf-bucket-12345
```

✅ **Creates**

* One S3 bucket

---

## 2️⃣ EC2 Instance (Amazon Linux)

```yaml
AWSTemplateFormatVersion: "2010-09-09"
Description: Launch a basic EC2 instance

Resources:
  MyEC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.micro
      ImageId: ami-0fc5d935ebf8bc3bc   # Amazon Linux (example)
      KeyName: my-key
      Tags:
        - Key: Name
          Value: CF-Demo-Instance
```

✅ **Creates**

* One EC2 instance
  ⚠️ AMI ID is **region-specific**

---

## 3️⃣ EC2 + Security Group

```yaml
AWSTemplateFormatVersion: "2010-09-09"
Description: EC2 with Security Group

Resources:
  MySecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Allow SSH
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: 22
          ToPort: 22
          CidrIp: 0.0.0.0/0

  MyEC2:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.micro
      ImageId: ami-0fc5d935ebf8bc3bc
      SecurityGroups:
        - !Ref MySecurityGroup
```

✅ **Creates**

* Security Group
* EC2 instance using that SG

---

## 4️⃣ Using Parameters (Dynamic Input)

```yaml
AWSTemplateFormatVersion: "2010-09-09"
Description: CloudFormation with parameters

Parameters:
  InstanceType:
    Type: String
    Default: t2.micro
    AllowedValues:
      - t2.micro
      - t2.small
      - t2.medium

Resources:
  MyEC2:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: !Ref InstanceType
      ImageId: ami-0fc5d935ebf8bc3bc
```

✅ **Benefits**

* Reusable template
* No hardcoding

---

## 5️⃣ Outputs Example

```yaml
AWSTemplateFormatVersion: "2010-09-09"
Description: Outputs example

Resources:
  MyBucket:
    Type: AWS::S3::Bucket

Outputs:
  BucketName:
    Description: Name of the created bucket
    Value: !Ref MyBucket
```

✅ **Shows**

* Output values after stack creation

---

## 6️⃣ Mapping Example (Region-Based AMI)

```yaml
AWSTemplateFormatVersion: "2010-09-09"

Mappings:
  RegionMap:
    us-east-1:
      AMI: ami-0fc5d935ebf8bc3bc
    ap-south-1:
      AMI: ami-0ad21ae1d0696ad58

Resources:
  EC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.micro
      ImageId: !FindInMap
        - RegionMap
        - !Ref AWS::Region
        - AMI
```

✅ **Used for**

* Multi-region deployments

---

## 7️⃣ Conditions Example

```yaml
AWSTemplateFormatVersion: "2010-09-09"

Parameters:
  EnvType:
    Type: String
    AllowedValues:
      - dev
      - prod

Conditions:
  IsProd: !Equals [!Ref EnvType, prod]

Resources:
  ProdBucket:
    Type: AWS::S3::Bucket
    Condition: IsProd
```

✅ **Creates resource only if**

* Environment = `prod`

---

## 8️⃣ Simple VPC (Minimal)

```yaml
AWSTemplateFormatVersion: "2010-09-09"

Resources:
  MyVPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: 10.0.0.0/16
      Tags:
        - Key: Name
          Value: CF-VPC
```

---

## 🧠 Key CloudFormation Sections (Must Know)

| Section    | Purpose              |
| ---------- | -------------------- |
| Parameters | User inputs          |
| Mappings   | Static lookup values |
| Conditions | Conditional logic    |
| Resources  | Actual AWS resources |
| Outputs    | Export values        |

---

## 🚀 How to Deploy

```bash
aws cloudformation create-stack \
  --stack-name my-stack \
  --template-body file://template.yaml
```

Delete:

```bash
aws cloudformation delete-stack --stack-name my-stack
```

---
