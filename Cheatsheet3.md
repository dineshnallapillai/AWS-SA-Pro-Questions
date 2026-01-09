# AWS SET 3 — ULTRA-DENSE EXAM CHEAT SHEET

## 🌐 NETWORKING / CONNECTIVITY

### Multi-site On-prem Connectivity
- **VPN CloudHub** → Low-cost hub-and-spoke VPN (multiple sites)
- **Transit Gateway** → Modern hub for VPC ↔ VPC ↔ On-prem (preferred if option exists)
- **Direct Connect** → High bandwidth, costly (not hub-and-spoke by default)

### Expand VPC CIDR
- ❌ Cannot resize primary CIDR
- ✅ Add secondary IPv4 CIDRs (up to 4)

### Block Malicious IPs FAST
- ✅ Network ACL (subnet-level, stateless, explicit deny)
- ❌ Security Groups (no deny rules)

### NAT Performance Issue
- NAT Instance → timeouts under load
- ✅ Replace with NAT Gateway

---

## 🔐 IDENTITY / AUTH / SECURITY

### Hybrid LDAP / AD Authentication

| Requirement | Service |
|------------|---------|
| On-prem AD auth only | AD Connector |
| AWS-hosted AD + trust | Managed Microsoft AD |
| Custom auth logic | Identity Broker + STS |

### Identity Federation Pattern (LDAP / SAML / OIDC)

```
User → IdP (LDAP/SAML/OIDC)
     → Identity Broker
     → STS
     → Temporary credentials
     → AWS resources
```

- ❌ LDAP users NEVER log into IAM directly

### Third-Party Vendor Access (Least Privilege)
- IAM Role + ExternalId
- Prevents confused deputy attack
- ❌ Never share access keys

### Mobile Apps (Millions of Users)
- ❌ Static access keys in app
- ✅ STS AssumeRoleWithWebIdentity
- ✅ Cognito + IAM Role

### Revoke STS Access Immediately
- IAM → Role → Revoke active sessions
- ❌ STS dashboard does NOT exist

---

## ☁️ S3 / CLOUDFRONT

### Private S3 via CloudFront
- ✅ Origin Access Identity (OAI)
- ❌ IAM users for CloudFront

### Private Content Access

| Use case | Solution |
|----------|----------|
| Single object | Signed URL |
| Multiple objects | Signed Cookies |

### SSE-C (Customer-Provided Keys)

Required headers (REST):
```
x-amz-server-side-encryption-customer-algorithm
x-amz-server-side-encryption-customer-key
x-amz-server-side-encryption-customer-key-MD5
```

---

## ⚡ PERFORMANCE / SCALABILITY

### Clickstream / Real-time Events
- Keyword = "clickstream" → **Kinesis**
- Kinesis Streams + consumers
- ❌ SQS / Redshift for real-time

### Reduce Page Load Time (Cost-Effective)
- ✅ CloudFront caching
- ✅ ElastiCache (sessions / frequent reads)
- ❌ Aggressive Auto Scaling

### DynamoDB Overload Protection
- ✅ SQS decoupling
- ❌ More WCUs as first option

### ElastiCache Limitation
- Only Redis and Memcached
- ❌ Apache Ignite NOT supported

---

## 🗄️ STORAGE / MIGRATION

### Storage Gateway Selection

| Requirement | Gateway |
|------------|---------|
| ≤512 TB, local primary | Stored Volume |
| ≤1 PB, cloud primary | Cached Volume |

### Snow Family

| Data Size | Service |
|-----------|---------|
| TB-scale | Snowball Edge |
| PB-scale | Snowmobile |
| Small + fast net | S3 sync |

### EBS Volume Choice (Cold DB Data)
- **st1** → throughput-optimized HDD
- **sc1** → cheapest, slowest
- **io1** → expensive ❌

---

## 🧠 DATABASE / ANALYTICS

### RDS Multi-AZ Failover
- Automatic
- CNAME flips to standby
- No IP change needed

### Batch Analytics Impacting OLTP
- ✅ RDS Read Replicas
- ✅ SNS email notification
- ❌ Redshift as OLTP DB

### DynamoDB → Lambda Trigger
- ✅ DynamoDB Streams
- ❌ CloudWatch alarms

---

## 🧰 COMPUTE / DEPLOYMENT

### One-time EMR Job (48 hrs)
- Master + Core → On-Demand
- Task → Spot

### EC2 → DynamoDB Access
- ✅ IAM Role (instance profile)
- ❌ IAM users on EC2

### Multi-SSL on ONE EC2
- ✅ Multiple ENIs + multiple EIPs
- ❌ Enhanced networking ≠ ENIs

---

## 🔍 MONITORING / GOVERNANCE

### Multi-Region Monitoring
- ✅ Single CloudWatch Dashboard

### Enforce Resource Tagging (Org-wide)
- ✅ CloudFormation resource tags
- ✅ AWS Service Catalog
- ❌ AWS Config (detects only)

---

## 🚨 DDoS / WAF

| Layer | Service |
|-------|---------|
| L3/L4 floods | Shield Advanced |
| L7 (SQLi/XSS) | AWS WAF |
| Notifications + DRT | Shield Advanced |

---

## 🚀 DEPLOYMENT STRATEGY

### Blue/Green NOT Recommended When:
- App requires deployment awareness / feature flags
- COTS apps with rigid upgrade processes

---

## 🧠 MEMORY HOOKS (EXAM GOLD)

- **Clickstream** → Kinesis
- **Hybrid auth** → STS
- **Mobile app** → Temp creds
- **Block IP** → NACL
- **Private S3** → OAI
- **Least privilege vendor** → ExternalId
- **DB reads spike** → Read Replicas
- **Fast failover** → Route 53 latency
