# AI Engineering with Codex

A practical handbook for **AI-assisted software engineering, Data/AI, cloud architecture, coding, audits, and long-running repository work** with ChatGPT, Work, and Codex.

This repository is about more than context recovery. It provides an operating model for deciding:

- **where** to work: Chat, Work, or Codex;
- **which model** to use;
- **how much reasoning** to allocate;
- **when to use one agent or multiple agents**;
- **how to structure repository context** so agents can work reliably;
- **how to plan, implement, validate, review, and merge safely**;
- **how to resume long-running work without relying on conversation memory**.

> **Core principle:** use conversations for active work; use the repository, issues, specs, ADRs, tests, and Pull Requests as durable engineering memory.

---

## 30-second routing rule

![30-second model and workflow routing](./docs/images/09-thirty-second-routing.svg)

For serious repository-centered work, a practical default is:

> **Codex + GPT-6.1 Sol High → Max if needed → Astra High/Max when stakes or ambiguity increase → Ultra only when independent workstreams genuinely benefit from parallelism.**

For simple high-volume tasks, start lower with Luna or GPT-6.1 Sol Medium.

For the hardest quality-first reasoning and coding, Astra is the flagship option.

> Model names, pricing, availability, Ultra, and Ultrafast are an **October 2026 snapshot**. Durable engineering rules are intentionally separated from volatile product facts.

---

## The four decisions

![Model, reasoning, surface, and orchestration](./docs/images/01-four-dimensions.svg)

A good AI engineering workflow separates four choices:

1. **Model** — base capability.
2. **Reasoning** — how much reasoning effort to allocate.
3. **Surface** — Chat, Work, or Codex.
4. **Orchestration** — single-agent or multi-agent.

Do not solve every problem by selecting the biggest model at the highest reasoning level.

---

## What this repository covers

| Area | What you will find |
| --- | --- |
| **Surface selection** | Chat vs Work vs Codex |
| **Model selection** | Luna, GPT-6 Sol, GPT-6.1 Sol, Astra |
| **Reasoning** | Low → Medium → High → XHigh → Max |
| **Orchestration** | Max vs Ultra, single-agent vs multi-agent |
| **Coding** | model/reasoning playbook for features, refactors, debugging, architecture |
| **Data/AI & Cloud** | routing for architecture, Databricks/Terraform-style work, audits, inventory |
| **Repository context** | `AGENTS.md`, specs, ADRs, docs, tests as durable memory |
| **Issue-driven execution** | self-contained issues, `Agent Ready`, dependencies, execution order |
| **Planning** | when to plan first vs explore → implement → verify |
| **Git / PR workflow** | branch/worktree isolation, review, merge gates, stale/squash PR handling |
| **Validation** | CI, security, integration, E2E, evidence levels |
| **Long-running work** | recovery from issue + PR + Git/repository state |
| **Agent safety** | least privilege, bounded loops, human responsibility, fresh-context review |
| **Evaluation** | choose the lightest model/reasoning configuration that clears your quality bar |

---

## Visual quick reference

### Choose the environment

![Chat vs Work vs Codex](./docs/images/02-chat-vs-work-vs-codex.svg)

### Choose the model

![Model selection](./docs/images/03-model-selection.svg)

<details>
<summary><strong>Reasoning levels</strong></summary>

![Reasoning levels](./docs/images/04-reasoning-levels.svg)

</details>

<details>
<summary><strong>Max vs Ultra</strong></summary>

![Max vs Ultra](./docs/images/05-max-vs-ultra.svg)

</details>

<details>
<summary><strong>Coding playbook</strong></summary>

![Coding playbook](./docs/images/06-coding-playbook.svg)

</details>

<details>
<summary><strong>Data/AI, cloud, architecture, and audit routing</strong></summary>

![Data AI cloud audit routing](./docs/images/07-data-ai-cloud-audit-routing.svg)

</details>

### Delivery workflow and evidence

![Git workflow, merge gates, and evidence levels](./docs/images/08-git-workflow-gates-evidence.svg)

---

## Repository contents

