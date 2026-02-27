# Day 3 - AWS EC2 Complete Guide & Interview Preparation 🚀

> Your comprehensive handbook for Amazon Elastic Compute Cloud (EC2) - from basics to cracking DevOps interviews! 🎯

---

## 📑 Table of Contents
1. [What is EC2?](#-what-is-ec2)
2. [Core Components](#-core-components)
3. [Instance Types Deep Dive](#-instance-types-)
4. [Storage Options](#-storage-options-)
5. [Networking Basics](#-networking-basics-)
6. [Security & Access](#-security--access-)
7. [Step-by-Step Launch Guide](#-step-by-step-launch-guide-)
8. [Best Practices](#-best-practices-)
9. [Interview Questions & Answers](#-interview-questions--answers-)
10. [Quick Reference Commands](#-quick-reference-commands-)

---

## 🎯 What is EC2?

**Amazon Elastic Compute Cloud (EC2)** is a web service that provides resizable compute capacity in the cloud. It allows you to:
- 🖥️ Run virtual servers (instances) on-demand
- ⚡ Scale capacity up or down automatically
- 💰 Pay only for what you use (per second billing)
- 🌍 Deploy globally across multiple regions

**Key Benefits:**
- Elasticity: Scale resources automatically 📈
- Control: Root access to your servers 🎮
- Flexible: Choose from multiple OS and software configurations 🧩
- Integrated: Works with S3, RDS, VPC, Lambda, etc. 🔗

---

## 🧩 Core Components

### 1. Amazon Machine Image (AMI) 🖼️
- Template containing OS, application server, and applications
- Types: AWS provided, Marketplace, Community, Custom (your own)

### 2. Instance Types 💪
Categorized by use case:
- **T-series**: Burstable performance (dev/test)
- **M-series**: General purpose (balanced)
- **C-series**: Compute optimized (CPU intensive)
- **R-series**: Memory optimized (databases)
- **G-series**: GPU instances (ML, graphics)
- **I-series**: Storage optimized (NoSQL, data warehouses)

### 3. Key Pairs 🔐
- Public-key cryptography for secure login
- `.pem` (Linux) or `.ppk` (Windows) files
- **NEVER share your private key!** ⚠️

### 4. Security Groups 🛡️
- Virtual firewalls at instance level
- Stateful: Return traffic automatically allowed
- Rules: Inbound (ingress) and Outbound (egress)
- Default: Deny all inbound, allow all outbound

### 5. Elastic IP (EIP) 📍
- Static IPv4 address for dynamic cloud computing
- Remains associated with your account until released
- 💸 **Caution**: Charges apply when not in use

---

## 💻 Instance Types Explained

| Family | Use Case | Examples |
|--------|----------|----------|
| **T2/T3/T3a/T4g** | Low cost, burst | t3.micro (Free tier eligible) |
| **M5/M6g** | General purpose | Web servers, small DBs |
| **C5/C6g** | Compute intensive | Batch processing, gaming |
| **R5/R6g** | Memory intensive | SAP, Oracle, in-memory caches |
| **I3/I3en** | Storage optimized | NoSQL, data warehousing |
| **G4/P3** | GPU accelerated | ML, video encoding |

**Naming Convention**: `[Family][Generation][Size][Attributes]`
Example: `m5.xlarge` = General purpose, 5th gen, extra large

---

## 💾 Storage Options

### 1. EBS (Elastic Block Store) 💿
- Persistent block storage
- **Types:**
  - **gp3**: General purpose SSD (default) ⚡
  - **io2/io1**: Provisioned IOPS SSD (databases)
  - **st1**: Throughput optimized HDD (big data)
  - **sc1**: Cold HDD (archival)
- Can detach and attach to different instances
- Snapshot backup to S3 📸

### 2. Instance Store (Ephemeral) 💨
- Physical storage attached to host
- **Lost when stopped** ⚠️
- High I/O performance
- Good for temp files, cache

### 3. EFS (Elastic File System) 📁
- Managed NFS service
- Shared across multiple AZs and instances
- Auto-scaling storage

---

## 🌐 Networking Basics

### VPC (Virtual Private Cloud) 🏠
- Isolated network environment
- CIDR block range (e.g., 10.0.0.0/16)

### Subnets 🕸️
- **Public**: Has route to Internet Gateway (IGW)
- **Private**: No direct internet access (NAT Gateway instead)
- **Placement**: Different AZs for HA

### IP Addressing 🏷️
- **Private IP**: Internal communication only
- **Public IP**: Internet accessible (changes on stop/start)
- **Elastic IP**: Static public IP (persistent)

---

## 🔒 Security & Access

### IAM Roles vs Key Pairs
- **IAM Roles**: Preferred for EC2 → AWS service access (no credentials stored)
- **Key Pairs**: For SSH/RDP access to OS

### Security Group Best Practices:
✅ Principle of least privilege  
✅ Restrict SSH (port 22) to your IP only  
✅ Use separate SGs for different tiers  
❌ Never use 0.0.0.0/0 for sensitive ports  

### User Data Scripts 📝
- Bootstrap scripts run at first launch
- Automate installations and configurations

---

## 🛠️ Step-by-Step Launch Guide

### Step 1: Sign in to AWS Console 🚪
Navigate to [AWS Management Console](https://console.aws.amazon.com) and login

### Step 2: Navigate to EC2 Dashboard 🖱️
- Services → Compute → EC2
- OR Search "EC2" in top bar

### Step 3: Launch Instance 🚀
Click **"Launch Instance"** button

### Step 4: Choose AMI 🎨
1. Select **Quick Start** (for beginners)
2. Choose **Amazon Linux 2** or **Ubuntu Server 22.04 LTS** (Free tier eligible)
3. Architecture: 64-bit (x86)

### Step 5: Choose Instance Type 💪
- Select **t2.micro** (Free tier eligible) ✅
- Or t3.micro for better performance

### Step 6: Configure Key Pair 🔑
1. Select **"Create new key pair"** (if first time)
2. Name: `my-ec2-key`
3. Type: RSA
4. Format: `.pem` (Linux/Mac) or `.ppk` (Windows)
5. **Download and save securely!** 📥

### Step 7: Network Settings 🌐
- **VPC**: Default VPC (or your custom VPC)
- **Subnet**: Any available (preferably with public IP auto-assign)
- **Auto-assign Public IP**: Enable ✅
- **Firewall**: Create security group
  - Name: `allow-ssh-http`
  - Description: "SSH and HTTP access"
  
**Inbound Rules:**
- Type: SSH, Port: 22, Source: My IP (dropdown) 🛡️
- Type: HTTP, Port: 80, Source: Anywhere (0.0.0.0/0)

### Step 8: Configure Storage 💾
- **Size**: 8-30 GB (Free tier: up to 30GB)
- **Type**: gp3 (general purpose SSD)
- **Delete on Termination**: ✅ (for dev/test)

### Step 9: Advanced Details (Optional) ⚙️
Scroll to **User data** and paste:
```bash
#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "<h1>Hello from EC2! 🚀</h1>" > /var/www/html/index.html
```

### Step 10: Launch! 🎉
- Click **"Launch Instance"**
- View Instances → Wait for "Instance State: Running" ✅
- Note the **Public IPv4 address**

### Step 11: Connect to Instance 🔌

**Via SSH (Linux/Mac):**
```bash
chmod 400 my-ec2-key.pem
ssh -i my-ec2-key.pem ec2-user@<public-ip>
# For Ubuntu: ssh -i key.pem ubuntu@<public-ip>
```

**Via Windows (PuTTY):**
1. Convert .pem to .ppk using PuTTYgen
2. PuTTY → Host: ec2-user@<public-ip>
3. Connection → SSH → Auth → Browse for .ppk file
4. Open

### Step 13: Clean Up 🧹
- **IMPORTANT**: Stop/Terminate when done to avoid charges!
- Actions → Instance State → Terminate

### 🎯 Complete EC2 Launch Playlist
**📺 [AWS Tutorial: Launch Your First EC2 Instance (Official)](https://www.youtube.com/watch?v=Z3SYDTMP3d4)**  
*Duration: 15 mins | Level: Beginner*  
Step-by-step visual guide from AWS engineers covering the entire launch process, security groups, and key pairs.

---

## ⭐ Best Practices

### Cost Optimization 💰
- Use **Reserved Instances** or **Savings Plans** for predictable workloads
- Enable **Auto Scaling** to match demand
- Use **Spot Instances** for fault-tolerant workloads (up to 90% savings)
- Regularly review and delete unused EBS volumes

### Security Hardening 🛡️
- Enable **CloudTrail** for audit logging
- Use **Systems Manager Session Manager** instead of direct SSH (no open port 22)
- Regularly patch OS using **Patch Manager**
- Encrypt EBS volumes using KMS keys

### High Availability 🏗️
- Deploy across **Multiple Availability Zones**
- Use **Elastic Load Balancer (ELB)** with EC2
- Configure **Auto Scaling Groups (ASG)**
- Create **Golden AMIs** for quick recovery

### Monitoring 📊
- Enable **CloudWatch** detailed monitoring for production
- Set up **CloudWatch Alarms** for CPU/disk
- Install **CloudWatch Agent** for memory metrics
- Use **AWS Systems Manager** for inventory and patching

---

## Summary

**What:** Virtual servers (VMs) in AWS cloud - scalable, on-demand compute.

**🔑 Key Components:**
- **AMI**: OS template (Linux/Windows)
- **Instance Types**: T/M (general), C (compute), R (memory), G (GPU)
- **Storage**: EBS (persistent) vs Instance Store (temporary)
- **Security Groups**: Firewall rules (ports 22/80/443)
- **Key Pairs**: SSH access credentials

**🚀 Launch in 5 Steps:**
1. Select AMI + Instance Type (t2.micro for free tier)
2. Configure network (VPC, public IP, Security Groups)
3. Add storage (8-30GB gp3)
4. Select/create Key Pair (.pem file)
5. Launch & Connect via SSH/RDP

**💰 Pricing:** On-Demand (pay hourly), Reserved (1-3yr savings ~72%), Spot (90% off, interruptible)

**⚡ Pro Tips:**
- Use Security Groups wisely (restrict SSH to your IP)
- Stop instances when idle to save money
- Create AMIs/snapshots for backups
- Use IAM roles, never hardcode credentials

**🎯 Use Cases:** Web hosting, databases, batch processing, ML training, dev/test environments

---

*Last Updated: 2024*  
*Author: SalvatoreOps*  
*License: Public Domain / Free to share*

---
