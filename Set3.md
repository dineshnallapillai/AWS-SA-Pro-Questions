# SET 3 – KEY CONCEPTS & EXAM NOTES (COMPLETE)

## 1️⃣ Multi-site VPN hub-and-spoke connectivity

**Correct service:** AWS VPN CloudHub

### Key rule
- Multiple on-prem sites
- Low-cost
- Hub-and-spoke
- VPN-based (not Direct Connect)

👉 **VPN CloudHub = hub-and-spoke VPN for branch offices**

### Exam trap
- Direct Connect everywhere = ❌ expensive
- VPC peering ≠ on-prem site-to-site

---

## 2️⃣ Transit Gateway vs VPN CloudHub

- **VPN CloudHub** → legacy VPN hub-and-spoke
- **Transit Gateway** → modern, scalable hub for:
  - VPC ↔ VPC
  - VPC ↔ On-prem
  - Multi-region

👉 **If Transit Gateway is an option, it becomes the correct answer**

---

## 3️⃣ Restrict S3 access to CloudFront only

**Correct:** Origin Access Identity (OAI)

### Rule
- S3 private
- Users must NOT access S3 URL directly
- CloudFront is the only reader

👉 **OAI + bucket policy = standard solution**

### Exam trap
- IAM user for CloudFront ❌
- Distribution ID as principal ❌

---

## 4️⃣ EC2 ↔ EC2 communication + config history

**Correct (2):**
- Security Groups + NACLs (allow traffic)
- AWS Config (track historical security changes)

### Rules
- Same AZ, different subnets → routing already exists
- NACL + SG decide traffic
- AWS Config = configuration history

### Exam trap
- Systems Manager ≠ config history

---

## 5️⃣ Long-term GraphQL API hosting (3+ years)

**Correct:** Reserved EC2 + CloudFront

### Rules
- Long-term steady usage → Reserved Instances
- Global low latency → CloudFront
- Spot = ❌ for long-running APIs

---

## 6️⃣ LDAP auth + S3 per-user access (hybrid)

**Correct:** Identity Broker + LDAP + STS federation

### Correct flow
```
User → LDAP → Identity Broker → STS → Temporary creds → S3
```

### Key rule
- LDAP NEVER logs into IAM directly
- STS federation is mandatory

### Exam trap
- "LDAP credentials login to IAM" ❌

---

## 7️⃣ On-prem AD → AWS integration

**Correct:** AD Connector

### Decision table
- Want AWS-managed AD → Managed Microsoft AD
- Want on-prem AD auth only → AD Connector

👉 **Hybrid auth = AD Connector**

---

## 8️⃣ DDoS (L3–L7) + SQLi/XSS protection

**Correct (2):**
- AWS Shield Advanced
- AWS WAF

### Rules
- L3/L4 floods → Shield
- L7 attacks → WAF
- Notifications + DRT → Shield Advanced only

---

## 9️⃣ Clickstream analytics (real-time)

### Golden rule
**If you see "clickstream" → Kinesis**

- Real-time ingestion
- Streaming analytics

---

## 🔟 Real-time click behavior processing

**Correct:** Kinesis Data Streams + consumers

### Why
- EMR = batch ❌
- Redshift = warehouse ❌
- SQS = queue, not stream ❌

---

## 1️⃣1️⃣ AD SSO to AWS resources

**Correct:** AWS Managed Microsoft AD (trust relationship)

### Clarification
- If LDAP stays on-prem → AD Connector
- If AWS hosts AD → Managed Microsoft AD

---

## 1️⃣2️⃣ POSIX block storage, 1,000 TB, active access

**Correct:** Storage Gateway – Cached Volumes

### Rules
- Cached volumes → up to 1 PB
- Stored volumes → 512 TB max
- Glacier/S3 ❌ (not POSIX/block)

---

## 1️⃣3️⃣ Storage Gateway replay attack protection

**Correct:** CHAP for iSCSI

### Rule
- Replay attacks → challenge/response auth
- CHAP works only with iSCSI

---

## 1️⃣4️⃣ Multiple SSL certs on ONE EC2

**Correct:** Multiple ENIs + multiple EIPs

### Rules
- ENI = virtual NIC
- One EC2 → many ENIs → many certs
- Enhanced networking ≠ ENIs

---

## 1️⃣5️⃣ Blogging platform + CloudFront + lifecycle

**Correct:**
- Single S3 bucket (partitioned)
- CloudFront restricted access

### Why
- Simple
- Cost-effective
- Lifecycle friendly

---

## 1️⃣6️⃣ Revoke STS credentials immediately

**Correct:** IAM → Revoke active sessions (Role)

### Rules
- STS creds auto-expire
- Manual revoke via IAM role
- STS dashboard does NOT exist

---

## 1️⃣7️⃣ 80 TB data migration, slow internet

**Correct:** AWS Snowball Edge

### Rules
- Snowball → TB scale
- Snowmobile → PB scale
- CLI sync ❌ (25 Mbps too slow)

---

## 1️⃣8️⃣ Mobile app → S3 securely (millions of users)

**Correct:** STS / Web Identity Federation

### Rules
- Never store long-term keys in app
- Use temporary credentials
- Cognito or STS federation

---

