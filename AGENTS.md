# Long-Running Task Recovery

For long-running or interruptible tasks, maintain a `TASK_STATE.md` file.

Keep it updated with:

- original objective;
- completed work;
- remaining work;
- important architectural or implementation decisions;
- modified or important files;
- tests and validations already performed;
- known issues, risks, or blockers;
- next action.

If the conversation context appears incomplete, or if a task is resumed after an interruption, reconstruct the current state before making new changes.

Inspect, at minimum:

1. `TASK_STATE.md`;
2. `git status`;
3. `git diff`;
4. the relevant repository files;
5. any remaining conversation context that is still available.

Then:

- identify what has already been completed;
- identify what remains to be done;
- do **not** repeat completed work;
- continue from the repository's actual current state.
