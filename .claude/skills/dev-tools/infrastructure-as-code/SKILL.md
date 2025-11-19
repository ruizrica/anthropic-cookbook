---
name: infrastructure-as-code
description: Define infrastructure using Terraform, CloudFormation, and Pulumi for reproducible deployments
---

# Infrastructure as Code

## Terraform

```hcl
# main.tf
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = var.aws_region
}

resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"

  tags = {
    Name = "main-vpc"
  }
}

resource "aws_subnet" "public" {
  vpc_id     = aws_vpc.main.id
  cidr_block = "10.0.1.0/24"

  tags = {
    Name = "public-subnet"
  }
}

resource "aws_instance" "app" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = var.instance_type

  subnet_id = aws_subnet.public.id

  tags = {
    Name = "app-server"
  }
}

resource "aws_s3_bucket" "storage" {
  bucket = "myapp-storage-${random_id.bucket_suffix.hex}"
}

resource "random_id" "bucket_suffix" {
  byte_length = 8
}

# variables.tf
variable "aws_region" {
  description = "AWS region"
  default     = "us-east-1"
}

variable "instance_type" {
  description = "EC2 instance type"
  default     = "t3.micro"
}

# outputs.tf
output "instance_public_ip" {
  value = aws_instance.app.public_ip
}

output "bucket_name" {
  value = aws_s3_bucket.storage.id
}
```

## AWS CloudFormation

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: 'Application infrastructure'

Parameters:
  InstanceType:
    Type: String
    Default: t3.micro
    AllowedValues:
      - t3.micro
      - t3.small
      - t3.medium

Resources:
  VPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: 10.0.0.0/16
      Tags:
        - Key: Name
          Value: MainVPC

  PublicSubnet:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId: !Ref VPC
      CidrBlock: 10.0.1.0/24
      MapPublicIpOnLaunch: true

  AppInstance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: !Ref InstanceType
      ImageId: ami-0c55b159cbfafe1f0
      SubnetId: !Ref PublicSubnet

  S3Bucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: !Sub 'myapp-${AWS::AccountId}'

Outputs:
  InstancePublicIP:
    Value: !GetAtt AppInstance.PublicIp
  BucketName:
    Value: !Ref S3Bucket
```

## Pulumi (TypeScript)

```typescript
import * as aws from "@pulumi/aws";
import * as pulumi from "@pulumi/pulumi";

const vpc = new aws.ec2.Vpc("main-vpc", {
    cidrBlock: "10.0.0.0/16",
});

const subnet = new aws.ec2.Subnet("public-subnet", {
    vpcId: vpc.id,
    cidrBlock: "10.0.1.0/24",
    mapPublicIpOnLaunch: true,
});

const instance = new aws.ec2.Instance("app-server", {
    ami: "ami-0c55b159cbfafe1f0",
    instanceType: "t3.micro",
    subnetId: subnet.id,
});

const bucket = new aws.s3.Bucket("storage", {
    bucket: "myapp-storage",
});

export const instanceIp = instance.publicIp;
export const bucketName = bucket.id;
```

## Best Practices

1. **Version control**: Store IaC in Git
2. **State management**: Use remote state (S3, Terraform Cloud)
3. **Modularize**: Create reusable modules
4. **Use variables**: Parameterize configurations
5. **Validate**: Run `terraform validate`
6. **Plan before apply**: Review changes
7. **Separate environments**: Dev, staging, prod
