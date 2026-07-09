# Domain 1 — Design Secure Architectures (30%)

---

## Q1 — Private, encrypted S3 access for PHI  *(Multiple response — choose TWO)*
**Domain:** 1 — Design Secure Architectures · **Difficulty:** 🟡 Medium · **Concept:** Scope S3 access to a single VPC and enforce encryption on upload.

**Scenario:** A healthcare analytics company stores protected health information (PHI) in an Amazon S3 bucket. A fleet of EC2 instances in a **private subnet with no internet gateway and no NAT** processes these objects. Compliance mandates that (1) traffic to the bucket must **never traverse the public internet**, and **only this application's VPC** may reach the bucket; and (2) every object must be **encrypted at rest**, and any upload that does not request server-side encryption must be **rejected**. The team wants the **LEAST operational overhead**.

**Question:** Which **TWO** actions together satisfy the requirements with the LEAST operational overhead? **(Choose TWO.)**

**Options:**
- A. Create a **gateway VPC endpoint for S3** and attach a bucket policy that **denies any request where `aws:sourceVpce` is not the endpoint ID**.
- B. Route the instances' S3 traffic through a **NAT gateway** in a public subnet so they reach S3 "privately."
- C. Enable **S3 Block Public Access** and rely on it to restrict access to only the application's VPC.
- D. Attach a bucket policy that **denies `s3:PutObject` when the SSE request header is absent**, relying on S3's default SSE-S3 for encryption at rest.
- E. Create an **interface VPC endpoint** for S3 and pair it with a **public bucket ACL** so the instances can reach it.
- F. Generate **presigned URLs** for every object and distribute them to the instances.

<details>
<summary>▶ Reveal answer &amp; explanation</summary>

**✅ Correct answers: A and D**

**Concept tested:** Combining a *gateway VPC endpoint* (private path + `aws:sourceVpce` policy condition) with an *upload-time encryption guardrail*.

**Why A and D are correct:**
- **A** keeps traffic on the AWS private network (no internet path) *and* — via the `aws:sourceVpce` deny condition — ensures **only requests originating from this VPC's endpoint** can access the bucket. Gateway endpoints for S3 are added to the route table via a managed prefix list, cost nothing, and require no appliances → minimal ops. Satisfies requirement 1.
- **D** enforces requirement 2: the policy **rejects any PUT that doesn't request encryption**, and S3's default SSE-S3 guarantees objects land encrypted. Low overhead, no key management to run.

**Why the others fail:**
- **B:** A NAT gateway still egresses to the **public S3 endpoint over the internet** — it does not create a private path and does not scope access to the VPC. It also adds hourly + data-processing cost.
- **C:** Block Public Access stops *public* ACLs/policies; it does **not** restrict access to a specific VPC. A good baseline, but it doesn't meet requirement 1.
- **E:** Contradicts itself — a **public bucket ACL** violates "only this VPC," and for in-VPC S3 access the **gateway** endpoint is the standard (free) choice.
- **F:** Presigned URLs per object is enormous operational overhead and still doesn't confine access to the VPC.

**Real-world nuance / trap:** Candidates often pick Block Public Access as the "security" answer, but it's a *public* guardrail, not a network-scoping control. Confining access to a VPC is the job of the **gateway endpoint + `aws:sourceVpce`** condition.

