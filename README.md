# Spec-Driven Development Workflow for Claude Code

A structured, phase-gated workflow system for Claude Code that guides feature development through requirements, design, and implementation phases with explicit approval gates.

## Overview

This workflow system helps teams build features systematically by breaking down development into three distinct phases:

1. **Requirements Phase**: Gather and document user stories with EARS-formatted acceptance criteria
2. **Design Phase**: Create technical architecture and component designs
3. **Tasks Phase**: Generate actionable implementation checklists
4. **Execution Phase**: Execute tasks one-by-one with progress tracking

Each phase requires explicit user approval before proceeding, ensuring alignment and reducing rework.

## Installation

Copy the files to your Claude Code project directory:

```
.claude/
├── commands/
│   └── spec-mode.md
└── agents/
    ├── spec-requirements.md
    ├── spec-design.md
    ├── spec-tasks.md
    └── spec-executor.md
```

## Quick Start

```
/spec-mode add user authentication with JWT tokens
```

This will guide you through:
1. Defining requirements and user stories
2. Designing the technical architecture
3. Creating an implementation task checklist
4. Executing tasks incrementally

## Workflow Phases

### Phase 1: Requirements

**Agent**: `spec-requirements`

Creates `.claude/specs/<spec-name>/requirements.md` with:
- Feature overview and value proposition
- User stories in "As a..., I want..., so that..." format
- EARS-formatted acceptance criteria (Event-driven, State-driven, etc.)
- Edge cases and error handling scenarios
- Success criteria and metrics
- Open questions for clarification

**Approval Gate**: User must approve requirements before proceeding to design.

### Phase 2: Design

**Agent**: `spec-design`

Creates `.claude/specs/<spec-name>/design.md` with:
- System architecture with Mermaid diagrams
- Design decisions and rationales
- Component interfaces and responsibilities
- Data models and validation rules
- Error handling strategies
- Testing strategy (unit, integration, edge cases)
- Implementation notes and integration points

**Approval Gate**: User can approve, request changes, or return to requirements.

### Phase 3: Tasks

**Agent**: `spec-tasks`

Creates `.claude/specs/<spec-name>/tasks.md` with:
- Numbered, sequential implementation tasks
- File paths for each task
- Requirements references
- Test-driven development approach
- Clear coding objectives for each step

**Approval Gate**: User can approve, request changes, or return to previous phases.

### Phase 4: Execution

**Agent**: `spec-executor`

Executes individual tasks from `tasks.md`:
- Reads requirements, design, and tasks for context
- Implements one task at a time
- Verifies against acceptance criteria
- Marks tasks complete
- Reports progress and next steps

**Execution is manual**: User controls which tasks to execute and when.

## Usage Examples

### Start a New Feature

```
/spec-mode implement a dark mode toggle in settings
```

### Execute Tasks

After spec workflow completes:

```
Execute task 1 from dark-mode-toggle
```

### Check Progress

```
What's the next task for dark-mode-toggle?
```

### Resume Work

If you return later:

```
Execute the next incomplete task from dark-mode-toggle
```

## Benefits

### Structured Planning
- Forces upfront thinking about requirements and design
- Reduces scope creep and ambiguity
- Documents decisions and rationale

### Approval Gates
- Explicit user review at each phase
- Opportunity to iterate before implementation
- Can return to previous phases if needed

### Incremental Implementation
- Tasks are small, focused, and testable
- Clear progress tracking
- Easy to resume work

### Test-Driven Approach
- Testing integrated throughout task list
- Edge cases addressed explicitly
- Quality built in from the start

### Documentation as Artifact
- Requirements, design, and tasks serve as project documentation
- Easy to onboard new contributors
- Captures context for future reference

## File Structure

```
.claude/specs/<spec-name>/
├── requirements.md    # User stories and acceptance criteria
├── design.md         # Technical architecture and components
└── tasks.md          # Implementation checklist
```

## EARS Format Reference

The workflow uses EARS (Easy Approach to Requirements Syntax) for acceptance criteria:

| Pattern | Template | Use Case |
|---------|----------|----------|
| **Ubiquitous** | The system shall &lt;behavior&gt; | Always-on requirements |
| **Event-Driven** | When &lt;trigger&gt;, the system shall &lt;behavior&gt; | Response to events |
| **State-Driven** | While &lt;state&gt;, the system shall &lt;behavior&gt; | Behavior during states |
| **Optional** | Where &lt;condition&gt;, the system shall &lt;behavior&gt; | Feature variations |
| **Complex** | If &lt;condition&gt;, then the system shall &lt;behavior&gt; | Conditional logic |
| **Unwanted** | If &lt;condition&gt;, then the system shall NOT &lt;behavior&gt; | Prohibited actions |

## Best Practices

### Requirements Phase
- Focus on user value and outcomes
- Make acceptance criteria testable
- Identify edge cases early
- Keep user stories small and independent (INVEST principles)

### Design Phase
- Use Mermaid diagrams for visual clarity
- Follow existing project conventions
- Include working code examples, not pseudocode
- Document design decisions and trade-offs

### Tasks Phase
- Each task should produce working, integrated code
- No big jumps in complexity between tasks
- Reference specific requirements for traceability
- Focus only on coding tasks (no deployment, training, etc.)

### Execution Phase
- Complete one task at a time
- Verify against acceptance criteria
- Don't over-engineer or add extra features
- Review progress regularly

## Advanced Usage

### Iterating on a Phase

If requirements or design need changes during a later phase:

```
Go back to requirements
```

The workflow will allow you to return to any previous phase.

### Custom Spec Names

The workflow derives a spec name from your description, but you can provide your own:

```
/spec-mode add caching layer
```

Suggested name: `caching-layer`
Custom name: `redis-cache-integration`

### Resuming Incomplete Specs

If you have an existing spec with incomplete tasks:

```
What tasks are left in achievement-system?
Execute task 5 from achievement-system
```

## Agents Reference

### `spec-requirements`
Gathers requirements through iteration, produces structured requirements.md with EARS criteria.

### `spec-design`
Researches codebase patterns, creates technical design with architecture and components.

### `spec-tasks`
Converts design into actionable, test-driven implementation tasks.

### `spec-executor`
Implements individual tasks, verifies against requirements, marks progress.

## Limitations

- Only tracks coding tasks (not deployment, user testing, etc.)
- Requires discipline to follow the workflow
- May feel heavy for very simple features
- Works best with clear, well-defined features

## Contributing

To extend this workflow:

1. Add new agents in `.claude/agents/`
2. Update `/spec-mode` command to invoke new agents
3. Maintain approval gates between phases
4. Keep documentation up to date

## License

This workflow system is provided as-is for use with Claude Code projects.
# spec-mode-claude-code
