# 🌐 VPC & Networking Explained

> **Author:** SalvatoreOps &nbsp;|&nbsp; **Date:** March 15, 2026 &nbsp;|&nbsp; **License:** MIT

---

## 📖 What is a VPC?

A **Virtual Private Cloud (VPC)** is a logically isolated section of a cloud provider's network where you can launch resources in a virtual network you define. Think of it as your own private data center — but in the cloud.

```
┌─────────────────────────────────────────────────────────┐
│                     CLOUD PROVIDER                      │
│                                                         │
│   ┌─────────────────────────────────────────────────┐   │
│   │               YOUR VPC (10.0.0.0/16)           │   │
│   │                                                 │   │
│   │  ┌──────────────────┐  ┌──────────────────┐    │   │
│   │  │  PUBLIC SUBNET   │  │  PRIVATE SUBNET  │    │   │
│   │  │  (10.0.1.0/24)   │  │  (10.0.2.0/24)   │    │   │
│   │  │                  │  │                  │    │   │
│   │  │  🖥️  Web Server  │  │  🗄️  Database    │    │   │
│   │  │  🌐  Load Balancer│  │  🔒  App Server  │    │   │
│   │  └────────┬─────────┘  └──────────────────┘    │   │
│   │           │  NAT Gateway                        │   │
│   └───────────┼─────────────────────────────────────┘   │
│               │ Internet Gateway                         │
└───────────────┼─────────────────────────────────────────┘
                │
              🌍 Internet
```

---

## 🗂️ Core Networking Concepts

### 1. CIDR Blocks

**CIDR (Classless Inter-Domain Routing)** defines IP address ranges.

| CIDR Notation   | Range                        | # of IPs |
|-----------------|------------------------------|-----------|
| `10.0.0.0/8`    | 10.0.0.0 – 10.255.255.255    | 16M       |
| `10.0.0.0/16`   | 10.0.0.0 – 10.0.255.255      | 65,536    |
| `10.0.1.0/24`   | 10.0.1.0 – 10.0.1.255        | 256       |
| `10.0.1.0/28`   | 10.0.1.0 – 10.0.1.15         | 16        |

> 💡 **Rule of thumb:** The higher the prefix number (e.g. `/28`), the smaller the network.

---

### 2. Subnets

Subnets divide your VPC into smaller, manageable segments.

```
VPC: 10.0.0.0/16
│
├── 🌐 Public Subnet  (10.0.1.0/24)   → Has route to Internet Gateway
│       └── Resources here CAN reach the internet
│
└── 🔒 Private Subnet (10.0.2.0/24)   → No direct internet route
        └── Resources here are ISOLATED from direct internet access
```

---

### 3. Internet Gateway vs NAT Gateway

```
                          INTERNET
                             │
                    ┌────────┴────────┐
                    │  Internet       │
                    │  Gateway (IGW)  │   ← Two-way traffic
                    └────────┬────────┘
                             │
             ┌───────────────┼──────────────┐
             │               │              │
    ┌────────┴──────┐   ┌────┴────────┐     │
    │ PUBLIC SUBNET │   │ NAT GATEWAY │     │
    │               │   │             │     │
    │  🌐 Web Server│   └────┬────────┘     │
    └───────────────┘        │              │
                    ┌────────┴──────┐       │
                    │ PRIVATE SUBNET│       │
                    │               │       │
                    │  🗄️  Database │       │
                    │  (outbound    │       │
                    │   only via    │       │
                    │   NAT)        │       │
                    └───────────────┘       │
```

| Gateway        | Direction     | Use Case                        |
|----------------|---------------|---------------------------------|
| Internet GW    | Inbound + Outbound | Public-facing resources    |
| NAT Gateway    | Outbound only | Private resources needing updates |

---

### 4. Security Groups vs NACLs

```
 INCOMING REQUEST
        │
        ▼
┌───────────────────┐
│      NACL         │  ← Network ACL (Subnet-level, stateless)
│  Subnet Boundary  │     Checks BOTH inbound & outbound rules
└───────┬───────────┘
        │
        ▼
┌───────────────────┐
│  Security Group   │  ← Instance-level, stateful
│  (EC2 / Resource) │     If inbound allowed → return traffic auto-allowed
└───────────────────┘
```

| Feature         | Security Group   | Network ACL        |
|-----------------|------------------|--------------------|
| Level           | Instance         | Subnet             |
| Stateful?       | ✅ Yes           | ❌ No (stateless)  |
| Allow/Deny?     | Allow only       | Allow & Deny       |
| Rule evaluation | All rules        | In order (numbered)|

---

### 5. Route Tables

Every subnet has a **route table** that defines where network traffic goes.

```
Route Table (Public Subnet)
┌──────────────────┬─────────────────┐
│   Destination    │     Target      │
├──────────────────┼─────────────────┤
│  10.0.0.0/16     │  local          │  ← VPC-internal traffic
│  0.0.0.0/0       │  igw-xxxxxxxx   │  ← All other traffic → Internet
└──────────────────┴─────────────────┘

Route Table (Private Subnet)
┌──────────────────┬─────────────────┐
│   Destination    │     Target      │
├──────────────────┼─────────────────┤
│  10.0.0.0/16     │  local          │  ← VPC-internal traffic
│  0.0.0.0/0       │  nat-xxxxxxxx   │  ← All other traffic → NAT
└──────────────────┴─────────────────┘
```

