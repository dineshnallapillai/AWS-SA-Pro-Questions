# SET 4 – KEY CONCEPTS & EXAM NOTES - Exam Notes

## 1️⃣ Messaging & Integration

### ActiveMQ / MQTT migration

**Amazon MQ** is the ONLY managed service that supports:
- ActiveMQ
- JMS, NMS, MQTT, AMQP, STOMP
- SNS / SQS ≠ broker replacements

**Exam rule:**
> "Minimal change + ActiveMQ" → Amazon MQ

---

## 2️⃣ Web Security (DDoS, SQLi, Bots)

### Best protection stack
- **AWS WAF** → Layer 7 (SQL injection, XSS, bad bots)
- **AWS Shield Advanced** → Layer 3/4 DDoS + notifications + DRT
- CloudFront alone is not enough
- NACL IP blocking is reactive and not scalable

**Exam rule:**
> L3/L4 + L7 attacks → Shield Advanced + WAF

---

## 3️⃣ S3 Security & Data Protection

### Prevent public data leaks
- **S3 Block Public Access** (bucket or account level)
  - Fastest
  - Strongest
  - Exam-favorite

### PII discovery
- **Amazon Macie**
  - Detects PII
  - Uses ML

### Access tracking
- **CloudTrail GET Object**

### Never use:
- Inspector (EC2 only)
- GuardDuty (threat detection, not data classification)

---

## 4️⃣ EC2 Governance & Compliance

### Restrict AMIs (post-deployment enforcement)
- AWS Config rules
- Trigger Lambda remediation
- Notify via SNS

### Why not IAM?
- IAM blocks deployment (violates requirement)

**Exam rule:**
> "Detect after launch" → AWS Config

---

## 5️⃣ Outbound Internet from Private Subnets

### PCI / payment gateway access
- **NAT Gateway + Elastic IP**
  - Scalable
  - HA
  - Managed
- Never NAT Instance (unless cost is explicitly requested)

---

## 6️⃣ CloudFront & HTTPS

### When custom domain is NOT required
- Use default CloudFront certificate
- Set Viewer Protocol Policy = HTTPS only

### HTTPS enforcement

**Valid:**
- Redirect HTTP → HTTPS
- HTTPS only
- Default CloudFront cert (if no custom domain)

**Invalid:**
- Self-signed certs
- S3 certificate storage

---

## 7️⃣ Streaming & Data Durability

### Kinesis data loss prevention
- Kinesis Data Firehose → S3
- Kinesis S3 Connector

### Never use:
- EBS backups
- Cross-AZ Kinesis replication (not supported)

---

## 8️⃣ Security Auditing & Vulnerability Analysis

| Requirement | Service |
|-------------|---------|
| Config compliance | AWS Config |
| Vulnerability scans | Amazon Inspector |

**Best combo:** AWS Config + Amazon Inspector

---

## 9️⃣ CI/CD & Zero Downtime Deployments

### Strict capacity, rollback, no downtime
- Blue/Green
- Elastic Beanstalk – Immutable
- AWS CodeDeploy

❌ Rolling / All-at-once = downtime risk

---

## 🔟 IAM – Least Privilege

### Developers (no IAM admin)
- **PowerUserAccess**
  - Full service access
  - No IAM control

❌ AdministratorAccess = violates least privilege

---

## 1️⃣1️⃣ API Gateway Errors

### HTTP 504
- Lambda execution > 29 seconds
- API Gateway timeout

❌ Not throttling  
❌ Not IAM issue

---

## 1️⃣2️⃣ S3 Cross-Domain Access

### Different domain accessing S3
- **CORS configuration**
- Bucket policy ≠ CORS

---

## 1️⃣3️⃣ Elastic Beanstalk Deployments

### Invalid deployment policy
❌ Swap Environment URLs (not a policy)

### Valid:
- Rolling
- Rolling with additional batch
- Immutable

---

## 1️⃣4️⃣ Hybrid Connectivity

### Private on-prem dependencies
- AWS Direct Connect
- Transit VPC

❌ Elastic IP  
❌ Internet Gateway

---

## 1️⃣5️⃣ IPSec VPN Guarantees

### IPSec provides:
- ✅ Encryption
- ✅ Data integrity
- ✅ Peer authentication

❌ End-to-end identity authentication

---

## 1️⃣6️⃣ Performance Improvement (Minimal Change)
- CloudFront
- Read Replicas
- ALB + Auto Scaling

---

## 1️⃣7️⃣ Storage Gateway DR

### Gateway-Stored Volumes
- Snapshot → EBS
- Attach to EC2 during DR

---

## 1️⃣8️⃣ Encryption at Rest (EBS)

### Valid:
- OS-level encryption
- Third-party tools
- Encrypt before write

