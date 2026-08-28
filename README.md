# Codex Instructions

A reference repository for **working effectively with Codex on software projects**, especially when a project becomes long-running, complex, or needs to survive context loss and interrupted sessions.

The goal is not to prescribe a specific technology stack. The approach is deliberately **stack-agnostic** and can be adapted to any language, framework, database, ORM, cloud platform, CI/CD system, or deployment model.

> **Core principle:** the conversation is where work happens; the repository is where durable context, decisions, specifications, verification, and recovery live.

---

## Repository contents

| File | Purpose |
| --- | --- |
| [`AGENTS.md`](./AGENTS.md) | Persistent instructions for Codex, including long-running task recovery rules. |
| [`prompt.md`](./prompt.md) | Reusable prompts for starting, resuming, and organizing Codex work. |
| [`codex-large-software-guide-agnostic.md`](./codex-large-software-guide-agnostic.md) | Detailed, stack-agnostic guide for building and maintaining software with Codex. |
| `README.md` | Human-oriented overview and entry point to this repository. |

---

## 1. Operating model

For a significant software project, durable knowledge should not exist only in a conversation history. It should be versioned in the repository.

```text
Human intent
    ↓
Specifications
    ↓
Architecture / Security / Data Model
    ↓
Code
    ↓
Tests
    ↓
Git
    ↓
Review
```

Codex can contribute at every stage, but **the repository remains the source of truth**.

In practice:

- give Codex bounded, explicit tasks;
- keep persistent rules in `AGENTS.md`;
- document significant behavior in specifications;
- version important architectural decisions;
- evolve tests and documentation together with the code;
- review the diff and run project validations before considering work complete;
- use `TASK_STATE.md` for long-running or interruptible tasks.

---

## 2. `AGENTS.md`

`AGENTS.md` is the persistent instruction layer for Codex inside a repository.

It should remain:

- concise;
- normative;
- stable;
- actionable.

Typical content includes:

- build, test, lint, and validation commands;
- mandatory conventions;
- important architectural constraints;
- security rules;
- documentation rules;
- restrictions and safety constraints;
- what Codex must verify before completing a task.

It should **not** become an exhaustive product manual.

The `AGENTS.md` in this repository focuses on one recurring operational problem: making long-running work recoverable through a maintained `TASK_STATE.md` file.

---

## 3. Long-running tasks and recovery

For a task that may exceed one session or the available context window, maintain:

```text
TASK_STATE.md
```

A useful structure is:

```markdown
# Task State

## Objective

## Completed

## In Progress

## Remaining

## Decisions

## Important / Modified Files

## Validation Performed

## Known Issues / Risks

## Next Action
```

When resuming an interrupted task, reconstruct the real state of the work from:

1. `TASK_STATE.md`;
2. `git status`;
3. `git diff`;
4. the repository files;
5. any conversation context that is still available.

Then determine what is already complete, what remains, and **do not repeat completed work**.

Copy-ready prompts are available in [`prompt.md`](./prompt.md).

---

## 4. Recommended project structure

For a significant product, the guide recommends a conceptual structure such as:

```text
AGENTS.md
README.md

specs/
  modules/
  architecture/
  security/
  data-model/
  decisions/
  prompts/

docs/

src/ or equivalent
tests/
scripts/
```

This is a reference structure, not a requirement. Smaller projects should use only the documentation they genuinely need.

### Functional specifications

Module specifications describe **what the software must do**, independently of implementation technology.

They typically cover:

- purpose;
- actors;
- domain concepts;
- business rules;
- data involved;
- main flows;
- edge cases;
- permissions;
- acceptance criteria.

### Architecture specification

Recommended location:

```text
specs/architecture/system-architecture.md
```

It should capture only system-wide structural choices: major components, layer responsibilities, important boundaries, critical flows, external dependencies, and major technical constraints.

### Security specification

Recommended location:

```text
specs/security/security-model.md
```

It can describe:

- authentication;
- authorization;
- roles and permissions;
- tenant / organization / workspace isolation;
- sensitive data;
- auditability;
- secret management;
- key security risks and controls.

### Data model specification

Recommended location:

```text
specs/data-model/domain-model.md
```

This file describes the **conceptual domain model**, independently of Prisma, Hibernate, Entity Framework, SQLAlchemy, Django ORM, or any other persistence technology.

```text
specs/data-model/domain-model.md
        ↓
ORM model / SQL schema / documents / collections
        ↓
migrations
        ↓
real database
```

The ORM or physical schema implements the domain model; it should not be the only documentation of that model.

---

## 5. Recommended Codex workflow

For a new feature:

```text
Requirement
    ↓
Functional specification
    ↓
Architecture / Security / Data impact
    ↓
Plan when needed
    ↓
Incremental implementation
    ↓
Tests and validation
    ↓
Diff review
    ↓
Documentation
    ↓
Commit / PR
```

For larger tasks, [`prompt.md`](./prompt.md) also suggests preparing the work before moving into Codex:

1. brainstorm and clarify the requirement;
2. generate or validate the implementation plan;
3. prepare the final Codex prompt;
4. gather the required inputs and references;
5. execute the task in Codex.

A useful organizational convention is:

- **one Codex project per product or repository context**;
- **a separate thread for a distinct feature or workstream when isolation improves clarity**.

This is a recommendation, not a Codex requirement.

---

## 6. Definition of Done

A significant feature is not complete merely because code has been written.

Depending on the project, completion should include relevant checks such as:

- specifications are consistent with the implementation;
- unit and integration tests pass;
- end-to-end tests pass when relevant;
- build / compilation succeeds;
- linting succeeds;
- type checking succeeds;
- migrations are valid;
- security implications have been reviewed;
- important UI flows have been verified;
- architecture and implementation remain aligned;
- no secrets or unintended files were introduced;
- `git diff` has been reviewed;
- no unrelated changes were added.

Project-specific commands should live in that project's `AGENTS.md`.

---

## 7. Common anti-patterns

Avoid:

- treating Codex conversation memory as the source of truth;
- combining unrelated features in one large task;
- using the ORM schema as the only domain documentation;
- turning `README.md` into exhaustive architecture documentation;
- turning `AGENTS.md` into a large handbook;
- hard-coding business data without justification;
- performing unrelated refactors while implementing a feature;
- ignoring migrations, tests, or diff review;
- committing secrets or sensitive production data;
- over-engineering simple requirements;
- redoing already completed work after a context interruption.

---

## 8. Detailed guide

For the full methodology, examples, prompts, Git workflow, ADR guidance, architecture/security/data-model reviews, deployment practices, and reference structures, see:

**[`codex-large-software-guide-agnostic.md`](./codex-large-software-guide-agnostic.md)**

The guide is intentionally detailed and should be treated as **optional reference material**. Codex does not need to reread it for every task. Persistent rules belong in `AGENTS.md`, while task execution should focus primarily on the relevant specifications, code, tests, and current repository state.

---

## 9. Where information should live

| Information | Recommended location |
| --- | --- |
| Persistent Codex rule | `AGENTS.md` |
| Functional behavior | `specs/modules/` |
| System architecture | `specs/architecture/` |
| Security model | `specs/security/` |
| Conceptual / logical data model | `specs/data-model/` |
| Durable architectural decision | `specs/decisions/` |
| Installation and usage | `README.md` |
| Operations / runbooks | `docs/` |
| Reusable prompt | `specs/prompts/` or `prompt.md` |
| Temporary long-task state | `TASK_STATE.md` |
| Actual implementation | code + migrations + tests |

The goal is to make the repository understandable and recoverable enough that a human or a new Codex thread can continue the work without depending on a previous conversation.