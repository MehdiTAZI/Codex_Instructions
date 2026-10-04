# Reusable Codex Prompts

## 1. Issue-Driven Task Context

### Create or improve a self-contained issue

```text
Prepare or update the GitHub issue for this task so that another human or LLM can execute or resume it without access to this conversation.

Use one issue per distinct task. Before creating a new issue, check for an equivalent existing issue and enrich it instead of creating a duplicate.

The issue must contain, as relevant:

- Objective
- Context / problem
- Expected behavior / outcome
- Scope
- Out of scope
- Acceptance criteria
- Architectural, security, and implementation constraints
- Dependencies / Depends on
- Phase / Priority / Execution Order / Agent Ready when used by the project
- Relevant files, specs, references, examples, or errors
- Validation / tests
- Definition of Done

The issue is the source of truth for WHY, WHAT, constraints, and the expected result.
Execution Order sequences ready work; Depends on represents actual prerequisites.
Do not over-specify HOW unless a specific technical or architectural choice is required.
Do not leave required context only in this conversation.
```

### Start work from an issue

```text
Work from this GitHub issue as the source of truth.

Before implementation:
1. read AGENTS.md and the relevant repository documentation/specs;
2. read the complete issue and its dependencies;
3. inspect the current Git state and relevant code;
4. verify that the issue is self-contained enough to execute safely;
5. for a complex or risky change, prepare a concise implementation plan before modifying code.

Then:
- create/use the task branch;
- implement only the required scope;
- run the relevant tests and validations;
- review the diff;
- create/update the Pull Request linked to the issue.

The PR must explain what changed, the implementation approach, relevant impacts, tests/validations, documentation changes, and known limitations.
Do not close the issue until the change is merged and the acceptance criteria / Definition of Done are satisfied.
```

### Resume after an interruption or context loss

```text
Continue work on the GitHub issue.

Before proceeding, reconstruct the actual state from:
1. the issue body and relevant issue comments;
2. linked Pull Requests and reviews;
3. git status;
4. git diff;
5. relevant repository files, specs, tests, and Git history.

Identify what is already complete and what remains.
Do not redo completed work.

If the issue is missing context required for safe continuation, update the issue first so it becomes self-contained again.

Then continue through validation, PR/review, merge, and issue closure according to the repository workflow.
```

### Deprecated: Long-Running Tasks / TASK_STATE.md

The older workflow based on maintaining `TASK_STATE.md` for long-running tasks is **deprecated**.

Do not create or maintain `TASK_STATE.md` by default. Use a self-contained GitHub issue plus the linked PR and repository/Git state for durable context and recovery.

Only use a task-state file when a specific repository or workflow explicitly requires it.

---

## 2. Prompt Preparation Workflow

For larger or more expensive Codex tasks, prepare the work before execution:

1. Brainstorm and clarify the requirement in ChatGPT.
2. Generate or validate the implementation plan.
3. Create or enrich the self-contained GitHub issue.
4. Prepare the required inputs, references, examples, or specifications.
5. Execute the issue in Codex.
6. Validate, review, and open/update the linked PR.

### Suggested organization

- Use one Codex project for each product or repository context.
- Use a separate chat/thread for a distinct feature or workstream when isolation improves clarity.
- Treat the GitHub issue and repository as durable context; conversations are temporary working context.

These are organizational recommendations, not hard Codex requirements.
