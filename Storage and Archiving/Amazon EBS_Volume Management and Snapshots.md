# 💾 Amazon EBS Lab: Volume Management and Snapshots

## 📘 Overview
This project demonstrates how to use **Amazon Elastic Block Store (EBS)** with **EC2 instances** to provide scalable, high-performance block storage.  
You’ll learn how to **create, attach, mount, snapshot, and restore** EBS volumes for persistent data management and recovery in AWS.

---

## 🧭 Architecture Workflow
1. Create a new EBS volume  
2. Attach the volume to an EC2 instance  
3. Format and mount the volume  
4. Create a snapshot of the volume  
5. Restore a new volume from the snapshot and reattach it to the instance  

---

## 🎯 Objectives
By the end of this lab, you will be able to:
- ✅ Create an **EBS volume**
- ✅ Attach and mount an **EBS volume** to an EC2 instance
- ✅ Create a **snapshot** of an EBS volume
- ✅ Restore an EBS volume from a **snapshot**

---

## ⚙️ Steps Taken

### **Task 1 — Creating a New EBS Volume**
- Created a **1 GiB General Purpose SSD (gp2)** volume in the same Availability Zone as the EC2 instance (`us-west-2a`)  
- Added a tag: `Name = My Volume`

---

### **Task 2 — Attaching the Volume**
- Attached **My Volume** to the EC2 instance as device `/dev/sdb`  
- Verified the volume status: **In-use**

---

### **Task 3 — Connecting to the EC2 Instance**
- Used **EC2 Instance Connect** to open a terminal session on the instance  

---

### **Task 4 — Creating and Configuring the File System**
1. Created an **ext3 filesystem**:
   ```bash
   sudo mkfs -t ext3 /dev/sdb
   ```
2. Created mount point and mounted the volume:
   ```bash
   sudo mkdir /mnt/data-store
   sudo mount /dev/sdb /mnt/data-store
   ```
3. Configured **auto-mount on reboot** via `/etc/fstab`  
4. Verified mount using:
   ```bash
   df -h
   ```
5. Created and verified a test file:
   ```bash
   sudo sh -c "echo some text has been written > /mnt/data-store/file.txt"
   cat /mnt/data-store/file.txt
   ```

---

### **Task 5 — Creating an EBS Snapshot**
- Created a snapshot of *My Volume* and named it **My Snapshot**  
- Stored snapshot in **Amazon S3** for durability  
- Deleted the test file to simulate data loss  

---

### **Task 6 — Restoring the Snapshot**
1. Created a **new volume** from the snapshot (Name: *Restored Volume*, same Availability Zone)  
2. Attached it to the EC2 instance as `/dev/sdc`  
3. Mounted the restored volume:
   ```bash
   sudo mkdir /mnt/data-store2
   sudo mount /dev/sdc /mnt/data-store2
   ```
4. Verified the restored file exists:
   ```bash
   ls /mnt/data-store2/file.txt
   ```

---

## ✅ Results
- Successfully **created and attached** EBS volumes  
- Mounted volumes with **persistent configuration** using `/etc/fstab`  
- **Created and restored** an EBS snapshot  
- Verified **data restoration** and **file integrity**

---

## 🧠 Key Takeaways
- **EBS volumes** are block-level storage devices that can be attached to EC2 instances  
- **Snapshots** act as **durable backups**, restorable as new volumes  
- **Auto-mount configurations** ensure persistence after reboot  
- **Snapshots store only used data blocks**, reducing storage costs  

---

## 🧰 Tools and AWS Services Used
- **Amazon EC2** – Compute service  
- **Amazon EBS** – Block storage for persistent data  
- **Amazon S3** – Snapshot storage backend  
- **AWS Management Console / CLI** – Resource management interface

