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
