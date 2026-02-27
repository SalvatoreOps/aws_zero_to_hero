# Day 6 - AWS S3 (Simple Storage Service) - Quick Reference 🪣

## 📌 Overview
Amazon S3 is object storage with **99.999999999% (11 9's)** durability & **99.99%** availability. Store unlimited data across AWS Regions with industry-leading scalability, security, and performance.

---

## 🏗️ Core Concepts

| Concept | Description | Limits |
|---------|-------------|---------|
| 🪣 **Buckets** | Global containers for objects | 100/account (soft limit) |
| 📦 **Objects** | Files + metadata + unique key | 0 bytes – 5 TB |
| 🔑 **Keys** | Unique ID (filename with path) | 1,024 bytes max |
| 🌐 **Regions** | Geographic storage locations | Data stays within region |

---

## 💾 Storage Classes Comparison

| Class | Durability | Availability | Min Storage | Retrieval | Best For |
|-------|------------|--------------|-------------|-----------|----------|
| **Standard** 🚀 | 11 9's | 99.99% | None | Milliseconds | Frequently accessed data |
| **Intelligent-Tiering** 🧠 | 11 9's | 99.99% | None | Milliseconds | Unknown/changing patterns |
| **Standard-IA** 💾 | 11 9's | 99.9% | 30 days | Milliseconds | Backups, infrequent access |
| **One Zone-IA** 📍 | 11 9's | 99.5% | 30 days | Milliseconds | Recreatable data (single AZ) |
| **Glacier Instant** ⚡ | 11 9's | 99.9% | 90 days | Milliseconds | Archive needing fast access |
| **Glacier Flexible** 🧊 | 11 9's | 99.99% | 90 days | 1 min – 12 hrs | Long-term archive |
| **Glacier Deep** 🥶 | 11 9's | 99.99% | 180 days | 12 – 48 hrs | Compliance/cheapest storage |
| **Express One Zone** 💨 | 11 9's | 99.95% | None | Single-digit ms | High-performance apps |

---

## 🔐 Security & Access Control

- **Encryption** 🔒
  - SSE-S3 (AWS managed keys)
  - SSE-KMS (KMS keys + audit trail)
  - SSE-C (Customer-provided keys)
  - Client-side encryption

- **Access Control** 🛡️
  - IAM Policies (user-based)
  - Bucket Policies (resource-based JSON)
  - ACLs (legacy – disable recommended)
  - Block Public Access (enable by default!)

- **Compliance** 📜
  - Object Lock (WORM): Governance & Compliance modes
  - Legal Hold (indefinite retention)
  - Versioning (protect against deletion)

---

## ⚡ Key Features

| Feature | What It Does |
|---------|-------------|
| 🔄 **Versioning** | Keep multiple object versions; recover from overwrites |
| 📅 **Lifecycle Policies** | Auto-transition to cheaper classes; auto-delete old objects |
| 🌍 **Cross-Region Replication** | Copy objects to other regions (DR, compliance) |
| 🏠 **Same-Region Replication** | Copy within region (aggregation, account separation) |
| 🔔 **Event Notifications** | Trigger Lambda/SNS/SQS on uploads/deletes |
| 🔍 **S3 Select** | SQL queries on objects (reduce data transfer) |
| 📊 **S3 Inventory** | Audit reports of all objects & metadata |
| 📈 **Storage Lens** | Organization-wide analytics & recommendations |
| 🌐 **Static Website Hosting** | Host HTML sites (HTTP only – use CloudFront for HTTPS) |

---

## 🚀 Performance Optimization

- **Multipart Upload** 📤 – Required for >5 GB, recommended for >100 MB
- **Transfer Acceleration** ⚡ – Use CloudFront edge locations for uploads
- **Byte-Range Fetches** 📥 – Download partial files; parallelize requests
- **Prefix Scaling** 📂 – Virtually unlimited request rates per prefix

---

## 💰 Pricing Factors

- **Storage** 💾 – Per GB/month (varies by class)
- **Requests** 📞 – PUT/GET/LIST charges (per 1,000)
- **Retrieval** 📤 – Glacier classes charge per GB retrieved
- **Data Transfer** 🌐 – Inbound = Free | Outbound to internet = $$
- **Minimums** ⏱️ – IA: 30 days | Glacier: 90 days | Deep Archive: 180 days

---

## ✅ Best Practices

**Security** 🔒
- Enable versioning & MFA Delete
- Use least-privilege IAM policies
- Encrypt everything (prefer SSE-KMS)
- Enable Block Public Access at account level

**Cost** 💵
- Use Intelligent-Tiering for unpredictable access
- Lifecycle policies to auto-archive old data
- Clean up incomplete multipart uploads
- Compress before uploading

**Performance** 🏎️
- CloudFront for frequently accessed content
- Parallel uploads/downloads
- S3 Transfer Acceleration for long-distance

---

## 🚧 Limits & Constraints

| Limit | Value |
|-------|-------|
| Max Object Size | 5 TB |
| Single PUT Size | 5 GB (use multipart above) |
| Multipart Parts | 10,000 max |
| Bucket Name Length | 3–63 characters |
| Key Length | 1,024 bytes |
| Metadata Size | 2 KB |
| Buckets/Account | 100 (soft limit) |

---

## 🔌 Common Integrations

- **AWS Lambda** – Event-driven processing
- **Amazon Athena** – SQL queries on S3
- **CloudFront** – CDN & HTTPS
- **EMR/Glue** – Big data & ETL
- **Snowball** – Physical data transfer

---

## 🆘 Quick Troubleshooting

| Issue | Check |
|-------|-------|
| **403 Forbidden** | IAM policies, Bucket policies, Block Public Access, object existence |
| **Slow Performance** | Storage class (Glacier?), Transfer Acceleration enabled?, CloudFront? |
| **High Costs** | Storage class distribution, incomplete uploads, data transfer out |

---

## 📝 Summary

**Amazon S3** is the industry standard for cloud object storage, offering virtually unlimited scalability, military-grade durability (11 9's), and flexible storage classes to optimize costs from milliseconds to years. 

**Key Takeaways:**
- 🪣 **Universal Storage**: Store anything from static websites to data lakes, with automatic scaling from bytes to petabytes
- 💰 **Cost Optimized**: 8 storage classes let you balance access speed vs. cost, with Intelligent-Tiering automating the decisions
- 🔒 **Secure by Design**: Encryption at rest/transit, fine-grained IAM controls, Object Lock for compliance, and Block Public Access to prevent data leaks
- 🌍 **Global & Resilient**: Built-in cross-region replication, 99.99% availability, and multi-AZ durability protect against data loss and outages
- ⚡ **Performance**: Single-digit millisecond latency for hot data, with options like Transfer Acceleration and Express One Zone for extreme performance needs.

Whether you're hosting a static website, building a data lake, archiving compliance records, or serving machine learning models, S3 provides the foundation for modern cloud architectures with pay-as-you-go pricing and no upfront commitments

---

*Last Updated: 2024*  
*Author: SalvatoreOps*  
*License: Public Domain / Free to share*

---