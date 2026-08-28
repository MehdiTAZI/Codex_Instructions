
# Long running tasks

<!-- for long running tasks -->

#Before running first prompt

Since this is a long-running task, maintain a TASK_STATE.md file containing:

What has been completed
What remains to be done
Decisions made
The next step

Update it regularly so the work can be resumed correctly in case of interruption or context compaction.

# After limit reachsed and quota back again ( continue mode ) 

Continue the work from where you left off. Before resuming, check the current state of the repository, the modified files, the git diff, what has already been completed, and what remains to be done. Do not redo anything that has already been completed. Then continue until the original task is fully completed.

Before proceeding, accurately reconstruct the current state of the work using:
1. TASK_STATE.md
2. git status
3. git diff
4. the repository files
5. the remaining available conversation history
Identify what has already been completed and what still needs to be done. Do not redo completed tasks.
Then continue the original task through to completion.




<!-- PROMPT GENERATION WORKFLOW -->
# PROMPT GENERATION WORKFLOW 

1 - Brainstorm in ChatGPT Chat ( High ) --> to not consume tokens

2 - Generate the Plan

3 - Generate the Prompt

4 - Generate necessary inputs

5 - Go to Codex

A - Each project is for a new product

B - Each Chat is for a new feature or functionaly
