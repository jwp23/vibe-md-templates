# Claude Context Blueprint

Purpose: This is the main context file that tells the AI which documents to read and how to resolve conflicting instructions between them.

> This document serves as the central reference point for understanding project requirements, operating instructions, and development plans for the **Vision Synthetic Monitor** project.

---

## Project Overview

**Vision Synthetic Monitor** is a synthetic monitoring system that validates Fenway PACS medical image viewer functionality and measures time-to-first-image (TTFI) performance across customer environments.

- **Primary Function:** Detect viewer outages before users report them
- **Secondary Function:** Measure and track viewer performance metrics
- **Technology Stack:** k6 browser automation, Terraform, Grafana Cloud, GitLab CI/CD

---

## Required Context Files

To fully understand the project requirements, operating instructions, and development plan, I must read the following files in this directory:

- **`prd.md`**: Product requirements defining monitoring features, user stories, and roadmap
- **`infra.md`**: Infrastructure documentation covering k6 scripts, Terraform, GCS, GitLab CI/CD, and deployment architecture
- **`security.md`**: Security requirements including SOC2 controls, secrets management, and compliance
- **`sbom.md`**: Software Bill of Materials listing approved technologies, k6 modules, and Terraform providers

I will always consult these files to ensure I have the most up-to-date information before proceeding with any task.

---

## Changelog Usage

Whenever I am asked about previous commits, need to understand previous changes, or need to create a new commit, I will consult:

- **`changelog.md`**: Project changelog tracking version history, feature additions, bug fixes, and completed beads issues

---

## Beads Issue Tracking

This project uses **beads** (`bd`) for issue tracking. Issue prefix: `vsm`

### The Iron Rule

```
NO CODE WITHOUT A BEAD FIRST
```

When user requests work ("implement X", "add Y", "fix Z"):

1. **STOP** - Do not write any code yet
2. **CREATE** - `bd create "Title" --type feature|task|bug`
3. **CLAIM** - `bd update <id> --status in_progress`
4. **THEN CODE** - Only now may you implement

**No exceptions:**
- Not for "quick fixes"
- Not for "simple changes"
- Not for "I'll create the bead after"
- Not for "let me just explore first"

### Session End Protocol

Before ending any session, complete ALL steps in order:

1. File issues for remaining/discovered work
2. Close completed issues: `bd close <id>`
3. Sync beads: `bd sync --from-main` (critical for ephemeral branches)
4. Commit and push: `git add . && git commit -m "..." && git push`

### Quick Reference

```bash
bd ready                   # See unblocked work
bd create "Title" --type feature|task|bug  # Create issue
bd update vsm-XXX --status in_progress     # Claim work
bd close vsm-XXX                           # Complete work
bd sync --from-main                        # Sync on ephemeral branches
```

For detailed commands, issue types, priorities, dependencies, and edge cases, see the `beads-workflow` skill.

### Workflow Commands

Use these commands to manage sessions:
- `/gogogo` - Start a session (loads context, shows ready work, breaks work into tasks)
- `/story` - Add new feature (updates prd.md, creates initial bead)
- `/wrapup` - End a session (runs tests, closes beads, syncs, commits)

---

## Conflict Resolution Matrix

When instructions in different context files conflict, follow this order of precedence:

| Priority | Document | Scope | Override Rule |
|----------|----------|-------|---------------|
| **1** | `security.md`, `sbom.md` | Safety & supply chain | **Override all other documents** |
| **2** | `infra.md` | Runtime environment | Override incompatible feature requests |
| **3** | `CLAUDE.md` | Global conventions | Baseline project rules |
| **4** | `prd.md` | Feature requirements | May refine but not violate higher constraints |
| **5** | `beads-workflow` skill | Process/how-to | Governs plan creation and execution |

### Conflict Resolution Process

If you find a conflict, you **MUST**:

1. **State the conflict clearly** - Identify the specific conflicting instructions and their sources
2. **Follow the precedence rule** - Apply the priority order defined above
3. **Recommend minimal edits** - Suggest changes to harmonize the sources, starting with the lowest-authority document

---

## Key Project Constraints

### What This Project Does
- Validates medical image viewer loads exams successfully
- Measures time-to-first-image (TTFI) performance
- Captures screenshots for debugging
- Sends metrics to Grafana dashboards

### What This Project Does NOT Do
- Monitor login/navigation failures (graceful exit, no alerts)
- Process or store PHI (all data is scrubbed)
- Handle alerting logic (delegated to Grafana Cloud)
- Modify the PACS viewer application

### Alerting Policy
Only alert on viewer-specific failures:
- `viewer_failure`: Viewer window or canvas issues → **Alert**
- `exam_not_found`: Test data missing → **Alert**
- `non_viewer_exit`: Login, navigation, infrastructure → **No alert** (graceful exit)

---

## Documentation Conventions

### File Locations

| Type | Location | Purpose |
|------|----------|---------|
| **README.md** | `/README.md` | How to use the project and navigate the codebase |
| **Technical docs** | `/docs/` | Deep-dive technical documentation |
| **AI context files** | `/.claude/` | AI instructions, PRD, workflow rules |
| **Inline code docs** | Same directory as code | JSDoc comments in `.js` files |

### README.md Content

The root `README.md` should contain:
- **Project overview** - What this project does (1-2 paragraphs)
- **Quick start** - How to run locally, prerequisites, setup steps
- **Project structure** - Directory layout and what each folder contains
- **Configuration** - Environment variables, config files
- **Deployment** - How to deploy changes (CI/CD overview)
- **Links** - References to `/docs/` for detailed technical docs

### Rules

- **ALWAYS** place new technical documentation in `/docs/`
- **NEVER** create documentation files in `.claude/` (reserved for AI context only)
- **README.md** is for humans navigating the repo - keep it practical and concise
- Use kebab-case for doc filenames: `ttfi-extraction.md`, `deployment-guide.md`
- Reference `/docs/` from README.md for detailed technical information
