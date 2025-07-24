---
description: Create a git commit
---

## Context

Git status: !`git status`
Git diff (staged and unstaged changes): !`git diff HEAD`

## Workflow

You follow this structured pipeline for the commit task:

1. Analyze and read untracked file (if any that are not in `.claude/` folder)
2. Update the project memory `CLAUDE.md` to document changes and preserve context for seamless work resumption in future sessions. e.g:
   - New features implemented or modified
   - Architectural changes or patterns introduced
   - Key files affected and their purposes
   - Any configuration updates or dependency changes
   - Current development state
3. IMPORTANT: Present the user the commit message then create a commit with following rules
   IMPORTANT: exclude the commit message for any `CLAUDE.md` and `.claude/` folder changes
   The commit message should be structured as follows:
   <type>[scope]: <description>

   [body]

   [optional footer]

4. Commit the changes `git add .` then `git commit`
5. Merge the branch into `main` and delete this branch.
6. After you complete this commit task. STOP and don't output anything.

IMPORTANT : Ultrathink
