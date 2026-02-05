---
description: Wrap up the current session - commit changes, update docs, and prepare for next session
---

## Session Wrap-Up Checklist

Please perform the following wrap-up tasks to close this session cleanly:

### 1. Git & Code Status
- Run `git status` to check for any uncommitted changes
- If there are changes, use `/conventional-commits` to stage and commit
- Push all commits to the remote repository

**Note:** Do NOT use `git add -f` for gitignored files (e.g., `.beads/`, `.claude/`). If these directories are gitignored, their changes are tracked separately from the main repo. Just skip committing them.

### 2. Beads Issue Updates
Update issue tracking for work completed this session:
- Close completed issues: `bd close <id>`
- Update in-progress issues: `bd update <id> --status in_progress`
- Create issues for any discovered follow-up work: `bd create "<title>"`
- Add comments for context if needed: `bd comment <id> "<note>"`

### 3. Documentation Updates
Check and update the following files as needed:
- `.claude/changelog.md` - Add entries for any completed features or bug fixes from this session

**If `.claude/` is gitignored:** The changelog is local documentation only. Update it for your own reference but don't try to commit it.

### 4. Build Verification
- Run your project's build command to ensure everything compiles (e.g., `npm run build`, `cargo build`, `go build`, etc.)
- Note any warnings or errors that should be addressed in the next session

> **Note:** Customize the build command for your specific project stack.

### 5. Run Quality Gates (Project-Specific)
If the project has a `beads-workflow` skill, invoke it to run project-specific tests:
- Check `.claude/skills/beads-workflow/SKILL.md` for testing commands
- Run the project's test suite before closing issues
- Note any failures that need follow-up issues

**If no project-specific skill exists**, ask the user about test commands.

### 6. Sync and Push
Ensure all work is synced and pushed to remote:
```bash
git pull --rebase
bd sync --from-main    # Critical for ephemeral/feature branches
git push
git status  # Must show "up to date with origin"
```

**Critical:**
- Work is NOT complete until `git push` succeeds
- Use `bd sync --from-main` on feature branches to pull beads updates from main

### 7. Session Summary
Provide a brief summary including:
- **Completed:** What was accomplished in this session
- **In Progress:** Any work that is partially complete and its current state
- **Blockers:** Any issues or blockers encountered
- **Next Steps:** Clear list of what to work on next (reference beads issue IDs)

### 8. Handoff Message
End with a clear handoff message that can be used to resume work, such as:
"Ready to resume. Next session: [specific task or feature to continue] (see issue <id>)"

### 9. Improve Session Startup (Optional)
Evaluate if anything learned during this session should be added to `.claude/commands/gogogo.md` to help future sessions:
- New context files that should be loaded at startup
- Additional checks that would have been helpful
- Project-specific setup steps discovered during work
- Environment variables or dependencies that caused issues

If improvements are identified, offer to update the gogogo.md file.
