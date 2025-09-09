# Terraform  VPC EC2 Multi-Environment Setup  

This repository contains Terraform code to provision **AWS infrastructure** with multiple environments (`dev`, `staging`, and `prod`) using **Terraform Workspaces** and `.tfvars` files.  

## 📂 Project Structure  

customVPC-workspace/
├─ main.tf               
├─ variables.tf           
├─ workspace/
│  ├─ dev.tfvars   
│  ├─ stage.tfvars        
│  └─ prod.tfvars         


## 🚀 Features  
- **Multi-environment setup** with Terraform Workspaces.  
- Creates **VPC, Public & Private Subnets**.  
- Configures **Internet Gateway & Route Tables**.  
- Deploys **Security Groups** allowing SSH (22) & HTTP (80).  
- Provisions **Public & Private EC2 instances**.  

## ⚙️ Prerequisites  
- [Terraform](https://developer.hashicorp.com/terraform/downloads) installed.  
- AWS account with access keys configured (`aws configure`).  
- Existing EC2 **Key Pair** in AWS (update the `keypair` variable).  

---

## 🛠 Usage  

### 1️⃣ Initialize Terraform  

terraform init

### 2️⃣ Create a Workspace for Each Environment

```sh
terraform workspace new dev
terraform workspace new stage
terraform workspace new prod
```

### 3️⃣ Select Workspace

```sh
terraform workspace select dev
```

### 4️⃣ Apply Configuration with Environment-Specific Vars

```sh
terraform apply -var-file=workspace/dev.tfvars
```

For staging:

```sh
terraform workspace select stage
terraform apply -var-file=workspace/stage.tfvars
```

For production:

```sh
terraform workspace select prod
terraform apply -var-file=workspace/prod.tfvars
```

### 5️⃣ Destroy Resources

```sh
terraform destroy -var-file=workspace/dev.tfvars
```

---

## 📜 Terraform Code

### `main.tf`

```hcl
provider "aws" {
  region = "us-east-1"
}

locals {
  env = terraform.workspace
}

# VPC 
resource "aws_vpc" "main" {
  cidr_block           = var.vpc_cidr
  enable_dns_support   = true
  enable_dns_hostnames = true
  tags = {
    Name = "${local.env}-vpc"
  }
}

# Subnets 
resource "aws_subnet" "public" {
  vpc_id                  = aws_vpc.main.id
  cidr_block              = var.public_subnet_cidr
  map_public_ip_on_launch = true
  availability_zone       = var.az1
  tags = {
    Name = "${local.env}-public-subnet"
  }
}

resource "aws_subnet" "private" {
  vpc_id            = aws_vpc.main.id
  cidr_block        = var.private_subnet_cidr
  availability_zone = var.az2
  tags = {
    Name = "${local.env}-private-subnet"
  }
}

# Internet Gateway 
resource "aws_internet_gateway" "igw" {
  vpc_id = aws_vpc.main.id
  tags = {
    Name = "${local.env}-igw"
  }
}

# Route Tables 
resource "aws_route_table" "public_rt" {
  vpc_id = aws_vpc.main.id
  tags = {
    Name = "${local.env}-public-rt"
  }
}

resource "aws_route" "public_internet" {
  route_table_id         = aws_route_table.public_rt.id
  destination_cidr_block = "0.0.0.0/0"
  gateway_id             = aws_internet_gateway.igw.id
}

resource "aws_route_table_association" "public_assoc" {
  subnet_id      = aws_subnet.public.id
  route_table_id = aws_route_table.public_rt.id
}

resource "aws_route_table" "private_rt" {
  vpc_id = aws_vpc.main.id
  tags = {
    Name = "${local.env}-private-rt"
  }
}

resource "aws_route_table_association" "private_assoc" {
  subnet_id      = aws_subnet.private.id
  route_table_id = aws_route_table.private_rt.id
}

# Security Group 
resource "aws_security_group" "ec2_sg" {
  name        = "${local.env}-ec2-sg"
  description = "Allow SSH + HTTP"
  vpc_id      = aws_vpc.main.id

  ingress {
    description = "SSH"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    description = "HTTP"
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags = {
    Name = "${local.env}-sg"
  }
}

# EC2 Instances 

# Public instance
resource "aws_instance" "public_instance" {
  ami                    = var.ami
  instance_type          = var.instance_type
  subnet_id              = aws_subnet.public.id
  vpc_security_group_ids = [aws_security_group.ec2_sg.id]
  key_name               = var.keypair

  tags = {
    Name = "${local.env}-public-instance"
  }
}

# Private instance
resource "aws_instance" "private_instance" {
  ami                    = var.ami
  instance_type          = var.instance_type
  subnet_id              = aws_subnet.private.id
  vpc_security_group_ids = [aws_security_group.ec2_sg.id]
  key_name               = var.keypair

  tags = {
    Name = "${local.env}-private-instance"
  }
}
```

---

### `variables.tf`

```hcl
variable "vpc_cidr" {}
variable "public_subnet_cidr" {}
variable "private_subnet_cidr" {}
variable "az1" {}
variable "az2" {}
variable "ami" {}
variable "instance_type" { default = "t2.micro" }
variable "keypair" {}
```

---

### `workspace/dev.tfvars`

```hcl
vpc_cidr           = "10.0.0.0/16"
public_subnet_cidr = "10.0.1.0/24"
private_subnet_cidr= "10.0.2.0/24"
az1                = "us-east-1a"
az2                = "us-east-1b"
ami                = "ami-0360c520857e3138f"
instance_type      = "t2.micro"
keypair            = "saurav"
```

---

### `workspace/stage.tfvars`

```hcl
vpc_cidr            = "10.1.0.0/16"
public_subnet_cidr  = "10.1.1.0/24"
private_subnet_cidr = "10.1.2.0/24"
az1                 = "us-east-1a"
az2                 = "us-east-1b"
ami                 = "ami-0360c520857e3138f"   
instance_type       = "t2.small"                
keypair             = "saurav"
```

---

### `workspace/prod.tfvars`

```hcl
vpc_cidr            = "10.2.0.0/16"
public_subnet_cidr  = "10.2.1.0/24"
private_subnet_cidr = "10.2.2.0/24"
az1                 = "us-east-1a"
az2                 = "us-east-1b"
ami                 = "ami-0360c520857e3138f"   
instance_type       = "t2.nano"                
keypair             = "saurav"
```

---

## 🧹 Cleanup

To avoid extra AWS costs, always run:

```sh
terraform destroy -var-file=workspace/dev.tfvars
```

(or `stage.tfvars` / `prod.tfvars`) after testing.

