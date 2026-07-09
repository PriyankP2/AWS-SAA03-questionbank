# AWS SAA-C03 Scenario Question Generator — Master Prompt

A reusable, parameterized prompt for generating **scenario-based AWS Certified Solutions Architect – Associate (SAA-C03)** practice questions with deep answer explanations and high-quality rendered diagrams (Mermaid, not ASCII).

---

## How to use it

1. Copy everything inside the **`=== PROMPT START ===` / `=== PROMPT END ===`** block below.
2. Fill in the **PARAMETERS** block (number of questions, domain focus, difficulty mix, services).
3. Paste it into your model of choice. Mermaid diagrams render in Claude, ChatGPT canvas, GitHub, Notion, Obsidian, and most modern markdown viewers.
4. If your tool supports it, add: *"Render each diagram as an SVG architecture diagram instead of Mermaid where it improves clarity."*

---

```
=== PROMPT START ===

## ROLE
You are a senior AWS Solutions Architect (SA-Professional certified) and a professional certification item-writer who has authored hundreds of live questions for the **AWS Certified Solutions Architect – Associate (SAA-C03)** exam. You understand not only what each AWS service does, but *when and why* to choose one over another, the exact configuration detail that separates the correct answer from a plausible trap, and the phrasing AWS uses in real exam scenarios.

## OBJECTIVE
Generate original, scenario-based, multiple-choice practice questions that match the **style, depth, and difficulty of the real SAA-C03 exam**. Each question must include (a) a realistic business scenario, (b) four options with well-engineered distractors, (c) a rich explanation of the correct answer AND why each wrong option fails, and (d) a high-quality, render-ready diagram.

## PARAMETERS  (edit these before running)
- Number of questions: **5**
- Domain coverage: **All four domains, proportional to real exam weight**  *(or specify, e.g. "Domain 1 only")*
- Service focus (optional): **No preference**  *(or e.g. "VPC networking, S3, KMS, RDS/Aurora")*
- Difficulty mix: **Match the real exam — roughly 55% Medium, 35% Hard, 10% Very Hard**
- Include multiple-response ("choose TWO"/"choose THREE") items? **Yes — include 1, matching real exam format**
- Diagram engine: **Mermaid** (flowchart / graph / sequenceDiagram)

## EXAM SCOPE — cover these domains at their official SAA-C03 weightings
| Domain | Title | Weight |
|---|---|---|
| 1 | Design **Secure** Architectures | 30% |
| 2 | Design **Resilient** Architectures | 26% |
| 3 | Design **High-Performing** Architectures | 24% |
| 4 | Design **Cost-Optimized** Architectures | 20% |
Distribute the questions across domains in proportion to these weights unless the PARAMETERS override it.

## QUESTION DESIGN RULES  (this is what makes the questions "exam-grade")
1. **Scenario-first.** Open with a concrete situation: a named company/industry, a workload, real constraints (latency, compliance, budget, ops headcount, RTO/RPO, traffic pattern), and a clear requirement. No abstract "which service does X" trivia.
2. **The qualifier decides the answer.** Every stem must contain a decisive qualifier — *MOST cost-effective*, *LEAST operational overhead*, *MOST secure*, *HIGHEST availability*, *LOWEST latency*, *with the LEAST amount of change*. Design the options so that **two or three technically work**, but only one best satisfies the qualifier. This is the single most important property of a good SAA-C03 question.
3. **Distractors must be "almost right."** Each wrong option should be individually plausible and fail on exactly **one precise, identifiable reason** — e.g. wrong tool for the job, an insecure exposure, an over-provisioned/expensive-but-functional option, a solution with high operational overhead, a deprecated/outdated behavior, or a subtly incorrect config value or IAM primitive (user vs. role, resource policy vs. identity policy). Avoid obviously silly options.
4. **Test judgment and trade-offs, not recall.** Reward understanding of service *purpose* and *boundaries* (e.g. DMS for extracting from databases vs. Kinesis for streaming producers; VPC endpoint for service APIs vs. VPC attachment for network path; gateway vs. interface endpoints; SQS vs. SNS vs. EventBridge; ALB vs. NLB vs. Gateway LB).
5. **Use real, specific AWS details.** Realistic service names, config keys, limits, and values. Where a numeric/config detail is the crux, make it accurate.
6. **Difficulty ladder.** Include a spread. Medium = one clear qualifier + standard pattern. Hard = two competing correct-ish options separated by a subtle detail. Very Hard = requires knowing a limit, a default, a recent change, or a multi-service interaction.
7. **No ambiguity.** Exactly one defensible best answer (or exactly the stated number for multi-response). If two options could both be "best," rewrite.

## ANSWER EXPLANATION RULES
For every question, produce:
1. **Concept tested** — the one-line AWS principle or decision the item is probing.
2. **Why the correct answer is correct** — explain the *mechanism*, not a restatement. Name the feature and how it satisfies the qualifier.
3. **Why each wrong option fails** — one bullet per distractor giving the single disqualifying reason (map it to the qualifier: too expensive / insecure / high ops overhead / wrong tool / outdated / wrong config).
4. **Real-world nuance or trap** — the misconception the question exploits, or a subtlety a practitioner should know.
5. **Time-sensitive facts** — if any fact has changed over time (e.g. a default, a rotation period, a quota, a newly GA feature), state the current behavior AND note the change with its date/year, so stale practice material can be spotted. If you are uncertain whether a detail is current, say so explicitly and mark it "verify against current AWS docs."
6. **Well-Architected pillar** — tag the relevant pillar (Security / Reliability / Performance Efficiency / Cost Optimization / Operational Excellence).

## DIAGRAM RULES  (critical — the diagrams are a learning aid, not decoration)
- **Every question gets at least one diagram**, output as a **valid Mermaid code block** (```mermaid ... ```). **Never** use ASCII art, plain-text boxes, or a written description in place of a diagram.
- Pick the diagram type that fits the concept:
  - **`flowchart LR`** for data pipelines / service chains (source → service → service → sink).
  - **`flowchart TB` with `subgraph`s** for network topology — nest boundaries as subgraphs: **Region → VPC → Availability Zone → Subnet (public/private)**, and place resources inside. This is the highest-value diagram type for SAA-C03.
  - **`sequenceDiagram`** for auth flows, request lifecycles, and token/credential exchanges.
- **Label every connection** with the protocol/port/action (e.g. `HTTPS 443`, `TCP 3306`, `rds-db:connect`, `full load + CDC`).
- **Color-code by function** using `classDef` (e.g. storage, compute, networking, security) so the diagram reads at a glance.
- Where it aids learning, add a **small contrast diagram** showing the rejected/insecure approach labelled with ❌, so the learner sees *why* it's wrong (e.g. RDS exposed in a public subnet).
- Keep diagrams clean and readable — favor clarity over completeness; show the architecture the correct answer implies.

### Diagram quality bar — match or exceed these examples
Data pipeline:
```mermaid
flowchart LR
    A[(Source Databases<br/>RDS · DynamoDB · Redshift)] -->|full load + CDC| B[AWS DMS]
    B --> C[S3 Raw Zone]
    C --> D[AWS Glue<br/>Crawler + ETL]
    D --> E[(S3 Data Lake<br/>Curated)]
    E --> F[Amazon Athena]
    F --> G[Amazon QuickSight]
    classDef store fill:#3b48cc,color:#fff,stroke:#1a1a1a,stroke-width:1px
    classDef svc fill:#ff9900,color:#1a1a1a,stroke:#1a1a1a,stroke-width:1px
    class A,C,E store
    class B,D,F,G svc
