# Reusable Codex Prompts

## 1. Long-Running Tasks

### Before starting a long-running task

```text
Complete this task through to completion.

Because this is a long-running task, maintain a TASK_STATE.md file containing:

- what has been completed;
- what remains to be done;
- decisions made;
- modified or important files;
- validations already performed;
- known issues or blockers;
- the next step.

Update it regularly so the work can be resumed correctly in case of interruption or context compaction.
```

### Resume after an interruption or quota reset

```text
Continue.

Before proceeding, accurately reconstruct the current state of the work using:

1. TASK_STATE.md
2. git status
3. git diff
4. the repository files
5. the remaining available conversation history

Identify what has already been completed and what still needs to be done.
Do not redo completed work.

Then continue the original task through to completion.
```

---

## 2. Prompt Preparation Workflow

For larger or more expensive Codex tasks, prepare the work before execution:

1. Brainstorm and clarify the requirement in ChatGPT.
2. Generate or validate the implementation plan.
3. Generate the final Codex prompt.
4. Prepare the required inputs, references, examples, or specifications.
5. Execute the task in Codex.

### Suggested organization

- Use one Codex project for each product or repository context.
- Use a separate chat/thread for a distinct feature or workstream when isolation improves clarity.

These are organizational recommendations, not hard Codex requirements.
