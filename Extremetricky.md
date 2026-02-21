# 🧨 EXTREME TRICK SCENARIOS

## (Where 2 Answers Look Correct)

Each includes:
- 🎯 Scenario
- ⚠ The Two Tempting Answers
- 🧠 Why One Is More Correct

---

### 1️⃣ Lambda DB Overload Under Spike

**Scenario:** High-concurrency Lambda calling Aurora. DB max connections exceeded.

**Tempting Answers:**
- A) Increase Aurora instance size
- B) Implement RDS Proxy

🧠 **Correct: RDS Proxy**

**Why?**
- Instance size increases connection limit but does not solve connection storm
- RDS Proxy pools connections
- Question is about connection exhaustion, not CPU

---

### 2️⃣ Multi-Region DR with RPO 24h

**Scenario:** App uses DynamoDB. RPO 24h, RTO 2h.

**Tempting Answers:**
- A) DynamoDB Global Tables
- B) Scheduled export to S3 + restore process

🧠 **Correct: Scheduled export**

**Why?**
- Global Tables provide near-zero RPO (overkill + expensive)
- Requirement explicitly says RPO 24h
- Match solution to requirement exactly

---

### 3️⃣ SaaS Private Connectivity

**Scenario:** Enterprise wants private connectivity to SaaS in AWS.

**Tempting Answers:**
- A) VPC Peering
- B) AWS PrivateLink

🧠 **Correct: PrivateLink**

**Why?**
- Peering is not scalable per customer
- Peering exposes CIDRs
- PrivateLink is purpose-built for SaaS

---

### 4️⃣ Direct Connect Expansion to Multi-Region

**Scenario:** Company expanding to 3 Regions.

**Tempting Answers:**
- A) Create private VIF per VPC
- B) Use Direct Connect Gateway

🧠 **Correct: Direct Connect Gateway**

**Why?**
- DXGW supports multi-region
- Private VIF per VPC does not scale cleanly

---

### 5️⃣ API Abuse from One Client

**Scenario:** Single API key causing errors.

**Tempting Answers:**
- A) Increase Lambda reserved concurrency
- B) Apply usage plan throttling

🧠 **Correct: Usage plan**

**Why?**
- Concurrency affects compute, not client fairness
- Throttling must happen at API layer

---

### 6️⃣ Cross-Region Static Website HA

**Scenario:** Need region failover for static site.

**Tempting Answers:**
- A) Modify app code to switch buckets
- B) S3 CRR + CloudFront origin failover

🧠 **Correct: CRR + CloudFront**

**Why?**
- Managed failover preferred
- App modification increases operational overhead

---

### 7️⃣ NAT Costs High Due to S3 Traffic

**Scenario:** Private EC2 pulling heavy S3 data.

**Tempting Answers:**
- A) Upgrade NAT instance
- B) S3 Gateway Endpoint

🧠 **Correct: Gateway Endpoint**

**Why?**
- Removes NAT data processing charges entirely
- Cost optimization focus

---

### 8️⃣ Mobile App Direct AWS Access

**Scenario:** Mobile app uploads directly to S3.

**Tempting Answers:**
- A) Embed IAM user keys
- B) STS Web Identity Federation

🧠 **Correct: STS**

**Why?**
- IAM keys in mobile = security violation
- Always temporary credentials

---

### 9️⃣ Multi-Account AD Integration

**Scenario:** 300 AWS accounts need centralized login.

**Tempting Answers:**
- A) Configure SAML in each account
- B) IAM Identity Center + SCIM

🧠 **Correct: Identity Center**

**Why?**
- Centralized governance
- Scalable across Organizations

---

### 🔟 EC2 Needs Two Network Zones

**Scenario:** Web + Management traffic separated.

**Tempting Answers:**
- A) Two security groups on one ENI
- B) Two ENIs in different subnets

🧠 **Correct: Two ENIs**

**Why?**
- Separate routing domains required
- Security groups alone insufficient

---

### 1️⃣1️⃣ HLS Streaming Architecture

**Scenario:** Need HLS playback, no expertise.

**Tempting Answers:**
- A) EC2 transcoding pipeline
- B) Elastic Transcoder

🧠 **Correct: Elastic Transcoder**

**Why?**
- Managed service preferred
- Lower operational burden

---

### 1️⃣2️⃣ VPN to Direct Connect Migration

**Scenario:** Migrating hybrid connectivity.

**Tempting Answers:**
- A) Delete VPN first
- B) Keep VPN until DX validated

🧠 **Correct: Keep VPN**

**Why?**
- Zero downtime principle
- Always validate before cutover

---

### 1️⃣3️⃣ On-Prem DB Write Overload

**Scenario:** Web app overwhelming DB.

**Tempting Answers:**
- A) Increase DB size
- B) Introduce SQS buffering

🧠 **Correct: SQS**

**Why?**
- Decoupling > vertical scaling

