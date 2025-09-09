# 🌐 Terraform & AWS CLI Installation & AWS Configuration on Ubuntu

This guide explains how to install **Terraform**, **AWS CLI**, and configure AWS credentials on **Ubuntu**.

---

##  Install Terraform

### 1️⃣ Add HashiCorp GPG key and repository

```bash
wget -O - https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(grep -oP '(?<=UBUNTU_CODENAME=).*' /etc/os-release || lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
```

### 2️⃣ Update package list and install Terraform

```bash
sudo apt update && sudo apt install terraform -y
```

### Verify Terraform installation

```bash
terraform -version
```

---

## Install AWS CLI

### 1️⃣ Download AWS CLI installer

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
```

### 2️⃣ Install unzip if not installed

```bash
sudo apt install unzip -y
```

### 3️⃣ Unzip the installer

```bash
unzip awscliv2.zip
```

### 4️⃣ Install AWS CLI

```bash
sudo ./aws/install
```

### Verify AWS CLI installation

```bash
aws --version
```

---

## Configure AWS CLI

```bash
aws configure
```

Enter your AWS credentials when prompted:

```
AWS Access Key ID [None]: YOUR_ACCESS_KEY_ID
AWS Secret Access Key [None]: YOUR_SECRET_ACCESS_KEY
Default region name [None]: us-east-1
Default output format [None]: json
```

# 🌐 Infrastructure as Code (IaC) & Terraform Basics

---

##  1️⃣ Introduction to IAC

**Definition**:
IaC means writing code (instead of clicking manually in AWS/Azure GUI) to create and manage infrastructure like servers, networks, databases, etc.

**Idea**:
Just like you use code to build an app, you use code to build infrastructure.

###  Benefits:

*  Repeatable → Same setup every time.
*  Automated → Saves manual effort.
*  Version-controlled → Stored in Git, so changes are trackable.
*  Scalable → Easy to deploy infra across multiple environments (Dev, Test, Prod).

👉 **Example**:
Instead of manually creating an EC2 in AWS Console, you write a Terraform file and just run `terraform apply`.

---

## ✅ 2️⃣ Why we need IAC (Difference between Shell Script, Ansible, and IAC tools like Terraform)

| Aspect              | Shell Script                                          | Ansible                                    | IAC Tool (Terraform)                                                 |
| ------------------- | ----------------------------------------------------- | ------------------------------------------ | -------------------------------------------------------------------- |
| **Purpose**         | Automates tasks (e.g., install software, copy files). | Config management + automation.            | Full infra provisioning (VMs, networks, DBs).                        |
| **State awareness** | No state awareness → runs blindly.                    | Limited state tracking.                    | Maintains state file → knows what exists and what needs change.      |
| **Idempotency**     | ❌ No → may create duplicates.                         | ✅ Yes → ensures final state.               | ✅ Yes → ensures infrastructure matches code.                         |
| **Cloud support**   | Not cloud-focused.                                    | Supports cloud but mainly for config mgmt. | Designed for multi-cloud infra provisioning.                         |
| **Example**         | Bash script: `apt-get install nginx`                  | Ansible Playbook to install Nginx          | Terraform code to create EC2 + attach security group + install Nginx |

👉 **In short**:
Shell Script = manual automation.
Ansible = config management & software deployment.
Terraform (IAC) = provisioning complete infra in a controlled, declarative way.

---

## ✅ 3️⃣ Terraform Language (Basic Syntax)

Terraform files are written in HCL (HashiCorp Configuration Language).

### Example:

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "my_ec2" {
  ami           = "ami-12345678"
  instance_type = "t2.micro"
}
```

* **Provider** → Defines which cloud/service you are using (AWS, Azure, GCP).
* **Resource** → Defines what infra you want (EC2, VPC, S3, etc.).
* **Arguments** → Settings inside resources (ami, instance\_type).

👉 It’s declarative → you say *what* you want, Terraform figures out *how* to do it.

---

## ✅ 4️⃣ Enlist the Blocks used in Terraform Language

* **provider** → Defines the provider (AWS, Azure, etc.)

```hcl
provider "aws" {
  region = "us-east-1"
}
```

* **resource** → Defines infrastructure resources

```hcl
resource "aws_instance" "example" {
  ami           = "ami-12345"
  instance_type = "t2.micro"
}
```

* **variable** → Input values (like parameters)

```hcl
variable "region" {
  default = "us-east-1"
}
```

* **output** → Shows values after deployment

```hcl
output "instance_ip" {
  value = aws_instance.example.public_ip
}
```

* **module** → Group of Terraform files reused as a package

```hcl
module "vpc" {
  source = "./modules/vpc"
}
```

* **locals** → Define local variables

```hcl
locals {
  env = "dev"
}
```

* **data** → Fetch existing info (e.g., latest AMI)

```hcl
data "aws_ami" "latest" {
  most_recent = true
  owners      = ["amazon"]
}
```

---

## ✅ 5️⃣ Terraform file that creates an EC2 instance on AWS

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "my_ec2" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"

  tags = {
    Name = "MyFirstEC2"
  }
}
```

---

## ✅ 6️⃣ Terraform Script to Deploy Security Group with HEREDOC in UserData

### 1️⃣ Introduction to Security Groups

A Security Group acts as a virtual firewall for your instance to control inbound and outbound traffic. Terraform allows you to define and manage Security Groups using Infrastructure as Code.

---

### 2️⃣ Terraform Script for Security Group

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_security_group" "web_sg" {
  name_prefix = "web-sg-"
  description = "Allow inbound HTTP and SSH traffic"

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    from_port   = 22
    to_port     = 22
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
    Name = "web-sg"
  }
}
```

