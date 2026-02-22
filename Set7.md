# AWS SOLUTIONS ARCHITECT – MASTER REVISION NOTES

## 1️⃣ Multi-Account & AWS Organizations Architecture

### 🔹 Transit Gateway in Multi-Account Setup

- Create TGW in networking/management account
- Share using **AWS RAM** (NOT SCP)
- Use CloudFormation StackSets to:
  - Create VPC in new accounts
  - Create TGW attachment
  - Associate attachment
- Automate onboarding for new accounts

#### 🚨 Exam Traps
- SCP does NOT share resources
- TGW VPC attachment ≠ TGW peering attachment
- Manual attachment = not scalable

---

### 🔹 Cost Allocation & Chargeback

- Tag resources (e.g., `costCenter=compliance`)
- Activate tag in management account billing console
- Use Cost and Usage Report (CUR) for org-wide breakdown
- Store CUR in S3
- No Lambda needed for basic tag cost breakdown

#### 🚨 Exam Traps
- Activating tags only in member accounts → wrong
- Trusted Advisor ≠ cost allocation tool

---

## 2️⃣ Hybrid Networking & Direct Connect

### 🔹 Direct Connect + Internet Access

**Correct VPC Route Table Design:**

| Destination | Target |
|-------------|--------|
| 0.0.0.0/0 | Internet Gateway |
| On-prem CIDRs | VGW (DX) |

**Do NOT:**
- Advertise default route from on-prem unless full tunnel intended
- Use dual default routes
- Associate two route tables to same subnet

---

### 🔹 Public vs Private VIF

| VIF Type | Use |
|----------|-----|
| Public VIF | S3, DynamoDB public endpoints |
| Private VIF | VPC private IP access |
| Transit VIF | Transit Gateway |

**If accessing S3 via Direct Connect:**
- Use Public VIF
- Advertise specific routes
- Avoid default route advertisement

---

## 3️⃣ High Availability (HA) & Load Balancing

### 🔹 Web App HA Pattern

```
Route 53 → ELB (Multi-AZ) → Auto Scaling EC2
```

**Best practice:**
- Multi-AZ deployment
- Health checks
- DNS alias to ELB

**Alternative:**
- Route 53 multi-value answer + EIP + health checks (less optimal)

#### 🚨 Traps
- NAT instance ≠ public web entry point
- CloudFront ≠ load balancer replacement
- Single AZ ≠ HA

---

### 🔹 Cloud Bursting (Hybrid Scaling)

**When on-prem overloaded:**

```
Route 53
   ↓
  ELB
   ↓            ↓
On-Prem     EC2 (Auto Scaling)
```

Used for traffic spikes without full migration.

---

## 4️⃣ Compute Scaling & Cost Optimization

### 🔹 Mixed SLA Workloads Strategy

**Workload:**
- 65% SLA-critical
- 35% non-SLA

**Correct pattern:**
- 3 AZ distribution
- On-Demand with Capacity Reservations for SLA
- Spot for non-critical

**Why Capacity Reservations?**
- Guarantees AZ capacity
- Savings Plans do NOT guarantee capacity

---

### 🔹 Spot vs On-Demand vs Savings Plan

| Model | Use |
|-------|-----|
| Spot | Interruptible workloads |
| Savings Plan | Predictable baseline |
| Capacity Reservation | Strict SLA capacity guarantee |

---

## 5️⃣ Storage & EBS Performance

### 🔹 RAID 0 + EBS Bottleneck

**Symptom:**
- Added volumes
- No IOPS increase
- CPU increased

**Cause:**
- EC2 instance EBS bandwidth limit reached

**Solution:**
- Upgrade instance type

**Bottleneck Hierarchy:**
1. Application
2. OS
3. RAID
4. **EC2 EBS bandwidth** (most common)
5. Volume limit

---

### 🔹 Ephemeral vs EBS vs In-Memory

**If:**
- Subset processing
- No durability needed
- Cost sensitive

**Choose:**
- Ephemeral (instance store)

**Long-term storage:**
- S3

---

## 6️⃣ Data Architecture & Analytics

### 🔹 Data Lake Pattern

```
S3 → Processing (EC2/EMR/Lambda) → Redshift/Analytics
```

**S3:**
- Long-term storage
- Encryption
- Cost-effective

