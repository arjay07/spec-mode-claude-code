Parameters: spec_name, task_number

## Workflow

### Step 1: Check for Subtasks
Read `.claude/specs/{spec_name}/tasks.md` and determine if {task_number} is a parent task with subtasks.

A parent task has subtasks if there are tasks numbered like `{task_number}.1`, `{task_number}.2`, etc. For example:
- Task `1` is a parent if tasks `1.1`, `1.2`, `1.3` exist
- Task `2.1` is NOT a parent (it's a subtask itself, unless `2.1.1` exists)

### Step 2: Execute Based on Task Type

**If {task_number} is a parent task with subtasks:**
1. Collect all subtasks (e.g., for task `1`: find `1.1`, `1.2`, `1.3`, etc.)
2. Execute each subtask in order, following Steps 2a-2c for each:

   **Step 2a: Mark Subtask In Progress**
   Change the subtask checkbox from `- [ ]` to `- [-]`

   **Step 2b: Execute Subtask**
   Use the Task tool with subagent_type='spec-executor' and provide the following prompt:
   ```
   Execute task {subtask_number} from the '{spec_name}' spec located at `.claude/specs/{spec_name}/`.

   Implement the task following the requirements and design documents. Do NOT modify tasks.md - just implement the functionality.
   ```

   **Step 2c: Mark Subtask Complete**
   After the agent completes, change the subtask checkbox from `- [-]` to `- [x]`

3. After all subtasks are complete, also mark the parent task `{task_number}` as complete with `- [x]`

**If {task_number} is a single task (no subtasks):**
1. Mark task in progress: Change checkbox from `- [ ]` to `- [-]`
2. Use the Task tool with subagent_type='spec-executor' and provide the following prompt:
   ```
   Execute task {task_number} from the '{spec_name}' spec located at `.claude/specs/{spec_name}/`.

   Implement the task following the requirements and design documents. Do NOT modify tasks.md - just implement the functionality.
   ```
3. Mark task complete: Change checkbox from `- [-]` to `- [x]`

### Step 3: Summarize
Briefly confirm to the user what was completed:
- For parent tasks: list all subtasks that were executed
- For single tasks: show what was implemented
