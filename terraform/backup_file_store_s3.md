Here is the **simplified Terraform code** that only:

1. Launches an EC2 instance.
2. Uses S3 as the Terraform backend (for storing the state file).

---

### ✅ 1️⃣ Terraform Code 

main.tf

```hcl
provider "aws" {
  region = "us-east-1"
}

# EC2 Instance
resource "aws_instance" "web_server" {
  ami           = "ami-0c55b159cbfafe1f0"  # Example: Amazon Linux 2 AMI (update as needed)
  instance_type = "t2.micro"
  key_name      = "saurav"

  tags = {
    Name = "Terraform-EC2-Instance"
  }
}
```

---

### ✅ 2️⃣ Example Commands to Run

```bash
# Initialize Terraform and configure the S3 backend
terraform init

# Validate the configuration
terraform validate

# Apply to launch EC2 instance
terraform apply
# Confirm by typing "yes"

# To destroy the EC2 instance
terraform destroy
# Confirm by typing "yes"
```

---

### ✅ 3️⃣ Example README.md

```markdown
# Terraform EC2 Instance Launch Example

This project demonstrates using Terraform to launch a simple EC2 instance with the Terraform state stored in an S3 bucket.

## 🔧 Prerequisites
- AWS CLI configured
- Terraform installed
- Existing S3 bucket: `saurav123`

## 📁 Project Structure

```

.
├── main.tf
├── backend.tf
└── README.md

````

## ✅ Backend Configuration

```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
  backend "s3" {
    bucket = "saurav123"
    key    = "terraform.tfstate"
    region = "us-east-1"
  }
}
````

## ⚡ Steps to Run

1. Initialize Terraform:

```bash
terraform init
```

2. Apply the configuration:

```bash
terraform apply
```

3. Destroy the infrastructure:

```bash
terraform destroy
```

## ⚠️ Important Notes

* Make sure the S3 bucket `saurav123` exists before running.
* Use a correct AMI ID available in your AWS region.

```