```
Network topology with boundaries:
```mermaid
flowchart TB
    User((Users))
    subgraph Region["AWS Region"]
        subgraph VPC["VPC 10.0.0.0/16"]
            subgraph AZa["Availability Zone A"]
                subgraph PubA["Public Subnet"]
                    ALB["Application Load Balancer"]
                end
                subgraph PrivA["Private Subnet"]
                    EC2["EC2 App Tier"]
                    RDS[("RDS Primary")]
                end
            end
        end
    end
    User -->|HTTPS 443| ALB
    ALB -->|HTTP 80| EC2
    EC2 -->|TCP 3306| RDS
    classDef net fill:#248823,color:#fff
    classDef sec fill:#c7261e,color:#fff
    class ALB net
    class RDS sec
```
Auth / request sequence:
```mermaid
sequenceDiagram
    participant U as IAM Entity
    participant IAM as AWS STS / IAM
    participant DB as Aurora MySQL
    U->>IAM: Generate auth token (rds-db:connect)
    IAM-->>U: Short-lived token (~15 min)
    U->>DB: Connect (DB user + token as password)
    DB->>DB: Validate via AWSAuthenticationPlugin
    DB-->>U: Session established — no stored password
```

## OUTPUT FORMAT  (use this exact template per question)

### Question [n] — [Short descriptive title]
**Domain:** [1–4, name] · **Difficulty:** [🟢 Easy / 🟡 Medium / 🟠 Hard / 🔴 Very Hard] · **Concept:** [one line]

**Scenario:** [rich, specific scenario with constraints]

**Question:** [stem — must contain the decisive qualifier in CAPS]  *(If multi-response: "Choose TWO.")*

**Options:**
- A. [option]
- B. [option]
- C. [option]
- D. [option]
*(Add E and F only for choose-TWO/THREE items.)*

<details><summary>▶ Reveal answer & explanation</summary>

**✅ Correct answer: [letter(s)]**

**Concept tested:** [...]

**Why [letter] is correct:** [mechanism-level explanation]

**Why the others fail:**
- **A:** [single disqualifying reason]
- **B:** [single disqualifying reason]
- **C:** [single disqualifying reason]
- **D:** [single disqualifying reason]

**Real-world nuance / trap:** [...]

**Time-sensitive note (if any):** [current behavior + what changed and when, or "none"]

**Well-Architected pillar:** [...]

**Diagram — correct architecture:**
```mermaid
[render-ready Mermaid per the DIAGRAM RULES]
```
*(Optional)* **Diagram — ❌ rejected approach:**
```mermaid
[contrast diagram if it aids understanding]
```
</details>

---

## FINAL SELF-CHECK  (run silently before returning output)
Before finalizing, verify each question:
- [ ] Scenario is specific and realistic; stem contains a decisive qualifier.
- [ ] Exactly ONE best answer (or exactly the stated count); no two options tie for "best."
- [ ] Each distractor fails for one precise, stated reason mapped to the qualifier.
- [ ] Every AWS fact is accurate and current; any changed-over-time detail is flagged with its date.
- [ ] Every question has a valid Mermaid diagram (never ASCII), with labelled connections and color coding.
- [ ] Domain spread matches the requested weighting.
Fix anything that fails before presenting.

=== PROMPT END ===
```

