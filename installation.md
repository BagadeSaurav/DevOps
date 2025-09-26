Installation ubuntu tools 

## 🚀 Install AWS CLI on Ubuntu

### 1. Install unzip (required)

```bash
sudo apt update -y
sudo apt install unzip -y
```

### 2. Download the AWS CLI v2 bundle

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
```

### 3. Extract the installer

```bash
unzip awscliv2.zip
```

### 4. Run the installer

```bash
sudo ./aws/install
```

### 5. Verify installation

```bash
aws --version
```

✅ Expected output: `aws-cli/2.x.x Python/3.x Linux/x86_64`

---

## ⚙️ Configure AWS CLI

To connect your AWS account with the CLI, configure your credentials:

```bash
aws configure
```

It will ask for:

* **AWS Access Key ID**
* **AWS Secret Access Key**
* **Default region name** (e.g., `ap-south-1` for Mumbai)
----------

# 🚀 Install Git on Ubuntu

### **Step 1: Update System Packages**

```bash
sudo apt update -y
sudo apt upgrade -y
```

---

### **Step 2: Install Git**

```bash
sudo apt install git -y
```

---

### **Step 3: Verify Installation**

```bash
git --version
```

✅ Example output: `git version 2.41.0`

---

### **Step 4: Configure Git (Optional but Recommended)**

Set your **username** and **email** for commits:

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

Check configuration:

```bash
git config --list
```

------------------
Install Docker on Ubuntu

```bash
# Switch to root user
sudo -i

# Update package index
apt update -y

# Install Docker (from Ubuntu repo)
apt install docker.io -y

# Enable Docker service to start on boot
systemctl enable docker

# Start Docker service
systemctl start docker

# Check Docker status
systemctl status docker

# Verify installation
docker --version

# Run test container
docker run hello-world
```

---
Install Docker on Ubuntu (Recommended Official Way)
link of document follow steps

https://docs.docker.com/engine/install/ubuntu/

---

# 🚀 Install Terraform on Ubuntu (APT Repo Method)

### 1. Add HashiCorp GPG key

```bash
wget -O - https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
```

### 2. Add HashiCorp repository

```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(grep -oP '(?<=UBUNTU_CODENAME=).*' /etc/os-release || lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
```

### 3. Update package list and install Terraform

```bash
sudo apt update && sudo apt install terraform -y
```

### 4. Verify installation

```bash
terraform -version
```

✅ Expected output: `Terraform vX.X.X`

---

👉 This method is better than manually downloading ZIPs because it allows **easy upgrades** with:

```bash
sudo apt update && sudo apt upgrade -y
```
-------------

Here’s a polished **Markdown (.md)** version of your Jenkins installation guide 👇

---

# 🚀 Install Jenkins on Ubuntu

Follow these steps to install Jenkins on **Ubuntu**:

---

## 1. Update System Packages

```bash
sudo apt update -y && sudo apt upgrade -y
```

---

## 2. Install Java Development Kit (JDK)

Jenkins requires Java to run. It is recommended to install **OpenJDK 17** or later.

```bash
sudo apt install openjdk-17-jdk -y
```

✅ Verify Java installation:

```bash
java -version
```

---

## 3. Add Jenkins Repository Key

```bash
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
```

---

## 4. Add Jenkins Repository

```bash
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/" | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
```

---

## 5. Install Jenkins

```bash
sudo apt update
sudo apt install jenkins -y
```

---

## 6. Verify Jenkins Service Status

```bash
sudo systemctl status jenkins
```

✅ You should see **active (running)** in the output.

---

## 7. Adjust Firewall (if UFW is active)

If you have UFW enabled, open **port 8080** to access Jenkins.

```bash
sudo ufw allow 8080
sudo ufw enable   # If not already enabled
```

---

## 8. Initial Jenkins Setup

1. Open your browser and go to:

   ```
   http://your_server_ip_or_domain:8080
   ```

2. Retrieve the initial admin password:

   ```bash
   sudo cat /var/lib/jenkins/secrets/initialAdminPassword
   ```

3. Copy the password, paste it into the Jenkins setup page, and continue with the installation wizard.

---


# 🛡️ Add User to Sudoers with `visudo` jenkins

### 1. Open sudoers file safely

```bash
sudo visudo
```

> This opens the `/etc/sudoers` file in a safe editor (usually `nano` or `vi`).
> **Do not edit `/etc/sudoers` directly** — mistakes can lock you out.

---

### 2. Add the user with sudo privileges

At the end of the file, add a line like:

```
jenkins ALL=(ALL) NOPASSWD:ALL
```

* **jenkins** → replace with the username you want to give sudo privileges.
* **NOPASSWD** → optional, allows running sudo commands without typing a password.
* **ALL=(ALL) ALL** → allows full sudo access.

---



 Perfect! Let’s install **SonarQube using Docker** on Ubuntu (or any Linux system with Docker installed). I’ll give you a **step-by-step guide with proper permissions and Docker setup**.

---

# 🚀 Install SonarQube using Docker

### **Prerequisites**

1. Docker installed and running

   ```bash
   sudo apt update -y
   sudo apt install docker.io -y
   sudo systemctl enable docker
   sudo systemctl start docker
   ```
2. Docker Compose (optional but recommended)

   ```bash
   sudo apt install docker-compose -y
   ```

---

### **Step 1: install Docker **

```bash
sudo apt update -y
sudo apt install docker.io -y

```

---

### **Step 2: Run SonarQube Container**

```bash
docker run -d --name sonarqube \
  -p 9000:9000 \
  sonarqube:10.6-community
```

**Explanation:**

* `-d` → Run container in background
* `--name sonarqube` → Container name
* `-p 9000:9000` → Map container port 9000 to host port 9000
* `SONAR_ES_BOOTSTRAP_CHECKS_DISABLE=true` → Disable Elasticsearch bootstrap checks for small systems

---

### **Step 3: Verify Container**

```bash
docker ps
```

✅ You should see a container named `sonarqube` running.

---

### **Step 4: Access SonarQube**

* Open a browser and go to:

```
http://your_server_ip:9000
```

* Default credentials:

  * **Username:** `admin`
  * **Password:** `admin`

---









