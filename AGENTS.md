# Issue-Driven Task Context

For any substantial task, use a **self-contained GitHub issue** as the durable source of task context.

The issue must be understandable and actionable by a human or LLM with repository access **without relying on previous chat history**.

## Issue as source of truth

The issue should capture, as relevant:

- objective;
- context / problem;
- expected behavior or outcome;
- scope;
- out of scope;
- acceptance criteria;
- architectural, security, or implementation constraints that are intentional;
- dependencies;
- execution order / readiness metadata when the project uses them;
- relevant files, specs, references, errors, or examples;
- validation / tests;
- Definition of Done.

The issue owns the task's **WHY, WHAT, constraints, and expected result**. Leave the **HOW** to the contributor unless a specific technical or architectural choice is itself a requirement.

Do not leave information required for future continuation only in the conversation. Put durable decisions in the issue, repository documentation, specs, or ADRs as appropriate.

## Execution workflow

Use the following lifecycle:

```text
Issue
  ↓
Branch
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

The Pull Request must reference the issue and summarize:

- what changed;
- the implementation approach;
- architecture / security / data impacts when relevant;
- tests and validations performed;
- documentation updated;
- known limitations or remaining follow-up.

Close the issue only after the change is merged and its acceptance criteria / Definition of Done are satisfied.

## Resume after interruption or context loss

Before making new changes, reconstruct the current state from:

1. the GitHub issue;
2. linked Pull Requests, reviews, and relevant issue comments;
3. `git status`;
4. `git diff`;
5. the relevant repository files, specs, tests, and Git history.

Then identify what is already complete, what remains, and **do not repeat completed work**.

If the issue no longer contains enough context for another contributor to continue safely, update the issue before proceeding.

## Deprecated: Long-Running Tasks / `TASK_STATE.md`

The previous `Long-Running Task Recovery` workflow based on maintaining `TASK_STATE.md` is **deprecated**.

Do not create or maintain `TASK_STATE.md` as the default context-recovery mechanism. Existing task-state content should be migrated into the relevant GitHub issue when useful.

Only use a task-state file when a specific repository or workflow explicitly requires it.
