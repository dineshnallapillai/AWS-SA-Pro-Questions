# AWS FINAL BOSS SCENARIOS

## 1️⃣ Global FinTech Payment Platform

### 🎯 Scenario

A financial API:
- Runs in 2 Regions (active-active)
- Uses DynamoDB
- RPO ≈ 0
- RTO < 5 minutes
- External payment provider requires static whitelisted IPs
- Must minimize latency globally

### ⚠ Hidden Traps
- Using Multi-AZ only
- Using Route53 failover routing
- Whitelisting ELB IPs
- Using NAT per instance

### 🧠 Correct Architecture Direction

- DynamoDB Global Tables
- Route 53 latency routing
- Private subnets + centralized NAT Gateways with Elastic IPs
- ALB per region
- Optional: CloudFront for edge caching

**Why?**
- RPO ≈ 0 → Global Tables mandatory
- RTO < 5 min → Active-active, not failover
- Static outbound IP → NAT with EIP
- Latency routing improves global performance

**This question tests:** HA + DR + networking + external constraint simultaneously.

---

## 2️⃣ Enterprise Multi-Account SaaS Governance

### 🎯 Scenario

Company has:
- 500 AWS accounts
- On-prem Active Directory
- Marketplace must be restricted
- Central login required
- Automatic provisioning for new accounts

### ⚠ Hidden Traps
- SAML in each account
- IAM deny policies instead of SCP
- Manual stack deployments

### 🧠 Correct Architecture Direction

- IAM Identity Center + SAML to AD
- SCIM provisioning
- SCP at root
- Private Marketplace control
- StackSets (service-managed, auto deployment)

**Why?**
- This combines identity + governance + automation
- This is classic SAP cross-domain scenario

---

## 3️⃣ Massive Analytics with Periodic Compute

### 🎯 Scenario

- 400 TB stored in S3
- Monthly analytics job runs for 72 hours
- High throughput required
- Cost must be minimized
- No always-on file servers

### ⚠ Hidden Traps
- EFS permanently mounted
- Large EBS Multi-Attach
- Always-on EC2 cluster

### 🧠 Correct Architecture Direction

- Store data in S3
- Mount temporary FSx for Lustre
- Use Auto Scaling compute fleet
- Delete FSx after job completion
- Archive rarely accessed data to Glacier

**Why?**
- Ephemeral high-performance pattern
- This tests storage tiering + cost optimization

---

## 4️⃣ High-Traffic Serverless API with Noisy Clients

### 🎯 Scenario

- API Gateway → Lambda → Aurora
- One customer flooding requests
- DB connections exhausted
- Customers see 5xx errors
- Must protect backend + maintain fairness

### ⚠ Hidden Traps
- Increase DB size
- Increase Lambda concurrency
- Add read replica

### 🧠 Correct Architecture Direction

**Layered solution:**
- API Gateway Usage Plan throttling
- RDS Proxy
- Move DB connection outside handler

**Why?**
This is layered problem:
- Edge control
- Connection pooling
- Resource optimization

---

## 5️⃣ Hybrid Enterprise Migration at Scale

### 🎯 Scenario

- 1,200 on-prem servers
- Need dependency mapping
- Need EC2 right-sizing
- Need centralized patching after migration
- Single reporting dashboard

### ⚠ Hidden Traps
- Trusted Advisor
- Inspector for patching
- Manual spreadsheet sizing

### 🧠 Correct Architecture Direction

- Application Discovery Agent
- Migration Hub
- Systems Manager Patch Manager
- Hybrid Activation

**This tests:** migration + operations integration.

---

## 6️⃣ Media Streaming Platform Global Rollout

### 🎯 Scenario

- Mobile users globally
- Requires HLS playback
- Uploads large files
- Must minimize backend EC2
- Archive originals cheaply

### ⚠ Hidden Traps
- EC2 transcoding cluster
- EFS storage
- Serving from Glacier

### 🧠 Correct Architecture Direction

- Elastic Transcoder
- S3 storage
- S3 Lifecycle to Glacier
- CloudFront distribution
- Direct upload via STS + Multipart

**This tests:** media + serverless + lifecycle.

---

## 7️⃣ Multi-Region Disaster Recovery Strategy

