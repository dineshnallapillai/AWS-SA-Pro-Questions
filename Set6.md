# AWS Compute & Scaling Patterns - Complete Guide

## 1️⃣ Compute & Scaling Patterns

### 🖥 EC2 Fundamentals

**Amazon EC2**

- EBS must be in same AZ as EC2
- Multiple ENIs allowed
- Separate security groups per ENI
- Keep instances in private subnets when possible

---

### 📈 Auto Scaling Best Practices

- Always deploy across multiple AZs
- Use ALB (Layer 7) for web apps
- Spot = batch / non-critical workloads
- Don't auto-assign public IPs in Auto Scaling for outbound whitelisting

---

### ⚡ Serverless Scaling

**AWS Lambda**

- Lambda scales horizontally → connection explosion risk
- Move DB connection outside handler
- Use RDS Proxy for pooling
- Provisioned Concurrency ≠ DB fix

---

## 2️⃣ Storage Architecture

### 📦 Amazon S3 Patterns

**Amazon S3**

**Use S3 for:**
- Static assets
- Media storage
- Backup
- Cross-region replication
- Mobile direct uploads

**Avoid:**
- EFS for media storage
- EBS for scalable object delivery

---

### 🔁 Cross-Region Replication

- Use CRR for resilience
- Pair with CloudFront origin failover
- Don't use Lambda to replicate S3 manually

---

### 🧊 Lifecycle Optimization

- **Hot** → Standard
- **Warm** → IA
- **Archive** → Glacier / Deep Archive
- Never serve streaming content from Glacier

---

### 🗂 Gateway Endpoint Optimization

- Heavy S3 traffic from private subnet → **S3 Gateway VPC Endpoint**
- Removes NAT data processing cost

---

## 3️⃣ Database Architecture

### 🧠 DynamoDB

**Amazon DynamoDB**

**Capacity model rules:**
- Predictable load → Provisioned + Auto Scaling
- Unpredictable → On-demand

**RPO:**
- RPO 24h → Scheduled replication
- Near-zero RPO → Global Tables

**Write overload:**
- Use SQS buffering

---

### 🐬 Aurora / RDS

**Amazon Aurora**

- Aurora preferred over standard RDS for HA
- Use Multi-AZ for production
- Use reader endpoint for read scaling
- Use RDS Proxy for Lambda

---

## 4️⃣ Networking & Hybrid Connectivity

### 🌐 VPC Design Rules

- Private EC2 → NAT → Internet
- Centralize outbound IP for whitelisting
- NAT must be sized properly
- IGW does NOT have public IP

---

### 🔗 Direct Connect

**AWS Direct Connect**

**Migration rule:**
1. Keep VPN active
2. Prefer DX via BGP
3. Validate
4. Then remove VPN

**Multi-Region:**
- Use Direct Connect Gateway

---

### 🔒 Private SaaS Connectivity

**AWS PrivateLink**

- Consumer creates Interface Endpoint
- Provider exposes Endpoint Service
- Restrict via security group
- Never use VPC peering for SaaS

---

### 🧭 Route 53 Cross-Account

**Amazon Route 53**

**Private hosted zone:**
1. Authorize association
2. Associate VPC
3. Remove authorization

---

## 5️⃣ Security & Identity

### 👤 Mobile & Web Identity

**AWS Security Token Service**

- Use Web Identity Federation
- Temporary credentials only
- Never embed IAM keys in mobile apps

---

### 🏢 Enterprise Identity

**AWS IAM Identity Center**

- Integrate with AD via SAML
- Use SCIM for sync
- Use ABAC for conditional access
- Centralize access across Organizations

---

### 🔐 Encryption

**S3 encryption options:**
- SSE-KMS
- SSE-C
- Client-side encryption

**Note:** SSL = in transit only

---

### 🛡 API Protection

**Amazon API Gateway**

- Use usage plans
- Throttle per API key
- Return 429
- Protect backend

---

## 6️⃣ Serverless & Event-Driven Patterns

### 🔔 SQS Decoupling

**Amazon Simple Queue Service**

**Use for:**
- Write buffering
- Burst smoothing
- Worker decoupling
- Batch jobs

---

### 🎥 Media Processing

**Amazon Elastic Transcoder**  
**Amazon Rekognition**

**Pattern:**
```
S3 → SQS → Lambda → Rekognition
```

**HLS:**
- Elastic Transcoder
- S3
- CloudFront

---

### 🚀 Serverless Deployment

**AWS CodeDeploy**

**Use:**
- SAM
- Canary deployment
- CloudWatch alarms
- Automatic rollback

---

## 7️⃣ Migration & Discovery

### 🔍 Discovery Phase

**AWS Application Discovery Service**

1. Install Discovery Agent
2. Collect performance + process + network
3. Feed into Migration Hub

---

### 📊 Migration Planning

**AWS Migration Hub**

- Group applications
- Generate EC2 sizing recommendations
- Plan migration waves
- Never use Trusted Advisor for migration planning

---

## 8️⃣ Governance & Multi-Account

### 🏛 AWS Organizations

**AWS Organizations**

- Use SCP for control
- Centralize governance in management account
- Use StackSets for org-wide deployment

---

### 🧾 Cost Reporting

- Generate CUR in management account
- Use QuickSight for OU breakdown

---

## 9️⃣ Resilience & HA

### 🌍 Multi-AZ

- ALB across AZs
- Aurora Multi-AZ
- Auto Scaling across AZs

---

### 🌎 Multi-Region

- Use CRR for S3
- Use DX Gateway for networking
- Match DR to RPO

---

## 🔟 Cost Optimization Master Rules

- Remove EC2 when managed service exists
- Avoid NAT for S3 traffic
- Archive rarely accessed content
- Use Spot for batch
- Scale with Lambda when possible
- Don't over-engineer multi-region
- Avoid manual replication when managed replication exists
