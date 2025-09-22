# 🚀 AWS Lab – Build a VPC and Launch a Web Server.

## 🎯 Objectives
After completing this lab, I should be able to:

- Create a **Virtual Private Cloud (VPC)**
- Create **public and private subnets**
- Configure a **security group**
- Launch an **EC2 instance** into a VPC and configure it as a **web server**

---

## ⏱️ Duration
This lab takes approximately **45 minutes** to complete.

---

## 📌 Scenario
I am working for a Fortune 100 customer that requires a secure and scalable network setup in AWS.  
In this lab, I will build a custom **Amazon VPC** with multiple subnets, configure routing, and launch a web server on **Amazon EC2**.  

The final architecture will include:

- 1 VPC (10.0.0.0/16)
- 2 Public Subnets
- 2 Private Subnets
- 1 Internet Gateway
- 1 NAT Gateway
- Public & Private Route Tables
- 1 Security Group (allowing HTTP access)
- 1 EC2 instance running Apache Web Server

![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/main/Images/N-images-/lab-1/Screenshot%20(1844).png?raw=true)

---

## 🛠️ Tasks

### **Task 1: Create your VPC**
- Create a VPC named **Lab VPC** (`10.0.0.0/16`)
- Create **1 public subnet** (`10.0.0.0/24`)
- Create **1 private subnet** (`10.0.1.0/24`)
- Attach **Internet Gateway**
- Add **NAT Gateway** for private subnet
- Configure route tables (Public & Private)

### **Task 2: Create additional subnets**
- Add **Public Subnet 2** (`10.0.2.0/24`)
- Add **Private Subnet 2** (`10.0.3.0/24`)
- Associate subnets with route tables

### **Task 3: Configure route tables**
- Associate **Public Subnet 2** with Public Route Table
- Associate **Private Subnet 2** with Private Route Table

### **Task 4: Create a Security Group**
- Name: **Web Security Group**
- Allow inbound traffic:
  - **HTTP (80)** from Anywhere IPv4
- Associate with Lab VPC

### **Task 5: Launch a Web Server**
- Launch EC2 instance named **Web Server 1**
- Use **Amazon Linux 2 AMI (HVM)**, `t3.micro`
- Subnet: **Public Subnet 2**
- Enable **Public IP**
- Attach **Web Security Group**
- Add **User Data script** to install Apache & deploy app:

```bash
#!/bin/bash
# Install Apache Web Server and PHP
yum install -y httpd mysql php
# Download Lab files
wget https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-100-RESTRT-1/267-lab-NF-build-vpc-web-server/s3/lab-app.zip
unzip lab-app.zip -d /var/www/html/
# Start web server
chkconfig httpd on
service httpd start
