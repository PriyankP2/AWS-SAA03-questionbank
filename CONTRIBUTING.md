# Contributing

Thanks for helping build this community question bank! Whether you're adding your first question or fixing a typo, you're welcome here. This guide keeps everything consistent and high quality.

## Ways to contribute

| I want to… | Do this |
|------------|---------|
| Suggest a question (no coding) | [Open an issue](../../issues/new/choose) → **Add / propose a question** |
| Report a wrong answer or outdated fact | [Open an issue](../../issues/new/choose) → **Report an error** |
| Add or edit a question directly | Fork → branch → edit the domain file → open a Pull Request |
| Ask a study question / discuss | Use the **Discussions** tab |

## The quality bar (please read)

A good SAA-C03 question tests **judgment and trade-offs**, not recall. Before submitting, make sure yours has all of these:

1. **A realistic scenario** — a company/workload with real constraints (latency, compliance, budget, ops headcount, RTO/RPO, traffic pattern).
2. **A decisive qualifier in the stem** — `MOST cost-effective`, `LEAST operational overhead`, `HIGHEST availability`, `LOWEST latency`, `with the LEAST change`. This is what makes one answer *best* when several technically work.
3. **Four options** (or five/six for a "choose TWO/THREE"), where the distractors are **plausible** and each **fails for one precise reason** — wrong tool, insecure exposure, higher overhead, or an outdated behavior. No obviously silly options.
4. **Exactly one best answer** (or exactly the stated count).
5. **An explanation** that covers: the concept tested, *why the correct answer is correct* (the mechanism), *why each distractor fails*, the real-world trap, and any **time-sensitive fact with its date**.
6. **A Mermaid diagram** — never ASCII. Use a network topology (`flowchart TB` with `subgraph`s for VPC/AZ/subnet), a data-flow (`flowchart LR`), or a `sequenceDiagram` for auth/request flows. Label every connection.
7. **Sources** for any fact that could have changed (AWS docs links).

Copy the exact structure from [`QUESTION_TEMPLATE.md`](QUESTION_TEMPLATE.md).

### A note on answer positions
Don't cluster the correct answer on one letter (e.g. everything is "A"). Vary it so people learn the concept, not a pattern.

## Pull Request workflow

```bash
# 1. Fork the repo on GitHub, then clone your fork
git clone https://github.com/<your-username>/aws-saa-c03-question-bank.git
cd aws-saa-c03-question-bank

# 2. Create a branch
git checkout -b add-question-vpc-endpoints

# 3. Add your question to the correct domain file in questions/
#    (follow QUESTION_TEMPLATE.md exactly)

# 4. Commit and push
git add .
git commit -m "Add question: S3 gateway endpoint vs NAT (Domain 1)"
git push origin add-question-vpc-endpoints

# 5. Open a Pull Request against this repo's main branch
```

Number questions within each domain file sequentially (Q1, Q2, Q3…). As a domain file grows large, maintainers may split it into one-file-per-question — that's fine.

## Review checklist (what maintainers look for)

- [ ] Follows `QUESTION_TEMPLATE.md`
- [ ] Decisive qualifier present
- [ ] One best answer; distractors each fail for a stated, precise reason
- [ ] Explanation is mechanism-level, not a restatement
- [ ] Mermaid diagram included and renders
- [ ] Time-sensitive facts sourced and dated
- [ ] Answer letters not clustered

## Code of Conduct

By participating, you agree to uphold our [Code of Conduct](CODE_OF_CONDUCT.md). Be kind, be constructive, help newcomers.
