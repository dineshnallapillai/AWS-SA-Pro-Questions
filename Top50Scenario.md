# TOP 50 SCENARIO-BASED AWS EXAM TRAPS

## 🖥 Compute & Scaling

### 1️⃣ Lambda app hitting RDS max connections under load

⚠ **Trap:** Increase DB instance size  
✅ **Correct:** Use RDS Proxy + connection reuse

---

### 2️⃣ Auto Scaling web app needs static outbound IP

⚠ **Trap:** Whitelist ELB IP  
✅ **Correct:** Route outbound via NAT + Elastic IP

---

### 3️⃣ EC2 instances in private subnet downloading 2TB daily from S3

⚠ **Trap:** Upgrade NAT  
✅ **Correct:** Use S3 Gateway VPC Endpoint

---

### 4️⃣ Deployment rollback must be automatic

⚠ **Trap:** Script rollback with CLI  
✅ **Correct:** Use CodeDeploy Canary + CloudWatch alarms

---

### 5️⃣ Spot instances for payment processing tier

⚠ **Trap:** Cost savings  
✅ **Correct:** Spot not for critical workloads

---

## 📦 Storage

### 6️⃣ Stream high-res videos globally

⚠ **Trap:** Store on EBS + serve from EC2  
✅ **Correct:** S3 + CloudFront

---

### 7️⃣ Need cross-region S3 failover

⚠ **Trap:** Lambda replication  
✅ **Correct:** Enable S3 Cross-Region Replication

---

### 8️⃣ Serve streaming from Glacier

⚠ **Trap:** Low cost  
✅ **Correct:** Glacier is archival only

---

### 9️⃣ Encrypt S3 data

⚠ **Trap:** Enable HTTPS  
✅ **Correct:** SSE-KMS or SSE-C

---

### 🔟 Upload large files from mobile app

⚠ **Trap:** Route through EC2  
✅ **Correct:** STS + S3 Multipart Upload

---

## 🧠 DynamoDB / Databases

### 1️⃣1️⃣ Weekly spike, predictable base load

⚠ **Trap:** Switch to On-Demand  
✅ **Correct:** Provisioned + Auto Scaling + Reserved

---

### 1️⃣2️⃣ Need RPO 24h cross-region

⚠ **Trap:** Global Tables  
✅ **Correct:** Scheduled replication

---

### 1️⃣3️⃣ Write-heavy app overloading DB

⚠ **Trap:** Increase DB size  
✅ **Correct:** Introduce SQS buffering

---

### 1️⃣4️⃣ RDS read scaling issue

⚠ **Trap:** Multi-AZ  
✅ **Correct:** Add read replicas

---

### 1️⃣5️⃣ Aurora multi-AZ requirement

⚠ **Trap:** Snapshot replication  
✅ **Correct:** Use built-in Multi-AZ cluster

---

## 🌐 Networking

### 1️⃣6️⃣ SaaS must be privately consumed

⚠ **Trap:** VPC Peering  
✅ **Correct:** AWS PrivateLink

---

### 1️⃣7️⃣ Private hosted zone not resolving cross-account

⚠ **Trap:** Edit /etc/resolv.conf  
✅ **Correct:** Authorize & associate VPC

---

### 1️⃣8️⃣ Expand Direct Connect to multiple Regions

⚠ **Trap:** Attach private VIF to each VPC  
✅ **Correct:** Use Direct Connect Gateway

---

### 1️⃣9️⃣ Migrating from VPN to Direct Connect

⚠ **Trap:** Delete VPN first  
✅ **Correct:** Prefer DX via BGP, validate, then remove VPN

---

### 2️⃣0️⃣ IDS solution in VPC

⚠ **Trap:** Promiscuous mode  
✅ **Correct:** Host-based agents or reverse proxy

---

## 🔐 Identity & Security

### 2️⃣1️⃣ Mobile app accessing DynamoDB directly

⚠ **Trap:** Embed IAM user credentials  
✅ **Correct:** Web Identity Federation + STS

---

### 2️⃣2️⃣ Multi-account SSO with on-prem AD

⚠ **Trap:** Configure SAML per account  
✅ **Correct:** IAM Identity Center + SCIM

---

### 2️⃣3️⃣ Throttle one abusive API client

⚠ **Trap:** Increase Lambda concurrency  
✅ **Correct:** API Gateway Usage Plan

---

### 2️⃣4️⃣ Protect SSL private key

⚠ **Trap:** Store on EC2 with file permissions  
✅ **Correct:** Terminate SSL at ELB

---

### 2️⃣5️⃣ Need per-user S3 folder access

⚠ **Trap:** Create IAM users  
✅ **Correct:** Identity broker + STS scoped policy

---

## 🔔 Serverless & Event