---

### 1️⃣4️⃣ IDS in VPC

**Scenario:** Need intrusion detection.

**Tempting Answers:**
- A) Enable promiscuous mode
- B) Host-based IDS agents

🧠 **Correct: Host-based**

**Why?**
- Promiscuous mode unsupported in VPC

---

### 1️⃣5️⃣ Cross-Account Private Hosted Zone

**Scenario:** DNS not resolving.

**Tempting Answers:**
- A) Create new hosted zone
- B) Authorize & associate VPC

🧠 **Correct: Authorization flow**

**Why?**
- Private zone must be associated

---

### 1️⃣6️⃣ Serverless Deployment Rollback

**Scenario:** Need automatic rollback.

**Tempting Answers:**
- A) Manual script revert
- B) CodeDeploy Canary

🧠 **Correct: Canary**

**Why?**
- Automation + CloudWatch alarms

---

### 1️⃣7️⃣ Multi-Tenant Isolation

**Scenario:** Shared DynamoDB table.

**Tempting Answers:**
- A) Table per tenant
- B) Partition key + IAM condition

🧠 **Correct: Partition + IAM**

**Why?**
- Scalable, cost-effective, SaaS pattern

---

### 1️⃣8️⃣ EC2 Patch Across Hybrid

**Scenario:** On-prem + EC2 unified patching.

**Tempting Answers:**
- A) Inspector
- B) Systems Manager Patch Manager

🧠 **Correct: Patch Manager**

**Why?**
- Inspector scans, does not patch

---

### 1️⃣9️⃣ Large Monthly Batch Compute

**Scenario:** 300TB processed monthly.

**Tempting Answers:**
- A) EBS multi-attach
- B) FSx for Lustre + S3

🧠 **Correct: FSx**

**Why?**
- Ephemeral high-performance design

---

### 2️⃣0️⃣ Static Egress IP for Scaling App

**Scenario:** Payment provider IP whitelist.

**Tempting Answers:**
- A) Whitelist ELB
- B) NAT + Elastic IP

🧠 **Correct: NAT**

**Why?**
- ELB handles inbound only

---

## 🧠 How To Win These Questions

**When 2 answers look correct, ask:**

1. Which one reduces operational overhead?
2. Which one matches requirement precisely?
3. Which one uses managed service?
4. Which one aligns with AWS Well-Architected principles?
5. Which one scales better long-term?

---

# PART 1 — 15 "Almost Identical Answer" Traps

## (Micro-wording differences that change the answer)

Each case has two nearly identical options. Only one fits the requirement exactly.

---

### 1️⃣ "Highly Available" vs "Multi-Region"

**Scenario:** Web app must survive AZ failure.

- A) Deploy across multiple Availability Zones
- B) Deploy across multiple Regions

✅ **Correct: A**

**Why?**
- Requirement is AZ-level HA, not regional DR
- Multi-region is over-engineering unless specified

---

### 2️⃣ "Encrypt data at rest" vs "Encrypt data in transit"

- A) Enable HTTPS
- B) Enable SSE-KMS

✅ **Correct: B**

HTTPS ≠ at-rest encryption

---

### 3️⃣ "Minimize operational overhead"

- A) Build EC2-based custom solution
- B) Use managed AWS service

✅ **Correct: B**

If wording says minimize operational overhead, always prefer managed

---

### 4️⃣ "Control client request rate"

- A) Increase Lambda reserved concurrency
- B) Configure API Gateway usage plan

✅ **Correct: B**

Reserved concurrency controls compute. Usage plan controls clients.

---

### 5️⃣ "Limit database connections"

- A) Increase DB instance size
- B) Use RDS Proxy

✅ **Correct: B**

Instance size increases capacity. Proxy manages connection pooling.

---

### 6️⃣ "Private connectivity to SaaS"

- A) VPC Peering
- B) AWS PrivateLink

✅ **Correct: B**

Peering exposes CIDRs. PrivateLink is purpose-built for SaaS.

---

### 7️⃣ "Archive rarely accessed data"

- A) Move to S3 Standard-IA
- B) Move to Glacier Deep Archive

✅ **Correct depends on access pattern:**
- Access occasionally → Standard-IA
- Archive, rarely retrieved → Glacier

The exam tests exact wording.

---

### 8️⃣ "RPO 24 hours"

- A) DynamoDB Global Tables
- B) Scheduled replication

✅ **Correct: B**

Global Tables provide near-zero RPO (overkill)

---

### 9️⃣ "Temporary credentials"

- A) IAM user with access keys
- B) STS AssumeRole

✅ **Correct: B**

IAM user keys are long-term

---

### 🔟 "Cost-effective batch processing"

- A) On-Demand EC2
- B) Spot Instances

✅ **Correct: B**

Unless workload is critical

---

### 1️⃣1️⃣ "Cross-account DNS resolution"

