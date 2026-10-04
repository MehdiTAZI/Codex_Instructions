# Reusable AI Engineering with Codex Prompts

These prompts are templates. Adapt them to the repository and task instead of copying them mechanically.

---

## 1. Create or improve a self-contained issue

```text
Prepare or update the GitHub issue for this task so that another human or LLM can execute or resume it without access to this conversation.

Before creating a new issue, check whether an equivalent issue already exists. Enrich the existing one instead of duplicating it.

Include, as relevant:
- Objective
- Context / problem
- Expected behavior / outcome
- Scope
- Out of scope
- Acceptance criteria
- Architecture / security / implementation constraints that are intentional
- Dependencies / Depends on
- Phase / Priority / Execution Order / Agent Ready when used
- Relevant files, specs, references, examples, logs, or errors
- Validation / tests
- Definition of Done

The issue is the source of truth for WHY, WHAT, constraints, and the expected result.
Do not over-specify HOW unless a specific implementation or architecture choice is itself a requirement.
Do not leave required context only in this conversation.
```

---

## 2. Start implementation from an issue

```text
Work from this GitHub issue as the source of truth.

Before modifying code:
1. read the applicable AGENTS.md instructions;
2. read the complete issue and dependencies;
3. inspect the real repository and current Git state;
4. read relevant specs / ADRs / tests;
5. confirm that the issue is sufficiently self-contained;
6. if the change is complex, risky, cross-layer, migration-heavy, or ambiguous, prepare a concise implementation plan first.

Then:
- create or use the dedicated branch/worktree;
- implement only the required scope;
- preserve existing invariants;
- avoid unrelated refactors;
- run the relevant build/lint/type/test/integration/security checks;
- review the final diff;
- update documentation/specs when behavior or architecture changed;
- create/update the PR linked to the issue.

The PR must describe the implementation HOW, impacts, tests/evidence, docs, limitations, and follow-up.
Do not close the issue until merge and satisfaction of acceptance criteria / Definition of Done.
```

---

## 3. Plan a complex or risky change

```text
Before implementation, inspect the repository and prepare a plan.

Include:
- objective and current-state understanding;
- affected components/files;
- invariants that must remain true;
- architecture/data/security implications;
- dependencies;
- migration and rollback concerns;
- implementation increments;
- validation strategy;
- main risks and unknowns.

Do not invent repository structure that does not exist.
Flag uncertainty explicitly.
Keep unrelated refactors out of scope.

Stop after the plan if human review/approval is required for this change.
```

---

## 4. Review a diff or Pull Request

```text
Review this change as an engineering Pull Request.

Prioritize:
1. correctness / bugs / regressions;
2. security / authorization / isolation;
3. spec / ADR / invariant alignment;
4. data model / migrations;
5. missing or weak tests;
6. evidence quality;
7. operability / rollback / observability;
8. maintainability;
9. style only after substantive issues.

Check for:
- scope creep;
- stale assumptions;
- integration gaps;
- missing E2E validation for cross-system changes;
- claims that exceed the actual evidence;
- dependency or migration hazards.

Do not manufacture cosmetic findings to increase comment count.
For risky changes, review from fresh context instead of relying on the implementation conversation.
```

---

## 5. Resume after interruption or context loss

```text
Continue work on this GitHub issue.

Reconstruct the actual state from:
1. the issue body and relevant comments;
2. linked Pull Requests and reviews;
3. git status;
4. git diff;
5. relevant repository files, specs, tests, and Git history.

Identify:
- what is already complete;
- what remains;
- unresolved acceptance criteria;
- blockers / dependencies;
- validation already performed and still required.

Do not redo completed work.

If durable context is missing, update the issue or repository documentation before continuing.
Do not create TASK_STATE.md unless this repository explicitly requires it.
```

---

## 6. Select surface, model, reasoning, and orchestration

```text
Recommend the lightest configuration that should reliably satisfy this task.

Decide separately:
1. Surface: Chat, Work, or Codex
2. Model: Luna, GPT-6.1 Sol, Astra, or another justified option
3. Reasoning: Low / Medium / High / XHigh / Max
4. Orchestration: single-agent or multi-agent

Consider:
- whether a repository is central;
- ambiguity;
- technical complexity;
- blast radius;
- security/architecture importance;
- omission cost;
- task volume;
- whether workstreams are genuinely independent;
- coordination overhead;
- latency/cost sensitivity.

Default for serious repo work: Codex + GPT-6.1 Sol High.
Escalate to Max if the problem remains difficult.
Escalate to Astra when stakes/ambiguity justify the quality increase.
Use Ultra only when independent workstreams genuinely benefit from parallel agents.

Explain the trade-off briefly.
Treat model availability/pricing as a dated snapshot and verify current product documentation if the decision depends on it.
```

---

## 7. Run a high-stakes audit

```text
Audit the available code, configuration, documentation, tests, and architecture as independent evidence.

Use distinct passes:
1. detailed local consistency pass;
2. architecture/security challenge;
3. cross-cutting omission/contradiction pass when parallel workstreams are useful.

Look for:
- contradictions inside the same document/codebase;
- document vs implementation mismatches;
- missing structural controls;
- unsafe defaults;
- security / permission / secret risks;
- data-quality and migration risks;
- missing observability / rollback / DR;
- CI/CD gaps;
- untested assumptions;
- claims unsupported by real evidence.

Distinguish:
- static review;
- local validation;
- dry-run/plan;
- integration;
- real deployment;
- operational proof.

Do not claim production validation without production-grade evidence.
```

---

## 8. Prepare a feature prompt

```text
Write the task contract using:

Goal
Context
Constraints
Done when

Reference the issue/specs/repository artifacts that contain durable context.
State important rationale and invariants.
Avoid prescribing hidden reasoning steps.
Avoid unrelated changes.
Make acceptance criteria machine-verifiable where possible.
```

---

## 9. Context reset / fresh-thread handoff

```text
Prepare the durable context needed for a fresh agent/thread to continue safely.

Do not summarize the whole conversation.
Instead ensure the repository/issue/PR contains:
- objective and scope;
- decisions and rationale;
- current implementation state;
- relevant files/specs;
- completed validation;
- remaining work;
- risks/blockers;
- next action.

Then the new context should start by reading those durable artifacts and the current Git state.
```
