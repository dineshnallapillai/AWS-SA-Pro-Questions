# AWS Solutions Architect (Professional)
## High-Yield Exam Takeaways – Set 2

---

## 1️⃣ Network Security, DDoS & Traffic Blocking

### Network ACL vs Security Group

**Security Groups:**
- Stateful
- ❌ Cannot explicitly deny
- Allow rules only

**Network ACL:**
- Stateless
- ✅ Can explicitly DENY IPs
- Best for blocking attacking IP ranges quickly

**📌 Exam rule:** "Block specific IPs immediately & cost-effectively" → NACL

### AWS Shield

**Shield Standard** – Free, automatic

**Shield Advanced:**
- Paid (~$3k/month)
- DDoS detection + CloudWatch alerts
- SLA + DRT support

**📌 Use Shield Advanced when:**
- SYN floods, UDP floods
- Need alerts + advanced mitigation

### AWS WAF

**Protects against:**
- SQL Injection
- XSS
- L7 attacks

**Works with:**
- ALB
- API Gateway
- CloudFront

❌ Cannot stop volumetric DDoS alone

**📌 SQL Injection** → WAF  
**📌 DDoS (L3/L4)** → Shield

---

## 2️⃣ SSL / TLS / Certificates (VERY EXAM-HEAVY)

### ACM Rules

**ALB certificates:**
- Must exist in same region as ALB

**CloudFront:**
- Certificates must be in us-east-1

**ACM private keys:**
- ❌ Cannot be exported
- Best for separation of duties

**📌 Exam trap:** "Use ACM cert in multiple regions on ALB" → ❌  
Must request per region

### Separation of Duties (DevOps vs Security)

**✔️ Correct pattern:**
- Store cert in ACM
- SSL termination at ELB
- IAM policy restricts cert access

**❌ Wrong:**
- Store cert in S3
- SCP to "grant access" (SCP never grants)

### CloudHSM

**Used when:**
- Private keys must never leave hardware
- Regulatory / government / banking
- Can offload SSL/TLS

**📌 CloudHSM ≠ ACM**
- CloudHSM = customer-managed keys
- ACM = managed certificates

---

## 3️⃣ Identity, Federation & Access Control

### Federated Access (Hybrid)

**To access AWS Console without IAM users:**

**✔️ Required:**
- On-prem SAML IdP (ADFS, Shibboleth)
- IAM SAML Provider
- IAM Role with trust to IdP
- STS federation

**📌 Exam shortcut:** "Corporate login → AWS Console" → SAML + IAM Role

### IAM Best Practices

- Never embed access keys
- **Use IAM Roles for:**
  - EC2
  - ECS
  - Lambda

**📌 EC2 → SQS / DynamoDB:** IAM Role + Instance Metadata

### SCP Gotcha (Very Common)

- SCP does NOT apply to service-linked roles
- **SCP:**
  - Filters permissions
  - Never grants
  - Explicit DENY always wins

**📌 ECS service-linked roles ignore SCPs**

---

## 4️⃣ Compute, Scaling & Load Balancing

### Auto Scaling + ELB

- Default ASG health check = EC2
- **To terminate unhealthy instances:**
  - ✅ Change ASG health check to ELB

**📌 Exam rule:** "ELB marks unhealthy but instance not replaced" → Change ASG health check type

### ECS Scaling

- ECS does NOT use ASG scaling directly
- **Use:**
  - Service Auto Scaling
  - CloudWatch alarms

**📌 ECS scalability** → Service Auto Scaling

### Route 53 Load Distribution (Exam nuance)

**✔️ BOTH are valid:**
- ALB + Alias record (Best practice)
- Multivalue Answer Routing (DNS-level LB)

**📌 Exam rule:** If question says "Choose 2", multivalue routing is VALID

---

## 5️⃣ Databases & Storage (OLTP vs Analytics)

### OLTP vs OLAP

| Workload | Service |
|----------|---------|
| OLTP | RDS, Aurora |
| OLAP | Redshift |
| Key-value | DynamoDB |
| Cache | ElastiCache |
| Search | OpenSearch |

**📌 Redshift ≠ OLTP**

### RDS Multi-AZ

**Failover behavior:**
- CNAME switch to standby
- No IP change
- Automatic

**📌 Exam answer:** "What happens on RDS failover?" → CNAME updated

### Redshift Workload Isolation

- Use WLM (Workload Management queues)
- Prevent long analytics from blocking short queries

---

## 6️⃣ S3 – Security, Versioning & Lifecycle

### Pre-Signed URLs

- Temporary access (minutes → days)
- **Best for:**
  - Sharing private objects
  - Time-bound access

**📌 Signed URLs** = CloudFront  
**📌 Pre-Signed URLs** = S3

### S3 Versioning Behavior

- **Objects before versioning:** VersionId = null
- **Objects after versioning:** Alphanumeric VersionId
- Updates create new versions

**📌 Very common exam scenario**

### S3 Encryption (SSE-S3)

- Each object encrypted with unique key
- Key encrypted with master key
- AES-256
- Keys rotated automatically

### S3 Lifecycle + Cost Optimization

- **Frequently accessed (1–2 months)** → S3 Standard
- **Rare access** → IA
- **Archive (hours–days wait)** → Glacier

**📌 24h retrieval acceptable** → Glacier

---

## 7️⃣ CloudFormation – Data Preservation

### DeletionPolicy

| Policy | Meaning |
|--------|---------|
| Retain | Resource kept |
| Snapshot | Backup then delete |
| Delete | Default |

**📌 S3** → Retain  
**📌 RDS** → Snapshot (cost-aware)

---

## 8️⃣ Monitoring, Compliance & Automation

### AWS Config

**Tracks:**
- Resource changes
- Compliance
- WAF rule changes

**Can trigger:**
- EventBridge
- Lambda remediation

**📌 Config ≠ blocking**  
**📌 Config** = detection + audit

### Patch Management

**Systems Manager Patch Manager**

**Use:**
- Patch baselines
- Maintenance Windows

**📌 Least effort** → Patch Manager + Maintenance Window

---

## 9️⃣ Caching & Performance

### Caching Strategy

**CloudFront:**
- Static content
- Global edge caching

**ElastiCache:**
- Redis / Memcached
- In-memory app data

**📌 DynamoDB ≠ cache**  
**📌 ElastiCache ≠ CDN**

---

## 🔟 Messaging & Decoupling

### High-Traffic Checkout / Burst Systems

**Use:**
- ALB + ASG
- SQS for buffering
- DynamoDB backend

**📌 SQS absorbs spikes**

---

## 🔑 FINAL EXAM MINDSET (SET 2)

- **Prefer managed services**
- **Separate security, compute, data**
- **DNS solutions can be valid (exam!)**
- **SCP never grants permissions**
- **WAF ≠ DDoS**
- **CloudFront ≠ Global Accelerator for S3**
- **Service-linked roles bypass SCPs**
