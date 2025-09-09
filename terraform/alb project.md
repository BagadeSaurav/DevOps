# 🚀 AWS ALB + ASG Setup with Home and Cloth Applications

This Terraform project provisions an **Application Load Balancer (ALB)** with two Auto Scaling Groups (ASGs), serving:
- **Home App** at `/`
- **Cloth App** at `/cloth`

---

## 📂 Project Structure

```text
.
├── main.tf                   # Core infrastructure resources
├── variables.tf              # Variables for easy customization
├── output.tf                 # Outputs ALB DNS name
├── terraform.tfstate         # Terraform state (auto-generated)
├── README.md                  # Project documentation


## ✅ Features
- Secure Security Group (HTTP, SSH access)
- Home and Cloth applications served by separate ASGs
- ALB with listener and routing rules
- Health checks for Target Groups
- CloudWatch alarms + Scaling Policies
- Outputs ALB DNS name for access

---

## ⚡ Prerequisites

- Terraform installed ([Install Terraform](https://developer.hashicorp.com/terraform/downloads))
- AWS CLI configured with credentials
- Existing SSH keypair in AWS (`key_name` in variables)

---

## ⚙️ Variables

```hcl
variable "ami" {
  default = "ami-0360c520857e3138f"
  type    = string
}

variable "instance_type" {
  default = "t2.micro"
  type    = string
}

variable "availability_zones" {
  default = ["us-east-1a", "us-east-1b", "us-east-1c"]
}

variable "key_name" {
  default = "saurav"
}
