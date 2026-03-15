# ⚖️ AWS ELB & Auto Scaling — A Visual Guide

> **Author:** SalvatoreOps
> **Date:** March 15, 2026
> **License:** MIT

---

## 📌 Table of Contents

- [What is ELB?](#-what-is-elb)
- [Types of Load Balancers](#-types-of-load-balancers)
- [How ELB Works](#-how-elb-works)
- [What is Auto Scaling?](#-what-is-auto-scaling)
- [How Auto Scaling Works](#-how-auto-scaling-works)
- [ELB + Auto Scaling Together](#-elb--auto-scaling-together)
- [Summary](#-summary)
- [Free Resources](#-free-resources)

---

## 🔀 What is ELB?

**Elastic Load Balancing (ELB)** automatically distributes incoming application traffic across multiple targets — EC2 instances, containers, IP addresses, and Lambda functions — in one or more Availability Zones.

> Think of ELB as a **smart traffic cop** 🚦 that stands in front of your application and evenly routes user requests so no single server gets overwhelmed.

```
                        ┌─────────────┐
         User A ──────► │             │──────► EC2 Instance 1
         User B ──────► │  LOAD       │──────► EC2 Instance 2
         User C ──────► │  BALANCER   │──────► EC2 Instance 3
         User D ──────► │             │──────► EC2 Instance 4
                        └─────────────┘
```

---

## 🧱 Types of Load Balancers

| Type | Layer | Best For |
|------|-------|----------|
| **ALB** (Application) | Layer 7 (HTTP/HTTPS) | Web apps, microservices, path-based routing |
| **NLB** (Network) | Layer 4 (TCP/UDP) | Ultra-high performance, static IPs |
| **GLB** (Gateway) | Layer 3 | 3rd-party virtual appliances (firewall, IDS) |
| **CLB** (Classic) | Layer 4/7 | Legacy workloads (not recommended for new apps) |

---

## 🔄 How ELB Works

```
┌────────────────────────────────────────────────────────────┐
│                        INTERNET                            │
└────────────────────────┬───────────────────────────────────┘
                         │
                         ▼
              ┌──────────────────────┐
              │   Route 53 (DNS)     │
              └──────────┬───────────┘
                         │
                         ▼
              ┌──────────────────────┐
              │  Application Load    │  ◄── Health Checks
              │  Balancer (ALB)      │      every 30s
              └───┬──────────┬───────┘
                  │          │
         ┌────────┘          └────────┐
         ▼                            ▼
  ┌────────────┐              ┌────────────┐
  │  AZ-1      │              │  AZ-2      │
  │ EC2 (healthy)             │ EC2 (healthy)
  └────────────┘              └────────────┘

  ✅ Healthy instances receive traffic
  ❌ Unhealthy instances are automatically removed
```

**Key features:**
- 🏥 **Health checks** — continuously monitors instance health
- 🔒 **SSL/TLS termination** — offloads HTTPS decryption
- 🍪 **Sticky sessions** — routes same user to same instance
- 📊 **Access logs** — full request visibility

---

## 📈 What is Auto Scaling?

**AWS Auto Scaling** automatically adjusts the number of EC2 instances in response to demand. It ensures you have the **right number of instances** at the right time — no more, no less.

> Auto Scaling is like having a **self-staffing restaurant** 🍽️ — it opens more checkout lanes when it gets busy and closes them when it's quiet.

```
  Low Traffic         Normal Traffic        High Traffic (Peak)
  ┌─────────┐         ┌─────────┐           ┌─────────┐
  │ 2 EC2s  │  ────►  │ 4 EC2s  │  ────►    │ 8 EC2s  │
  └─────────┘         └─────────┘           └─────────┘
       ▲                                          │
       └──────────── Scale IN ◄──────────────────┘
```

---

## ⚙️ How Auto Scaling Works

```
┌──────────────────────────────────────────────────────────┐
│               AUTO SCALING GROUP (ASG)                   │
│                                                          │
│   Min: 2 instances   Desired: 4   Max: 10 instances      │
│                                                          │
│  ┌────────────┐  CloudWatch Alarm (CPU > 70%)            │
│  │  Scaling   │ ──────────────────────────────► SCALE OUT│
│  │  Policy    │                                 +2 EC2s  │
│  └────────────┘ ──────────────────────────────► SCALE IN │
│                  CloudWatch Alarm (CPU < 20%)    -2 EC2s  │
└──────────────────────────────────────────────────────────┘
```

### Scaling Policy Types

```
┌──────────────────┬──────────────────────────────────────────┐
│ Policy Type      │ How it Works                             │
├──────────────────┼──────────────────────────────────────────┤
│ Target Tracking  │ Keep CPU at 50% — ASG adjusts itself     │
│ Step Scaling     │ Scale by N when alarm threshold crossed  │
│ Scheduled        │ Scale up at 9am, down at 6pm daily       │
│ Predictive       │ ML-based — forecasts future demand       │
└──────────────────┴──────────────────────────────────────────┘
```

---

## 🤝 ELB + Auto Scaling Together

This is where the **real power** lies. ELB and Auto Scaling are designed to work hand-in-hand:

```
                    ┌──────────────────────────────────────────┐
                    │           AWS Auto Scaling Group          │
   ┌─────────┐      │  ┌──────┐  ┌──────┐  ┌──────┐  ┌──────┐ │
   │   ELB   │ ◄───►│  │ EC2  │  │ EC2  │  │ EC2  │  │ EC2  │ │
   └─────────┘      │  └──────┘  └──────┘  └──────┘  └──────┘ │
        ▲           │       ▲ New instance auto-registered      │
        │           └──────────────────────────────────────────┘
        │
   User Traffic
   (distributed
    evenly)

  ✅ New instances → automatically added to ELB target group
  ❌ Terminated instances → automatically removed from ELB
```

### Real-World Flow

```
1. Traffic spikes  →  CloudWatch detects CPU > 70%
2. ASG launches 2 new EC2 instances
3. Instances register with ELB target group
4. ELB starts routing traffic to new instances
5. Traffic drops  →  CPU < 20%
6. ASG terminates 2 instances (connection draining first)
7. ELB deregisters those instances gracefully
```

---

## 🧾 Summary

| Feature | ELB | Auto Scaling |
|---|---|---|
| **Purpose** | Distribute traffic | Adjust capacity |
| **Reacts to** | Incoming requests | CloudWatch metrics |
| **Ensures** | No server overload | Cost + performance balance |
| **Works with** | EC2, Lambda, containers | EC2, ECS, DynamoDB, RDS |
| **Key benefit** | High availability | Elasticity & cost savings |

> 🏆 **Best practice:** Always use ELB + Auto Scaling together. ELB handles **distribution**, Auto Scaling handles **capacity** — together they give you a resilient, cost-efficient, highly available architecture.

---

## 📚 Free Resources

| Resource | Link |
|---|---|
| 📖 AWS ELB Official Docs | [docs.aws.amazon.com/elasticloadbalancing](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/what-is-load-balancing.html) |
| 📖 AWS Auto Scaling Docs | [docs.aws.amazon.com/autoscaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html) |
| 🎓 AWS Free Tier (hands-on) | [aws.amazon.com/free](https://aws.amazon.com/free/) |
| 🎥 freeCodeCamp AWS Course | [youtube.com/freecodecamp](https://www.youtube.com/watch?v=ulprqHHWlng) |
| 📝 AWS Well-Architected Framework | [aws.amazon.com/architecture/well-architected](https://aws.amazon.com/architecture/well-architected/) |
| 🃏 AWS Cheat Sheets (TutorialsDojo) | [tutorialsdojo.com/aws-cheat-sheets](https://tutorialsdojo.com/aws-cheat-sheets/) |
| 🛠️ AWS Architecture Icons | [aws.amazon.com/architecture/icons](https://aws.amazon.com/architecture/icons/) |

---

## 📄 License

```
MIT License

Copyright (c) 2026 SalvatoreOps

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files, to deal in the Software
without restriction, including without limitation the rights to use, copy,
modify, merge, publish, distribute, sublicense, and/or sell copies of the
Software.
```

---

<div align="center">

Made with ❤️ by **SalvatoreOps** · March 15, 2026

*"Scale smart. Balance always."*

</div>