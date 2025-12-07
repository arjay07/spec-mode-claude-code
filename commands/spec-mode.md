Usage: /spec-mode <feature description> - Initiate spec-driven development workflow for a new feature

# Spec Mode

## Description
Initiate spec-driven development workflow for a new feature. Orchestrates the full
requirements → design → tasks workflow with approval gates at each stage.

## Workflow Instructions

When this command is invoked, follow this workflow:

### Step 1: Initialize the Spec

1. Parse the feature description from the user's command
2. Derive a spec-name in kebab-case (e.g., "achievement system" → "achievement-system")
3. Confirm the spec-name with the user using AskUserQuestion, or let them provide an alternative
4. Check if `.claude/specs/<spec-name>/` already exists:
   - If yes: Ask user to resume, overwrite, or choose different name
   - If no: Create the directory

### Step 2: Requirements Phase

1. Display status:
   ```
   ══════════════════════════════════════════
     SPEC WORKFLOW: <spec-name>
     Phase: REQUIREMENTS
   ══════════════════════════════════════════
   ```

2. Use the **Task tool** to invoke the `spec-requirements` agent:
   - subagent_type: "spec-requirements"
   - prompt: "Create requirements for the '<spec-name>' spec. Feature description: <feature description>. Create the requirements document at .claude/specs/<spec-name>/requirements.md"

3. After the agent completes, read the requirements.md file and present a summary

4. Use AskUserQuestion to get approval:
   - "Approve and proceed to Design"
   - "Request changes to Requirements"

5. **If changes requested**: Re-invoke spec-requirements agent with the feedback
6. **If approved**: Proceed to Design Phase

### Step 3: Design Phase

1. Display status:
   ```
   ══════════════════════════════════════════
     SPEC WORKFLOW: <spec-name>
     Phase: DESIGN
   ══════════════════════════════════════════
   ```

2. Use the **Task tool** to invoke the `spec-design` agent:
   - subagent_type: "spec-design"
   - prompt: "Create the technical design for the '<spec-name>' spec. The requirements are in .claude/specs/<spec-name>/requirements.md. Create design.md in the same directory."

3. After the agent completes, read the design.md file and present a summary

4. Use AskUserQuestion to get approval:
   - "Approve and proceed to Tasks"
   - "Request changes to Design"
   - "Go back to Requirements"

5. **If changes requested**: Re-invoke spec-design agent with feedback
6. **If go back**: Return to Step 2
7. **If approved**: Proceed to Tasks Phase

### Step 4: Tasks Phase

1. Display status:
   ```
   ══════════════════════════════════════════
     SPEC WORKFLOW: <spec-name>
     Phase: TASKS
   ══════════════════════════════════════════
   ```

2. Use the **Task tool** to invoke the `spec-tasks` agent:
   - subagent_type: "spec-tasks"
   - prompt: "Create implementation tasks for the '<spec-name>' spec. The requirements and design are in .claude/specs/<spec-name>/. Create tasks.md in the same directory."

3. After the agent completes, read the tasks.md file and present a summary

4. Use AskUserQuestion to get approval:
   - "Approve - Workflow complete"
   - "Request changes to Tasks"
   - "Go back to Design"
   - "Go back to Requirements"

5. **If changes requested**: Re-invoke spec-tasks agent with feedback
6. **If go back**: Return to appropriate phase
7. **If approved**: Proceed to Completion

### Step 5: Completion

1. Display completion status:
   ```
   ══════════════════════════════════════════
     SPEC WORKFLOW COMPLETE: <spec-name>
   ══════════════════════════════════════════

   Created:
     ✓ .claude/specs/<spec-name>/requirements.md
     ✓ .claude/specs/<spec-name>/design.md
     ✓ .claude/specs/<spec-name>/tasks.md

   Total tasks: <N>
   ══════════════════════════════════════════
   ```

2. Use AskUserQuestion:
   - "Start executing task 1 now"
   - "I'll execute tasks later"

3. **If execute now**: Use Task tool to invoke spec-executor for task 1
4. **If later**: Inform user: "To execute tasks, say: 'Execute task 1 from <spec-name>'"

## Critical Rules

- **MUST** get explicit user approval before transitioning between phases
- **MUST** allow user to go back to any previous phase
- **MUST** use the Task tool to invoke specialized agents (spec-requirements, spec-design, spec-tasks)
- **MUST NOT** skip phases or auto-approve
- **MUST NOT** auto-execute tasks (always ask first)

## Example

```
/spec-mode add an achievement system to track player accomplishments
```

Creates:
- `.claude/specs/achievement-system/requirements.md`
- `.claude/specs/achievement-system/design.md`
- `.claude/specs/achievement-system/tasks.md`
