# 3_terraform_s3_and_vpc.md

Remove citation references from documentation
---

## PART 1: S3 (Simple Storage Service)

### 1. S3 – Core Concepts

S3 is used to store:

- Buckets (containers)  
- Objects (files) inside buckets  

You can:

- Create buckets  
- Upload objects  
- Enable **static website hosting**  
- Enable **bucket versioning** to keep multiple versions of objects [web:27][web:34]  

---

### 2. Create S3 Bucket via Terraform

Create S3 bucket
resource "aws_s3_bucket" "s3" {
bucket = "myterraform-bucket-n2-offline"

tags = {
Name = "bucket-01"
}
}

 

---

### 3. Block Public Access (Example From Notes)

resource "aws_s3_bucket_public_access_block" "example" {
bucket = aws_s3_bucket.s3.id

block_public_acls = false
block_public_policy = false
ignore_public_acls = false
restrict_public_buckets = false
}

 

> Note: In real setups, you often set these to `true` to block public access unless explicitly allowed.

---

### 4. Enable Bucket Versioning

resource "aws_s3_bucket_versioning" "versioning-example" {
bucket = aws_s3_bucket.s3.id

versioning_configuration {
status = "Enabled"
}
}

 

- Enables object versioning for rollback and history. [web:27][web:34]  

---

### 5. Upload Object to S3

resource "aws_s3_object" "object1" {
bucket = aws_s3_bucket.s3.id
key = "terraform-object" # object name in S3
source = "C:/Users/Administrator/Downloads/jenkins-02.pem"
}

text

---

### 6. Terraform Commands

terraform apply -auto-approve # Create S3 + related resources
terraform destroy -auto-approve # Destroy all created resources

 

---

## PART 2: VPC (Virtual Private Cloud)

### 7. VPC Overview

VPC = your own isolated network in AWS.

Main components (as per notes):

- Internet Gateway (IGW)  
- Public Subnet  
- Private Subnet  
- Route Tables (Public + Private)  
- NAT Gateway (conceptually, not in this exact code)  
- Security Groups, NACLs (not all shown in the snippet)  

---

### 8. Terraform VPC Code (From Notes)

#### 1️⃣ Create VPC

resource "aws_vpc" "vpc" {
cidr_block = "10.0.0.0/24"
instance_tenancy = "default"

tags = {
Name = "myvpc"
}
}

 

#### 2️⃣ Public Subnet

resource "aws_subnet" "pub-sub" {
vpc_id = aws_vpc.vpc.id
cidr_block = "10.0.0.0/25"

tags = {
Name = "public-subnet"
}
}

 

#### 3️⃣ Private Subnet

resource "aws_subnet" "pri-sub" {
vpc_id = aws_vpc.vpc.id
cidr_block = "10.0.0.128/25"

tags = {
Name = "private-subnet"
}
}

 

#### 4️⃣ Public Route Table

resource "aws_route_table" "pubrt" {
vpc_id = aws_vpc.vpc.id

tags = {
Name = "Public RT"
}
}

 

#### 5️⃣ Private Route Table

resource "aws_route_table" "prirt" {
vpc_id = aws_vpc.vpc.id

tags = {
Name = "Private RT"
}
}

 

#### 6️⃣ Internet Gateway

resource "aws_internet_gateway" "igw" {
vpc_id = aws_vpc.vpc.id

tags = {
Name = "IGW"
}
}

 

#### 7️⃣ Public Route to Internet

resource "aws_route" "r" {
route_table_id = aws_route_table.pubrt.id
destination_cidr_block = "0.0.0.0/0"
gateway_id = aws_internet_gateway.igw.id
}

 

#### 8️⃣ Associate Subnets with Route Tables

Public subnet → Public RT
resource "aws_route_table_association" "a" {
subnet_id = aws_subnet.pub-sub.id
route_table_id = aws_route_table.pubrt.id
}

Private subnet → Private RT
resource "aws_route_table_association" "b" {
subnet_id = aws_subnet.pri-sub.id
route_table_id = aws_route_table.prirt.id
}

 

---

### 9. VPC Summary

- Public subnet has a route to IGW via Public RT → internet access. [web:35][web:28]  
- Private subnet uses Private RT with no IGW route → isolated from direct internet. [web:35][web:28]  
- Terraform builds entire network stack: VPC, subnets, RTs, IGW, associations. 
