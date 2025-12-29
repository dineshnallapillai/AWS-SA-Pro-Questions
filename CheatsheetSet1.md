# 🧠 AWS Solutions Architect (Professional)
## 1-Page High-Yield Exam Cheat Sheet – Set 1

---

## 🔐 SECURITY & IAM

### IAM & Cross-Account
* **Resource-based policy** → user keeps own permissions (S3, KMS)
* **IAM Role** → temporary access, assume role
* Least privilege always

### AWS Organizations
* **SCP** = allow/deny AWS services at account/OU level
* SCP applies to root + all IAM users
* IAM policies cannot restrict root

### CloudTrail
* Enable Global Service Events
* Use new dedicated S3 bucket
* Protect logs with:
  * IAM + bucket policies
  * MFA Delete

### Threats & DDoS
* **Immediate IP block** → Network ACL
* **L3/L4 DDoS** → AWS Shield Advanced
* **L7 (HTTP)** → AWS WAF
* **GuardDuty** = detect only, not block

---

## 🌐 NETWORKING & HYBRID

### VPC Peering
* Private IP routing
* ❌ No transitive routing
* Manual route tables
* Not scalable hub-and-spoke

### Direct Connect
* **Public VIF** → S3, DynamoDB, public AWS services
* **Private VIF** → VPCs
* **DX + VPN** = most cost-effective redundancy
* **Second DX** = best performance, not cost-effective

### BYOIP
* Use when vendors whitelist your IPs
* Bring existing public IP range into AWS
* Requires ROA + authorization

---

## 🗄️ STORAGE

### Amazon S3
* Object storage
* Durable & scalable
* ❌ Not for rapidly changing shared data

### Amazon EFS
* Shared POSIX file system
* Thousands of Linux EC2 instances
* Multi-AZ by default
* File locking + strong consistency
* Minimal management

### Disaster Recovery (RTO)
* **Best RTO** → Restore from S3 + EBS snapshots
* ❌ Avoid Glacier, Tape, Storage Gateway in DR path

---

## 🖥️ COMPUTE & RELIABILITY

### Reliability Pillar
* Horizontal scaling > vertical
* Multi-AZ mandatory
* Multi-Region via Route 53 (Weighted / Failover)

### EC2 Purchasing
* **Spot** → fault-tolerant workloads only
* **EMR** → Spot + On-Demand/Reserved mix
* Never Spot-only for critical components

---

## 🗃️ DATABASES & ANALYTICS

### RDS
* **Multi-AZ** = synchronous replication
* Standby NOT readable
* For availability, not scaling

### Read Replicas
* Asynchronous
* Read scaling & reporting
* Can be promoted

### ElastiCache
* Performance accelerator
* ❌ Not system of record
* ❌ Not reliable analytics source

### DynamoDB Global Tables
* Multi-region, multi-master
* Automatic replication
* Ideal for global mobile apps

### Redshift
* OLAP analytics only
* Not transactional
* Works with S3 ingestion

---

## 📡 STREAMING & IOT

### Amazon Kinesis Data Streams
* Real-time ingestion & analytics
* Millions of events/sec
* IoT, clickstreams, live dashboards

**Exam trigger:** "every second", "real-time", "streaming" → Kinesis

---

## 🔁 MIGRATION

### Database Migration
**Heterogeneous (Oracle → PostgreSQL):**
1. **SCT** → schema & code
2. **DMS** → data
* DMS does not convert schema

### Large Data Transfer
* **<10 TB** → Internet
* **10–100 TB** → Snowball
* **PB-scale (urgent)** → Multiple Snowballs
* **Snowmobile** → 10+ PB, long lead time

---

## 🛠️ SYSTEMS MANAGER

| Service | Purpose |
|---------|---------|
| State Manager | Bootstrap & enforce config |
| Patch Manager | OS patching |
| Session Manager | Secure access |
| Run Command | One-time commands |

**Exam trigger:** "Install software at startup" → State Manager

---

## 🔑 ENCRYPTION & COMPLIANCE

### SSL / Keys
* **CloudHSM** → keys never leave hardware
* Deploy in multiple AZs

### S3 Encryption
* **SSE-S3** → AWS-managed ❌ (gov/finance)
* **SSE-KMS** → AWS-managed ❌
* **SSE-C** → customer-managed ✅

---

## 🧠 FINAL EXAM RULES (MEMORIZE)

* **Shared Linux storage (1000+)** → EFS
* **Real-time data** → Kinesis
* **Global writes** → DynamoDB Global Tables
* **Schema conversion** → SCT
* **Best RTO** → S3 + EBS
* **Cost-effective DX redundancy** → DX + VPN
* **Gov-grade key control** → CloudHSM
