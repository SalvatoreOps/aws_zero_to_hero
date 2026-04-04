# ☁️ Amazon EC2 — Explained

> **Elastic Compute Cloud** · AWS · Infrastructure as a Service (IaaS)

---

**Author:** SalvatoreOps
**Date:** March 15, 2026
**License:** MIT.

---

## 📖 What is EC2?

**Amazon EC2 (Elastic Compute Cloud)** is a web service that provides resizable compute capacity in the cloud. Simply put — it lets you rent virtual machines (called **instances**) on AWS and run your applications on them, paying only for what you use.

Think of EC2 as renting a computer in Amazon's data center, except you can spin one up in seconds, scale to thousands, and shut them down whenever you want

---

## 🏗️ High-Level Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        AWS CLOUD                            │
│                                                             │
│   ┌──────────────────────────────────────────────────────┐  │
│   │                  VPC (Virtual Network)               │  │
│   │                                                      │  │
│   │   ┌────────────────┐     ┌────────────────────────┐  │  │
│   │   │  Public Subnet │     │    Private Subnet      │  │  │
│   │   │                │     │                        │  │  │
│   │   │  ┌──────────┐  │     │  ┌──────────────────┐  │  │  │
│   │   │  │ EC2 Web  │  │────▶│  │  EC2 App Server  │  │  │  │
│   │   │  │  Server  │  │     │  └──────────────────┘  │  │  │
│   │   │  └──────────┘  │     │          │             │  │  │
│   │   └────────────────┘     │  ┌───────▼──────────┐  │  │  │
│   │           │              │  │   RDS Database   │  │  │  │
│   │     ┌─────▼─────┐        │  └──────────────────┘  │  │  │
│   │     │  Internet │        └────────────────────────┘  │  │
│   │     │  Gateway  │                                     │  │
│   │     └─────▲─────┘                                     │  │
│   └───────────┼───────────────────────────────────────────┘  │
│               │                                              │
└───────────────┼──────────────────────────────────────────────┘
                │
            👤 User / Internet
```

---

## ⚙️ Core Concepts

### 1. 🖥️ Instances
An **instance** is a virtual server in the cloud. You choose the OS, CPU, RAM, and storage.

```
Instance Types:
├── t3.micro    →  2 vCPU,  1 GB RAM   (Free tier eligible)
├── m5.large    →  2 vCPU,  8 GB RAM   (General purpose)
├── c5.xlarge   →  4 vCPU,  8 GB RAM   (Compute optimized)
├── r5.large    →  2 vCPU, 16 GB RAM   (Memory optimized)
└── p3.2xlarge  →  8 vCPU, 61 GB RAM   (GPU / ML workloads)
```

---

### 2. 💾 Storage Options

| Type | Name | Use Case |
|------|------|----------|
| 🟦 EBS | Elastic Block Store | Persistent disk attached to instance |
| 🟩 EFS | Elastic File System | Shared file storage across instances |
| 🟨 S3 | Simple Storage Service | Object storage (images, backups) |
| 🟥 Instance Store | Ephemeral Storage | Temporary, fast, lost on stop |

---

### 3. 🔐 Security Groups

Security Groups act as a **virtual firewall** for your EC2 instance.

```
Inbound Rules Example:
┌────────────┬──────────┬─────────────┬────────────────────┐
│ Protocol   │  Port    │   Source    │     Purpose        │
├────────────┼──────────┼─────────────┼────────────────────┤
│ TCP        │  22      │  My IP      │  SSH Access        │
│ TCP        │  80      │  0.0.0.0/0  │  HTTP (Web)        │
│ TCP        │  443     │  0.0.0.0/0  │  HTTPS (Secure)    │
│ TCP        │  3000    │  VPC CIDR   │  App Port          │
└────────────┴──────────┴─────────────┴────────────────────┘
```

---

### 4. 🌍 Regions & Availability Zones

```
AWS Global Infrastructure
│
├── us-east-1 (N. Virginia)
│       ├── us-east-1a  ← Availability Zone (AZ)
│       ├── us-east-1b  ← Availability Zone (AZ)
│       └── us-east-1c  ← Availability Zone (AZ)
│
├── ap-south-1 (Mumbai)
│       ├── ap-south-1a
│       └── ap-south-1b
│
└── eu-west-1 (Ireland)
        └── ...
