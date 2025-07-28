---
description: Implement code based on the specified spec document
argument-hint: Spec document number (e.g., 003)
allowed-tools: Read, Write, Edit, MultiEdit, TodoWrite, Bash(npm run lint), Bash(npx tsc --noEmit), Bash(git branch:*), Bash(git switch:*), Bash(mkdir:*)
---

- .claude/plan: !`ls -p .claude/plan`
- Project Structure: !`git ls-files --others --exclude-standard --cached`

You follow this structured pipeline for the implement this task number $ARGUMENT

1. Read the task number #$ARGUMENT file
2. The branch specification by describing with feat/, fix/ and chore/ and it should be structured as follows:
   `<type>/<description>`

   - main: The main development branch
   - feat/: For new features (e.g., feat/add-login-page)
   - fix/: For bug fix (e.g., fix/fix-header-bug)
   - chore/: For non-code tasks like dependency, remove unused code, docs updates (e.g., chore/update-dependencies)

3. IMPORTANT: BRANCH CHECK before ANY other action, check current branch:
   ```bash
   git branch
   IF on main/master → IMMEDIATELY create feature branch:
   git switch -c feat/[feature-name]
   ```
4. Implement based on the spec document
5. IMPORTANT: Run LINT then TYPE CHECK after all file modification:
   `npm run lint` then `npx tsc --noEmit`
6. IMPORTANT: If any errors, warnings, or unused code are detected, ultrathink and validate whether to add, remove, or adjust code based on the user stories and acceptance criteria.
   Every fix must be intentional and aligned with the intended functionality and requirements based on the spec document.
   IMPORTANT: Use `exit_plan_mode` to present the fix to user and wait for user approval to apply the fix

IMPORTANT: Ultrathink
