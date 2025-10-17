# 🧩 AWS Lab 174 — Scaling and Load Balancing Your Architecture

## Overview
This lab demonstrates how to use *Elastic Load Balancing (ELB)* and *Amazon EC2 Auto Scaling* to automatically scale infrastructure and distribute traffic across multiple EC2 instances.

You start with a single web server instance and end with a fully load-balanced, auto-scaling architecture across multiple Availability Zones.

---

## 🧱 Steps Taken

### *Task 1 — Create an AMI from an EC2 Instance*
1. Opened the EC2 Management Console → *Instances*
2. Selected *Web Server 1 (running)* → *Actions → Image and templates → Create image*
3. Entered:
   - *Image name:* Web Server AMI
   - *Description:* Lab AMI for Web Server
4. Clicked *Create image*
5. Copied the *AMI ID* for use in the launch template

---

### *Task 2 — Create a Load Balancer*
1. Navigated to *Load Balancers → Create load balancer → Application Load Balancer*
2. Configured:
   - *Name:* LabELB
   - *VPC:* Lab VPC
   - *Availability Zones:* Public Subnet 1 & Public Subnet 2  
   - *Security Group:* Web Security Group (permits HTTP)
3. Created a *Target Group:*
   - *Type:* Instances  
   - *Name:* lab-target-group
4. Associated *LabELB* with lab-target-group
5. Verified creation and copied the *DNS name* of the load balancer

---

### *Task 3 — Create a Launch Template*
1. Navigated to *Launch Templates → Create launch template*
2. Configured:
   - *Name:* lab-app-launch-template
   - *Description:* A web server for the load test app
   - *AMI:* Web Server AMI
   - *Instance type:* t3.micro
   - *Key pair:* None
   - *Security group:* Web Security Group
3. Clicked *Create launch template*

---

### *Task 4 — Create an Auto Scaling Group*
1. Selected lab-app-launch-template → *Actions → Create Auto Scaling group*
2. Configured:
   - *Name:* Lab Auto Scaling Group
   - *VPC:* Lab VPC
   - *Private Subnets:* Private Subnet 1 & Private Subnet 2
3. Attached Load Balancer:
   - *Target group:* lab-target-group
   - *Health check type:* ELB
4. Scaling Configuration:
   - *Desired capacity:* 2  
   - *Minimum capacity:* 2  
   - *Maximum capacity:* 4  
   - *Scaling policy:* Target tracking → Average CPU utilization → Target value: 50%
5. Added tag:
   - *Key:* Name
   - *Value:* Lab Instance
6. Created the Auto Scaling group and verified two instances launched successfully.

---

### *Task 5 — Verify Load Balancing*
- Confirmed two instances named *Lab Instance* were running in EC2.
- Verified *Target Group health* = healthy for both.
- Opened the *Load Balancer DNS* in a browser and confirmed the Load Test app was served.

---

### *Task 6 — Test Auto Scaling*
1. Checked *CloudWatch → Alarms*: AlarmHigh (CPU > 50%) and AlarmLow were OK.
2. Generated high load from the Load Test app.
3. Observed:
   - AlarmHigh triggered → Auto Scaling launched more instances.
   - Verified additional EC2 instances running.

---

### *Task 7 — Terminate Web Server 1*
- Terminated the original *Web Server 1* instance since its AMI was already used.

---

### *Optional Challenge*
- Created an AMI using *AWS CLI* for automation.
- Used *EC2 Instance Connect* with AWS credentials to generate AMI via CLI commands.

---

## ✅ Results
- Successfully created an AMI from an EC2 instance  
- Configured a load balancer across multiple Availability Zones  
- Set up a launch template and Auto Scaling group  
- Enabled automatic scaling using CloudWatch alarms  
- Verified load balancing and scaling worked as expected  

---

## 🧠 Key Takeaways
- *Elastic Load Balancing (ELB)* distributes traffic for high availability.  
- *Auto Scaling* adjusts instance count automatically based on demand.  
- *Launch Templates* ensure consistent instance provisioning.  
- *CloudWatch* provides metrics and triggers for automated scaling.
