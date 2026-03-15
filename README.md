<div align="center">

# ☁️ AWS Zero → Hero

**One AWS service per day. 31 days. Zero to production-ready cloud engineer.**

![Challenge](https://img.shields.io/badge/Open_Learning-Challenge-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white)
![Days](https://img.shields.io/badge/31_Days-31_Services-232F3E?style=for-the-badge&logo=amazonaws&logoColor=FF9900)
![Level](https://img.shields.io/badge/Level-Beginner_Friendly-2ECC71?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-7C5CBF?style=for-the-badge)

| 🗓️ Services | ⏱️ Days | 📦 Weeks | 🚀 Possibilities |
|:-----------:|:-------:|:--------:|:----------------:|
| **31**      | **31**  | **4**    | **∞**            |

</div>

---

## 📖 What is AWS Zero to Hero?

A structured, self-paced learning journey designed to take you from **zero cloud experience** to confidently building on AWS. Each day focuses on a **single AWS service** — theory, hands-on practice, and a mini project.

No previous AWS experience needed. Just show up every day.

> **Challenge rule:** Spend at least 30 minutes per day exploring the service, build something small, and document what you learned. Consistency beats intensity.

---

## 🏗️ Cloud Architecture You'll Build

By the end of 30 days, you'll understand how these AWS services connect to form a real-world production architecture.

```
                        ┌─────────────────── AWS Cloud ──────────────────────────────────┐
                        │  ┌─────────────────── VPC ──────────────────────────────────┐  │
                        │  │                                                           │  │
                        │  │              ┌─────────────┐                             │  │
                        │  │         ┌──▶ │  CloudFront │  (CDN)                      │  │
  ┌──────────┐          │  │         │   └─────────────┘                             │  │
  │  Users   │──────────┼──┼──▶ Route 53 ──▶ ┌─────────────┐   ┌──────┐  ┌───────┐ │  │
  │ Browser  │          │  │         │   └──▶ │     ALB     │──▶│ EC2  │─▶│  RDS  │ │  │
  └──────────┘          │  │         │        │Load Balancer│   └──────┘  └───────┘ │  │
                        │  │         │        └─────────────┘      │                │  │
                        │  │         │                          ┌──────┐  ┌─────────┤  │
                        │  │         └──▶ ┌────┐ (Storage)     │Lambda│─▶│DynamoDB │  │
                        │  │              │ S3 │               └──────┘  └─────────┘  │  │
                        │  │              └────┘                   │                  │  │
                        │  │                               ┌───────┴──────┐           │  │
                        │  │                               │IAM  CloudWatch│           │  │
                        │  │                               │Security  Logs │           │  │
                        │  │                               └──────────────┘           │  │
                        │  └───────────────────────────────────────────────────────────┘  │
                        └────────────────────────────────────────────────────────────────┘
```

---

## ⚙️ How Each Day Works

| Step | Action | Description |
|:----:|--------|-------------|
| 📖 **01** | **Read the Docs** | Spend 10 min reading the official AWS documentation overview for the day's service |
| 🛠️ **02** | **Hands-On Lab** | Deploy and configure the service via console, CLI, or Terraform — break things, fix them |
| 🔗 **03** | **Connect the Dots** | Integrate today's service with something you built on a previous day |
| ✍️ **04** | **Document It** | Write a short summary in your notes or GitHub — teaching is the best way to learn |

---

## 🗺️ 30-Day Learning Path

### 📅 Week 0 — Before You Begin

| Day | Topic | Focus |
|-----|-------|-------|
| `D00` | 🌐 **Introduction to AWS** | What is cloud computing, AWS global infrastructure, regions & AZs, core service categories, free tier setup, and the AWS console walkthrough |

> 💡 **Day 0 goal:** Create your AWS account, explore the console, understand the big picture — then hit Day 1 with confidence.

<details>
<summary>📖 <strong>Day 0 — What You'll Learn</strong></summary>

<br>

**What is Cloud Computing?**
- On-premise vs cloud vs hybrid models
- IaaS / PaaS / SaaS explained with AWS examples
- Why businesses move to the cloud (cost, scale, speed)

**AWS Global Infrastructure**
- **Regions** — independent geographic locations (e.g. `us-east-1`, `ap-south-1`)
- **Availability Zones (AZs)** — isolated data centres within a region
- **Edge Locations** — CDN endpoints used by CloudFront
- **Local Zones** — low-latency extension of a region

**AWS Core Service Categories**

| Category | Example Services |
|----------|-----------------|
| Compute | EC2, Lambda, ECS |
| Storage | S3, EBS, EFS |
| Database | RDS, DynamoDB, ElastiCache |
| Networking | VPC, Route 53, CloudFront |
| Security | IAM, WAF, Shield, KMS |
| DevOps | CodePipeline, CodeBuild, CodeDeploy |
| Monitoring | CloudWatch, X-Ray, CloudTrail |
| AI / ML | Bedrock, SageMaker, Rekognition |

**Account Setup Checklist**
- [ ] Create a new AWS account at [aws.amazon.com](https://aws.amazon.com)
- [ ] Enable MFA on the root account
- [ ] Set a billing alert at $5 in CloudWatch
- [ ] Explore the AWS Management Console
- [ ] Install and configure the AWS CLI v2
- [ ] Bookmark the [AWS Docs](https://docs.aws.amazon.com)

**Key Concepts to Know Before Day 1**
- **ARN** — Amazon Resource Name, a unique identifier for every AWS resource
- **Region vs AZ** — deploy across AZs for high availability
- **Shared Responsibility Model** — AWS secures the cloud; you secure what's *in* the cloud
- **Pay-as-you-go** — you only pay for what you use, when you use it

</details>

---

### 📅 Week 1 — Foundations

| Day | Service | Focus |
|-----|---------|-------|
| `D01` | 🔑 **IAM & Security** | Users, roles, policies, least privilege |
| `D02` | 💻 **EC2 Basics** | Launch, connect, security groups, key pairs |
| `D03` | 🌐 **VPC & Networking** | Subnets, route tables, IGW, NAT |
| `D04` | 🪣 **S3 Storage** | Buckets, versioning, static hosting, lifecycle |
| `D05` | ⚖️ **ELB & Auto Scaling** | Load balancers, target groups, scaling policies |
| `D06` | 🗂️ **EFS & EBS** | Block vs file storage, snapshots, mounting |
| `D07` | 🏗️ **Build Week-1 Project** | Deploy a simple web app using all week-1 services |

### 📅 Week 2 — Databases & Compute

| Day | Service | Focus |
|-----|---------|-------|
| `D08` | 🐬 **RDS & Aurora** | Managed SQL, multi-AZ, read replicas |
| `D09` | ⚡ **DynamoDB** | NoSQL, partition keys, GSI, on-demand capacity |
| `D10` | 🔥 **ElastiCache** | Redis, Memcached, caching strategies |
| `D11` | λ **Lambda** | Functions, triggers, environment variables, layers |
| `D12` | 🚪 **API Gateway** | REST & HTTP APIs, stages, throttling |
| `D13` | 📦 **ECS & Fargate** | Containers, task definitions, services |
| `D14` | ☸️ **EKS Intro** | Managed Kubernetes, node groups, kubectl |

### 📅 Week 3 — DevOps & Messaging

| Day | Service | Focus |
|-----|---------|-------|
| `D15` | 🔁 **CodePipeline** | CI/CD orchestration, stages, approvals |
| `D16` | 🔨 **CodeBuild** | Build environments, buildspec.yml, artifacts |
| `D17` | 🚀 **CodeDeploy** | Blue/green, rolling, in-place deployments |
| `D18` | 📬 **SQS & SNS** | Queues, topics, fan-out patterns, DLQ |
| `D19` | 🌊 **Kinesis** | Data streams, Firehose, real-time analytics |
| `D20` | 📧 **SES & EventBridge** | Email delivery, event-driven architecture |
| `D21` | 🏗️ **Build Week-3 Pipeline** | Full CI/CD pipeline with messaging integration |

### 📅 Week 4 — Advanced & Production

| Day | Service | Focus |
|-----|---------|-------|
| `D22` | 🌍 **CloudFront & Route 53** | CDN, DNS, latency routing, health checks |
| `D23` | 👁️ **CloudWatch & X-Ray** | Metrics, logs, alarms, distributed tracing |
| `D24` | 📋 **CloudFormation** | IaC, stacks, nested stacks, drift detection |
| `D25` | 🏢 **Organizations & Billing** | Multi-account, SCPs, Cost Explorer, budgets |
| `D26` | 🔐 **Secrets Manager** | Secret rotation, KMS, parameter store |
| `D27` | 🛡️ **WAF & Shield** | Web ACLs, DDoS protection, rate limiting |
| `D28` | 🤖 **Bedrock & AI** | Foundation models, RAG, Claude on AWS |
| `D29` | 🧪 **Review & Refine** | Revisit weak spots, optimize costs, security audit |
| `D30` | 🏆 **Final Capstone** | Build and deploy a full production-grade app |

---

## 🧰 Toolbox

Everything you need — most of it free:

| Tool | Purpose |
|------|---------|
| `AWS Free Tier` | Your cloud playground — 12 months free on most services |
| `AWS CLI v2` | Interact with AWS from the terminal |
| `Terraform` | Infrastructure as Code for reproducible deployments |
| `VS Code` | Code editor with great AWS extensions |
| `GitHub` | Version control and portfolio showcase |
| `draw.io` | Diagram your architectures |

> 💡 **Tip:** Create a dedicated AWS account using the Free Tier. Set a **billing alert at $5** so you never get surprised. Almost everything in this challenge can be done for free.

---

## 🚀 Getting Started

```bash
# 1. Fork this repository
git clone https://github.com/your-username/aws-zero-to-hero.git
cd aws-zero-to-hero

# 2. Install the AWS CLI
brew install awscli          # macOS
# or visit: https://aws.amazon.com/cli/

# 3. Configure your credentials
aws configure

# 4. Start Day 1!
open days/day-01-iam/README.md
```

---

## 📁 Repository Structure

```
aws-zero-to-hero/
├── days/
│   ├── day-01-iam/
│   │   ├── README.md        ← Theory + notes
│   │   └── terraform/       ← IaC for the day
│   ├── day-02-ec2/
│   └── ...
├── projects/
│   ├── week-1-webapp/
│   ├── week-3-pipeline/
│   └── capstone/
└── README.md
```

---

## 🤝 Contributing

Found a bug or want to add more services? PRs are welcome!

1. Fork the repo
2. Create your branch: `git checkout -b day-31-new-service`
3. Commit changes: `git commit -m 'Add Day 31: Amazon XYZ'`
4. Push and open a PR

---

## 📋 Summary

By completing this challenge you will have:

| Milestone | What You Gained |
|-----------|----------------|
| ✅ **Week 1** | Core AWS fundamentals — compute, networking, storage, and identity |
| ✅ **Week 2** | Databases and serverless patterns using RDS, DynamoDB, Lambda, and containers |
| ✅ **Week 3** | Full CI/CD pipelines and event-driven architecture with messaging services |
| ✅ **Week 4** | Production-grade skills — security, monitoring, IaC, and AI integration |
| 🏆 **Day 30** | A deployed capstone project you can show in your portfolio |

> After finishing this challenge you'll have the hands-on confidence to pursue **AWS certifications** (Cloud Practitioner → Solutions Architect Associate) and build real production systems from scratch.

---

## 📚 Free Resources

### 📖 Official AWS
| Resource | Link |
|----------|------|
| AWS Documentation | [docs.aws.amazon.com](https://docs.aws.amazon.com) |
| AWS Free Tier | [aws.amazon.com/free](https://aws.amazon.com/free) |
| AWS Skill Builder (free tier) | [skillbuilder.aws](https://skillbuilder.aws) |
| AWS Well-Architected Framework | [aws.amazon.com/architecture/well-architected](https://aws.amazon.com/architecture/well-architected/) |
| AWS Architecture Center | [aws.amazon.com/architecture](https://aws.amazon.com/architecture/) |

### 🎥 YouTube Channels
| Channel | Why Watch |
|---------|-----------|
| [AWS Events](https://www.youtube.com/@AWSEventsChannel) | re:Invent talks, deep dives, live demos |
| [TechWorld with Nana](https://www.youtube.com/@TechWorldwithNana) | Kubernetes, Docker, DevOps fundamentals |
| [Fireship](https://www.youtube.com/@Fireship) | Quick cloud and dev concepts in 100 seconds |
| [freeCodeCamp](https://www.youtube.com/@freecodecamp) | Full-length AWS certification prep courses |

### 🧪 Hands-On Practice
| Platform | What's Free |
|----------|-------------|
| [AWS CloudQuest](https://aws.amazon.com/training/digital/aws-cloud-quest/) | Gamified, role-based AWS practice labs |
| [KodeKloud](https://kodekloud.com) | Free tier labs for Docker, K8s, Terraform |
| [Play with Docker](https://labs.play-with-docker.com) | Free browser-based Docker playground |
| [Katacoda (O'Reilly)](https://www.katacoda.com) | Interactive scenarios for cloud and DevOps |

### 📝 Certification Paths (post-challenge)
```
AWS Cloud Practitioner (CLF-C02)   ← Great first cert, ~2 weeks prep
        │
        ▼
AWS Solutions Architect Associate (SAA-C03)   ← Most popular, ~2 months prep
        │
        ├──▶ AWS Developer Associate (DVA-C02)
        │
        └──▶ AWS SysOps Administrator Associate (SOA-C02)
```

---

## 👤 Author

| Field | Details |
|-------|---------|
| **Name** | SalvatoreOps |
| **GitHub** | [@SalvatoreOps](https://github.com/SalvatoreOps) |
| **Started** | March 15, 2026 |
| **Challenge Duration** | 30 days (Mar 15 – Apr 13, 2026) |

---

## 📄 License

```
MIT License

Copyright (c) 2026 SalvatoreOps

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
```

See the full [LICENSE](./LICENSE) file for details.

---

<div align="center">

Built with ☁️ & ☕ by **SalvatoreOps** &nbsp;·&nbsp; [Start with AWS Free Tier](https://aws.amazon.com/free/) &nbsp;·&nbsp; ⭐ Star this repo if it helped you!

`aws-zero-to-hero` &nbsp;·&nbsp; MIT License &nbsp;·&nbsp; © 2026 SalvatoreOps

</div>