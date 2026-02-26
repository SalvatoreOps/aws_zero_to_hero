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

## ❓ Interview Questions & Answers

### 🔵 Beginner Level

**Q1: What is EC2 and why is it called "Elastic"?**
**A:** EC2 is AWS's IaaS offering providing virtual machines. It's "Elastic" because it allows automatic scaling (up/down) based on demand, and you only pay for capacity used. You can increase/decrease resources (CPU, RAM) dynamically without hardware procurement delays.

**Q2: What's the difference between Security Groups and NACLs?**
**A:**
| Feature | Security Group | NACL |
|---------|---------------|------|
| Level | Instance level | Subnet level |
| State | Stateful (auto return) | Stateless (need explicit rules) |
| Rules | Allow only | Allow and Deny |
| Evaluation | All rules evaluated | Rules evaluated in order (number) |

**Q3: How do you secure an EC2 instance?**
**A:** 
- Use IAM Roles instead of access keys
- Restrict Security Groups (least privilege)
- Enable VPC Flow Logs
- Use private subnets for DBs/app servers
- Enable EBS encryption
- Regular patching and updates
- Use Systems Manager instead of SSH where possible

**Q4: What happens when you stop vs terminate an EC2?**
**A:**
- **Stop**: EBS-backed instance shuts down, EBS persists, public IP changes (unless EIP), no charge for compute (only storage)
- **Terminate**: Instance deleted permanently, root EBS deleted (if configured), EIPs detached, charges stop

**Q5: Difference between EBS and Instance Store?**
**A:** EBS is persistent network-attached storage (survives stop/start), while Instance Store is ephemeral physical storage (lost on stop/terminate). Instance Store offers higher IOPS but no durability guarantees.

---

### 🟡 Intermediate Level

**Q6: How does EC2 Auto Scaling work?**
**A:** Auto Scaling maintains application availability by automatically adding/removing EC2 instances based on:
- **Scaling Policies**: Target tracking, step scaling, simple scaling
- **CloudWatch Alarms**: Trigger on metrics (CPU > 70%)
- **Launch Templates**: Define instance configuration
- **Health Checks**: Replace unhealthy instances
- **Lifecycle Hooks**: Perform actions during scale events

**Q7: Explain the difference between T2/T3 Unlimited and Standard?**
**A:** Standard mode instances earn CPU credits when idle and spend them under load. Unlimited mode allows bursting above baseline indefinitely (additional charges apply if average CPU exceeds baseline over 24 hours).

**Q8: What is a Placement Group and types?**
**A:** Logical grouping of instances within single AZ for low latency:
- **Cluster**: Instances on same hardware (HPC, low latency)
- **Spread**: Instances on distinct hardware (critical apps, 7 per AZ limit)
- **Partition**: Instances in isolated partitions (HDFS, Cassandra)

**Q9: How do you troubleshoot an EC2 that won't boot?**
**A:**
1. Check System Logs (Actions → Monitor and troubleshoot → Get system log)
2. Check Instance Status Checks (System vs Instance status)
3. Review CloudTrail for configuration changes
4. Use Rescue Instance: Stop → Detach root vol → Attach to working instance → Fix fstab/grub → Reattach
5. Enable Serial Console access for debugging

**Q10: What is the difference between Horizontal and Vertical scaling in EC2?**
**A:**
- **Vertical**: Increasing instance size (t2.micro → t2.large). Downtime required, limits exist (hardware max).
- **Horizontal**: Adding more instances (2 → 10 instances). No downtime, preferred for cloud (Auto Scaling), requires load balancer.

---

### 🔴 Advanced/Scenario-Based

**Q11: Design a highly available web application using EC2.**
**A:**
```
Internet → Route53 (Geo DNS) 
    → CloudFront (CDN/Edge)
    → ALB (Multi-AZ)
        → ASG spanning 3 AZs
            - EC2 instances (min 2 per AZ)
            - Private subnets for app tier
        → RDS Multi-AZ (backend)
        - ElastiCache for session state
Monitoring: CloudWatch → SNS notifications
Backup: EBS snapshots, AMI automation
```
Key points: Multi-AZ deployment, Auto Scaling, Load Balancing, decoupled architecture, data backup strategy.

**Q12: Your EC2 instance is running slow. How do you troubleshoot?**
**A:**
1. **CloudWatch Metrics**: Check CPU Utilization, Network In/Out, Disk Ops
2. **Memory Check**: Install CloudWatch agent (memory not visible by default)
3. **EBS Volume**: Check I/O credits (gp2 burst balance), consider gp3/io2 if IOPS throttled
4. **Network**: Check if NAT Gateway/IGW bandwidth limits reached
5. **Application**: Review logs (/var/log/messages), check for memory leaks (top, free -m)
6. **CPU Steal**: Check if T2/T3 credits exhausted (high steal time in top)
7. **Resolution**: Scale vertically (bigger instance) or horizontally (more instances + ALB)