---

### 🔹 Kinesis Streaming Pattern

```
IoT Devices → Kinesis → Processing → Redshift
```

**Use when:**
- High-frequency data
- Near real-time analytics

**Do NOT use:**
- S3 daily batch for streaming
- SQS for analytics

---

### 🔹 Glue vs Redshift vs Athena

| Service | Role |
|---------|------|
| Glue | ETL |
| Redshift | Data warehouse |
| Athena | Query S3 |
| RDS | OLTP |

**If question says:**
- Transform raw data → Glue
- BI warehouse → Redshift

---

## 7️⃣ DynamoDB Optimization

### 🔹 Read Latency Issue

**Solution:**
- Enable DynamoDB Accelerator (DAX)

**DAX:**
- In-memory cache
- Microsecond latency
- Reduces read pressure

**Do NOT:**
- Increase EC2
- Change storage type (invalid)
- Blindly increase RCU if latency issue

---

## 8️⃣ Serverless Architecture Patterns

### 🔹 Lambda Canary Deployment

**Steps:**
1. Publish version
2. Create alias
3. Use `update-alias` with `routing-config`

**Never:**
- Use Route 53 weighted routing
- Use `update-function-configuration`

---

### 🔹 API Gateway + DynamoDB

**Two valid serverless patterns:**
1. REST API → Direct DynamoDB integration
2. HTTP API → Lambda → DynamoDB

Use REST API for direct AWS integration.

---

### 🔹 Redirect Service

**Best minimal solution:**

```
Route 53 → API Gateway → Lambda → 301 redirect
```

**Use:**
- ACM SAN certificate
- Custom domain

**Avoid:**
- EC2
- ALB
- CloudFront unless edge logic required

---

## 9️⃣ Security & Secrets

### 🔹 RDS Credential Storage

**Use:**
- AWS Secrets Manager

**CloudFormation resources:**
- Secret
- RotationSchedule
- Lambda rotation

**Never:**
- Parameter Store for rotation
- EventBridge custom scheduler

---

### 🔹 MAC Address Persistence

**Use:**
- Elastic Network Interface (ENI)
- MAC tied to ENI, not instance

---

### 🔹 AWS API Request Signing

**Uses:**
- HMAC-based signature (SigV4)

**Not:**
- SSL
- AES
- RSA

---

## 🔟 Disaster Recovery (RPO/RTO)

**Given:**
- RPO = 15 minutes
- RTO < 3 hours

**Correct approach:**
- Hourly DB backup
- 5-minute transaction log backups

**Not:**
- Glacier (too slow)
- Tape backups
- Synchronous on-prem replication

---

## 1️⃣1️⃣ Elastic Beanstalk + RDS Best Practice

- Create RDS separately
- Use DNS endpoint
- Use SG referencing (App SG → DB SG)
- **Never embed RDS in EB stack for prod**

---

## 1️⃣2️⃣ Transit Gateway & RAM

- Share TGW via RAM
- Automate VPC + attachment via StackSets
- Do not use SCP
- Do not confuse VPC attachment with TGW peering

---

## 1️⃣3️⃣ Glacier Vault Model

**Legacy Glacier supports notifications for:**
- Inventory Retrieval Complete
- Archive Retrieval Complete
- **No upload notifications**

---

## 1️⃣4️⃣ Core AWS Differentiators

- Elasticity
- Global infrastructure
- Pay-as-you-go pricing
- Broad service portfolio
- Security-first design

**Avoid marketing words like:**
- Pioneering
- Dynamic
- Trustworthy

---

## 🔥 CRITICAL EXAM PATTERNS TO MEMORIZE

| If question says → | Think: |
|-------------------|--------|
| Multi-account networking | TGW + RAM + StackSets |
| SLA under failure | Capacity Reservations |
| Read-heavy DynamoDB | DAX |
| Canary Lambda | Alias + routing-config |
| Hybrid routing | IGW default + specific DX routes |
| Cost allocation | Activate tag in management + CUR |
| Secret rotation | Secrets Manager + RotationSchedule |
| Storage scaling no IOPS gain | Instance bandwidth bottleneck |
| Long-term cheap storage | S3 |
| Temporary processing | Ephemeral |
| Serverless API | API Gateway + Lambda |
| Streaming IoT | Kinesis |
