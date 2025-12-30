# AWS SOLUTIONS ARCHITECT – SET 2
## 1-PAGE ULTRA-DENSE CHEAT SHEET

---

## 🔐 NETWORK SECURITY & DDoS

### Security Group
* Stateful
* ❌ No explicit DENY

### Network ACL
* Stateless
* ✅ Explicit DENY
* 👉 Fastest, cheapest way to block IPs

### AWS Shield
* **Standard:** Free, auto
* **Advanced:** Paid, alerts + DRT + SLA
* 👉 L3/L4 DDoS (SYN/UDP floods)

### AWS WAF
* SQL Injection, XSS (L7)
* Works with ALB / API Gateway / CloudFront
* ❌ Not for volumetric DDoS alone

---

## 🔑 SSL / TLS / CERTIFICATES

### ACM
* ALB cert must be in **SAME REGION**
* CloudFront cert must be in **us-east-1**
* Private key ❌ NOT exportable

### Separation of Duties
* Store cert in ACM
* Terminate SSL at ELB
* IAM policy → only security team

### CloudHSM
* Hardware-based keys
* Regulated environments
* SSL offload without exposing keys

---

## 👤 IDENTITY & ACCESS (FEDERATION)

### Federated Console Access
✔️ On-prem AD + SAML IdP  
✔️ IAM SAML Provider  
✔️ IAM Role (trusts IdP)  
✔️ STS  
❌ No IAM users needed

### IAM Roles
* EC2 / ECS / Lambda → **ALWAYS use roles**
* ❌ Never store access keys

### SCP
* ❌ Does NOT affect service-linked roles
* ❌ Does NOT grant permissions
* Explicit DENY always wins

---

## ⚖️ LOAD BALANCING & SCALING

### ASG Health Checks
* Default = EC2 only
* ✅ Use ELB health check to auto-replace bad instances

### ECS Scaling
* Use **Service Auto Scaling**
* ❌ ASG scaling ≠ ECS scaling

### Route 53
* ALB + Alias Record (best)
* Multivalue Answer Routing also valid (DNS-level LB)

---

## 🗄️ DATABASES & WORKLOADS

### OLTP
* RDS / Aurora

### OLAP
* Redshift

### RDS Multi-AZ Failover
* Automatic
* CNAME switch
* ❌ No IP change

### Redshift
* Use WLM queues to isolate workloads

---

## 📦 S3 – VERSIONING, SECURITY, COST

### Versioning
* Objects **BEFORE** enabling → VersionId = `null`
* Objects **AFTER** enabling → alphanumeric
* Updates create new versions

### Encryption (SSE-S3)
* Unique key per object
* Key encrypted with rotating master key
* AES-256

### Temporary Access
* **Pre-Signed URL** → S3
* **Signed URL** → CloudFront

### Lifecycle
* **Hot (1–2 months):** S3 Standard
* **Warm:** IA
* **Cold (24h wait OK):** Glacier

---

## 🧱 CLOUDFORMATION

### DeletionPolicy
* `Retain` → S3
* `Snapshot` → RDS
* `Delete` → default

📌 Never snapshot S3 (unsupported)

---

## 🛠️ MONITORING & AUTOMATION

### AWS Config
* Tracks config drift
* Compliance auditing
* Can trigger Lambda remediation
* Tracks WAF rule changes

### Patching
* Systems Manager Patch Manager
* Maintenance Windows
* Least effort solution

---

## ⚡ PERFORMANCE & CACHING

### Static Content
* CloudFront

### In-Memory Cache
* ElastiCache (Redis / Memcached)

❌ DynamoDB ≠ cache  
❌ ElastiCache ≠ CDN

---

## 📬 MESSAGING & BURST TRAFFIC

### High Traffic / Spikes
* ALB + ASG
* SQS for buffering
* DynamoDB backend

📌 SQS absorbs sudden load

---

## 🧠 EXAM ELIMINATION RULES

* **Need DENY** → NACL
* **SQL Injection** → WAF
* **DDoS** → Shield
* **Hybrid SSO** → SAML + IAM Role
* **ECS scaling** → Service Auto Scaling
* **OLTP** → RDS
* **OLAP** → Redshift
* **24h retrieval OK** → Glacier
* **Cert isolation** → ACM + ELB
* **Config tracking** → AWS Config
* **EC2 creds** → IAM Role
