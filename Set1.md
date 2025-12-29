# 🔑 AWS Solutions Architect (Pro-level)
## High-Yield Exam Takeaways – Set 1

---

## 1️⃣ Networking & Hybrid Connectivity

### 🔹 VPC Peering
- Private IP communication between VPCs
- No transitive routing
- Requires route table updates
- Best for simple, point-to-point VPC connectivity
- Not scalable for hub-and-spoke

**📌 Exam trigger:** "central VPC, multiple VPCs" → Transit Gateway, not peering

### 🔹 Direct Connect
- **Public VIF** → S3, DynamoDB, public AWS endpoints
- **Private VIF** → VPCs via VGW / DX Gateway
- **DX + VPN** = most cost-effective redundancy
- **Second DX** = best performance, NOT cost-effective

**📌 Exam trigger:**
- "Reduce latency to S3 from on-prem" → Public VIF
- "Cost-effective redundancy" → DX + VPN

### 🔹 BYOIP
- Used when external parties whitelist your IPs
- Bring your existing public IP range into AWS
- Create EIPs from your own IP pool
- Requires ROA + authorization

**📌 Exam trigger:** "Vendors won't change IP whitelist" → BYOIP

---

## 2️⃣ Security, IAM & Governance

### 🔹 IAM Cross-Account Access
- **Resource-based policies** (S3, KMS, etc.)
  - User stays in own account
  - No role switching required
- **Roles** when temporary access is acceptable

**📌 Exam trigger:** "User should not give up existing permissions" → Resource-based policy

### 🔹 AWS Organizations & SCPs
- SCPs restrict services at account / OU level
- SCPs apply to root + all IAM identities
- IAM policies cannot restrict root

**📌 Exam trigger:** "Centrally allow/deny services across accounts" → SCP

### 🔹 CloudTrail & Auditing
- Enable Global Service Events
- Use new, dedicated S3 bucket
- Protect logs with:
  - IAM policies
  - Bucket policies
  - MFA Delete

**📌 Exam trigger:** "Track IAM / API changes across regions" → CloudTrail (global)

### 🔹 DDoS & Threat Response
- **Immediate IP blocking** → Network ACL
- **L3/L4 DDoS** → AWS Shield Advanced
- **L7 (HTTP)** → AWS WAF
- **GuardDuty** = detection, not prevention

**📌 Exam trigger:** "Port scanning, network attacks" → NACL + Shield Advanced

---

## 3️⃣ Compute, Scaling & Reliability

### 🔹 Reliability Pillar
- Horizontal scaling > vertical
- Multi-AZ is mandatory
- Multi-Region via Route 53 (Weighted / Failover)
- Avoid single points of failure

**📌 Exam trigger:** "Recover from disruptions" → Auto Scaling + Multi-AZ

### 🔹 Spot Instances
- Good for fault-tolerant / batch workloads
- **NOT suitable alone for:**
  - EMR masters
  - Databases
  - Guaranteed performance

**📌 Exam rule:** EMR → Spot + On-Demand / Reserved mix

---

## 4️⃣ Storage (VERY IMPORTANT)

### 🔹 Amazon S3
- Object storage
- Durable, scalable
- **NOT ideal for:**
  - Rapidly changing shared data
  - POSIX file semantics

### 🔹 Amazon EFS
- Shared POSIX file system
- Multi-AZ by default
- Handles thousands of Linux servers
- Supports file locking & rapid updates

**📌 Exam trigger:** "1000+ Linux servers, shared data" → EFS

### 🔹 Storage Gateway
- Hybrid access
- **NOT ideal in DR recovery path**
- Best RTO = EBS snapshots + direct attach

**📌 Exam trigger:** "Best RTO" → Avoid Gateway in recovery

---

## 5️⃣ Databases & Analytics

### 🔹 RDS Multi-AZ
- Synchronous replication
- Standby is NOT readable
- For availability, NOT scaling

### 🔹 Read Replicas
- Asynchronous
- Used for read scaling & reporting
- Can be promoted

**📌 Exam trigger:** "Reporting without impacting prod DB" → Read Replica

### 🔹 ElastiCache
- Performance accelerator
- **NOT a system of record**
- **NOT reliable for analytics**

**📌 Exam trigger:** "Analytics / reporting" → Not ElastiCache

### 🔹 DynamoDB Global Tables
- Multi-region, multi-master
- Automatic replication
- Ideal for global mobile apps

**📌 Exam trigger:** "Writes in multiple regions" → Global Tables

### 🔹 Redshift
- OLAP analytics
- Not for transactional workloads
- Used with S3 & batch ingestion

---

## 6️⃣ Streaming, IoT & Real-Time Data

### 🔹 Amazon Kinesis Data Streams
- Real-time ingestion
- Millions of events/sec
- **Used for:**
  - IoT
  - Clickstreams
  - Real-time dashboards

**📌 Exam trigger:** "Every second", "real-time analytics" → Kinesis

---

## 7️⃣ Migration & Disaster Recovery

### 🔹 Database Migration
**Heterogeneous (Oracle → PostgreSQL):**
- **SCT** → schema & code
- **DMS** → data

**📌 Exam trigger:** "Transform schema" → SCT + DMS

### 🔹 Data Transfer
- **< 10 TB** → Internet
- **10–100 TB** → Snowball
- **PB-scale** → Multiple Snowballs
- **Snowmobile** → 10+ PB, long lead time

**📌 Exam trigger:** "Urgent PB migration" → Snowball, not Snowmobile

### 🔹 DR Strategy (RTO)
**Best RTO = restore from:**
- S3
- EBS snapshots

**Avoid:**
- Glacier
- Storage Gateway
- Tape (VTL)

---

## 8️⃣ Systems Manager (Often Confused)

| Service | Purpose |
|---------|---------|
| State Manager | Bootstrap & enforce config |
| Patch Manager | OS patching |
| Session Manager | Secure shell access |
| Run Command | One-time commands |

**📌 Exam trigger:** "Install software at startup" → State Manager

---

## 9️⃣ SSL, Encryption & Compliance

### 🔹 CloudHSM
- Keys never leave hardware
- **Required for:**
  - Government
  - Financial institutions
- Deploy in multiple AZs

### 🔹 S3 Encryption
- **SSE-S3** → AWS managed ❌ (gov use)
- **SSE-KMS** → AWS managed ❌
- **SSE-C** → customer-managed ✅

**📌 Exam trigger:** "Only authorized users can decrypt" → SSE-C

---

## 🧠 FINAL EXAM MINDSET

AWS exams are pattern recognition, not memorization.

**If you identify:**
- Scale + real-time → **Kinesis**
- Shared Linux storage → **EFS**
- Global writes → **DynamoDB Global Tables**
- Schema conversion → **SCT**
- Best RTO → **S3 + EBS**
- Cost-effective redundancy → **DX + VPN**
- Government key control → **CloudHSM**

**👉 the answer becomes obvious.**