**Q13: How would you migrate a database from on-prem to EC2 with minimal downtime?**
**A:**
1. **AWS DMS** (Database Migration Service) setup
2. Create EC2 instance with appropriate storage (io1/io2 for IOPS)
3. **Initial Load**: Full dump and restore during maintenance window
4. **CDC**: Enable ongoing replication using DMS (Change Data Capture)
5. **Testing**: Verify data integrity with Row Count and checksum validation
6. **Cutover**: Update DNS/Connection strings during low traffic, stop replication
7. **Rollback Plan**: Keep on-prem DB running until validation complete

**Q14: Explain EC2 Nitro System.**
**A:** Nitro is AWS's virtualization platform that offloads virtualization functions to dedicated hardware:
- **Nitro Hypervisor**: Lightweight, near bare-metal performance
- **Nitro Cards**: Handle networking, EBS, I/O, security encryption
- **Nitro Security Chip**: Hardware root of trust
Benefits: Better performance (network up to 100Gbps), higher resource efficiency, faster instance launch times, supports bare metal (Nitro Enclaves).

**Q15: How do you automate patching across 1000+ EC2 instances?**
**A:**
1. **AWS Systems Manager Patch Manager**: Define patch baselines
2. **Maintenance Windows**: Schedule non-disruptive times
3. **Automation Documents (SSM)**: Pre-approved patches
4. **Target Groups**: Tag-based grouping (Environment:Production)
5. **Approval Workflow**: Test in Dev → Staging → Production
6. **Compliance**: Patch Manager reporting + Config Rules for compliance
7. **Rollback**: AMI backups before patching, quick rollback via ASG replacement

**Q16: What is Spot Instance interruption handling?**
**A:** Spot instances can be interrupted with 2-minute warning. Strategies:
- **Spot Fleet**: Diversified instance types across pools
- **Checkpointing**: Save state to S3/EBS regularly
- **Interruption Handlers**: Lambda triggered by CloudWatch Events (EC2 Spot Instance Interruption Warning) to gracefully drain connections
- **Mixed Instances Policy**: ASG with On-Demand base + Spot capacity

**Q17: Difference between EC2 Hibernate vs Stop vs Terminate?**
**A:**
- **Terminate**: Delete everything (configurable EBS delete)
- **Stop**: OS shutdown, resources released, EBS persists, RAM lost
- **Hibernate**: OS suspended to disk (root EBS), RAM saved to EBS, faster boot (application state preserved), instance RAM must be < EBS size

**Q18: How do you achieve PCI-DSS compliance on EC2?**
**A:**
- Isolate in dedicated VPC with strict NACLs/SGs
- Encryption at rest (EBS) and in transit (TLS)
- No hardcoded credentials (IAM Roles + Secrets Manager)
- Logging: CloudTrail, VPC Flow Logs, OS logs → S3 with Object Lock
- Regular vulnerability scans (Amazon Inspector)
- Patch compliance (Systems Manager)
- Dedicated instances or Dedicated Hosts (if required)
- Access logging and MFA enforcement

**Q19: Explain EC2 Instance Connect vs SSH Key Pairs.**
**A:** EC2 Instance Connect provides browser-based SSH without managing .pem files. It:
- Uses temporary SSH keys pushed to instance metadata (valid 60 seconds)
- Integrates with IAM (user authentication via IAM policies)
- No need to open port 22 to internet (can use VPC endpoints)
- Audit trail in CloudTrail
- Supports tunneling for RDP
Traditional Key Pairs require file management and long-term key security.

**Q20: How would you reduce EC2 costs by 60% without impacting performance?**
**A:**
1. **Compute Savings Plans**: 1-3 year commitment (up to 66% savings)
2. **Spot Instances**: For CI/CD, batch processing (90% savings)
3. **Right Sizing**: Use AWS Compute Optimizer recommendations
4. **Graviton2/3 (ARM)**: 20% better price/performance vs x86
5. **EBS gp3**: 20% cheaper than gp2, better performance
6. **Stop Dev/Test**: Automated schedules (Lambda to stop/start)
7. **Reserved Capacity**: For baseline steady-state workloads

---

## 🎓 Final Tips for Interviews

1. **Know the Well-Architected Framework**: Always mention operational excellence, security, reliability, performance efficiency, cost optimization, and sustainability.

2. **Hybrid Cloud**: Be ready to discuss Direct Connect, VPN, and hybrid architectures.

3. **Serverless Comparison**: Know when to use EC2 vs ECS/EKS (containers) vs Lambda.

4. **Real Numbers**: Know rough performance metrics (EBS gp3 baseline 3000 IOPS, t3.micro baseline 10% CPU, etc.)

5. **Troubleshooting**: Always mention CloudWatch Logs and Systems Manager as first steps.

---

*Remember: Hands-on practice is key. Use AWS Free Tier to experiment!* 🚀

---