- A) Create new hosted zone
- B) Associate VPC with existing hosted zone

✅ **Correct: B**

Private hosted zones must be associated

---

### 1️⃣2️⃣ "Reduce NAT cost"

- A) Use NAT instance
- B) Use S3 Gateway Endpoint

✅ **Correct: B**

Gateway endpoint removes NAT usage entirely

---

### 1️⃣3️⃣ "Gradual deployment"

- A) Replace API endpoint
- B) Canary deployment with CodeDeploy

✅ **Correct: B**

Gradual traffic shifting required

---

### 1️⃣4️⃣ "Centralized access management"

- A) IAM roles in each account
- B) IAM Identity Center

✅ **Correct: B**

Enterprise-level solution

---

### 1️⃣5️⃣ "Serve static content globally"

- A) EC2 web servers
- B) S3 + CloudFront

✅ **Correct: B**

CloudFront provides global caching

---

# ⚔️ PART 2 — 20-Question HARD MODE Timed Simulation

**Answer mentally before reading explanation. These are professional-level.**

---

### Q1
Lambda to Aurora under spike, DB exhausted.

- A) Increase DB size
- B) Add RDS Proxy
- C) Increase Lambda memory
- D) Add read replica

**Correct: B**

---

### Q2
Need static outbound IP for scaling app.

- A) Whitelist ELB
- B) Use NAT + Elastic IP
- C) Assign public IP per instance
- D) Use IGW

**Correct: B**

---

### Q3
On-prem to 3 Regions via Direct Connect.

- A) Private VIF per VPC
- B) DX Gateway
- C) Public VIF
- D) VPN failover only

**Correct: B**

---

### Q4
Mobile app uploads directly to S3.

- A) IAM user keys
- B) STS federation
- C) EC2 proxy
- D) API Gateway

**Correct: B**

---

### Q5
One API key flooding requests.

- A) Increase DynamoDB capacity
- B) Throttle via API Gateway
- C) Increase Lambda concurrency
- D) Add WAF rule

**Correct: B**

---

### Q6
Multi-region static site failover.

- A) App logic switching
- B) CRR + CloudFront origin failover
- C) Manual bucket copy
- D) Route53 weighted

**Correct: B**

---

### Q7
Need minimal ops video HLS delivery.

- A) EC2 transcoding cluster
- B) Elastic Transcoder
- C) EFS storage
- D) Glacier origin

**Correct: B**

---

### Q8
Patch EC2 and on-prem servers centrally.

- A) Inspector
- B) Systems Manager Patch Manager
- C) Trusted Advisor
- D) OpsWorks

**Correct: B**

---

### Q9
Heavy S3 traffic from private subnet.

- A) Larger NAT
- B) NAT instance
- C) S3 Gateway Endpoint
- D) Interface Endpoint

**Correct: C**

---

### Q10
Cross-account private DNS not resolving.

- A) Modify resolv.conf
- B) Associate VPC with hosted zone
- C) New hosted zone
- D) Route53 public record

**Correct: B**

---

### Q11
RPO near zero for DynamoDB.

- A) Scheduled export
- B) Global Tables
- C) Backup restore
- D) DAX

**Correct: B**

---

### Q12
Minimize cost for monthly 300TB processing.

- A) EBS Multi-Attach
- B) FSx for Lustre + S3
- C) EFS
- D) Glacier

**Correct: B**

---

### Q13
Prevent unapproved Marketplace AMIs.

- A) IAM deny per account
- B) SCP at root
- C) Security group rule
- D) NACL

**Correct: B**

---

### Q14
Need gradual Lambda rollout.

- A) Manual swap
- B) SAM + CodeDeploy Canary
- C) New API endpoint
- D) Alias update only

**Correct: B**

---

### Q15
Private SaaS access required.

- A) VPC Peering
- B) PrivateLink
- C) VPN
- D) IGW

**Correct: B**

---

### Q16
Separate web and management traffic on one instance.

- A) Two security groups
- B) Two ENIs in separate subnets
- C) NACL rule
- D) Route table change

**Correct: B**

---

### Q17
Archive logs rarely accessed.

- A) S3 Standard
- B) S3 IA
- C) Glacier Deep Archive
- D) EBS

**Correct: C**

---

### Q18
High write DB overwhelmed.

- A) Bigger DB
- B) Add read replica
- C) SQS buffering
- D) DAX

**Correct: C**

---

### Q19
Enterprise AD across 400 accounts.

- A) SAML per account
- B) IAM Identity Center
- C) IAM users
- D) Cognito

**Correct: B**

---

### Q20
Need protection from DDoS globally.

- A) Scale EC2
- B) CloudFront + ELB + WAF
- C) NAT upgrade
- D) Increase DB

**Correct: B**

---

## 🧠 Final Reflection

**If you got:**

- **18–20 correct** → Professional-ready
- **15–17** → Strong Associate level
- **<15** → Revisit trap patterns
