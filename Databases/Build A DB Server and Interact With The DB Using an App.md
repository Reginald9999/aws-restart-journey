# 🏗️ Build Your DB Server and Interact With Your DB Using an App

This project demonstrates how I built and connected a **web application** to an **Amazon RDS MySQL database** using AWS-managed services..

It follows the lab **“Build Your DB Server and Interact With Your DB Using an App”**, designed to reinforce using AWS-managed relational databases in a secure, scalable environment.

---

## 🧭 Objectives
- Launch a **Multi-AZ Amazon RDS DB instance** with high availability  
- Configure network access using **Security Groups**  
- Create a **DB Subnet Group** to define private subnets  
- Connect a **web server application** to the RDS instance  
- Interact with the database via a **live web interface**
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/1d862815739bc24cfdc57c0c3c94328564e4cf49/Images/Lab%20Images/Database%20im/Build%20a%20DB%20server/Screenshot%20(2199).png)

![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/1d862815739bc24cfdc57c0c3c94328564e4cf49/Images/Lab%20Images/Database%20im/Build%20a%20DB%20server/Screenshot%20(2200).png)
---

## ☁️ AWS Services Used
- **Amazon RDS** – MySQL Database (Multi-AZ)
- **Amazon VPC** – Network and Subnet Management
- **Amazon EC2** – Web Application Server
- **AWS Identity and Access Management (IAM)** – Role-based access
- **Security Groups** – Traffic control between EC2 and RDS

---

## 🧩 Steps I Took

### **Task 1: Create a Security Group for the RDS DB Instance**
1. Opened **VPC** in the AWS Console.  
2. Created a new security group named **DB Security Group**.  
3. Description: *“Permit access from Web Security Group.”*  
4. Selected **Lab VPC** as the network.  
5. Added an inbound rule:  
   - **Type:** MySQL/Aurora (3306)  
   - **Source:** Web Security Group  
6. Clicked **Create Security Group**.

---

### **Task 2: Create a DB Subnet Group**
1. Navigated to **RDS → Subnet Groups**.  
2. Clicked **Create DB Subnet Group** and named it **DB Subnet Group**.  
3. Selected **Lab VPC**.  
4. Added two Availability Zones and their respective subnets:
   - 10.0.1.0/24 *(Private Subnet 1)*
   - 10.0.3.0/24 *(Private Subnet 2)*  
5. Clicked **Create**.

---

### **Task 3: Create a Multi-AZ Amazon RDS DB Instance**
1. Navigated to **RDS → Databases → Create Database**.  
2. Chose **Standard Create → MySQL → Dev/Test template**.  
3. Set **Multi-AZ DB Instance** for high availability.  
4. Configured:
   - **DB Instance Identifier:** lab-db  
   - **Master Username:** main  
   - **Password:** lab-password  
   - **Instance Class:** db.t3.medium  
   - **Storage Type:** General Purpose (SSD)  
5. Under Connectivity:
   - Selected **Lab VPC**  
   - Used existing **DB Security Group**  
   - Disabled Enhanced Monitoring & Automated Backups (for lab speed)  
   - **Initial Database Name:** lab  
6. Clicked **Create Database**.  
7. Waited for the instance status to become **Available**.  
8. Copied the **Endpoint** from *Connectivity & Security* for later use.

---

### **Task 4: Interact With the Database Using the Web App**
1. Copied the **Web Server IP Address** from AWS details.  
2. Opened it in a browser — the **web app loaded successfully**.  
3. Clicked the **RDS** link in the app menu.  
4. Entered database connection info:
   - **Endpoint:** (from RDS)
   - **Database:** lab
   - **Username:** main
   - **Password:** lab-password  
5. Clicked **Submit**.  
6. Verified the **Address Book app** loaded and stored contact data in the RDS instance.  
7. Tested **CRUD operations** (add, edit, delete).  
8. Confirmed data persisted and replicated across Availability Zones.

---

## ✅ Results
- Successfully launched a **highly available MySQL database** on Amazon RDS.  
- Configured **secure connectivity** between the web app and the database.  
- Demonstrated **real-time data storage, retrieval, and replication** using AWS-managed infrastructure.

---

## 💡 Key Takeaways
- AWS RDS simplifies **relational database deployment and scaling**.  
- **Multi-AZ architecture** ensures fault tolerance and high availability.  
- **Security Groups** and **Subnet Groups** enforce isolation and access control.  
- Applications integrate seamlessly with managed databases using **standard connection parameters**.

---

## 🧾 Next Steps
- Enable **Automated Backups** for production environments.  
- Add **CloudWatch Alarms** to monitor performance.  
- Integrate **AWS Secrets Manager** for secure credential storage.



