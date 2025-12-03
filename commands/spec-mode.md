# Spec Mode

## Description
Initiate spec-driven development workflow for a new feature. This command orchestrates
the full requirements → design → tasks workflow with approval gates at each stage.

## Usage
```
/spec-mode <feature description>
```

## Instructions

When this command is invoked:

1. Use the Task tool to invoke the `spec-orchestrator` agent with:
   - The feature description provided by the user
   - Instruction to begin the spec-driven development workflow

2. The orchestrator will guide you through:
   - **Requirements Phase**: Gather and document requirements (requirements.md)
   - **Design Phase**: Create technical design (design.md)
   - **Tasks Phase**: Generate implementation tasks (tasks.md)

3. You will be asked for approval at each stage before proceeding.

4. After tasks are approved, you can optionally begin executing tasks.

## Example

```
/spec-mode add an achievement system to track player accomplishments
```

This will start the spec workflow, creating:
- `.claude/specs/achievement-system/requirements.md`
- `.claude/specs/achievement-system/design.md`
- `.claude/specs/achievement-system/tasks.md`