### 2️⃣6️⃣ Replace RabbitMQ in AWS

⚠ **Trap:** SNS  
✅ **Correct:** SQS

---

### 2️⃣7️⃣ Webhook migration to serverless

⚠ **Trap:** ECS Fargate  
✅ **Correct:** API Gateway HTTP API + Lambda

---

### 2️⃣8️⃣ Media categorization pipeline

⚠ **Trap:** EC2 ML cluster  
✅ **Correct:** S3 → SQS → Lambda → Rekognition

---

### 2️⃣9️⃣ Need automatic Lambda rollback

⚠ **Trap:** Manual script  
✅ **Correct:** SAM + CodeDeploy Canary

---

### 3️⃣0️⃣ Large static file hosting with HA

⚠ **Trap:** EC2 web servers  
✅ **Correct:** S3 + CloudFront

---

## 🏛 Governance & Multi-Account

### 3️⃣1️⃣ Deploy SNS topic to all accounts

⚠ **Trap:** Create stack in each account  
✅ **Correct:** StackSet from management account

---

### 3️⃣2️⃣ Restrict marketplace software

⚠ **Trap:** IAM deny policy per account  
✅ **Correct:** SCP + Private Marketplace

---

### 3️⃣3️⃣ Need cost breakdown per OU

⚠ **Trap:** CUR in each account  
✅ **Correct:** CUR from management account

---

### 3️⃣4️⃣ Prevent developers from bypassing policy

⚠ **Trap:** Inline IAM deny  
✅ **Correct:** SCP at root

---

### 3️⃣5️⃣ New account auto-deployment needed

⚠ **Trap:** Manual stack creation  
✅ **Correct:** StackSet automatic deployment

---

## 🔄 Migration

### 3️⃣6️⃣ Need on-prem server dependency mapping

⚠ **Trap:** Systems Manager Agent  
✅ **Correct:** Application Discovery Agent

---

### 3️⃣7️⃣ Need EC2 instance recommendations

⚠ **Trap:** Trusted Advisor  
✅ **Correct:** Migration Hub

---

### 3️⃣8️⃣ Patch EC2 and on-prem together

⚠ **Trap:** Inspector  
✅ **Correct:** Systems Manager Patch Manager

---

### 3️⃣9️⃣ High-performance shared storage for 72-hour job

⚠ **Trap:** Large EBS Multi-Attach  
✅ **Correct:** FSx for Lustre + S3

---

### 4️⃣0️⃣ Massive write bursts to on-prem DB

⚠ **Trap:** Add read replica  
✅ **Correct:** SQS decoupling

---

## 💰 Cost Optimization

### 4️⃣1️⃣ NAT cost too high due to S3 traffic

⚠ **Trap:** Smaller NAT  
✅ **Correct:** S3 Gateway Endpoint

---

### 4️⃣2️⃣ Idle EC2 during off-hours

⚠ **Trap:** Leave running  
✅ **Correct:** Auto Scaling or Lambda replacement

---

### 4️⃣3️⃣ Long-term OpenSearch storage cost

⚠ **Trap:** Keep in hot nodes  
✅ **Correct:** UltraWarm + S3 Glacier

---

### 4️⃣4️⃣ Batch workers running 24/7

⚠ **Trap:** Large EC2  
✅ **Correct:** Spot + Auto Scaling on SQS depth

---

### 4️⃣5️⃣ Archive rarely accessed images

⚠ **Trap:** EFS  
✅ **Correct:** S3 lifecycle to Glacier

---

## 🧨 Advanced Architecture Thinking

### 4️⃣6️⃣ Multi-region active-active required

⚠ **Trap:** Multi-AZ  
✅ **Correct:** Global architecture (e.g., Global Tables / multi-region ALB)

---

### 4️⃣7️⃣ One API client causing backend errors

⚠ **Trap:** Increase DynamoDB capacity  
✅ **Correct:** Throttle at API Gateway

---

### 4️⃣8️⃣ Need least-privilege SaaS access

⚠ **Trap:** VPN  
✅ **Correct:** PrivateLink

---

### 4️⃣9️⃣ Want faster deployment detection

⚠ **Trap:** Manual monitoring  
✅ **Correct:** CloudWatch alarms + CodeDeploy

---

### 5️⃣0️⃣ Need predictable DR strategy

⚠ **Trap:** Over-engineer multi-region  
✅ **Correct:** Match RPO/RTO precisely

---

## 🧠 How to Use This

**When reading any exam question:**

1. Identify domain (network, storage, identity, etc.)
2. Spot the trap phrase
3. Ask: "Is there a managed service replacing this custom setup?"
4. Match solution to RPO/RTO or cost requirement
5. Control traffic at the edge, not backend