### Invalid:
- SSL/TLS (in-transit only)
- "Encrypted by default" (false)

---

## 1️⃣9️⃣ Lambda Traffic Shifting & Tracing

### 10% → 90%
- Canary deployment
- AWS X-Ray

---

## 2️⃣0️⃣ Redshift Disaster Recovery
- Cross-Region Snapshot Copy
- Snapshot Copy Grant (KMS)

---

## 2️⃣1️⃣ RTO / RPO Math

**RTO = recovery time**

Example:
- Disaster at 7:00 AM
- RTO = 3 hours
- ✅ Restore by 10:00 AM

---

## 2️⃣2️⃣ API Tracing
- **AWS X-Ray**
- CloudWatch ≠ request flow tracing

---

## 2️⃣3️⃣ VPN Single Point of Failure
- Customer Gateway (CGW) is SPOF
- **Fix:** Second CGW in another data center

---

## 2️⃣4️⃣ High-Availability Scaling (Cost-Effective)
- Reserved for baseline
- On-Demand + Spot for spikes
- Spread across AZs

---

## 2️⃣5️⃣ CloudFormation & Data Retention
- **DeletionPolicy: Retain** (S3)

---

## 2️⃣6️⃣ End-to-End HTTPS via CloudFront (Custom Domain + EC2)
- Third-party CA
- ACM cannot be used for EC2 origin

---

## 2️⃣7️⃣ AWS Organizations
- Centralized billing + policies
- SCPs ≠ permissions (they restrict)

---

## 2️⃣8️⃣ Global Logging (Audit-Grade)
- CloudTrail (all regions)
- New S3 bucket
- MFA Delete + Encryption

---

## 2️⃣9️⃣ External Auditors
- Cross-account IAM Role
- ReadOnlyAccess
- AssumeRole

---

## 3️⃣0️⃣ Oracle RAC Migration
- RAC not supported in RDS
- Use EC2 + EBS
- Data Lifecycle Manager for backups

---

## 3️⃣1️⃣ Storage Gateway Replay Attack Protection
- **CHAP for iSCSI**

---

## 3️⃣2️⃣ CloudFront HTTPS without Custom Domain
- Default CloudFront cert
- HTTPS only

---

## 3️⃣3️⃣ Lambda + VPC Errors (EC2ThrottledException)

### Causes:
- ❌ Insufficient subnet IPs
- ❌ Insufficient ENIs

---

## 3️⃣4️⃣ Cost-Optimized Hybrid Connectivity
- Direct Connect + VPN (failover)

---

## 3️⃣5️⃣ Secure DynamoDB Access (CloudFormation)
- IAM Role
- InstanceProfileName
- Never pass access keys

---

## 3️⃣6️⃣ SSL Certificate Storage

### Valid:
- ACM
- IAM certificate store

### Invalid:
- S3
- SSE options

---

## 3️⃣7️⃣ Private App, Internet Users
- SSL VPN
- Private subnet
- Client VPN

---

## 3️⃣8️⃣ VPC CIDR Expansion
- Add secondary CIDR blocks
- No deletion required

---

## 3️⃣9️⃣ RDS Multi-AZ Failover
- CNAME switch
- Standby promoted automatically

---

## 4️⃣0️⃣ OLTP + Analytics Isolation
- RDS Read Replicas
- SNS (email notification)

---

## 4️⃣1️⃣ Workflow Orchestration (Mechanical Turk)
- **Amazon SWF**
  - State tracking
  - Long-running workflows

---

## 4️⃣2️⃣ Redis Durability
- Multi-AZ + Automatic Failover
- AOF alone ≠ HA

---

## 4️⃣3️⃣ Serverless Orchestration
- **AWS Step Functions**
- NOT SWF (heavy)
- NOT Lambda alone

---

## 4️⃣4️⃣ Virtual Desktops
- WorkSpaces
- Directory Service
- VPN

---

## 4️⃣5️⃣ Reserved Instance Sharing

### Rules:
- Same instance type
- Same region
- Same AZ (if zonal RI)

→ DEV benefits, UAT does not

---

## 4️⃣6️⃣ Highly Available Database Migration
- RDS Multi-AZ + Read Replicas

---

## 🧠 FINAL EXAM META-PATTERNS (MEMORIZE)

- **Security detection ≠ prevention**
- **Config = after-the-fact enforcement**
- **IAM roles > access keys**
- **Multi-AZ = availability**
- **Read Replicas = scalability**
- **Shield + WAF = complete protection**
- **S3 Block Public Access > bucket policies**
- **NAT Gateway > NAT Instance**
- **X-Ray = tracing**
- **CloudTrail = auditing**
- **Macie = data discovery**
