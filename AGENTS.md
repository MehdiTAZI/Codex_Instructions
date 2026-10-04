# Repository Agent Contract

This repository is an **AI Engineering with Codex** handbook. Keep it coherent, English-only, evidence-based, and useful as durable engineering reference material.

## 1. Language

All active repository content must be **English**, including:
- Markdown;
- SVG labels;
- captions;
- examples;
- prompts;
- newly added documentation.

Do not introduce French prose except proper nouns or quoted historical material when explicitly required.

## 2. Source-of-truth hierarchy

Use:
- `README.md` as the concise entry point;
- `ai-engineering-with-codex-guide.md` as the detailed handbook;
- `AGENTS.md` as the canonical persistent agent contract;
- `prompt.md` for reusable prompts;
- `docs/images/` for visual quick references;
- GitHub issues as autonomous work units;
- PRs as implementation/evidence records.

Avoid duplicate instruction files that can drift.

## 3. Issue-driven task context

For substantial work, use a **self-contained GitHub issue**.

Before creating a new issue, check for an equivalent existing issue and enrich it instead of duplicating it.

The issue should capture, as relevant:
- Objective
- Context / problem
- Expected behavior / outcome
- Scope
- Out of scope
- Acceptance criteria
- Intentional architecture/security/implementation constraints
- Dependencies / `Depends on`
- `Phase`, `Priority`, `Execution Order`, `Agent Ready` when used
- Relevant files/specs/references/errors/examples
- Validation / tests
- Definition of Done

The issue owns **WHY / WHAT / constraints / expected result**. Leave implementation **HOW** to the contributor unless a technical choice is intentionally constrained.

`Execution Order` sequences ready work. `Depends on` represents actual prerequisites.

## 4. Execution workflow

Default lifecycle:

```text
Issue
  ↓
Branch / worktree
  ↓
Implementation
  ↓
Tests / validation
  ↓
Pull Request
  ↓
Review
  ↓
Merge
  ↓
Issue closure
```

Do not perform significant work directly on `main`.

Use separate branches/worktrees for independent workstreams.

For complex/risky/cross-layer work, plan before implementation. For routine bounded work, avoid unnecessary ceremony.

## 5. Pull Request requirements

A PR should reference the issue and explain:
- what changed;
- implementation approach;
- architecture/security/data impact when relevant;
- migrations;
- tests and validation;
- documentation updates;
- evidence;
- known limitations/follow-up.

Do not close the issue until the PR is merged and acceptance criteria / DoD are satisfied.

## 6. Validation and evidence

Use the relevant gates for the project:
- build / compilation;
- lint / format;
- type checking;
- unit tests;
- integration tests;
- E2E / smoke tests for cross-system changes;
- migration/data compatibility;
- static analysis;
- secret/security checks;
- diff review;
- docs/spec consistency.

An unresolved security finding or mandatory failing gate blocks merge.

Claims must match evidence:
- static review ≠ local validation;
- local validation ≠ integration;
- integration ≠ real deployment;
- deployment ≠ operational proof.

## 7. Review priorities

Review in this order:
1. correctness / regressions;
2. security / isolation;
3. spec / ADR / invariant alignment;
4. data model / migrations;
5. tests and evidence;
6. operability / rollback / observability;
7. maintainability;
8. style last.

For risky or final-pass work, prefer fresh-context independent review.

## 8. Context engineering

Use the smallest sufficient context:
- issue;
- applicable AGENTS instructions;
- relevant specs/ADRs;
- affected files;
- failing tests/logs;
- current diff;
- linked PR/reviews.

If a context becomes polluted by stale assumptions, reconstruct from durable repository state instead of continuing blindly.

## 9. Multi-agent rule

Use multiple agents only when workstreams are:
- independent;
- bounded;
- safely isolated;
- worth the coordination overhead.

Use one agent for ordered/shared-state work.

## 10. Human responsibility

Keep human ownership for:
- destructive actions;
- production changes;
- security/trust-boundary changes;
- irreversible migrations;
- high-impact architecture decisions;
- ambiguous product decisions.

Use least privilege and never add secrets or sensitive production data to prompts, issues, examples, fixtures, or evidence.

## 11. Model guidance

Model/product facts are a **dated October 2026 snapshot**. Do not silently convert them into timeless rules.

Durable default:
- choose the smallest configuration that satisfies the quality bar;
- for serious repo work, `GPT-6.1 Sol High` is the cost-aware default used by this handbook;
- escalate to Max, Astra, or Ultra only when task complexity, stakes, omission risk, or genuine parallelism justify it.

## 12. Resume after interruption

The old `TASK_STATE.md` default is deprecated.

Resume from:
1. issue;
2. linked PR/reviews/comments;
3. Git status / diff;
4. relevant repository files/specs/tests/history.

Identify completed work and do not repeat it. Update durable context before proceeding if the issue is no longer self-contained.