## 1️⃣9️⃣ Oracle RAC migration + patching + backups

**Correct:** EC2 + SSM Patch Manager + EBS snapshots

### Why
- RAC not supported in RDS
- Systems Manager = least effort for patching

---

## 2️⃣0️⃣ Infrequently accessed DB data (cheap + throughput)

**Correct:** EBS st1 (Throughput Optimized HDD)

### Rules
- sc1 → cheapest, lowest perf
- st1 → sequential throughput
- io1 → expensive ❌

---

## 2️⃣1️⃣ Multi-region EC2 monitoring

**Correct:** Single CloudWatch Dashboard

### Rule
- CloudWatch dashboards are global

---

## 2️⃣2️⃣ Enforce tagging across AWS accounts

**Correct (2):**
- CloudFormation resource tags
- AWS Service Catalog

### Why
- Config = detection, not enforcement
- Billing tags ≠ enforcement

---

## 2️⃣3️⃣ One-time EMR job (48 hrs)

**Correct:**
- Master + Core → On-Demand
- Task → Spot

### Rule
- Master/Core must be stable
- Task nodes are interruptible

---

## 2️⃣4️⃣ EC2 → DynamoDB + tracing

**Correct (2):**
- IAM Role for EC2
- AWS X-Ray daemon

### Never
- IAM users on EC2 ❌
- PowerUserAccess ❌

---

## 2️⃣5️⃣ NAT instance timeouts

### Root cause
- NAT instance connection limits

### Fix
- Replace NAT instance with NAT Gateway

---

## 2️⃣6️⃣ Block malicious IP immediately

**Correct:** Network ACL

### Rules
- NACL = subnet-level, fast deny
- SG = allow-only
- IAM ❌ network protection

---

## 2️⃣7️⃣ AWS Organizations admin control

**Correct:**
- Invite member accounts
- OrganizationAccountAccessRole

### Rule
- Master assumes role → full admin

---

## 2️⃣8️⃣ Highly available city-scale app

**Best:**
- ASG across 3+ AZs
- ALB
- Aurora Multi-Master
- Route 53 Alias

---

## 2️⃣9️⃣ CloudFront private content, same URLs

**Correct:** Signed Cookies

### Rule
- Signed URLs → single object
- Signed Cookies → multiple objects

---

## 3️⃣0️⃣ Third-party access (least privilege + unique ID)

**Correct:** IAM Role + ExternalId condition

### Golden rule
- ExternalId prevents confused deputy attacks

---

## 3️⃣1️⃣ DynamoDB change detection → Lambda

**Correct:** DynamoDB Streams

---

## 3️⃣2️⃣ ElastiCache datastore limitation

### Rule
- ElastiCache supports Redis & Memcached only
- Apache Ignite ❌

---

## 3️⃣3️⃣ Mobile app → S3 + DynamoDB (no backend API)

**Correct:**
- Cognito / STS AssumeRoleWithWebIdentity
- Temporary credentials ONLY

---

## 3️⃣4️⃣ Multi-region failover (short downtime)

**Correct:**
- Route 53 Latency-based routing
- Active-active
- Evaluate target health = YES

---

## 3️⃣5️⃣ Redshift queries hanging

**Correct (3):**
- PG_CANCEL_BACKEND
- STV_LOCKS / STL_TR_CONFLICT
- VACUUM

---

## 3️⃣6️⃣ Blue/Green deployment NOT recommended when

**Correct (2):**
- App must be deployment-aware (feature flags)
- COTS apps with rigid upgrade paths

---

## 3️⃣7️⃣ DynamoDB overload protection

**Correct:** SQS decoupling

---

## 3️⃣8️⃣ LDAP auth over VPN (hybrid)

**Correct (2):**
- Identity broker → STS federation
- App authenticates LDAP → assumes role

---

## 3️⃣9️⃣ Reduce global page load time (<3s)

**Correct (2):**
- CloudFront caching
- ElastiCache

---

## 4️⃣0️⃣ SSE-C implementation

**Required headers (REST):**
- `x-amz-server-side-encryption-customer-algorithm`
- `x-amz-server-side-encryption-customer-key`
- `x-amz-server-side-encryption-customer-key-MD5`

---

## 4️⃣1️⃣ CloudFront origin protocol (HTTP + HTTPS)

**Correct:** Match Viewer

---

## 4️⃣2️⃣ Internal-only app access (no public EC2)

**Correct:**
- SSL VPN
- Private subnet
- Client VPN access

---

## 4️⃣3️⃣ Expand VPC CIDR

**Correct:** Add secondary IPv4 CIDR blocks

---

## 4️⃣4️⃣ RDS Multi-AZ failover

**Correct:**
- CNAME flips to standby
- App reconnects transparently

---

## 4️⃣5️⃣ RDS analytics impact + email dashboard update

**Correct:**
- RDS Read Replicas
- SNS (email)

---

## 🧠 FINAL EXAM TAKEAWAYS (SET 3)

- **STS + federation** solves all hybrid auth
- **CloudFront + OAI / Signed Cookies** = private content
- **Read Replicas** protect OLTP
- **SQS** = decoupling, **SNS** = notification
- **Transit Gateway > VPN CloudHub** (if available)
- **Temporary credentials ALWAYS beat static keys**
