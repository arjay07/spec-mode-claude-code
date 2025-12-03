Parameters: spec_name, task_number

## Workflow

### Step 1: Mark Task In Progress
Read `.claude/specs/{spec_name}/tasks.md` and find task {task_number}. Change its checkbox from `- [ ]` to `- [-]` to mark it as in progress.

### Step 2: Execute Task
Use the Task tool with subagent_type='spec-executor' and provide the following prompt:

```
Execute task {task_number} from the '{spec_name}' spec located at `.claude/specs/{spec_name}/`.

Implement the task following the requirements and design documents. Do NOT modify tasks.md - just implement the functionality.
```

### Step 3: Mark Task Complete
After the agent completes and reports back, read `.claude/specs/{spec_name}/tasks.md` again and change the task checkbox from `- [-]` to `- [x]` to mark it complete.

### Step 4: Summarize
Briefly confirm to the user that task {task_number} is complete and show what was implemented.
