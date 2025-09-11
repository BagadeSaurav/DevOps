# AWS EKS Cluster Setup with Custom VPC Using Terraform

This Terraform configuration creates an **Amazon EKS (Elastic Kubernetes Service) Cluster** with a custom VPC, subnets, and an EKS managed node group.

---

## ✅ Architecture Overview

* Custom VPC (10.0.0.0/16)
* Two public subnets in different AZs
* Internet Gateway with Route Table for public internet access
* IAM Roles for EKS Cluster and Node Group
* EKS Cluster
* EKS Node Group (2 EC2 Instances of type `t2.medium`)

---

## ⚡ Prerequisites

* Install [Terraform](https://developer.hashicorp.com/terraform/downloads)
* Configure AWS CLI with credentials
* AWS account with permissions to create VPCs, EKS Clusters, IAM Roles, EC2 instances, etc.

---

## ✅ Terraform Files

```hcl
provider "aws" {
  region = "us-east-1"
}

# Create Custom VPC
resource "aws_vpc" "eks_vpc" {
  cidr_block           = "10.0.0.0/16"
  enable_dns_support   = true
  enable_dns_hostnames = true

  tags = {
    Name = "eks_custom_vpc"
  }
}

# Public Subnets
resource "aws_subnet" "eks_subnet_a" {
  vpc_id                  = aws_vpc.eks_vpc.id
  cidr_block              = "10.0.1.0/24"
  availability_zone       = "us-east-1a"
  map_public_ip_on_launch = true

  tags = {
    Name = "eks_subnet_a"
  }
}

resource "aws_subnet" "eks_subnet_b" {
  vpc_id                  = aws_vpc.eks_vpc.id
  cidr_block              = "10.0.2.0/24"
  availability_zone       = "us-east-1b"
  map_public_ip_on_launch = true

  tags = {
    Name = "eks_subnet_b"
  }
}

# Internet Gateway
resource "aws_internet_gateway" "eks_igw" {
  vpc_id = aws_vpc.eks_vpc.id

  tags = {
    Name = "eks_igw"
  }
}

# Route Table
resource "aws_route_table" "eks_route_table" {
  vpc_id = aws_vpc.eks_vpc.id

  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.eks_igw.id
  }

  tags = {
    Name = "eks_route_table"
  }
}

# Route Table Associations
resource "aws_route_table_association" "a" {
  subnet_id      = aws_subnet.eks_subnet_a.id
  route_table_id = aws_route_table.eks_route_table.id
}

resource "aws_route_table_association" "b" {
  subnet_id      = aws_subnet.eks_subnet_b.id
  route_table_id = aws_route_table.eks_route_table.id
}

# IAM Role for EKS Cluster
resource "aws_iam_role" "eks_cluster_role" {
  name = "eks_cluster_role"

  assume_role_policy = jsonencode({
    Version = "2012-10-17",
    Statement = [{
      Effect    = "Allow"
      Principal = { Service = "eks.amazonaws.com" }
      Action    = "sts:AssumeRole"
    }]
  })
}

resource "aws_iam_role_policy_attachment" "eks_cluster_AmazonEKSClusterPolicy" {
  role       = aws_iam_role.eks_cluster_role.name
  policy_arn = "arn:aws:iam::aws:policy/AmazonEKSClusterPolicy"
}

# EKS Cluster
resource "aws_eks_cluster" "eks_cluster" {
  name     = "custom-eks-cluster"
  role_arn = aws_iam_role.eks_cluster_role.arn

  vpc_config {
    subnet_ids = [
      aws_subnet.eks_subnet_a.id,
      aws_subnet.eks_subnet_b.id
    ]
  }

  depends_on = [
    aws_iam_role_policy_attachment.eks_cluster_AmazonEKSClusterPolicy
  ]
}

# IAM Role for EKS Node Group
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

resource "aws_iam_role_policy_attachment" "AmazonEKSWorkerNodePolicy" {
  role       = aws_iam_role.eks_node_group_role.name
  policy_arn = "arn:aws:iam::aws:policy/AmazonEKSWorkerNodePolicy"
}

resource "aws_iam_role_policy_attachment" "AmazonEKS_CNI_Policy" {
  role       = aws_iam_role.eks_node_group_role.name
  policy_arn = "arn:aws:iam::aws:policy/AmazonEKS_CNI_Policy"
}

resource "aws_iam_role_policy_attachment" "AmazonEC2ContainerRegistryReadOnly" {
  role       = aws_iam_role.eks_node_group_role.name
  policy_arn = "arn:aws:iam::aws:policy/AmazonEC2ContainerRegistryReadOnly"
}

# EKS Node Group
resource "aws_eks_node_group" "eks_node_group" {
  cluster_name    = aws_eks_cluster.eks_cluster.name
  node_group_name = "custom-node-group"
  node_role_arn   = aws_iam_role.eks_node_group_role.arn
  subnet_ids      = [
    aws_subnet.eks_subnet_a.id,
    aws_subnet.eks_subnet_b.id
  ]

  scaling_config {
    desired_size = 2
    max_size     = 3
    min_size     = 1
  }

  instance_types = ["t2.medium"]

  update_config {
    max_unavailable = 1
  }

  depends_on = [
    aws_iam_role_policy_attachment.AmazonEKSWorkerNodePolicy,
    aws_iam_role_policy_attachment.AmazonEKS_CNI_Policy,
    aws_iam_role_policy_attachment.AmazonEC2ContainerRegistryReadOnly
  ]
}

# Outputs
output "eks_cluster_endpoint" {
  description = "EKS Cluster API Endpoint"
  value       = aws_eks_cluster.eks_cluster.endpoint
}

output "eks_cluster_name" {
  description = "EKS Cluster Name"
  value       = aws_eks_cluster.eks_cluster.name
}

output "vpc_id" {
  description = "Custom VPC ID"
  value       = aws_vpc.eks_vpc.id
}

output "subnet_ids" {
  description = "Custom Subnet IDs"
  value       = [aws_subnet.eks_subnet_a.id, aws_subnet.eks_subnet_b.id]
}
```

---

## ✅ Commands to Deploy

```bash
# Initialize Terraform
terraform init

# Preview changes
terraform plan

# Apply the configuration
terraform apply
```

> Confirm `yes` when prompted by `terraform apply`

---

## ✅ Outputs After Apply

* **eks\_cluster\_endpoint** → API endpoint of EKS cluster
* **eks\_cluster\_name** → Name of the cluster
* **vpc\_id** → Custom VPC ID
* **subnet\_ids** → List of Subnet IDs

---

## ✅ Notes

* Make sure your AWS CLI is configured with sufficient privileges.
* Modify `desired_size`, `instance_types`, or CIDR blocks according to your requirements.

---

## ✅ Cleanup

```bash

terraform destroy
```

Would you like me to generate this as an actual `README.md` file for download?
