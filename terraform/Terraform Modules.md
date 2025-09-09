Here is the updated `README.md` file including a clear **tree diagram** of the folder structure:

---

# 🌍 Terraform Modules: VPC, Subnet, and EC2 Example

This guide explains how to create reusable **Terraform Modules** to provision AWS resources like **VPC**, **Subnet**, and **EC2 instances**, manage dependencies, and organize your Infrastructure as Code (IaC) for production-ready setups.

---

## 📌 1. What is a Terraform Module?

A **module** in Terraform is a container for multiple resources that are used together.
Think of it as a function in programming:

* Define once
* Pass input variables
* Get outputs
* Reuse anywhere

Your Terraform project itself is the **root module**.
Child modules can be created in a `modules/` folder and called from the root module.

---

## 📌 2. Folder Structure Setup

### ✅ Project Folder Tree Structure

```plaintext
terraform-project/
├── main.tf
├── variables.tf
├── outputs.tf
├── provider.tf
├── modules/
│   ├── vpc/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── outputs.tf
│   ├── subnet/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── outputs.tf
│   └── ec2/
│       ├── main.tf
│       ├── variables.tf
│       └── outputs.tf
```

---

## 📌 3. Root Module (`main.tf`)

```hcl
provider "aws" {
  region = "us-east-1"
}

module "vpc" {
  source   = "./modules/vpc"
  vpc_cidr = "10.0.0.0/16"
}

module "subnet" {
  source      = "./modules/subnet"
  vpc_id      = module.vpc.vpc_id
  subnet_cidr = "10.0.1.0/24"
}

module "ec2" {
  source        = "./modules/ec2"
  subnet_id     = module.subnet.subnet_id
  instance_type = "t2.micro"
}
```

---

## 📌 4. VPC Module (`modules/vpc`)

### ✅ main.tf

```hcl
resource "aws_vpc" "this" {
  cidr_block = var.vpc_cidr
  tags = {
    Name = "MyVPC"
  }
}
```

### ✅ variables.tf

```hcl
variable "vpc_cidr" {
  type = string
}
```

### ✅ outputs.tf

```hcl
output "vpc_id" {
  value = aws_vpc.this.id
}
```

---

## 📌 5. Subnet Module (`modules/subnet`)

### ✅ main.tf

```hcl
resource "aws_subnet" "this" {
  vpc_id     = var.vpc_id
  cidr_block = var.subnet_cidr
  tags = {
    Name = "MySubnet"
  }
}
```

### ✅ variables.tf

```hcl
variable "vpc_id" {}
variable "subnet_cidr" {}
```

### ✅ outputs.tf

```hcl
output "subnet_id" {
  value = aws_subnet.this.id
}
```

---

## 📌 6. EC2 Module (`modules/ec2`)

### ✅ main.tf

```hcl
data "aws_ami" "amazon_linux" {
  most_recent = true
  owners      = ["amazon"]

  filter {
    name   = "name"
    values = ["amzn2-ami-hvm-*-x86_64-gp2"]
  }
}

resource "aws_instance" "this" {
  ami           = data.aws_ami.amazon_linux.id
  instance_type = var.instance_type
  subnet_id     = var.subnet_id

  tags = {
    Name = "MyEC2"
  }
}
```

### ✅ variables.tf

```hcl
variable "instance_type" {}
variable "subnet_id" {}
```

### ✅ outputs.tf

```hcl
output "instance_id" {
  value = aws_instance.this.id
}
```

---

## 📌 7. Dependencies in Terraform

### ✅ Implicit Dependency

```hcl
subnet_id = aws_subnet.this.id
```

### ✅ Explicit Dependency

```hcl
resource "aws_instance" "this" {
  ami           = data.aws_ami.amazon_linux.id
  instance_type = "t2.micro"
  depends_on    = [aws_vpc.this]
}
```

---

## 📌 8. Terraform CLI Commands

### ✅ Initialize working directory

```bash
terraform init
```

### ✅ Validate configuration files

```bash
terraform validate
```

### ✅ Format configuration files

```bash
terraform fmt
```

### ✅ Generate execution plan

```bash
terraform plan
```

### ✅ Apply the execution plan

```bash
terraform apply
```

### ✅ Destroy the infrastructure

```bash
terraform destroy
```

### ✅ Show Terraform State

```bash
terraform show
```

---

## 🚀 Conclusion

This modular structure helps you build production-ready, maintainable Terraform configurations that promote **reuse**, **clarity**, and **scalability**.
Simply run the above commands to provision or destroy your infrastructure with ease.

---

🌟 Happy Terraforming!