**Time-sensitive note:** Since **January 5, 2023**, S3 applies **SSE-S3 to all new object uploads by default**, and encryption for new uploads **can no longer be disabled** ([AWS docs](https://docs.aws.amazon.com/AmazonS3/latest/userguide/default-encryption-faq.html)). So the policy in **D** now serves to *enforce a required encryption header / specific type* and reject non-conforming requests — exactly the compliance ask. (Separately, as of **April 6, 2026**, S3 disables **SSE-C** by default on new buckets.)

**Well-Architected pillar:** Security.

**Diagram — correct architecture:**
```mermaid
flowchart TB
    subgraph AWS["AWS Cloud (private backbone)"]
        subgraph VPC["Application VPC 10.0.0.0/16"]
            subgraph Priv["Private Subnet (no IGW / no NAT)"]
                EC2["EC2 Processing Fleet"]
            end
            GWEP{{"Gateway VPC Endpoint<br/>for Amazon S3"}}
        end
        S3[("S3 Bucket — PHI<br/>SSE-S3 by default")]
        POL["Bucket policy:<br/>Deny unless aws:sourceVpce = vpce-xxxx<br/>Deny PutObject missing SSE header"]
    end
    EC2 -->|"HTTPS over AWS network"| GWEP
    GWEP -->|"S3 prefix-list route"| S3
    POL -->|enforces| S3
    classDef net fill:#1a73c2,color:#fff,stroke:#0b3d66
    classDef store fill:#2e7d32,color:#fff,stroke:#14401a
    classDef comp fill:#e37400,color:#fff,stroke:#5c2c00
    classDef sec fill:#b3261e,color:#fff,stroke:#5c120d
    class GWEP net
    class S3 store
    class EC2 comp
    class POL sec
```

**Diagram — ❌ rejected NAT approach (Option B):**
```mermaid
flowchart LR
    EC2["EC2 (private subnet)"] --> NAT["NAT Gateway (public subnet)"]
    NAT --> IGW["Internet Gateway"]
    IGW -->|"public S3 endpoint over the internet"| S3[("Amazon S3")]
    classDef bad fill:#b3261e,color:#fff,stroke:#5c120d
    class NAT,IGW bad
```
*Traffic leaves to the public S3 endpoint and access is not confined to the VPC — fails requirement 1.*

</details>

---

## Q2 — Cross-account decryption with a customer managed KMS key
**Domain:** 1 — Design Secure Architectures · **Difficulty:** 🔴 Very Hard · **Concept:** The two-part KMS permission model for cross-account access.

**Scenario:** **Account A** owns a **customer managed KMS key** that encrypts objects in an S3 bucket in Account A. An application running under an **IAM role in Account B** must **decrypt** those objects. The security team created the role in Account B and added **`kms:Decrypt`** (scoped to the key's ARN) to that role's **identity-based IAM policy**. Decryption still fails with **`AccessDenied`**.

**Question:** What is required to grant Account B's role access **with least privilege**, and why did the current setup fail?

**Options:**
- A. Nothing more is needed — adding `kms:Decrypt` to the **Account B role's IAM policy alone** is sufficient for cross-account KMS access.
- B. **Also update the KMS key policy in Account A** to allow the Account B principal to call `kms:Decrypt` — cross-account KMS use requires **BOTH** the **key policy** (in the key's account) **AND** an **IAM policy** (in the caller's account) to allow the action.
- C. Switch the bucket to the **AWS managed key `aws/s3`** and grant Account B access to that key.
- D. Create an **IAM user with long-term access keys in Account A** and share those keys with Account B to perform the decryption.

<details>
<summary>▶ Reveal answer &amp; explanation</summary>

**✅ Correct answer: B**

**Concept tested:** For **cross-account** KMS, permission must be granted in **two places** — the **key's resource policy** *and* the **caller's identity policy**. Neither alone is enough.

**Why B is correct:** A KMS key policy is the **primary access-control document** for the key. For a principal in **another account** to use the key, the **key policy in the owning account (A)** must explicitly allow that external principal (commonly by trusting Account B's root and letting B's admins delegate via IAM), **and** the calling principal's **IAM policy in Account B** must allow the action. The current setup only did the Account B side — so KMS denies. Granting exactly `kms:Decrypt` (plus, for S3, typically `kms:GenerateDataKey` on writes) to the specific role keeps it **least privilege**.

**Why the others fail:**
- **A:** This is precisely what they already did, and it **fails**. Unlike same-account access (where the key policy can defer to IAM), cross-account access **requires the key policy to allow the external principal** as well.
- **C:** You **cannot edit the key policy of AWS managed keys** (like `aws/s3`), so you can't grant another account access to them — AWS managed keys **can't be shared cross-account**.
- **D:** Sharing **long-term access keys** across accounts is an insecure anti-pattern that violates least privilege and creates unmanaged credentials.

**Real-world nuance / trap:** The pervasive misconception is that an **IAM allow is sufficient**. It is — but **only within the same account**. The moment the principal is in a different account, the **key policy must independently authorize it**. (AWS *grants* are a third, fine-grained option, but the two-policy model is the core point.)

**Time-sensitive note:** Option C's key is an **AWS managed key**, whose rotation you can't control — AWS **rotates AWS managed keys yearly**, a schedule that **changed from every ~3 years to every ~1 year in May 2022**. Contrast with **customer managed keys**, which since **April 2024** support **configurable rotation periods of 90–2,560 days (up to 7 years)** plus **on-demand rotation** ([AWS announcement](https://aws.amazon.com/about-aws/whats-new/2024/04/aws-kms-automatic-key-rotation/)).

**Well-Architected pillar:** Security.

**Diagram — correct request/authorization flow:**
```mermaid
sequenceDiagram
    participant R as Account B Role
    participant S3 as S3 (Account A bucket)
    participant KMS as AWS KMS (Account A CMK)
    R->>S3: GetObject (SSE-KMS encrypted)
    S3->>KMS: Decrypt (on behalf of caller)
    Note over KMS: Check 1 — Key policy in Account A<br/>allows the Account B principal?
    Note over KMS: Check 2 — Caller's IAM policy in<br/>Account B allows kms:Decrypt?
    alt Both allow
        KMS-->>S3: Plaintext data key
        S3-->>R: Decrypted object ✅
    else Either missing (their case: key policy grant absent)
        KMS-->>R: AccessDenied ❌
    end
```

</details>

---