### 🎯 Scenario

- RPO 24 hours
- RTO 4 hours
- Uses RDS MySQL
- Budget constrained

### ⚠ Hidden Traps
- Aurora Global Database
- Real-time replication
- Multi-region active-active

### 🧠 Correct Architecture Direction

- Daily automated backups
- Cross-region snapshot copy
- Pre-created infrastructure via CloudFormation
- Route53 failover

**Match DR level to requirement — not over-engineer.**

---

## 8️⃣ Private SaaS Consumption by Hundreds of Customers

### 🎯 Scenario

- SaaS hosted in VPC
- Customers want private connectivity
- Must scale to hundreds of VPCs
- Minimal operational overhead

### ⚠ Hidden Traps
- VPC Peering per customer
- Site-to-Site VPN per customer

### 🧠 Correct Architecture Direction

- AWS PrivateLink Endpoint Service
- Customers create Interface Endpoints

**Why?**
- Scalable, isolated, no CIDR exposure

---

## 9️⃣ Secure Multi-Tenant Data Isolation

### 🎯 Scenario

- SaaS app
- Shared DynamoDB table
- Tenants must not access others' data
- Must scale to 10,000 tenants

### ⚠ Hidden Traps
- Table per tenant
- Separate AWS account per tenant

### 🧠 Correct Architecture Direction

- Partition key per tenant
- IAM policy condition: `dynamodb:LeadingKeys`
- ABAC

**This tests:** advanced IAM condition usage.

---

## 🔟 End-to-End Zero Downtime CI/CD Pipeline

### 🎯 Scenario

- CloudFront → API Gateway → Lambda
- Must deploy with:
  - Zero downtime
  - Gradual traffic shift
  - Automatic rollback
  - Alarm-based revert

### ⚠ Hidden Traps
- Manual alias switch
- Deploy new API endpoint
- Script-based rollback

### 🧠 Correct Architecture Direction

- AWS SAM
- CodeDeploy Canary
- Lambda versions + aliases
- CloudWatch alarms

**This tests:** deployment automation depth.

---

## 🧠 FINAL BOSS DECISION FRAMEWORK

**For any multi-layer SAP scenario, evaluate in this order:**

1. Is this HA or DR?
2. Is there a managed service replacing custom infra?
3. Is the requirement cost-sensitive?
4. Is there an external constraint (IP whitelist, compliance)?
5. Is traffic inbound or outbound?
6. Does identity need centralization?
7. Are we matching RPO/RTO exactly?

**If you can reason through all 10 without guessing, you are operating at SAP-level thinking.**

---

# KILL SHOT – 20 IMPOSSIBLE TRICK QUESTIONS

## 1️⃣
A company requires 99.99% availability for a web app. RPO and RTO are not specified.

- A) Deploy Multi-AZ in one Region
- B) Deploy active-passive Multi-Region
- C) Deploy active-active Multi-Region
- D) Backup to another Region

**Correct: A**

**Reason:** 99.99% does NOT automatically mean Multi-Region. Overengineering trap.

---

## 2️⃣
An API must reject abusive clients before hitting backend compute.

- A) WAF rate-based rules
- B) API Gateway usage plan
- C) Lambda reserved concurrency
- D) Increase DB capacity

**Correct: B**

**Why not WAF?** WAF is IP-based. Usage plan is API key–based client control.

---

## 3️⃣
Private EC2 pulling large S3 objects. NAT cost increasing. High security required.

- A) Replace NAT with NAT instance
- B) Use Interface VPC Endpoint for S3
- C) Use Gateway VPC Endpoint
- D) Move EC2 to public subnet

**Correct: C**

S3 uses Gateway endpoint, not Interface.

---

## 4️⃣
RDS database must survive AZ outage without manual intervention.

- A) Automated backups
- B) Read replica
- C) Multi-AZ deployment
- D) Snapshot replication

**Correct: C**

Read replica ≠ automatic failover unless promoted manually (RDS MySQL).

---

## 5️⃣
Global SaaS platform needs to expose service privately to 200 customers.

- A) VPC Peering
- B) PrivateLink
- C) Site-to-Site VPN
- D) Transit Gateway

**Correct: B**

Scalability + isolation requirement.

