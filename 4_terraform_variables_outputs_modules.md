# 4_terraform_variables_outputs_modules.md

# ✅ Terraform Variables, Outputs, and Modules – Full Notes

---

## 1. Terraform Variables

Variables store values for reuse and prevent hardcoding.

Benefits:

- Soft coding (change one place, reuse everywhere)  
- Easier configuration  
- Better security for sensitive values when combined with tfvars/backends  

---

### Syntax (variables.tf)

variable "resource_name" {
description = "description about the variable"
default = "value of the variable"
}

 

Access in `main.tf`:

var.resource_name

 

---

### Example variables.tf (From Notes)

variable "instance_type" {
description = "storing instance type"
default = "t2.micro"
}

variable "ami" {
description = "storing ami of instance"
default = "ami-09a38e2e7a3cc42de"
}

variable "Name" {
description = "specifying instance name"
default = "ubuntu-01"
}

 

---

### Using Variables in main.tf (From Notes)

resource "aws_instance" "ins" {
ami = var.ami
instance_type = var.instance_type
count = 3

tags = {
Name = var.Name
}
}

 

---

## 2. Terraform Outputs

Outputs print important values after `terraform apply`, such as:

- Instance ID  
- Public IP  
- Private IP  

**Syntax (outputs.tf):**

output "resource_name" {
description = "description"
value = <resource>.<attribute>
}

 

---

### Example Outputs (From Notes)

output "instance_id" {
description = "getting output of instance id"
value = aws_instance.ins.id
}

output "public_ip" {
description = "output of public IP"
value = aws_instance.ins.public_ip
}

 

- Good practice is to keep outputs in a dedicated `outputs.tf` file. [web:29][web:36]  

---

## 3. Terraform Modules

Modules help organize and reuse Terraform code.

Example folder structure (as per your notes):

terraform-offline/
├── EC2/
│ ├── main.tf
│ ├── variables.tf
│ └── output.tf
├── S3/
│ ├── main.tf
│ ├── variables.tf
│ └── output.tf
├── VPC/
│ ├── main.tf
│ ├── variables.tf
│ └── output.tf
├── provider.tf
├── terraform.tfstate
└── main.tf # where modules are called

 

---

### Calling Modules in Root main.tf

module "ec2" {
source = "./EC2"
}

module "s3" {
source = "./S3"
}

module "vpc" {
source = "./VPC"
}

 

- `source` points to local module folders (or registry/git URLs). [web:30][web:37]  

---

## 4. terraform refresh (From Notes)

terraform refresh

 

- Updates state file with the **current real-world** infrastructure. [web:36][web:29]  
- Useful when changes were made outside Terraform and you want state to reflect reality.

---

## 5. VPC Recap (From Combined Notes)

- VPC = isolated network in AWS  
- Contains:
  - IGW
  - Subnets
  - Route Tables
  - NAT, SG, NACL (not fully shown in code)  
- Terraform module structure keeps EC2, S3, VPC logic separate and reusable. [web:28][web:35]  