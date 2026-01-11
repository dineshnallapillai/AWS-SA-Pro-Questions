# AWS SOLUTIONS ARCHITECT – SET 4
## ULTRA-DENSE 1-PAGE EXAM SHEET

---

## 🔹 Messaging & Integration
- ActiveMQ / MQTT / JMS / NMS → **Amazon MQ**
- SNS/SQS ≠ broker replacement
- Minimal change + legacy messaging → **Amazon MQ**

---

## 🔹 Web Security (DDoS, SQLi, Bots)
- **AWS WAF** → SQL injection, XSS, bad bots (L7)
- **AWS Shield Advanced** → SYN/UDP floods (L3/L4), alerts, DRT
- CloudFront alone ≠ sufficient
- NACL IP blocking = reactive, not scalable

**Exam combo:** Shield Advanced + WAF

---

## 🔹 S3 Security & Data Protection
- **Prevent public objects:** S3 Block Public Access
- **PII discovery:** Amazon Macie
- **Track object access:** CloudTrail (GET Object)

❌ Inspector ≠ PII  
❌ GuardDuty ≠ data classification

---

## 🔹 EC2 Governance (AMI Control)
- Post-deployment detection → **AWS Config Rule + Lambda remediation**
- IAM AMI restrictions = blocks CI/CD (wrong)

---

## 🔹 Private Subnet → Internet (PCI / Payments)
- **NAT Gateway + Elastic IP**
  - HA, scalable, managed
- NAT Instance only if explicitly cost-driven

---

## 🔹 CloudFront + HTTPS
- **No custom domain:** Default CloudFront cert + HTTPS only
- **Enforce HTTPS:**
  - Redirect HTTP → HTTPS
  - HTTPS only
- **EC2 origin:** must use trusted CA (not ACM, not self-signed)

---

## 🔹 Kinesis Data Durability
- Prevent loss → **Kinesis Firehose → S3**
- Or Kinesis S3 Connector

❌ EBS backups  
❌ Cross-AZ Kinesis replication

---

## 🔹 Security & Compliance
- **AWS Config** → continuous compliance
- **Amazon Inspector** → EC2 vulnerability scans
- **Best pair** → Config + Inspector

---

## 🔹 Zero-Downtime Deployments
- Blue/Green + CodeDeploy
- Elastic Beanstalk Immutable

❌ Rolling / All-at-once (capacity drop)

---

## 🔹 IAM – Least Privilege
- Developers → **PowerUserAccess**

❌ AdministratorAccess (violates least privilege)

---

## 🔹 API Gateway Errors
- **HTTP 504** → Lambda runtime > 29 seconds

❌ Throttling  
❌ IAM auth issue

---

## 🔹 S3 Cross-Domain Access
- **CORS configuration**
- Bucket policy ≠ CORS

---

## 🔹 Elastic Beanstalk
❌ Swap Environment URLs (NOT a deployment policy)

**Valid:** Rolling, Rolling + batch, Immutable

---

## 🔹 Hybrid Connectivity
- Private on-prem dependency → **Direct Connect**
- Multi-VPC → **Transit VPC**

---

## 🔹 IPSec VPN Guarantees

### Provides:
- ✅ Encryption
- ✅ Data integrity
- ✅ Peer authentication

❌ End-to-end user identity authentication

---

## 🔹 Storage Gateway DR
- **Gateway-Stored Volume**
- Snapshot → EBS
- Attach to EC2 during DR

---

## 🔹 Encryption at Rest (EBS)

### Valid:
- OS-level encryption
- Third-party tools
- Encrypt before write

❌ SSL/TLS (in-transit only)  
❌ "Encrypted by default"

---

## 🔹 Lambda Deployments & Tracing
- 10% → 90% traffic → **Canary deployment**
- Tracing → **AWS X-Ray**

---

## 🔹 Redshift DR
- Cross-Region Snapshot Copy
- Snapshot Copy Grant (KMS)

---

## 🔹 RTO / RPO
- Disaster @ 7:00 AM
- RTO = 3 hrs → Restore by 10:00 AM

---

## 🔹 Observability
- **X-Ray** → request tracing
- **CloudWatch Logs** → logs only
- **CloudTrail** → API auditing

---

## 🔹 VPN High Availability
- Customer Gateway = SPOF
- **Fix** → Second CGW in another data center

---

## 🔹 Scaling (Cost-Effective HA)
- **Reserved** → baseline
- **On-Demand + Spot** → spikes
- Spread across AZs

---

## 🔹 CloudFormation Data Safety
- **DeletionPolicy: Retain** (S3)

---

## 🔹 Redis Durability
- Multi-AZ + Automatic Failover
- AOF alone ≠ HA

---

## 🔹 Serverless Orchestration
- **AWS Step Functions**

❌ SWF (heavy)  
❌ Lambda alone

---

## 🔹 Workspaces (VDI)
- Directory Service + VPN + WorkSpaces

---

## 🔹 Reserved Instances (Org Billing)

### Rules:
- Same instance type
- Same region
- Same AZ (zonal RI)

→ Only matching accounts benefit

---

## 🔹 Database HA
- RDS Multi-AZ + Read Replicas
- Read replicas ≠ HA (scaling only)

---

## 🔹 IAM Best Practice (Golden Rule)

**Never embed access keys**  
**Always use IAM Roles**

---

## 🧠 FINAL EXAM PATTERNS

- **Detect ≠ Prevent**
- **Config = after launch**
- **WAF = L7 | Shield = L3/L4**
- **NAT Gateway > NAT Instance**
- **X-Ray = tracing | CloudTrail = audit**
- **Macie = data discovery**
- **Immutable = safest deployment**