---

## 6️⃣
Lambda concurrency spike causing Aurora CPU exhaustion.

- A) Increase Aurora size
- B) Add read replicas
- C) Use RDS Proxy
- D) Increase Lambda memory

**Correct: C**

Connection storm, not CPU root cause.

---

## 7️⃣
Static website must be resilient to Region outage and require no code changes.

- A) Route53 weighted routing
- B) CRR + CloudFront origin failover
- C) Manual failover runbook
- D) Multi-AZ S3

**Correct: B**

S3 Multi-AZ is regional only.

---

## 8️⃣
Enterprise requires conditional access to accounts based on department attribute.

- A) IAM roles per account
- B) IAM Identity Center + ABAC
- C) IAM groups
- D) Cognito user pools

**Correct: B**

ABAC key word.

---

## 9️⃣
On-prem to AWS migration requires capturing process-level dependencies.

- A) Systems Manager Agent
- B) Application Discovery Agent
- C) Trusted Advisor
- D) CloudWatch Agent

**Correct: B**

Process-level mapping only via Discovery Agent.

---

## 🔟
Company wants cheapest archival storage with rare retrieval.

- A) S3 IA
- B) Glacier Instant Retrieval
- C) Glacier Deep Archive
- D) EBS snapshot

**Correct: C**

"Cheapest archival" = Deep Archive.

---

## 1️⃣1️⃣
Need static outbound IP from Auto Scaling group.

- A) Assign public IP to instances
- B) Use NAT Gateway with Elastic IP
- C) Whitelist ELB
- D) Use IGW

**Correct: B**

IGW does not provide static IP.

---

## 1️⃣2️⃣
Write-heavy DynamoDB workload predictable weekly spike.

- A) On-Demand mode
- B) Provisioned + Auto Scaling
- C) Global Tables
- D) DAX

**Correct: B**

Predictable workload → provisioned cheaper.

---

## 1️⃣3️⃣
Need centralized patching across EC2 and on-prem.

- A) AWS Inspector
- B) Systems Manager Patch Manager
- C) AWS Config
- D) OpsWorks

**Correct: B**

Inspector scans only.

---

## 1️⃣4️⃣
DR plan: RPO near zero for DynamoDB.

- A) Scheduled export
- B) Global Tables
- C) Backup restore
- D) DAX

**Correct: B**

Near-zero RPO requires active replication.

---

## 1️⃣5️⃣
One client flooding REST API.

- A) Increase DynamoDB WCU
- B) Increase Lambda reserved concurrency
- C) API Gateway usage plan
- D) CloudWatch alarm

**Correct: C**

Edge control.

---

## 1️⃣6️⃣
Private hosted zone not resolving in another account.

- A) Modify NACL
- B) Create new zone
- C) Authorize VPC association
- D) Add public DNS record

**Correct: C**

Cross-account authorization required.

---

## 1️⃣7️⃣
Video streaming global delivery with minimal ops.

- A) EC2 web servers
- B) EFS shared storage
- C) S3 + CloudFront
- D) Glacier direct

**Correct: C**

Managed + global caching.

---

## 1️⃣8️⃣
Expand Direct Connect to multi-Region.

- A) Add more private VIFs
- B) Use Direct Connect Gateway
- C) Add VPN backup
- D) Public VIF

**Correct: B**

DXGW required for multi-region.

---

## 1️⃣9️⃣
Multi-tenant SaaS isolation in DynamoDB.

- A) Table per tenant
- B) Separate AWS account per tenant
- C) Partition key + IAM condition
- D) Read replica per tenant

**Correct: C**

Scalable + least overhead.

---

## 2️⃣0️⃣
Need automatic rollback of Lambda deployment on error spike.

- A) Manual alias revert
- B) New API endpoint
- C) CodeDeploy Canary + alarms
- D) Increase memory

**Correct: C**

Only CodeDeploy integrates alarm-based rollback.

---

## 🧠 If You Missed Any — Why?

**Most common mental mistakes:**

1. Confusing HA with DR
2. Overengineering Multi-Region
3. Scaling compute instead of decoupling
4. Ignoring edge throttling
5. Not matching solution to RPO exactly
6. Choosing complex solution over managed service