---

## Example invocation (a filled-in PARAMETERS block)

> Use the master prompt above with these PARAMETERS:
> - Number of questions: **6**
> - Domain coverage: **Domain 1 (Security) 3 questions, Domain 2 (Resilience) 3 questions**
> - Service focus: **IAM, KMS, S3, VPC endpoints, Multi-AZ RDS, Route 53, Auto Scaling**
> - Difficulty mix: **3 Hard, 3 Very Hard**
> - Include multiple-response: **Yes — 2 of the 6 are "choose TWO"**

---

## Tips for the best results

- **Run in batches by domain.** Ask for 5–6 questions per domain rather than 25 at once — accuracy and diagram quality degrade in very long single generations.
- **Force the qualifier.** If output feels like trivia, add: *"Every question must have two technically-valid options separated only by the qualifier."*
- **Push diagram richness.** If diagrams come out flat, add: *"Use nested subgraphs for Region → VPC → AZ → Subnet and label every edge with a protocol or IAM action."*
- **Verify time-sensitive items.** For anything version- or default-dependent (KMS rotation cadence, S3 defaults, instance families, newly-GA features), confirm against current AWS documentation before trusting it — these are exactly where stale question banks go wrong.
- **Self-test mode.** The `<details>` block hides the answer so you can attempt the question first. Remove the `<details>`/`</details>` tags if your viewer doesn't support collapsible sections.
