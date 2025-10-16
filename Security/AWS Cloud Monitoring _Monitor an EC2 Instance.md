# 🖥️ Monitor an EC2 Instance using Amazon CloudWatch and SNS

## 📘 Overview
This project demonstrates how to **log, monitor, and alert on EC2 instance performance** using **Amazon CloudWatch** and **Amazon SNS**.  
The main objective was to create an **automated monitoring setup** that sends an **email alert** when an EC2 instance exceeds a defined CPU utilization threshold, while visualizing metrics in a **CloudWatch dashboard**.

---

## ⚙️ Tasks and Steps

### **Task 1: Configure Amazon SNS**
1. Opened **AWS Management Console** → **Simple Notification Service (SNS)**
2. Created a topic named `MyCwAlarm` (type: *Standard*)
3. Added an **email subscription** and confirmed it via AWS verification email
4. Verified subscription status changed from *Pending confirmation* → *Confirmed*

**✅ Outcome:** Successfully created an SNS topic for CloudWatch alarms.

---

### **Task 2: Create a CloudWatch Alarm**
1. Opened **CloudWatch** → **Metrics → EC2 → Per-Instance Metrics**
2. Selected the **CPUUtilization** metric for the *Stress Test EC2 instance*
3. Created an alarm named `LabCPUUtilizationAlarm` with:
   - Threshold: `> 60%`
   - Period: `1 minute`
   - Statistic: *Average*
4. Configured alarm to send a notification to the `MyCwAlarm` SNS topic when *In alarm*
5. Reviewed and created the alarm

**✅ Outcome:** Alarm sends an email when CPU usage exceeds 60%.

---

### **Task 3: Test the CloudWatch Alarm**
1. Accessed EC2 instance using **AWS Systems Manager Session Manager**
2. Ran stress test command:
   ```bash
   sudo stress --cpu 10 -v --timeout 400s
   ```
3. Opened a second terminal and ran `top` to monitor CPU usage
4. Refreshed the CloudWatch Alarms page until the status changed to *In alarm*
5. Checked email inbox for **AWS SNS Notification**

**✅ Outcome:** Verified that the alarm triggered successfully and sent an alert.

---

### **Task 4: Create a CloudWatch Dashboard**
1. Opened **CloudWatch → Dashboards → Create dashboard**
2. Named it `LabEC2Dashboard`
3. Added a **Line widget** using the `CPUUtilization` metric
4. Saved the dashboard

**✅ Outcome:** Created a real-time CPU utilization dashboard.

---

## 🏆 Summary of Achievements
- ✅ Created an SNS topic and confirmed email subscription
- ✅ Configured a CloudWatch alarm for CPU utilization > 60%
- ✅ Stress-tested EC2 instance and triggered alarm successfully
- ✅ Received SNS email alerts
- ✅ Built a CloudWatch dashboard to visualize metrics

---

## 🧰 Tools and AWS Services Used
- **Amazon EC2** – Compute instance for stress testing
- **Amazon CloudWatch** – Metrics, alarms, and dashboards
- **Amazon SNS** – Notification service for alerts
- **AWS Systems Manager Session Manager** – Secure instance access

---

## 💡 Key Takeaways
This lab showcases how **Amazon CloudWatch** integrates with **SNS** to build a **proactive EC2 monitoring solution**.  
By setting up alarms, alerts, and dashboards, I gained hands-on experience ensuring **visibility**, **performance tracking**, and **timely response** to system anomalies.

---




