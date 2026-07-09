# `good first issue` starter pack

Ten beginner-friendly questions to seed the issue tracker. **Open each as a separate GitHub Issue**, apply the **`good first issue`** and **`new-question`** labels, and let contributors claim one by commenting. Each is scoped to a single, well-known concept so a newcomer can write it using [`QUESTION_TEMPLATE.md`](../QUESTION_TEMPLATE.md).

> Tip: create the `good first issue` label first (Issues → Labels → New label), then open these. You can copy each block's **Title** and **Body** directly.

---

### 1. Title: Add a question — SQS standard vs FIFO queue
**Body:** Write a Domain 2 question where a workload needs **exactly-once, strictly ordered** processing. Correct answer: **FIFO queue**. Distractors: standard queue, SNS, Kinesis. Explain the ordering/de-dup guarantee and the throughput trade-off. Include a Mermaid diagram. Difficulty: 🟡 Medium.

### 2. Title: Add a question — S3 storage class for long-term archival (Glacier Deep Archive)
**Body:** Domain 4. Data must be retained for years, accessed **rarely**, retrieval in hours is acceptable, at the **lowest storage cost**. Correct: **S3 Glacier Deep Archive**. Distractors: Standard-IA, Intelligent-Tiering, One Zone-IA. Note retrieval time/cost. Diagram of the lifecycle. Difficulty: 🟡 Medium.

### 3. Title: Add a question — Route 53 routing policy for blue/green weighted rollout
**Body:** Domain 3 or 2. Gradually shift a percentage of traffic to a new stack. Correct: **weighted routing**. Distractors: failover, latency, geolocation. Explain each policy's purpose. Sequence or flow diagram. Difficulty: 🟡 Medium.

### 4. Title: Add a question — EFS vs EBS vs S3 for a shared file system across many instances
**Body:** Domain 3. Multiple EC2 instances across AZs need a **shared, POSIX file system**. Correct: **Amazon EFS**. Distractors: EBS (single-attach), S3 (object, not file system), instance store. Diagram with instances mounting EFS across AZs. Difficulty: 🟡 Medium.

### 5. Title: Add a question — ALB vs NLB vs Gateway Load Balancer
**Body:** Domain 3. Choose the load balancer for an **ultra-low-latency, millions-of-requests TCP** workload with a **static IP**. Correct: **NLB**. Distractors: ALB (L7 HTTP), GWLB (inline appliances), Classic LB. Explain the L4/L7 distinction. Diagram. Difficulty: 🟠 Hard.

### 6. Title: Add a question — IAM role for EC2 instead of embedding access keys
**Body:** Domain 1. An app on EC2 needs to call S3. Correct: attach an **IAM role (instance profile)** so it uses temporary credentials. Distractors: hardcode keys, store keys in user data, IAM user per instance. Explain temporary credentials. Diagram. Difficulty: 🟢 Easy–Medium.

### 7. Title: Add a question — Aurora Serverless v2 for spiky/unpredictable database load
**Body:** Domain 4/3. A dev/test or spiky workload should **scale capacity automatically** and avoid paying for idle. Correct: **Aurora Serverless v2**. Distractors: provisioned Aurora, largest instance, Multi-AZ. Note the auto-scaling capacity model. Diagram. Difficulty: 🟡 Medium.

### 8. Title: Add a question — CloudFront to reduce origin load and egress cost
**Body:** Domain 4/3. A site serves the **same cacheable assets** to global users, straining the origin and running up egress. Correct: **CloudFront** in front of the origin. Distractors: bigger origin, Global Accelerator, S3 Transfer Acceleration. Explain caching + cost. Diagram. Difficulty: 🟡 Medium.

### 9. Title: Add a question — cross-account access with an IAM role + external ID (confused deputy)
**Body:** Domain 1. A **third-party SaaS** needs access to your account. Correct: a **cross-account IAM role** the vendor assumes, protected with an **external ID**. Distractors: share access keys, make resources public, share root. Explain the confused-deputy problem. Sequence diagram. Difficulty: 🟠 Hard.

### 10. Title: Add a question — Kinesis Data Streams vs SQS for real-time, multiple-consumer streaming
**Body:** Domain 3. Many consumers must independently read an **ordered, replayable** real-time event stream. Correct: **Kinesis Data Streams**. Distractors: SQS standard, SNS, single-consumer queue. Explain shards, ordering, retention/replay. Diagram. Difficulty: 🟠 Hard.

---

**Reviewer note:** when merging these, keep correct-answer letters varied across the bank (avoid clustering on "A"), and require a source link for any time-sensitive fact.
