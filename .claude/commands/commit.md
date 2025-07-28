---
description: Create a git commit
allowed-tools: Read(CLAUDE.md), Edit, MultiEdit, TodoWrite, Bash(git add:*), Bash(git commit:*)
---

## Context

- Git status: !`git status`
- Git diff (staged and unstaged changes): !`git diff HEAD`
- Project Structure: !`git ls-files --others --exclude-standard --cached`
- Git diff untracked files (new files): !`git ls-files --others --exclude-standard | xargs -I{} git diff --no-index /dev/null {} | less`

## Workflow

You follow this structured pipeline for the commit task:

1. Analyze the Context
2. Update the project memory `CLAUDE.md` to document changes and preserve context for seamless work to resume in future sessions. e.g:
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
5. After you complete this commit task. STOP and don't output anything.

IMPORTANT : Ultrathink
