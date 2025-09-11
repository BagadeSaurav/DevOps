# Terraform AWS EKS Cluster Setup Using Default VPC

This project demonstrates how to create an AWS EKS (Elastic Kubernetes Service) cluster using Terraform, leveraging the **default VPC** and automatically selecting subnets from specified availability zones.

---

## ✅ Prerequisites

* Terraform installed
* AWS CLI installed and configured with credentials
* An AWS account with permission to create IAM Roles, EKS Clusters, Node Groups, etc.

---

## 📋 Architecture Overview

* **EKS Cluster**
* **EKS Managed Node Group**
* Uses the **default VPC and default subnets**
* IAM roles for EKS Cluster and Node Group with necessary policies
* Automatic filtering of subnets by availability zone

---

## 🚀 Steps to Deploy

### 1️⃣ Clone the Repository

```bash
git clone https://github.com/your-repo/eks-terraform.git
cd eks-terraform
```

### 2️⃣ Initialize Terraform

```bash
terraform init
```

### 3️⃣ Apply Terraform Configuration

```bash
terraform apply
```

You will be prompted to confirm the apply. Type `yes` to proceed.

---

## ✅ Terraform Configuration

```hcl
provider "aws" {
  region = "us-east-1"
}

data "aws_vpc" "default" {
  default = true
}

data "aws_subnets" "default" {
  filter {
    name   = "vpc-id"
    values = [data.aws_vpc.default.id]
  }
}

data "aws_subnet" "selected" {
  for_each = toset(data.aws_subnets.default.ids)
  id       = each.value
}

locals {
  allowed_azs = ["us-east-1a", "us-east-1b", "us-east-1c", "us-east-1d", "us-east-1f"]

  filtered_subnets = [
    for s in data.aws_subnet.selected :
    s.id if contains(local.allowed_azs, s.availability_zone)
  ]
}

resource "aws_iam_role" "eks_cluster_role" {
  name = "eks_cluster_role"

  assume_role_policy = jsonencode({
    Version = "2012-10-17",
    Statement = [{
      Effect    = "Allow"
      Principal = { Service = "eks.amazonaws.com" }
      Action    = ["sts:AssumeRole", "sts:TagSession"]
    }]
  })
}

resource "aws_iam_role_policy_attachment" "eks_cluster_AmazonEKSClusterPolicy" {
  role       = aws_iam_role.eks_cluster_role.name
  policy_arn = "arn:aws:iam::aws:policy/AmazonEKSClusterPolicy"
}

resource "aws_eks_cluster" "cluster" {
  name     = "cluster"
  role_arn = aws_iam_role.eks_cluster_role.arn

  vpc_config {
    subnet_ids = local.filtered_subnets
  }

  depends_on = [
    aws_iam_role_policy_attachment.eks_cluster_AmazonEKSClusterPolicy
  ]
}

resource "aws_iam_role" "eks_node_group_role" {
  name = "eks_node_group_role"

  assume_role_policy = jsonencode({
    Version = "2012-10-17",
    Statement = [{
      Effect    = "Allow"
      Principal = { Service = "ec2.amazonaws.com" }
      Action    = "sts:AssumeRole"
    }]
  })
}

resource "aws_iam_role_policy_attachment" "eks_node_AmazonEKSWorkerNodePolicy" {
  role       = aws_iam_role.eks_node_group_role.name
  policy_arn = "arn:aws:iam::aws:policy/AmazonEKSWorkerNodePolicy"
}

resource "aws_iam_role_policy_attachment" "eks_node_AmazonEKS_CNI_Policy" {
  role       = aws_iam_role.eks_node_group_role.name
  policy_arn = "arn:aws:iam::aws:policy/AmazonEKS_CNI_Policy"
}

resource "aws_iam_role_policy_attachment" "eks_node_AmazonEC2ContainerRegistryReadOnly" {
  role       = aws_iam_role.eks_node_group_role.name
  policy_arn = "arn:aws:iam::aws:policy/AmazonEC2ContainerRegistryReadOnly"
}

resource "aws_iam_role_policy_attachment" "eks_node_AmazonEKSWorkerNodeMinimalPolicy" {
  role       = aws_iam_role.eks_node_group_role.name
  policy_arn = "arn:aws:iam::aws:policy/AmazonEKSWorkerNodeMinimalPolicy"
}

resource "aws_eks_node_group" "example" {
  cluster_name    = aws_eks_cluster.cluster.name
  node_group_name = "node-group"
  node_role_arn   = aws_iam_role.eks_node_group_role.arn
  subnet_ids      = local.filtered_subnets
  instance_types  = ["t2.medium"]

  scaling_config {
    desired_size = 2
    max_size     = 3
    min_size     = 1
  }

  update_config {
    max_unavailable = 1
  }

  depends_on = [
    aws_iam_role_policy_attachment.eks_node_AmazonEKSWorkerNodePolicy,
    aws_iam_role_policy_attachment.eks_node_AmazonEKS_CNI_Policy,
    aws_iam_role_policy_attachment.eks_node_AmazonEC2ContainerRegistryReadOnly,
    aws_iam_role_policy_attachment.eks_node_AmazonEKSWorkerNodeMinimalPolicy
  ]
}

output "eks_cluster_endpoint" {
  description = "EKS Cluster API Endpoint"
  value       = aws_eks_cluster.cluster.endpoint
}

output "eks_cluster_name" {
  description = "EKS Cluster Name"
  value       = aws_eks_cluster.cluster.name
}

output "filtered_subnet_ids" {
  description = "Filtered Subnets used by EKS"
  value       = local.filtered_subnets
}
```

---

## ⚡ Output Example

After a successful `terraform apply`, you will get output similar to:

```text
eks_cluster_endpoint = "https://XXXXXXXX.gr7.us-east-1.eks.amazonaws.com"
eks_cluster_name     = "cluster"
filtered_subnet_ids  = ["subnet-xxxxxx", "subnet-yyyyyy"]
```

---

## 🔧 Clean Up Resources

```bash
terraform destroy
```

---

## 📚 Notes

* This setup uses the **default VPC**.
* Subnets are filtered by allowed availability zones.
* Instance type for nodes is `t2.medium`.
* Node group scaling config is set as desired=2, max=3, min=1.
