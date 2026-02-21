# AWS Security & Governance - Exam Guide

## 🔐 1️⃣ Security & Governance

### ✅ AWS Organizations

**Entity:** AWS Organizations

**Key Points:**
- Provides consolidated billing
- Centralized account management
- Uses SCPs (Service Control Policies)
- Management (payer) account + member accounts

**SCP Rules:**
- SCP sets maximum allowed permissions
- SCP does NOT grant permissions
- IAM policy must still explicitly allow action
- Explicit Deny always wins

---

### ✅ S3 Public Object Auto Remediation

**Pattern:**
1. Enable CloudTrail object-level logging
2. CloudWatch Events rule
3. SNS notification
4. Lambda to remediate (make object private)

**Correct logic:** Event-driven remediation, not scheduled scanning.

---

### ✅ Single Sign-On (SSO) / Federation

**Entity:** AWS Security Token Service

**Correct pattern:**
1. SAML-based IdP (ADFS)
2. IAM Role with trusted IdP
3. STS issues temporary credentials

**Never:**
- Create IAM users per employee
- Store long-term credentials in apps

---

## 🌐 2️⃣ Networking & Hybrid

### ✅ Direct Connect Gateway (Multi-Region)

**Entity:** AWS Direct Connect Gateway

**Use when:**
- On-prem → multiple VPCs
- Multiple regions
- Low latency + high bandwidth
- Cost-effective

**Pattern:** DX → DX Gateway → Private VIF → VGWs

---

### ✅ VPN for Secure Replication

**Entity:** AWS Site-to-Site VPN

**For:**
- Secure on-prem DB replication
- Hybrid secure channel

**Never replicate over open Internet.**

---

### ✅ HPC Placement Groups

**Entity:** Amazon EC2

**Cluster Placement Group:**
- Low latency
- High throughput
- All instances must be launched into placement group
- Cannot move existing instance → must relaunch

---

## 🖥 3️⃣ Compute & Auto Scaling

### ✅ CPU-based Scale-In

**Entity:** Amazon CloudWatch, Amazon EC2 Auto Scaling

**Pattern:**
- CloudWatch alarm (CPU ≤ 15%)
- Scale-in policy (-1 instance)

**Never use:**
- Scheduled scaling for metric-based triggers
- Manual SNS alerts

---

### ✅ Spot Fleet (Diversified Strategy)

**Entity:** Amazon EC2 Spot Fleet

**Use when:**
- Peak load optimization
- MOST cost-effective required
- Base load covered by Reserved
- Multi-AZ resilience needed

**Exam rule:**
- Base load → Reserved
- Peak load → Spot (diversified)

---

## 🗄 4️⃣ Storage & CDN

### ✅ S3 for Media Storage

**Entity:** Amazon S3

**Use S3 when:**
- Media files stored on EC2
- Replication job failing
- Need durability + scalability
- Remove instance dependency

**Never:**
- Store media permanently on EC2
- Use EFS unless POSIX required

---

### ✅ CloudFront

**Entity:** Amazon CloudFront

**Use when:**
- Static asset caching
- Global low latency
- Mobile apps slow image loading

**Correct pairing:** S3 → CloudFront

---

### ✅ Prevent S3 Direct Access

**Use:**
- Origin Access Identity (OAI)
- Remove public bucket access
- Only CloudFront can read

---

## 🗃 5️⃣ Databases

### 🔥 VERY IMPORTANT DISTINCTIONS

### ✅ RDS Multi-AZ vs Read Replica

**Entity:** Amazon RDS

| Feature | Multi-AZ | Read Replica |
|---------|----------|--------------|
| Purpose | HA | Read Scaling |
| Replication | Synchronous | Asynchronous |
| Failover | Automatic | Manual |
| Endpoint | Same | Different |

**Exam Rule:**
- High Availability → Multi-AZ
- Read-heavy workload → Read Replicas

---

### ✅ High Read Workload Fix

**Correct combination:**
- Launch Read Replicas
- Use Provisioned IOPS (if I/O bottleneck)

**Never:**
- Shard unless extreme scale
- Use OpenSearch for caching DB

---

### ✅ DynamoDB Scaling

**Entity:** Amazon DynamoDB

**To reduce load:**
- Use SQS buffer for burst writes
- Not RDS replacement
- Not launch multiple DBs manually

---

## 🧠 6️⃣ Caching

### ✅ Memcached vs Redis

**Entity:** Amazon ElastiCache for Memcached

**Use Memcached when:**
- Multithreaded
- Distributed
- Auto-discovery required
- Simple key/value
- Cost-effective

**Redis:**
- Single-threaded
- Advanced features
- More expensive

---

## ⚙️ 7️⃣ Serverless & Event-Driven

### ✅ SQS for Burst Handling

**Entity:** Amazon SQS

**Use when:**
- Millions of users
- Submission spikes
- Prevent DB throttling

**Pattern:** API → SQS → Lambda → DB

---

### ✅ SNS for Mobile Push

**Entity:** Amazon SNS

- SNS: Mobile push
- Not SES (email)

---

## 📊 8️⃣ Analytics & Big Data

### ✅ IoT + ETL + Data Warehouse Pattern

**Entity:** Amazon EMR, Amazon Redshift

**Correct pattern:**
```
IoT → S3 (Data Lake)
    → EMR (ETL)
    → Redshift (Warehouse)
    → S3 lifecycle → Glacier
```

**Never:**
- Use Athena for ingestion
- Use DynamoDB as data lake

---

## 🛠 9️⃣ Systems Manager & Recovery

### ✅ EC2 Auto Recovery

**Entity:** AWS Systems Manager

**Use:**
- EC2Rescue
- AWSSupport-ExecuteEC2Rescue
- Systems Manager Automation

**Never:**
- Build custom Lambda recovery
- Use OpsWorks

---

## 💰 1️⃣0️⃣ Cost Optimization Master Rules

| Scenario | Best Practice |
|----------|---------------|
| Steady load | Reserved |
| Peak load | Spot |
| Burst traffic | SQS |
| Media files | S3 |
| Static global content | CloudFront |
| Read-heavy DB | Read Replica |
| HA DB | Multi-AZ |
| On-prem multi-region | DX Gateway |
| Secure hybrid | VPN |
| Simple distributed cache | Memcached |

---

## 🎯 1️⃣1️⃣ CRITICAL EXAM TRAPS

**These came up multiple times:**

### ❌ Trap 1: Read Replica ≠ High Availability
- Multi-AZ = HA
- Read Replica = Performance

### ❌ Trap 2: SCP Grants Permissions
- No. SCP sets boundary only.

### ❌ Trap 3: Elastic IP Improves Performance
- No. It only provides static IP.

### ❌ Trap 4: CloudFront Caches Databases
- No. Only HTTP content.

### ❌ Trap 5: Use RDS for Massive Mobile Write Scaling
- DynamoDB + SQS is better.

### ❌ Trap 6: Overengineering
**Exam prefers:**
- Native managed solution
- Simpler architecture
- Least moving parts

---

## 🏆 FINAL ARCHITECT THINKING FRAMEWORK

**For every question, ask:**

1. Is this HA or performance?
2. Is this read-heavy or write-heavy?
3. Is this burst traffic?
4. Is storage instance-bound?
5. Is the solution managed and cost-effective?
6. Are we overengineering?
