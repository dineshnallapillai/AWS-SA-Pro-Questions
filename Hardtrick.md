# HARD MODE – 25 PROFESSIONAL-LEVEL SCENARIOS

## 🧠 1️⃣ Multi-Region Active-Active API

**Scenario:** A fintech API must run active-active in 2 regions with RPO ≈ 0 and RTO < 5 minutes. It uses DynamoDB.

⚠ **Trap:** Multi-AZ + backup restore  
✅ **Correct Direction:** DynamoDB Global Tables + Route 53 latency routing

---

## 🌐 2️⃣ Global SaaS with Private Connectivity

**Scenario:** Customers demand private access from their VPCs into your SaaS hosted in AWS.

⚠ **Trap:** VPC Peering per customer  
✅ **Correct Direction:** AWS PrivateLink Endpoint Service

---

## 🔄 3️⃣ High Write Burst Causing Downstream Failures

**Scenario:** Microservices architecture. One service spikes writes and overwhelms downstream services.

⚠ **Trap:** Increase instance size  
✅ **Correct Direction:** Introduce SQS buffering + circuit breaker

---

## 🗃 4️⃣ 500 TB Analytics, Monthly Processing Only

**Scenario:** Large dataset, processed once a month, high-performance compute required.

⚠ **Trap:** EFS permanent mount  
✅ **Correct Direction:** S3 + FSx for Lustre (ephemeral mount)

---

## 🧩 5️⃣ Lambda Cold Start Impacts Latency SLA

**Scenario:** Critical API has strict latency requirement under sudden traffic spikes.

⚠ **Trap:** Increase memory  
✅ **Correct Direction:** Provisioned Concurrency + warming strategy

---

## 🔐 6️⃣ Centralized IAM Across 300 Accounts

**Scenario:** Enterprise requires conditional access based on department tags.

⚠ **Trap:** IAM roles per account  
✅ **Correct Direction:** IAM Identity Center + ABAC + SCIM

---

## 📡 7️⃣ Hybrid DNS Fails Cross-Account

**Scenario:** Private hosted zone in shared services account. New VPC in another account cannot resolve.

⚠ **Trap:** Modify resolv.conf  
✅ **Correct Direction:** Route 53 association authorization flow

---

## 📉 8️⃣ NAT Costs Skyrocketing

**Scenario:** Large data movement to S3 from private subnets.

⚠ **Trap:** Smaller NAT instance  
✅ **Correct Direction:** S3 Gateway Endpoint

---

## 🏛 9️⃣ Marketplace Governance Across Org

**Scenario:** Security requires blocking unapproved AMIs across org.

⚠ **Trap:** IAM deny in each account  
✅ **Correct Direction:** SCP at root

---

## 🔍 🔟 Massive On-Prem Discovery Required

**Scenario:** Need to capture process-level dependency mapping for 1200 servers.

⚠ **Trap:** Systems Manager  
✅ **Correct Direction:** Application Discovery Agent

---

## 🔥 1️⃣1️⃣ Auto Scaling + Payment Gateway IP Whitelisting

**Scenario:** Scaling web tier, but payment provider only allows 4 IPs.

⚠ **Trap:** ELB IP whitelist  
✅ **Correct Direction:** Centralized NAT with Elastic IP

---

## 🧠 1️⃣2️⃣ RDS Failover Without App Restart

**Scenario:** App must failover DB without connection reset.

⚠ **Trap:** Use snapshot restore  
✅ **Correct Direction:** Aurora cluster endpoint

---

## 📦 1️⃣3️⃣ Cross-Region Static Asset Resilience

**Scenario:** Static website must survive regional outage.

⚠ **Trap:** App-level bucket switching  
✅ **Correct Direction:** S3 CRR + CloudFront origin failover

---

## 🛡 1️⃣4️⃣ One API Key Abusing System

**Scenario:** Single customer flooding API.

⚠ **Trap:** Increase Lambda concurrency  
✅ **Correct Direction:** API Gateway usage plan throttling

---

## 🚀 1️⃣5️⃣ Zero-Downtime Lambda Deployment

**Scenario:** Need gradual rollout + auto rollback on error spike.

⚠ **Trap:** Manual CLI script  
✅ **Correct Direction:** SAM + CodeDeploy Canary

---

## 🗂 1️⃣6️⃣ DR Strategy with RPO 24h, RTO 2h

**Scenario:** App using DynamoDB, minimal changes allowed.

⚠ **Trap:** Global Tables  
✅ **Correct Direction:** Scheduled export + cross-region restore plan

---

## 🧱 1️⃣7️⃣ Multi-Tenant SaaS Isolation

**Scenario:** Each tenant must not access other tenant's data in shared DynamoDB table.

⚠ **Trap:** Separate tables per tenant  
✅ **Correct Direction:** Partition key + IAM condition-based access

---

## 🛰 1️⃣8️⃣ Direct Connect Expanding to 3 Regions

**Scenario:** Single 1Gbps DX link today.

⚠ **Trap:** Add more private VIFs  
✅ **Correct Direction:** Direct Connect Gateway

---

## 🧬 1️⃣9️⃣ High-Throughput Log Analytics

**Scenario:** OpenSearch storage cost rising dramatically.

⚠ **Trap:** Increase data nodes  
✅ **Correct Direction:** UltraWarm + S3 archival

---

## 📱 2️⃣0️⃣ Mobile App Secure Direct S3 Access

**Scenario:** Users upload media directly.

⚠ **Trap:** IAM user keys in app  
✅ **Correct Direction:** STS Web Identity Federation

---

## ⚡ 2️⃣1️⃣ High Concurrency Lambda + Aurora

**Scenario:** Traffic surge causing DB exhaustion.

⚠ **Trap:** Bigger DB instance  
✅ **Correct Direction:** RDS Proxy + connection reuse

---

## 🏗 2️⃣2️⃣ Org-Wide SNS Deployment

**Scenario:** SNS topic must exist in all current & future accounts.

⚠ **Trap:** Manual stack deployment  
✅ **Correct Direction:** StackSet (service-managed, auto deploy)

---

## 🎥 2️⃣3️⃣ HLS Video Delivery, No In-House Expertise

**Scenario:** Need to deliver HLS video without building custom infrastructure.

⚠ **Trap:** EC2 transcoding cluster  
✅ **Correct Direction:** Elastic Transcoder + S3 + CloudFront

---

## 🧨 2️⃣4️⃣ DDoS Mitigation for Global App

**Scenario:** Traffic spikes from unknown sources.

⚠ **Trap:** Scale EC2 only  
✅ **Correct Direction:** CloudFront + ELB + WAF

---

## 🔄 2️⃣5️⃣ Write-Heavy App Overwhelming On-Prem DB

**Scenario:** EC2 writes synchronously to on-prem DB via VPN.

⚠ **Trap:** Increase VPN bandwidth  
✅ **Correct Direction:** Write to SQS → async worker flush

---

## 🧠 HARD MODE THINKING FRAMEWORK

**For every SAP-level question:**

1. Is this HA or DR?
2. Is there a managed alternative to custom infra?
3. Is this inbound or outbound traffic?
4. Is this scaling or rate control?
5. Is the requirement RPO-based or performance-based?
6. Is identity centralized or per-account?
7. Is cost optimization hidden in wording?
