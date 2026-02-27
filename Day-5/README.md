# Day 5 - AWS Security Essentials

A concise guide to implementing core AWS security measures to protect your cloud infrastructure.

## 📋 Overview

Securing AWS resources requires a **multi-layered approach**. This guide covers essential security controls—**Security Groups**, **Network ACLs**, and **IAM Policies**—all designed to uphold the **CIA Triad** (Confidentiality 🔐, Integrity ✅, Availability ⚡).

---

## 🛡️ Security Groups (Instance-Level Protection)

Your first line of defense for EC2 instances and ENIs.

**Key Features:**
- **Stateful** ✓ (Return traffic auto-allowed)
- **Instance-level** firewall
- **Allow rules only** (implicit deny by default)

**Best Practices:**
- Follow principle of **least privilege** 🔒
- Restrict source CIDRs (avoid 0.0.0.0/0) 🚫
- Use descriptive naming conventions 🏷️
- Regularly audit and remove unused rules 🧹

---

## 🌐 Network ACLs (Subnet-Level Protection)

Optional additional layer for subnet traffic control.

**Key Features:**
- **Stateless** (Return rules must be explicit) 🔄
- **Subnet-level** protection
- **Ordered rules** (evaluated by number 1-32766)
- **Allow AND Deny** rules ✅❌

**Best Practices:**
- Block specific IP ranges at NACL level before SG 🎯
- Leave gaps between rule numbers for future inserts 🔢
- Use as subnet-level firewall, not instance-level 🧱

---

## 👤 IAM Policies (Identity & Access Management)

Control **WHO** can do **WHAT** on **WHICH** resources.

**Key Components:**
- **Users**: Individual access (avoid long-term credentials) 👤
- **Groups**: Permission collections for teams 👥
- **Roles**: Temporary credentials for services/apps 🎭
- **Policies**: JSON documents defining permissions 📄

**Best Practices:**
- **Least Privilege**: Start with zero, grant minimum needed 🎯
- **Use Roles** instead of access keys where possible 🔑
- **Enable MFA** for root and privileged users 📱
- **Regular access reviews** and credential rotation 🔄

---

## 🔐 The CIA Triad in AWS

**Confidentiality** 🤐
- Encryption at rest (KMS, S3 SSE) and in transit (TLS/SSL)
- Private subnets and VPC endpoints
- Access logging and monitoring

**Integrity** 🛡️
- Checksums and hashing (S3 Object Lock)
- Version control and backup strategies
- Immutable infrastructure patterns

**Availability** ⚡
- Multi-AZ deployments
- Auto Scaling and Load Balancing
- DDoS protection (AWS Shield) and WAF

---

## ✅ Quick Implementation Checklist

- [ ] Default Security Groups restrict all inbound traffic
- [ ] NACLs block known malicious IP ranges
- [ ] IAM users have no permissions by default (explicit denies)
- [ ] Enable CloudTrail for API auditing 📝
- [ ] Enable GuardDuty for threat detection 🔍
- [ ] Rotate access keys every 90 days 🔄
- [ ] Tag resources for security tracking 🏷️

---

## 🚀 Summary

Implement **Security Groups** for instance protection, **NACLs** for subnet boundaries, and **IAM** for identity management. Together, they create a **defense-in-depth** strategy ensuring your AWS resources remain confidential, intact, and available

*Stay secure!* 🔒✨

---

*Last Updated: 2024*  
*Author: SalvatoreOps*  
*License: Public Domain / Free to share*

---