---

## 🏗️ Full Architecture Diagram

```
                          🌍 INTERNET
                               │
                    ┌──────────┴──────────┐
                    │   Internet Gateway  │
                    └──────────┬──────────┘
                               │
        ┌──────────────────────┼──────────────────────┐
        │          VPC: 10.0.0.0/16                    │
        │                      │                       │
        │   ┌─── AZ-1 ──────────────── AZ-2 ───────┐  │
        │   │                  │                    │  │
        │   │  ┌─────────────────────────────────┐  │  │
        │   │  │         PUBLIC SUBNETS          │  │  │
        │   │  │  10.0.1.0/24   │  10.0.3.0/24  │  │  │
        │   │  │  [Load Balancer] [Load Balancer]│  │  │
        │   │  │  [NAT Gateway]                  │  │  │
        │   │  └────────────┬────────────────────┘  │  │
        │   │               │                       │  │
        │   │  ┌─────────────────────────────────┐  │  │
        │   │  │        PRIVATE SUBNETS          │  │  │
        │   │  │  10.0.2.0/24   │  10.0.4.0/24  │  │  │
        │   │  │  [App Server]    [App Server]   │  │  │
        │   │  └────────────┬────────────────────┘  │  │
        │   │               │                       │  │
        │   │  ┌─────────────────────────────────┐  │  │
        │   │  │        DATABASE SUBNETS         │  │  │
        │   │  │  10.0.5.0/24   │  10.0.6.0/24  │  │  │
        │   │  │  [Primary DB]    [Replica DB]   │  │  │
        │   │  └─────────────────────────────────┘  │  │
        │   └────────────────────────────────────────┘  │
        └──────────────────────────────────────────────┘
```

---

## 🔗 VPC Peering & Connectivity Options

```
VPC-A (10.0.0.0/16)    VPC-B (172.16.0.0/16)
        │                        │
        └──── VPC Peering ───────┘   ← Private, no overlapping CIDR

        ┌─────────────────────────────────────────┐
        │              Transit Gateway            │
        │   (Hub-and-Spoke for multiple VPCs)     │
        └──┬──────────┬──────────┬───────────────┘
           │          │          │
         VPC-A      VPC-B      On-Premises
                               (via VPN / Direct Connect)
```

---

## ✅ Summary

| Concept          | What it Does                                      |
|------------------|---------------------------------------------------|
| **VPC**          | Isolated virtual network in the cloud             |
| **Subnet**       | Subdivides VPC into public/private segments       |
| **CIDR**         | Defines IP address ranges                         |
| **IGW**          | Allows public internet access (bidirectional)     |
| **NAT Gateway**  | Allows private resources to reach internet (outbound only) |
| **Security Group**| Stateful firewall at the instance level          |
| **NACL**         | Stateless firewall at the subnet level            |
| **Route Table**  | Controls where traffic is directed                |
| **VPC Peering**  | Private connection between two VPCs               |
| **Transit GW**   | Central hub connecting multiple VPCs & on-prem   |

---

## 📚 Free Resources

### 📘 Official Docs
- [AWS VPC Documentation](https://docs.aws.amazon.com/vpc/latest/userguide/)
- [Azure Virtual Network Docs](https://learn.microsoft.com/en-us/azure/virtual-network/)
- [GCP VPC Documentation](https://cloud.google.com/vpc/docs)

### 🎓 Free Learning Platforms
- [AWS Skill Builder (Free Tier)](https://skillbuilder.aws/) — Official AWS free training
- [Google Cloud Skills Boost](https://cloudskillsboost.google/) — Free labs & courses
- [freeCodeCamp – Networking Fundamentals](https://www.freecodecamp.org/)
- [Professor Messer – CompTIA Net+](https://www.professormesser.com/network-plus/n10-008/n10-008-video/n10-008-training-course/) — Free video course

### 📺 YouTube Channels
- **NetworkChuck** — Fun, beginner-friendly networking videos
- **TechWorld with Nana** — Cloud and DevOps concepts
- **Fireship** — Short, sharp tech explainers

### 🛠️ Hands-On Practice
- [AWS Free Tier](https://aws.amazon.com/free/) — Build a real VPC for free
- [Cisco Packet Tracer](https://www.netacad.com/courses/packet-tracer) — Network simulation tool
- [Katacoda / O'Reilly Scenarios](https://learning.oreilly.com/) — Interactive labs

---

## 📄 License

```
MIT License

Copyright (c) 2026 SalvatoreOps

Permission is hereby granted, free of charge, to any person obtaining a copy
of this README and associated documentation files, to deal in the content
without restriction, including without limitation the rights to use, copy,
modify, merge, publish, distribute, sublicense, and/or sell copies.
```

---

<div align="center">

Made with ❤️ by **SalvatoreOps** &nbsp;|&nbsp; March 15, 2026

*"The network is the computer." — Sun Microsystems*

</div>