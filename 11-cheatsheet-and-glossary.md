# 11. Cheatsheet, Provider Mapping, Glossary and Study Plan

---

## 1. Service mapping across providers

| Capability | AWS | Azure | GCP |
|---|---|---|---|
| Virtual machines | EC2 | Virtual Machines | Compute Engine |
| Autoscaling VMs | Auto Scaling Groups | VM Scale Sets | Managed Instance Groups |
| Managed Kubernetes | EKS | AKS | GKE |
| Serverless containers | Fargate / App Runner | Container Apps | Cloud Run |
| Functions | Lambda | Functions | Cloud Functions / Cloud Run functions |
| Batch | AWS Batch | Azure Batch | Batch |
| Object storage | S3 | Blob Storage | Cloud Storage |
| Block storage | EBS | Managed Disks | Persistent Disk / Hyperdisk |
| File storage | EFS / FSx | Azure Files / NetApp Files | Filestore |
| Relational (managed) | RDS / Aurora | Azure SQL / Database for PostgreSQL | Cloud SQL / AlloyDB / Spanner |
| NoSQL key-value/doc | DynamoDB | Cosmos DB | Firestore / Bigtable |
| In-memory cache | ElastiCache / MemoryDB | Azure Cache for Redis | Memorystore |
| Data warehouse | Redshift | Synapse / Fabric | BigQuery |
| Search | OpenSearch Service | Azure AI Search | (Elastic on GCP) / Vertex Search |
| Queue | SQS | Service Bus / Storage Queues | Cloud Tasks / Pub/Sub |
| Pub/sub topic | SNS / EventBridge | Service Bus topics / Event Grid | Pub/Sub / Eventarc |
| Stream | Kinesis / MSK | Event Hubs | Pub/Sub / Managed Kafka |
| Workflow | Step Functions | Logic Apps / Durable Functions | Workflows |
| Virtual network | VPC | Virtual Network (VNet) | VPC |
| Load balancer | ALB / NLB | Application Gateway / Load Balancer | Cloud Load Balancing |
| Global routing | Global Accelerator / Route 53 | Front Door / Traffic Manager | Global external LB |
| DNS | Route 53 | Azure DNS | Cloud DNS |
| CDN | CloudFront | Front Door / CDN | Cloud CDN |
| WAF / DDoS | WAF / Shield | WAF / DDoS Protection | Cloud Armor |
| Private service access | PrivateLink | Private Link | Private Service Connect |
| Dedicated connection | Direct Connect | ExpressRoute | Cloud Interconnect |
| IAM | IAM | Entra ID + RBAC | Cloud IAM |
| Secrets | Secrets Manager | Key Vault | Secret Manager |
| Key management | KMS / CloudHSM | Key Vault / Managed HSM | Cloud KMS / HSM |
| Org guardrails | Organizations + SCP | Management groups + Policy | Org Policy |
| Audit logs | CloudTrail | Activity Log | Cloud Audit Logs |
| Monitoring | CloudWatch / X-Ray | Azure Monitor / App Insights | Cloud Operations (Monitoring/Trace/Logging) |
| IaC (native) | CloudFormation / CDK | Bicep / ARM | Deployment Manager / Config Connector |
| CI/CD (native) | CodePipeline / CodeDeploy | Azure DevOps / GitHub Actions | Cloud Build / Cloud Deploy |
| Posture mgmt | Security Hub / GuardDuty | Defender for Cloud | Security Command Center |

> Service names change; verify against current provider docs before relying on exact names.

---

## 2. Decision tables

**Which data store?**

| Need | Start with |
|---|---|
| Transactions, joins, ad-hoc queries | Relational (PostgreSQL / MySQL) |
| Known key access at huge scale, predictable latency | Key-value / document NoSQL |
| Time series, write-heavy | Wide-column / time-series DB |
| Full-text, relevance, faceting | Search engine (secondary index) |
| Analytics over big history | Warehouse / lake |
| Blobs and files | Object storage |
| Ultra-low latency, ephemeral | In-memory cache |

**Which messaging?**

| Need | Use |
|---|---|
| Distribute jobs to workers | Queue |
| Notify many systems | Topic + queue per subscriber |
| Replay, ordering per key, analytics | Stream/log |
| Multi-step process with compensation | Orchestrated workflow |

**Which consistency?** Money/inventory/booking: strong. User-visible but tolerant (feeds, counters): eventual. Own-writes visible: read-your-writes.

**Which deployment?** Risky change: canary + flags. Need instant rollback: blue-green. Simple/low-risk: rolling.

---

## 3. Production-readiness checklist

**Security**
- [ ] Per-object authorization tested; tokens validated fully
- [ ] TLS everywhere; data encrypted at rest; keys managed and rotated
- [ ] Secrets in a secret store; no long-lived cloud keys
- [ ] Least-privilege roles per workload; no `*:*`
- [ ] Dependency + image scanning; SBOM; signed artifacts
- [ ] WAF, rate limiting, input validation, SSRF egress controls
- [ ] Audit logs centralized and immutable; threat model reviewed

