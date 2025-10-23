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

4. Description: *“Permit access from Web Security Group.”*  

5. Selected **Lab VPC** as the network.  

6. Added an inbound rule:  
   - **Type:** MySQL/Aurora (3306)  
   - **Source:** Web Security Group  

7. Clicked **Create Security Group**.
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/280dcdf4cdb9e4437fffc885bf7f331cbc8f4406/Images/Lab%20Images/Database%20im/Build%20a%20DB%20server/Screenshot%20(2205).png)
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/280dcdf4cdb9e4437fffc885bf7f331cbc8f4406/Images/Lab%20Images/Database%20im/Build%20a%20DB%20server/Screenshot%20(2206).png)
---

### **Task 2: Create a DB Subnet Group**
1. Navigated to **RDS → Subnet Groups**.  
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/280dcdf4cdb9e4437fffc885bf7f331cbc8f4406/Images/Lab%20Images/Database%20im/Build%20a%20DB%20server/Screenshot%20(2208).png)
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/280dcdf4cdb9e4437fffc885bf7f331cbc8f4406/Images/Lab%20Images/Database%20im/Build%20a%20DB%20server/Screenshot%20(2210).png)

3. Clicked **Create DB Subnet Group** and named it **DB Subnet Group**.  

4. Selected **Lab VPC**.  

5. Added two Availability Zones and their respective subnets:
   - 10.0.1.0/24 *(Private Subnet 1)*
   - 10.0.3.0/24 *(Private Subnet 2)*  
6. Clicked **Create**.
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/280dcdf4cdb9e4437fffc885bf7f331cbc8f4406/Images/Lab%20Images/Database%20im/Build%20a%20DB%20server/Screenshot%20(2213).png)
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/280dcdf4cdb9e4437fffc885bf7f331cbc8f4406/Images/Lab%20Images/Database%20im/Build%20a%20DB%20server/Screenshot%20(2214).png)
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/280dcdf4cdb9e4437fffc885bf7f331cbc8f4406/Images/Lab%20Images/Database%20im/Build%20a%20DB%20server/Screenshot%20(2215).png)
---

### **Task 3: Create a Multi-AZ Amazon RDS DB Instance**
1. Navigated to **RDS → Databases → Create Database**.  
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/280dcdf4cdb9e4437fffc885bf7f331cbc8f4406/Images/Lab%20Images/Database%20im/Build%20a%20DB%20server/Screenshot%20(2216).png)

3. Chose **Standard Create → MySQL → Dev/Test template**.  
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/280dcdf4cdb9e4437fffc885bf7f331cbc8f4406/Images/Lab%20Images/Database%20im/Build%20a%20DB%20server/Screenshot%20(2218).png)

5. Set **Multi-AZ DB Instance** for high availability.  
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/280dcdf4cdb9e4437fffc885bf7f331cbc8f4406/Images/Lab%20Images/Database%20im/Build%20a%20DB%20server/Screenshot%20(2221).png)

7. Configured:
   - **DB Instance Identifier:** lab-db  
   - **Master Username:** main  
   - **Password:** lab-password
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/280dcdf4cdb9e4437fffc885bf7f331cbc8f4406/Images/Lab%20Images/Database%20im/Build%20a%20DB%20server/Screenshot%20(2222).png)
   - **Instance Class:** db.t3.medium  
   - **Storage Type:** General Purpose (SSD)  
[![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/280dcdf4cdb9e4437fffc885bf7f331cbc8f4406/Images/Lab%20Images/Database%20im/Build%20a%20DB%20server/Screenshot%20(2223).png)

8. Under Connectivity:
   - Selected **Lab VPC**
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/280dcdf4cdb9e4437fffc885bf7f331cbc8f4406/Images/Lab%20Images/Database%20im/Build%20a%20DB%20server/Screenshot%20(2224).png)
   - Used existing **DB Security Group**
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/280dcdf4cdb9e4437fffc885bf7f331cbc8f4406/Images/Lab%20Images/Database%20im/Build%20a%20DB%20server/Screenshot%20(2226).png)
   - Disabled Enhanced Monitoring & Automated Backups (for lab speed)  
   - **Initial Database Name:** lab  
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/280dcdf4cdb9e4437fffc885bf7f331cbc8f4406/Images/Lab%20Images/Database%20im/Build%20a%20DB%20server/Screenshot%20(2227).png)


9. Clicked **Create Database**.  
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/280dcdf4cdb9e4437fffc885bf7f331cbc8f4406/Images/Lab%20Images/Database%20im/Build%20a%20DB%20server/Screenshot%20(2229).png)

11. Waited for the instance status to become **Available**.  

12. Copied the **Endpoint** from *Connectivity & Security* for later use.
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/280dcdf4cdb9e4437fffc885bf7f331cbc8f4406/Images/Lab%20Images/Database%20im/Build%20a%20DB%20server/Screenshot%20(2233).png)
---

### **Task 4: Interact With the Database Using the Web App**
1. Copied the **Web Server IP Address** from AWS details.  
2. Opened it in a browser — the **web app loaded successfully**.  
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/280dcdf4cdb9e4437fffc885bf7f331cbc8f4406/Images/Lab%20Images/Database%20im/Build%20a%20DB%20server/Screenshot%20(2234).png)

4. Clicked the **RDS** link in the app menu.  
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/280dcdf4cdb9e4437fffc885bf7f331cbc8f4406/Images/Lab%20Images/Database%20im/Build%20a%20DB%20server/Screenshot%20(2235).png)

6. Entered database connection info:
   - **Endpoint:** (from RDS)
   - **Database:** lab
   - **Username:** main
   - **Password:** lab-password  
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/280dcdf4cdb9e4437fffc885bf7f331cbc8f4406/Images/Lab%20Images/Database%20im/Build%20a%20DB%20server/Screenshot%20(2236).png)

7. Clicked **Submit**.  
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/280dcdf4cdb9e4437fffc885bf7f331cbc8f4406/Images/Lab%20Images/Database%20im/Build%20a%20DB%20server/Screenshot%20(2237).png)

9. Could not verify the **Address Book app** loaded and stored contact data in the RDS instance.  


11. Could not Test **CRUD operations** (add, edit, delete).  


13. Could not confirm data persisted and replicated across Availability Zones.


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



