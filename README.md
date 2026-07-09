# AWS Certified Solutions Architect – Associate (SAA-C03) — Community Question Bank

[![Contributions welcome](https://img.shields.io/badge/contributions-welcome-brightgreen.svg)](CONTRIBUTING.md)
[![License: CC BY-SA 4.0](https://img.shields.io/badge/License-CC%20BY--SA%204.0-lightgrey.svg)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-blue.svg)](CONTRIBUTING.md)

A free, open, community-built bank of **scenario-based practice questions** for the AWS Certified Solutions Architect – Associate (**SAA-C03**) exam — with **detailed answer explanations**, **why-each-distractor-fails** breakdowns, and **rendered diagrams** for every question.

Built for people who want to understand the *reasoning* behind the answer, not just memorize a letter. If you learn something here, **star the repo ⭐, share it, and add a question back**.

> ⚠️ **Disclaimer.** This is an independent, community-run study resource. It is **not affiliated with, endorsed by, or sponsored by Amazon.com, Inc. or Amazon Web Services (AWS)**. "AWS", "Amazon Web Services", and "AWS Certified Solutions Architect – Associate" are trademarks of Amazon.com, Inc. or its affiliates. Questions here are original practice items, **not** real exam questions. AWS services change often — always verify time-sensitive details against the [official AWS documentation](https://docs.aws.amazon.com/).

---

## What's inside

```
├── questions/                       # the question bank, organised by exam domain
│   ├── domain-1-security.md         # Design Secure Architectures (30%)
│   ├── domain-2-resilient.md        # Design Resilient Architectures (26%)
│   ├── domain-3-performance.md      # Design High-Performing Architectures (24%)
│   └── domain-4-cost-optimization.md# Design Cost-Optimized Architectures (20%)
├── prompts/
│   └── question-generator.md        # the prompt used to draft new questions
├── docs/                            # maintainer launch aids (welcome post, starter issues)
├── QUESTION_TEMPLATE.md             # the required format for every question
├── CONTRIBUTING.md                  # how to add questions or fix errors
└── CODE_OF_CONDUCT.md
```

**Launching with 20 questions across all four domains.** Maintainers: see [`docs/welcome-post.md`](docs/welcome-post.md) for a pinnable welcome message and [`docs/good-first-issues.md`](docs/good-first-issues.md) for ready-to-open starter issues.

## How to use it

1. Open a domain file under [`questions/`](questions/).
2. Read the scenario and attempt the question **before** revealing the answer — each answer is inside a collapsible `▶ Reveal` block so you can self-test.
3. Read the full explanation: *why the right answer is right*, *why each wrong option fails*, the *real-world trap*, and any *fact that has changed over time*.
4. Study the diagram to lock in the architecture.

**Viewing the diagrams:** they use [Mermaid](https://mermaid.js.org/), which **GitHub renders automatically**. In other editors, use a Mermaid-aware viewer or paste a block into [mermaid.live](https://mermaid.live).

## What makes these questions exam-grade

Every question follows the same quality bar (see [`QUESTION_TEMPLATE.md`](QUESTION_TEMPLATE.md)):

- **A decisive qualifier** in the stem (*MOST cost-effective*, *LEAST operational overhead*, *HIGHEST availability* …) so that among several technically-valid options, only one is *best*.
- **Distractors that fail on one precise, identifiable reason** — the wrong tool, an insecure exposure, higher operational overhead, or an outdated behavior — not obviously silly options.
- **A rendered diagram** (never ASCII) and, where useful, a ❌ contrast diagram of the rejected approach.
- **Time-sensitive facts flagged with dates**, so stale study material is easy to spot.

## Contributing

New questions, corrections, clearer explanations, and better diagrams are all welcome — beginners included. Start here:

- 📥 **Add a question** or 🐞 **report an error** → [open an issue](../../issues/new/choose)
- 🔧 **Contribute directly** → read [`CONTRIBUTING.md`](CONTRIBUTING.md), then open a Pull Request using [`QUESTION_TEMPLATE.md`](QUESTION_TEMPLATE.md)
- 💬 **Ask or discuss** → use the **Discussions** tab
- 🌱 Look for issues labelled **`good first issue`** to get started

## Roadmap

- [ ] Grow to **25+ questions per domain**
- [ ] Add a **quick-revision cheat-sheet** per domain (services + when to use)
- [ ] Tag questions by service (VPC, S3, KMS, RDS…) for focused study
- [ ] Optional: publish as a searchable static site (GitHub Pages)
- [ ] Optional: an interactive quiz mode

## License & attribution

Educational content (questions, explanations, diagrams) is licensed under **[CC BY-SA 4.0](LICENSE)** — share and adapt freely **with attribution**, and keep derivatives under the same license. Any scripts are additionally available under the MIT License.

## Acknowledgements

Thank you to everyone who contributes a question, a fix, or a clearer explanation. This resource is only as good as the community that builds it. 🙌
