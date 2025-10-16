# AWS Lab – Create Subnets and Allocate IP addresses in an Amazon VPC

## 🎯 Objectives
In this lab, I learned how to:
- Summarize a customer scenario and identify networking requirements.
- Create an **Amazon Virtual Private Cloud (VPC)**.
- Understand how to create **subnets** and allocate **IP addresses**.
- Familiarize myself with the **AWS Management Console**.
- Develop and document a solution to the customer’s issue.

---

## 🕒 Duration
This lab took approximately **1 hour** to complete.

---

## 📌 Scenario
A customer (Paulo Santos, a startup owner) requested help setting up a **VPC** with the following requirements:
- Around **15,000 private IP addresses** in the VPC.
- The VPC must use a **192.x.x.x private IPv4 range**.
- A **public subnet** with at least **50 IP addresses**.
- Internet Gateway for external connectivity.

As a **Cloud Support Engineer**, I was tasked to build this setup and provide a walkthrough.

---

## 🛠️ Steps Taken

### 1. Investigated Customer Requirements
- VPC requires ~15,000 private IPs → selected **192.168.0.0/18** (16,384 IPs).
- Public subnet requires ~50 IPs → allocated **192.168.1.0/26** (64 IPs).
- Confirmed ranges against [RFC 1918 private IP address ranges](https://datatracker.ietf.org/doc/html/rfc1918).
- Used [Subnet Calculator](https://www.subnet-calculator.com/) to validate CIDR blocks.

---

### 2. Created the VPC
- Opened **AWS Management Console → VPC**.
- Selected **Launch VPC Wizard**.
- Chose **VPC with a Single Public Subnet**.
- Configured:
  - **VPC CIDR Block:** `192.168.0.0/18`
  - **Subnet CIDR Block:** `192.168.1.0/26`
  - **VPC Name:** `First VPC`
  - **Subnet Name:** `Public Subnet`
  - Left default settings for Availability Zone and IPv6.
- Clicked **Create VPC** → received success message.

---

### 3. Verified the Setup
- Navigated to **Your VPCs** in the left menu → confirmed VPC was created.
- Verified subnet and IP allocations.
- Ensured Internet Gateway was attached.henji

---
## Screenshots
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/ca8794dd-2ccb-40c2-80c3-0a97ace2ef83" />

---

## 📩 Customer-Facing Response (Summary)
Hello Paulo,  

I have created your VPC with the required configuration:  
- **VPC CIDR:** 192.168.0.0/18 (16,384 IPs)  
- **Public Subnet CIDR:** 192.168.1.0/26 (64 IPs)  
- Internet Gateway attached for external connectivitpolelpolpoy.  

Your VPC is ready to host both private and public workloads as requested.  

Regards,  
*Cloud Support Engineer*  

---

## 📝 Key Learnings
- How to size VPCs and subnets using **CIDR notation**.  
- Difference between **public vs private subnets**.  
- Importance of private IP ranges for internal resources.  
- How to use **AWS VPC Wizard** for quick setup.

---
