# 🔐 IAM & Security — A Visual Guide

> **Author:** SalvatoreOps  
> **Date:** March 15, 2026  
> **License:** MIT License

---

## 📌 Table of Contents

- [What is IAM?](#what-is-iam)
- [Core IAM Concepts](#core-iam-concepts)
- [IAM Architecture Diagram](#iam-architecture-diagram)
- [Authentication vs Authorization](#authentication-vs-authorization)
- [The Principle of Least Privilege](#the-principle-of-least-privilege)
- [IAM Policy Flow](#iam-policy-flow)
- [Common IAM Roles & Permissions](#common-iam-roles--permissions)
- [Security Best Practices](#security-best-practices)
- [Threat Model Overview](#threat-model-overview)
- [Summary](#summary)
- [Free Resources](#free-resources)

---

## 🧩 What is IAM?

**Identity and Access Management (IAM)** is the framework of policies, processes, and technologies that ensures the **right people** have the **right access** to the **right resources** at the **right time**.

```
  ┌─────────────────────────────────────────────────┐
  │                    IAM TRIAD                    │
  │                                                 │
  │       👤 WHO        🔑 WHAT        🏛️ WHERE      │
  │    (Identity)    (Permission)    (Resource)     │
  │                                                 │
  │   "Alice"  ──►  "read-only"  ──►  "S3 bucket"  │
  └─────────────────────────────────────────────────┘
```

---

## 🔑 Core IAM Concepts

| Concept        | Description                                              | Example                          |
|----------------|----------------------------------------------------------|----------------------------------|
| **Identity**   | Who or what is making a request                          | User, Service Account, Role      |
| **Authentication** | Proving you are who you claim to be               | Password, MFA, Certificate       |
| **Authorization**  | What you are allowed to do after authentication   | Read, Write, Delete permissions  |
| **Policy**     | A document defining allowed/denied actions               | JSON policy attached to a role   |
| **Role**       | A set of permissions assignable to an identity           | `admin`, `developer`, `viewer`   |
| **Group**      | A collection of users sharing the same permissions       | `DevTeam`, `OpsTeam`             |

---

## 🏗️ IAM Architecture Diagram

```
                        ┌──────────────────────────────────┐
                        │         IDENTITY PROVIDER        │
                        │   (Active Directory / Okta / SSO)│
                        └────────────────┬─────────────────┘
                                         │ issues token
                                         ▼
  ┌─────────┐  request   ┌──────────────────────────────┐
  │  USER   │ ─────────► │       IAM ENGINE             │
  │  👤     │            │  ┌──────────┐ ┌───────────┐  │
  └─────────┘            │  │ AuthN    │ │  AuthZ    │  │
                         │  │(Who?)    │ │ (Can do?) │  │
  ┌─────────┐  request   │  └──────────┘ └───────────┘  │
  │ SERVICE │ ─────────► │        Policy Evaluator       │
  │  ⚙️     │            └──────────────┬───────────────┘
  └─────────┘                           │
                                        │ allow / deny
                          ┌─────────────▼─────────────┐
                          │         RESOURCES          │
                          │  🗄️ DB  ☁️ Cloud  📁 Files │
                          └───────────────────────────┘
```

---

## 🔒 Authentication vs Authorization

```
  ┌─────────────────────────────────────────────────────────┐
  │                                                         │
  │  🔐 AUTHENTICATION                                      │
  │  "Are you who you say you are?"                         │
  │                                                         │
  │   👤 ──► [Password] ──► [MFA Token] ──► ✅ Verified     │
  │                                                         │
  ├─────────────────────────────────────────────────────────┤
  │                                                         │
  │  🛡️ AUTHORIZATION                                       │
  │  "What are you allowed to do?"                          │
  │                                                         │
  │   ✅ Verified ──► [Check Policy] ──► [Role: viewer]     │
  │                         │                               │
  │                    ✅ Can READ                           │
  │                    ❌ Cannot DELETE                      │
  │                    ❌ Cannot WRITE                       │
  │                                                         │
  └─────────────────────────────────────────────────────────┘
```

> 🧠 **Think of it this way:** Authentication is your **ID card**. Authorization is the **list of doors** that ID card can open.

---

## ⚖️ The Principle of Least Privilege

> **"Give only the minimum access required — nothing more."**

```
  ❌ BAD PRACTICE                    ✅ GOOD PRACTICE
  ─────────────────                  ─────────────────
  Developer gets                     Developer gets
  full admin access                  only access to
  to everything                      dev environment

  👤 ──► 🔓 ALL RESOURCES            👤 ──► 🔓 dev-bucket
                                          🔒 prod-bucket
                                          🔒 billing
                                          🔒 user-management
```

### Why It Matters:
- 🚨 Limits blast radius of a breach
- 🔍 Easier to audit and trace actions
- ✅ Required for compliance (SOC2, ISO 27001, HIPAA)

---

## 📋 IAM Policy Flow

```
  User/Service Makes a Request
           │
           ▼
  ┌─────────────────────────┐
  │  Is there an explicit   │
  │       DENY policy?      │
  └────────┬────────────────┘
           │
      YES  │  NO
           │    │
           ▼    ▼
        DENY   Is there an explicit
                  ALLOW policy?
                      │
                 YES  │  NO
                      │   │
                      ▼   ▼
                   ALLOW  DENY (default)
```

> 🔴 **Explicit DENY always wins** — even if an ALLOW exists elsewhere.

---

## 🛡️ Security Best Practices

```
  ┌──────────────────────────────────────────────────────┐
  │              IAM SECURITY CHECKLIST                  │
  ├──────────────────────────────────────────────────────┤
  │  ✅  Enable Multi-Factor Authentication (MFA)        │
  │  ✅  Rotate credentials regularly (90-day policy)    │
  │  ✅  Use roles instead of long-lived access keys     │
  │  ✅  Never embed credentials in source code          │
  │  ✅  Apply Principle of Least Privilege              │
  │  ✅  Audit logs regularly (CloudTrail, etc.)         │
  │  ✅  Remove unused accounts & stale permissions      │
  │  ✅  Use groups to manage permissions, not per-user  │
  │  ✅  Enforce strong password policies                │
  │  ✅  Separate dev / staging / production environments│
  └──────────────────────────────────────────────────────┘
```

---

## 🌐 Threat Model Overview

```
  COMMON IAM ATTACK VECTORS
  ──────────────────────────

  1. CREDENTIAL THEFT
     👤 Attacker ──► stolen password ──► 🔓 Unauthorized access

  2. PRIVILEGE ESCALATION
     👤 Low-privilege user ──► exploits misconfiguration ──► 👑 Admin

  3. INSIDER THREAT
     👤 Trusted employee ──► abuses excess permissions ──► 💀 Data leak

  4. TOKEN HIJACKING
     👤 Attacker intercepts JWT/OAuth token ──► impersonates user

  DEFENSES:
  ─────────
  🛡️  MFA              → blocks stolen password attacks
  🔍  Audit Logging    → detects unusual access patterns
  ⏱️  Short-lived Tokens → reduces token hijacking impact
  🔒  Zero Trust       → never trust, always verify
```

---

## 📝 Summary

| Topic                      | Key Takeaway                                              |
|----------------------------|-----------------------------------------------------------|
| **IAM**                    | Manages who can access what resources                     |
| **Authentication**         | Verifies identity (passwords, MFA, certs)                 |
| **Authorization**          | Grants/restricts actions based on policies                |
| **Least Privilege**        | Only give the minimum permissions needed                  |
| **Explicit Deny**          | Always overrides any Allow                                |
| **Roles vs Users**         | Prefer roles — they're scalable and auditable             |
| **Zero Trust**             | Never implicitly trust — always verify                    |
| **MFA**                    | Essential first line of defense                           |

> 🔑 **Golden Rule:** Treat IAM like the **keys to your house** — hand them out carefully, take them back promptly, and always know who has a copy.

---

## 📚 Free Resources

### 📖 Documentation & Guides
- 🔗 [AWS IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/) — Official AWS IAM docs
- 🔗 [Google Cloud IAM Overview](https://cloud.google.com/iam/docs/overview) — GCP IAM documentation
- 🔗 [Azure RBAC Documentation](https://learn.microsoft.com/en-us/azure/role-based-access-control/) — Microsoft Azure
- 🔗 [OWASP Access Control Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Access_Control_Cheat_Sheet.html)

### 🎓 Free Courses & Learning
- 🔗 [AWS Cloud Practitioner Essentials (Free)](https://explore.skillbuilder.aws/learn/course/134) — covers IAM basics
- 🔗 [Google Cloud Skills Boost – IAM](https://cloudskillsboost.google/) — free labs available
- 🔗 [Microsoft Learn – Azure Security](https://learn.microsoft.com/en-us/training/paths/az-900-describe-azure-architecture-services/) — free path

### 🛠️ Tools
- 🔗 [IAM Access Analyzer (AWS)](https://aws.amazon.com/iam/features/analyze-access/) — find overly permissive policies
- 🔗 [Cloudsplaining](https://github.com/salesforce/cloudsplaining) — open-source IAM security assessment
- 🔗 [Pacu](https://github.com/RhinoSecurityLabs/pacu) — AWS exploitation framework (for learning/testing)

### 📰 Stay Updated
- 🔗 [Krebs on Security](https://krebsonsecurity.com/) — security news
- 🔗 [The Hacker News](https://thehackernews.com/) — daily security updates
- 🔗 [CISA Alerts](https://www.cisa.gov/news-events/cybersecurity-advisories) — official US advisories

---

## ⚖️ License

```
MIT License

Copyright (c) 2026 SalvatoreOps

Permission is hereby granted, free of charge, to any person obtaining a copy
of this documentation and associated files, to deal in it without restriction,
including without limitation the rights to use, copy, modify, merge, publish,
and distribute, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the material.

THE MATERIAL IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND.
```

---

<div align="center">

**Made with ❤️ by SalvatoreOps · March 15, 2026**

*"Security is not a product, but a process." — Bruce Schneier*

</div>
