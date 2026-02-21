# TOP 100 AWS SOLUTIONS ARCHITECT EXAM TRAPS

## 🖥 1️⃣ Compute & Scaling Traps (1–15)

1. EBS must be in same AZ as EC2. Not subnet. Not VPC.
2. You can attach EBS to a running instance.
3. Auto Scaling does NOT guarantee Multi-AZ unless configured.
4. ELB does not control outbound traffic.
5. Internet Gateway has no public IP.
6. NAT instance can bottleneck traffic.
7. NAT Gateway scales; NAT instance does not.
8. Spot instances are not for critical workloads.
9. Provisioned Concurrency does NOT solve DB connection issues.
10. Lambda scales by creating execution environments → can exhaust DB connections.
11. Move DB connection outside Lambda handler to reuse.
12. Reserved concurrency limits Lambda, not traffic rate.
13. EC2 public IP changes on stop/start (unless Elastic IP).
14. Use Elastic IP for static outbound whitelisting.
15. Do not place backend instances in public subnet unless required.

---

## 📦 2️⃣ Storage & S3 Traps (16–30)

**Amazon S3**

16. SSL encrypts in transit only.
17. SSE-KMS encrypts at rest.
18. S3 Cross-Region Replication requires versioning.
19. Glacier is not for active streaming.
20. EBS is not scalable object storage.
21. EFS is not a replacement for S3.
22. Use S3 Gateway Endpoint to remove NAT charges.
23. S3 event notifications can trigger SQS, Lambda, SNS.
24. Do not replicate S3 using Lambda if CRR exists.
25. Lifecycle policies reduce storage cost.
26. S3 bucket names are global.
27. S3 private bucket cannot be accessed via CloudFront without proper OAI/OAC.
28. Gateway endpoint is for S3 & DynamoDB only.
29. Interface endpoints are required for most other AWS services.
30. Glacier retrieval is not instant.

---

## 🧠 3️⃣ DynamoDB & Database Traps (31–45)

**Amazon DynamoDB**

31. On-demand mode is for unpredictable workloads.
32. Provisioned + Auto Scaling for predictable workloads.
33. RPO 24h does not require Global Tables.
34. Global Tables increase write cost.
35. Use DAX only for read-heavy latency improvement.
36. DynamoDB Streams used for near-real-time replication.
37. RDS read replica handles reads, not writes.
38. Aurora preferred for high availability.
39. RDS Proxy solves connection storms.
40. Cluster endpoint handles writes.
41. Reader endpoint distributes reads.
42. Multi-AZ ≠ Multi-Region.
43. Snapshot does not equal HA.
44. BASE model allows asynchronous processing.
45. SQS is best for write decoupling.

---

## 🌐 4️⃣ Networking & Hybrid Traps (46–60)

**Amazon VPC**

46. Private hosted zone only works in associated VPC.
47. Cross-account association requires authorization step.
48. Promiscuous mode not supported in VPC.
49. Security groups are stateful.
50. NACLs are stateless.
51. VPC peering is not transitive.
52. PrivateLink is for SaaS consumption.
53. Do not use VPC peering for SaaS.
54. Public VIF is not for VPC connectivity.
55. Use Direct Connect Gateway for multi-Region VPC access.
56. Keep VPN active during DX migration.
57. BGP determines routing preference.
58. NAT centralizes outbound IP.
59. IGW does not perform NAT.
60. Route table errors cause complete failure, not partial degradation.

---

## 🔐 5️⃣ Security & Identity Traps (61–75)

**AWS IAM Identity Center**

61. IAM Identity Center is required for multi-account SSO.
62. Do not create IAM users for each account in Organizations.
63. Use SAML for AD federation.
64. SCIM automates provisioning.
65. ABAC reduces policy sprawl.
66. STS temporary credentials are required for mobile apps.
67. Never embed IAM keys in mobile apps.
68. Login with Amazon is authentication, not AWS authorization.
69. IAM roles are preferred over IAM users.
70. Use least privilege via IAM policy conditions.
71. Use API Gateway usage plans for per-client throttling.
72. 429 errors are preferable to backend 5xx failures.
73. ELB does not provide IDS/IPS.
74. SSL termination should happen at ELB, not EC2.
75. Inspector scans vulnerabilities; it does not patch systems.

---

## 🔔 6️⃣ Serverless & Event Traps (76–85)

**Amazon Simple Queue Service**

76. SQS decouples producers and consumers.
77. Use Spot for SQS batch workers.
78. Lambda is ideal for event-driven processing.
79. HTTP API is cheaper than REST API.
80. API Gateway throttling protects backend.
81. CloudWatch alarms can trigger rollback in CodeDeploy.
82. Canary deployment reduces blast radius.
83. Do not manually swap API endpoints for rollback.
84. CloudFront origin failover is better than app-level failover.
85. CloudFront is global; S3 is regional.

---

## 🏛 7️⃣ Governance & Multi-Account Traps (86–95)

**AWS Organizations**

86. SCPs restrict permissions but do not grant them.
87. StackSets must be created in management account for service-managed mode.
88. Enable automatic deployment for new accounts.
89. CUR should be generated from management account.
90. Use QuickSight for cost dashboards.
91. Do not deploy separate stacks manually in each account.
92. Private Marketplace control requires SCP.
93. Trusted Advisor does not analyze on-prem workloads.
94. Systems Manager Patch Manager works for hybrid servers.
95. Use Hybrid Activation for on-prem SSM.

---

## 🎥 8️⃣ Media & AI Traps (96–100)

**Amazon Elastic Transcoder**  
**Amazon Rekognition**

96. Do not build custom transcoding cluster if managed service exists.
97. Store media in S3, not EBS.
98. Use lifecycle to archive originals.
99. Serve via CloudFront.
100. Glacier cannot be used as streaming origin.

---

## 🚨 10 SUPER TRAP PATTERNS (Memorize These)

1. **IGW does not have an IP**
2. **ELB does not control outbound traffic**
3. **Provisioned Concurrency ≠ DB fix**
4. **Multi-AZ ≠ Multi-Region**
5. **Snapshot ≠ HA**
6. **Authentication ≠ Authorization**
7. **Trusted Advisor ≠ Migration planning**
8. **Inspector ≠ Patch Manager**
9. **VPC Peering ≠ SaaS private connectivity**
10. **Glacier ≠ Active storage**