---

### 3️⃣ HEREDOC in UserData

What is HEREDOC?
HEREDOC (Here Document) is a multi-line string syntax in Terraform used to define large blocks of text or commands. It is often utilized in UserData to pass startup scripts to cloud instances.

---

#### Example with UserData:

```hcl
resource "aws_instance" "web_server" {
  ami           = "ami-12345678"
  instance_type = "t2.micro"

  user_data = <<-EOF
    #!/bin/bash
    yum update -y
    yum install -y httpd
    echo "Hello, World!" > /var/www/html/index.html
    systemctl start httpd
    systemctl enable httpd
  EOF

  tags = {
    Name = "web-server"
  }
}
```

Another Example:

```hcl
user_data = <<EOF
  ${file("index.sh")}
EOF
```

👉 **HEREDOC Syntax**:

* `<<-EOF`: Begins the HEREDOC. The `-` allows indentation.
* Content: Multi-line script or text.
* `EOF`: Ends the block.

---

## ✅ 4️⃣ Key Blocks in the Terraform Script

* **Provider Block**

```hcl
provider "aws" {
  region = "us-east-1"
}
```

* **Resource Block**

```hcl
resource "aws_security_group" "web_sg" {
  name_prefix = "web-sg-"
  description = "Allow inbound HTTP and SSH traffic"
}
```

* **Variable Block**

```hcl
variable "region" {
  default = "us-east-1"
}
```

* **Data Block**

```hcl
data "aws_ami" "latest" {
  most_recent = true
  owners      = ["self"]
}
```

* **Output Block**

```hcl
output "security_group_id" {
  value = aws_security_group.web_sg.id
}
```

---

## ✅ 5️⃣ Applying the Script

1. Initialize Terraform:

```bash
terraform init
```

2. Validate the script:

```bash
terraform validate
```

3. Plan the execution:

```bash
terraform plan
```

4. Apply the changes:

```bash
terraform apply
```
---
# 🌐 Terraform Loops & Important Commands

Terraform provides powerful constructs for iterating over collections like lists and maps.
The primary looping mechanisms are **count**, **for\_each**, and **for**.

---

## ✅ 1️⃣ count

**Definition**:
The `count` parameter allows you to specify how many instances of a resource to create.

**Usage**:
Works well for creating identical resources.

### ✅ Example:

```hcl
resource "aws_instance" "example" {
  count         = 3
  ami           = "ami-12345678"
  instance_type = "t2.micro"
}
```

👉 In this example, **three EC2 instances** are created.

### Accessing Instances:

```hcl
aws_instance.example[0]  # First instance  
aws_instance.example[1]  # Second instance  
aws_instance.example[2]  # Third instance  
```

---

## ✅ 2️⃣ for\_each

**Definition**:
The `for_each` meta-argument allows iterating over a **map** or **set** to create resources with distinct properties.

**Usage**:
Useful when resource properties vary.

### ✅ Example:

```hcl
provider "aws" {
  region = "us-west-2"
}

resource "aws_s3_bucket" "example" {
  for_each = {
    dev  = "dev-bucket-unique-1"
    prod = "prod-bucket-unique-2"
  }

  bucket = each.value
}
```

👉 This creates **two S3 buckets**:

* `dev-bucket-unique-1`
* `prod-bucket-unique-2`

### Accessing Instances:

```hcl
aws_s3_bucket.example["dev"]   # Dev bucket  
aws_s3_bucket.example["prod"]  # Prod bucket  
```

---

## ✅ 3️⃣ for Expression

**Definition**:
The `for` expression is used to **transform** or **filter** collections.

**Usage**:
Commonly used in variables and outputs.

### ✅ Example 1: Transform to Uppercase

```hcl
variable "names" {
  default = ["Alice", "Bob", "Charlie"]
}

output "uppercase_names" {
  value = [for name in var.names : upper(name)]
}
```

👉 Outputs: `["ALICE", "BOB", "CHARLIE"]`

---

### ✅ Example 2: Filter Names Longer Than 3 Characters

```hcl
output "filtered_names" {
  value = [for name in var.names : name if length(name) > 3]
}
```

👉 Filters names: `["Alice", "Charlie"]`

---

## ✅ Comparison Table

| Feature    | count                  | for\_each                  | for (expression)         |
| ---------- | ---------------------- | -------------------------- | ------------------------ |
| Input Type | Number                 | Map or Set                 | List, Map, or Set        |
| Use Case   | Create identical items | Create unique items        | Transform or filter data |
| Example    | EC2 instances          | S3 buckets with unique IDs | Modify list of names     |

---

## ✅ Terraform Commands and Provisioners

### ✅ 1️⃣ Taint Command

Marks a resource for **recreation** during the next `terraform apply`.

```bash
terraform taint <resource_name>
```

#### Example:

```bash
terraform taint aws_instance.my_instance
```

👉 Marks the EC2 instance for recreation.

---

### ✅ 2️⃣ Import Command

Imports existing infrastructure resources into Terraform state.

```bash
terraform import <resource_type>.<resource_name> <resource_id>
```

#### Example:

```bash
terraform import aws_instance.my_instance i-0abcd1234efgh5678
```

👉 Imports EC2 instance ID `i-0abcd1234efgh5678` as `aws_instance.my_instance`.

---

### ✅ 3️⃣ Destroy Command

Removes all resources defined in the configuration.

```bash
terraform destroy
```

#### Targeted Destroy (-target):

```bash
terraform destroy -target=<resource_type>.<resource_name>
```

##### Example:

```bash
terraform destroy -target=aws_instance.my_instance
```
