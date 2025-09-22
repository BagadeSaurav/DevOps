Here’s a **clean, step-by-step documentation** for your setup in Markdown format. I’ve organized it for clarity and ease of use:

---

# EasyCRUD Docker Deployment Documentation

## Overview

This guide explains how to deploy the **EasyCRUD** application using **Jenkins**, **AWS EC2**, **RDS (MariaDB)**, and **Apache**. It includes steps for backend and frontend deployment, connecting to the database, and running the application.

---

## Step 1: Create AWS RDS

1. Log in to your AWS console.
2. Navigate to **RDS > Databases > Create database**.
3. Choose **MariaDB** as the engine.
4. Configure database settings:

   * DB instance class: `db.t2.medium`
   * Storage: As needed
   * Database name: `student_db`
   * Username: `root`
   * Password: `Redhat123`
5. Launch the RDS instance and note the **endpoint URL**.

---

## Step 2: Create EC2 Instance

1. Launch a new EC2 instance:

   * Instance type: `t2.medium`
   * OS: Ubuntu
   * Key pair: Choose or create one
2. Add the following **User Data script** to install Jenkins and dependencies:

```bash
#!/bin/bash
# Update system
apt-get update -y
apt-get upgrade -y

# Install Java
apt install openjdk-17-jdk -y

# Add Jenkins GPG key
mkdir -p /etc/apt/keyrings
wget -O /etc/apt/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key

# Add Jenkins repository
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" | tee /etc/apt/sources.list.d/jenkins.list > /dev/null

# Install Jenkins
apt-get update -y
apt-get install -y jenkins

# Start and enable Jenkins
systemctl start jenkins
systemctl enable jenkins
```

---

## Step 3: Configure Jenkins User Permissions

1. Connect to your EC2 instance:

```bash
ssh -i your-key.pem ubuntu@<ec2-ip>
sudo -i
```

2. Edit sudoers file:

```bash
visudo
```

3. Add the following line to allow Jenkins to run commands without a password:

```text
jenkins ALL=(ALL) NOPASSWD:ALL
```

---

## Step 4: Fork and Update Repository

1. Fork the repository: [EasyCRUD Docker](https://github.com/BagadeSaurav/EasyCRUD-docker.git)
2. Modify configuration files:

**Backend (`backend/src/main/resources/application.properties`):**

```properties
server.port=8081
spring.datasource.url=jdbc:mariadb://<RDS-ENDPOINT>:3306/student_db
spring.datasource.username=root
spring.datasource.password=Redhat123
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MariaDBDialect
```

**Frontend (`frontend/.env`):**

```env
VITE_API_URL = "http://<EC2-IP>:8081/api"
```

---

## Step 5: Configure Jenkins Pipeline

1. Open Jenkins in your browser: `http://<EC2-IP>:8080`
2. Create a **New Pipeline Job**.
3. Use the following **Pipeline Script**:

```groovy
pipeline {
    agent any

    stages {
        stage('Install Packages') {
            steps {
                sh '''
                  sudo apt update -y
                  sudo apt install -y openjdk-17-jdk maven nodejs npm apache2
                  java -version
                  mvn -version
                  node -v
                  npm -v
                '''
            }
        }

        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/BagadeSaurav/EasyCRUD-docker.git'
            }
        }

        stage('Build Backend') {
            steps {
                dir('backend') {
                    sh 'mvn clean install -DskipTests=true'
                }
            }
        }

        stage('Run Backend') {
            steps {
                dir('backend/target') {
                    sh 'nohup java -jar student-registration-backend-0.0.1-SNAPSHOT.jar > app.log 2>&1 &'
                }
            }
        }

        stage('Install Frontend Dependencies') {
            steps {
                dir('frontend') {
                    sh 'npm install'
                }
            }
        }

        stage('Build React App') {
            steps {
                dir('frontend') {
                    sh 'npm run build'
                }
            }
        }

        stage('Deploy to Apache') {
            steps {
                sh '''
                  sudo systemctl start apache2
                  sudo rm -rf /var/www/html/*
                  sudo cp -rf frontend/dist/* /var/www/html/
                '''
            }
        }
    }
}
```

4. Apply and save the pipeline.
5. Click **Build Now** to deploy the application.

---

## Step 6: Verify Backend Connection

1. Connect to EC2 instance:

```bash
ssh -i your-key.pem ubuntu@<EC2-IP>
cd /var/lib/jenkins/workspace/<job-name>/backend/target
```

2. Run the backend manually (optional):

```bash
java -jar student-registration-backend-0.0.1-SNAPSHOT.jar
```

3. Open the frontend in your browser (`http://<EC2-IP>`), enter data, and verify it is saved to **RDS**.

---