| File | Purpose |
| --- | --- |
| [`ai-engineering-with-codex-guide.md`](./ai-engineering-with-codex-guide.md) | full operating handbook and detailed methodology |
| [`AGENTS.md`](./AGENTS.md) | canonical persistent instructions for agents working on this repository |
| [`prompt.md`](./prompt.md) | reusable prompts for issue preparation, planning, execution, review, and recovery |
| [`docs/images/`](./docs/images/) | English vector diagrams used throughout the handbook |
| `README.md` | concise entry point and visual quick reference |

---

## Start here

| Goal | Read |
| --- | --- |
| choose Chat, Work, or Codex | [Guide §3](./ai-engineering-with-codex-guide.md#3-choose-the-surface-before-the-model) |
| choose a model | [Guide §4](./ai-engineering-with-codex-guide.md#4-model-selection--october-2026-snapshot) |
| choose reasoning / Max / Ultra | [Guide §§5–7](./ai-engineering-with-codex-guide.md#5-reasoning-levels) |
| code or debug a repository | [Guide §8](./ai-engineering-with-codex-guide.md#8-coding-playbook) |
| architecture / Data/AI / cloud / audit | [Guide §9](./ai-engineering-with-codex-guide.md#9-dataai-cloud-architecture-and-audit-playbook) |
| structure durable repository context | [Guide §§10–14](./ai-engineering-with-codex-guide.md#10-the-repository-is-durable-memory) |
| execute a task end-to-end | [Guide §§15–20](./ai-engineering-with-codex-guide.md#15-the-default-engineering-loop) |
| review / merge safely | [Guide §§20–25](./ai-engineering-with-codex-guide.md#20-merge-gates) |
| resume long-running work | [Guide §26](./ai-engineering-with-codex-guide.md#26-long-running-work-and-recovery) |
| reuse execution prompts | [`prompt.md`](./prompt.md) |

---

## Issue-driven engineering context

Long-running recovery is one part of the system.

For substantial work, use a **self-contained GitHub issue** that another human or LLM can understand without the chat that created it.

The issue captures:

- objective;
- context / problem;
- expected behavior;
- scope / out of scope;
- acceptance criteria;
- architecture/security constraints;
- dependencies;
- relevant files/specs;
- validation;
- Definition of Done.

The issue owns **WHY / WHAT / constraints / expected result**.

The Pull Request owns more of the implementation **HOW**, actual changes, tests, evidence, and limitations.

Recommended lifecycle:

> **Issue → branch/worktree → implementation → validation → PR → review → merge → issue closure**

The previous default based on `TASK_STATE.md` is **deprecated**. Resume from the issue, linked PR/reviews, Git state, repository files, specs, and tests.

---

## Engineering defaults

These are defaults, not universal laws:

- use the **smallest sufficient context**;
- use the **lightest model/reasoning level that clears the quality bar**;
- plan before coding when the blast radius or ambiguity is high;
- avoid ceremony for small bounded tasks;
- keep one primary intention per issue/branch/PR;
- do not work directly on `main` for significant changes;
- prefer deterministic automation when deterministic automation is enough;
- use agents when interpretation/adaptation is actually needed;
- use multi-agent only for independent bounded workstreams;
- require stronger verification as autonomy increases;
- review correctness/security/spec alignment before style;
- do not claim more than the evidence proves;
- keep human responsibility for destructive, production, security, and structural decisions.

---

## OpenAI snapshot sources

The model-routing section is based on official OpenAI references verified for October 2026:

- Models: https://developers.openai.com/api/docs/models
- GPT-6.1 Sol: https://developers.openai.com/api/docs/models/gpt-6.1-sol
- GPT-6 Astra: https://developers.openai.com/api/docs/models/gpt-6-astra
- Reasoning: https://developers.openai.com/api/docs/guides/reasoning
- Multi-agent: https://developers.openai.com/api/docs/guides/responses-multi-agent
- Work & Codex: https://help.openai.com/en/articles/20001275-chatgpt-work-and-codex
- Rate card / plan-specific options: https://help.openai.com/en/articles/11481834-chatgpt-rate-card-business-enterpriseedu-credit-based-pricing

For durable guidance, use the engineering principles. For model names/prices/availability, re-check the current source.