**Resilience**
- [ ] Multi-AZ everywhere stateful and stateless
- [ ] Timeouts on every remote call; retries with backoff+jitter on idempotent ops only
- [ ] Circuit breakers/bulkheads for key dependencies; documented fallbacks
- [ ] Queues with DLQs and alarms; consumers idempotent
- [ ] Backups tested; RPO/RTO documented and met in drills
- [ ] Graceful shutdown; readiness vs liveness probes correct
- [ ] Capacity survives loss of one AZ

**Performance**
- [ ] SLOs defined on p95/p99, not averages
- [ ] Load tested to failure; autoscaling verified
- [ ] Caching with stampede protection; hit ratio monitored
- [ ] Indexes reviewed via `EXPLAIN`; no N+1; pagination by keyset
- [ ] Connection pools sized; heavy work async

**Operations and cost**
- [ ] Traces/metrics/logs correlated by trace id; SLO burn alerts with runbooks
- [ ] IaC only; drift detection; policy-as-code in pipeline
- [ ] Canary or blue-green with automated rollback
- [ ] Tags, budgets, anomaly alerts; unit cost tracked
- [ ] Quotas reviewed; alerts at 70-80%

---

## 4. Glossary

| Term | Meaning |
|---|---|
| AZ | Availability Zone: isolated datacenter group within a region |
| BOLA/IDOR | Accessing another user's object by changing an id |
| Bulkhead | Resource isolation so one failure cannot consume everything |
| CAP / PACELC | Consistency vs availability under partition; else latency vs consistency |
| CDC | Change Data Capture: stream of database changes |
| CQRS | Separate write and read models |
| DLQ | Dead-letter queue for messages that repeatedly fail |
| Error budget | Allowed unreliability implied by an SLO |
| Idempotent | Repeating the operation has the same effect as doing it once |
| Jitter | Randomness added to delays to avoid synchronized retries |
| mTLS | Mutual TLS: both sides present certificates |
| Outbox | Pattern for atomic "update DB + publish event" |
| Quorum | Majority agreement (`R + W > N`) |
| RPO / RTO | Max tolerable data loss / downtime |
| SLI / SLO / SLA | Measurement / internal target / contractual promise |
| Saga | Sequence of local transactions with compensations |
| Split brain | Two nodes both believing they are leader |
| Stampede | Many concurrent cache misses hammering the origin |
| STRIDE | Spoofing, Tampering, Repudiation, Info disclosure, DoS, Elevation |
| Zero trust | No implicit trust from network location; verify every request |

---

## 5. Four-week study plan (progressive)

| Week | Focus | Do |
|---|---|---|
| 1 | Big picture + principles (01, 04) | Do the estimation exercises; draw the reference architecture from memory; read ch 10 |
| 2 | Resilience + security (02, 03) | Implement retry+jitter and a circuit breaker; threat-model one of your own apps with STRIDE |
| 3 | Distributed + cloud core (05, 06, 07) | Build a queue + worker with DLQ and idempotency; design a DynamoDB-style table from access patterns |
| 4 | Cloud networking, ops, cost (08, 09) | Sketch a 3-tier VPC with SG rules; write a Terraform module; estimate monthly cost of ch 10's design |

**Daily loop (30 to 45 min):** read one anchor concept through all six levels, redraw its diagram without looking, then answer its self-check questions aloud.

---

## 6. Review questions (mixed)

1. Compute the availability of a request that depends serially on 4 services at 99.95% each.
2. Explain the difference between liveness and readiness probes, and what breaks if liveness checks the database.
3. Why does a CDN with long TTL plus versioned filenames beat purge-based invalidation?
4. Design keys for a chat app in a key-value store: list rooms for a user and last 50 messages per room.
5. What are the five expand/contract steps for renaming a column with zero downtime?
6. When would you choose a stream over a queue? Over a topic?
7. A function-triggered pipeline overloads your DB during spikes. Give three mitigations.
8. Explain "11 nines durability" and why it does not remove the need for backups.
9. List what IAM evaluation does when an allow and an explicit deny both apply.
10. Compare warm standby vs active-active in RPO, RTO, cost and complexity.
11. Why is multi-region usually a business decision rather than a purely technical one?
12. Derive the number of instances needed for 6,000 RPS at 50 ms average latency, 10 concurrent requests per instance (Little's Law), then add AZ-loss headroom.

*(Answer sketch for 12: concurrency = 6,000 x 0.05 = 300; /10 = 30 instances; with 3 AZs and N+1 AZ tolerance, run 45.)*

---

## 7. Where to go deeper

- *Designing Data-Intensive Applications* (Kleppmann): the best single book for chapters 03, 04, 05, 07.
- Google SRE books (free online): SLOs, error budgets, incident response.
- Provider Well-Architected frameworks and reference architectures.
- AWS Builders' Library; Azure Architecture Center; Google Cloud Architecture Center.
- OWASP Top 10, OWASP API Security Top 10, ASVS.
- Papers: Dynamo, Raft ("In Search of an Understandable Consensus Algorithm"), Spanner, Google Dapper.
