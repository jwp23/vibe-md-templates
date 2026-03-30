---
name: beads-plan
description: Use when starting work on a feature bead from PRD, before writing any code. Use when bead needs breakdown into implementation steps.
---

# Beads Plan

## Overview

Break a feature bead into implementation sub-tasks. The plan lives in beads, not markdown files.

**Announce at start:** "I'm using beads-plan to break this feature into implementation steps."

## The Iron Rule

```
NO CODE WITHOUT A PLAN FIRST
```

When picking up a feature bead:

1. **STOP** - Do not implement yet
2. **EXPLORE** - Understand the codebase
3. **DESIGN** - Determine approach and steps
4. **BREAK DOWN** - Create child beads
5. **DOCUMENT** - Add notes to parent bead
6. **THEN STOP** - User reviews plan before execution

## The Process

### Step 1: Load Feature Context

```bash
bd show <bead-id>          # Read the feature bead
bd list --parent <bead-id> # Check for existing children (if epic)
```

Also read relevant PRD sections and context files to understand requirements.

### Step 2: Explore Codebase

Investigate to understand:
- What files need to change?
- What patterns exist to follow?
- What dependencies are involved?
- What tests need writing?

**For single features:** Use Task tool with Explore agent for thorough investigation.

**For epics with multiple children:** Use parallel Explore subagents (see "Parallel Exploration for Epics" below).

### Step 3: Design Approach

Determine:
- Implementation sequence (what depends on what)
- Key files to modify/create
- Testing strategy
- Any risks or blockers

### Step 4: Create Child Beads

Break into atomic, testable steps. Each child should be completable in one focused session.

```bash
# Create children with --parent
bd create "Add X to file Y" --type task --parent <parent-id>
bd create "Write tests for X" --type task --parent <parent-id>
bd create "Update config for X" --type task --parent <parent-id>

# Set dependencies if order matters
bd dep add <later-bead> <earlier-bead>
```

**Granularity guide:**
- Too big: "Implement TTFI measurement"
- Right size: "Add synthPerfDump polling loop"
- Right size: "Extract TTFI from dump response"
- Right size: "Send TTFI to Grafana metrics"

## Planning Epics with Existing Children

When the bead is an **epic** that already has child features (created by `/story`):

```bash
bd list --parent <epic-id>  # See existing children
```

**You must break down each child feature into atomic tasks:**

1. For each child feature under the epic:
   - Run `bd show <child-id>` to understand it
   - Determine if it's already atomic (single-session task)
   - If NOT atomic, create grandchildren tasks under it

2. **Atomicity test:** Can this be completed in one focused session?
   - Yes → Leave as-is
   - No → Break into smaller tasks with `--parent <child-id>`

### Parallel Exploration for Epics

When planning an epic with multiple child features, use **parallel Explore subagents** to investigate each feature area simultaneously. This keeps the main context clean and speeds up planning.

**Step 1: Identify independent vs dependent features**

Review child features and categorize:
- **Independent:** Can be explored without knowing how siblings are implemented
- **Dependent:** Requires decisions from a sibling feature first

**Step 2: Dispatch parallel subagents for independent features**

```
For each independent child feature, dispatch in parallel:

Tool: Task
Parameters:
  subagent_type: "Explore"
  prompt: |
    Explore the codebase for implementing: "<feature title>"

    Context from PRD: <paste relevant story details>

    Investigate:
    1. What files need to change?
    2. What existing patterns should we follow?
    3. What are the key implementation steps?
    4. Any risks or dependencies?

    Return a summary I can use to create implementation tasks.
```

**Step 3: Synthesize results**

After subagents return:
1. Review each exploration summary
2. Identify cross-feature dependencies discovered
3. Create beads for each feature's tasks
4. Set up `bd dep add` for any dependencies found

**When NOT to parallelize:**
- Single feature (just use one Explore agent)
- All features are dependent on each other (plan sequentially)
- Features touch the same files (conflicts likely)

### Handling Implementation Dependencies

Sometimes a child feature **cannot be planned** because its implementation depends on decisions made while implementing a sibling.

**Example:**
- Feature A: "Add config system"
- Feature B: "Use config for render modes"

You can't break down Feature B until you know HOW the config system works (file format, API, location).

**When you encounter this:**

1. **Mark the dependency explicitly:**
   ```bash
   bd dep add <feature-B> <feature-A>
   ```

2. **STOP and raise to user:**
   ```
   I cannot fully plan "<Feature B>" because it depends on implementation
   decisions in "<Feature A>".

   Specifically: [explain what decision is needed]

   Options:
   1. Plan Feature A first, implement it, then return to plan Feature B
   2. Make the decision now (e.g., "config will be JSON in src/config.js")
   3. Create a placeholder task and refine after Feature A is done

   Which approach would you prefer?
   ```

3. **Do NOT guess** at implementation details to unblock planning
4. **Do NOT skip** the blocked feature without user acknowledgment

### Step 5: Document Approach

Add implementation notes to parent bead:

```bash
bd update <parent-id> --notes "$(cat <<'EOF'
## Approach
- Brief description of implementation strategy

## Key Files
- path/to/file.js - what changes here
- path/to/other.js - what changes here

## Risks/Notes
- Any gotchas or considerations
EOF
)"
```

### Step 6: Present Plan and STOP

Show the user:
- List of child beads created
- Dependency order
- Parent bead notes (the approach)
- Any features that couldn't be fully planned (and why)

**CRITICAL: STOP HERE.**

Say: "Plan created. Child beads are ready for execution. Review and run `/beads-execute <parent-id>` when ready."

Do NOT:
- Start implementing
- Mark any beads in_progress
- Write any code

## Red Flags - STOP

- Jumping to code without breaking down the feature
- Creating only 1-2 child beads for a complex feature
- Skipping codebase exploration
- Not adding notes to parent bead
- Starting execution without user approval
- Guessing implementation details to unblock dependent planning
- Skipping a blocked feature without raising to user

## Session Boundary

This skill is designed to END a session. The plan persists in beads.

Next session can pick up with `/beads-execute <parent-id>`.