```

> 💡 **Best practice:** Deploy across multiple AZs for high availability.

---

## 🚀 EC2 Lifecycle

```
          ┌─────────────┐
          │   Launch    │  ← Choose AMI, instance type, config
          └──────┬──────┘
                 │
          ┌──────▼──────┐
          │   Pending   │  ← Instance is starting up
          └──────┬──────┘
                 │
          ┌──────▼──────┐
          │   Running   │  ← ✅ Your instance is live!
          └──────┬──────┘
                 │
        ┌────────┴────────┐
        │                 │
 ┌──────▼──────┐   ┌──────▼──────┐
 │   Stopped   │   │ Terminated  │  ← Permanently deleted
 │  (Paused)   │   └─────────────┘
 └──────┬──────┘
        │
 ┌──────▼──────┐
 │   Running   │  ← Can restart
 └─────────────┘
```

---

## 💰 Pricing Models

| Model | Description | Best For |
|-------|-------------|----------|
| 🔵 **On-Demand** | Pay per hour/second, no commitment | Dev, testing, unpredictable workloads |
| 🟢 **Reserved** | 1–3 year commitment, up to 75% cheaper | Steady-state production workloads |
| 🟡 **Spot** | Bid on spare capacity, up to 90% cheaper | Batch jobs, fault-tolerant apps |
| 🟣 **Dedicated Host** | Physical server for your use only | Compliance requirements |
| 🟠 **Savings Plans** | Flexible pricing commitment | Mixed workloads |

---

## 🛠️ Quick Start — Launch Your First EC2

```bash
# Using AWS CLI

# 1. Launch an EC2 instance (Amazon Linux 2, t2.micro)
aws ec2 run-instances \
  --image-id ami-0c55b159cbfafe1f0 \
  --instance-type t2.micro \
  --key-name my-key-pair \
  --security-group-ids sg-xxxxxxxx \
  --subnet-id subnet-xxxxxxxx

# 2. Check instance status
aws ec2 describe-instances --instance-ids i-xxxxxxxxxxxxxxxxx

# 3. Connect via SSH
ssh -i "my-key-pair.pem" ec2-user@<public-ip>

# 4. Stop the instance
aws ec2 stop-instances --instance-ids i-xxxxxxxxxxxxxxxxx

# 5. Terminate (delete) the instance
aws ec2 terminate-instances --instance-ids i-xxxxxxxxxxxxxxxxx
```

---

## 📊 EC2 vs Other Compute Services

```
When to use what:
─────────────────────────────────────────────────────────────
EC2          → Full control, custom OS, long-running servers
Lambda       → Event-driven, short functions, serverless
ECS / EKS    → Containerized workloads (Docker / Kubernetes)
Lightsail    → Simple, beginner-friendly VPS alternative
Elastic BT   → Managed platform, less config than EC2
─────────────────────────────────────────────────────────────
```

---

## ✅ Summary

| Feature | Detail |
|---------|--------|
| **Service Type** | IaaS — Infrastructure as a Service |
| **Launched** | 2006 |
| **Billing** | Per second (min 60s) for most instances |
| **OS Support** | Linux, Windows, macOS |
| **Key Features** | Auto Scaling, Load Balancing, EBS, AMIs, VPC |
| **Free Tier** | 750 hrs/month of `t2.micro` for 12 months |

> EC2 is the backbone of AWS. It gives you complete control over your compute environment — from choosing the OS to configuring networking and storage. Whether you're hosting a website, running a backend API, or training ML models, EC2 can handle it.

---

## 📚 Free Learning Resources

| Resource | Link |
|----------|------|
| 📘 AWS Official EC2 Docs | https://docs.aws.amazon.com/ec2 |
| 🎓 AWS Free Training (Skill Builder) | https://explore.skillbuilder.aws |
| 🎥 FreeCodeCamp AWS Course (YouTube) | https://youtube.com/c/freecodecamp |
| 📖 AWS Whitepapers | https://aws.amazon.com/whitepapers |
| 🧪 AWS Free Tier (Hands-on) | https://aws.amazon.com/free |
| 🗺️ AWS Architecture Center | https://aws.amazon.com/architecture |
| 💬 AWS re:Post Community | https://repost.aws |
| 📝 Tutorials Dojo EC2 Cheat Sheet | https://tutorialsdojo.com/amazon-ec2 |

---

## 📄 License

```
MIT License

Copyright (c) 2026 SalvatoreOps

Permission is hereby granted, free of charge, to any person obtaining a copy
of this README and associated documentation, to use, copy, modify, merge,
publish, or distribute freely, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.
```

---

<div align="center">

---

**📅 Date:** March 15, 2026
**✍️ Author:** SalvatoreOps
**📄 License:** MIT License

<div align="center">
  
```
MIT License — Free to use, share, and modify with attribution.
Copyright © 2026 SalvatoreOps
```

</div>

*Made with ❤️ for the cloud community*
☁️ *Keep calm and cloud on.*

---

</div>


</div>
