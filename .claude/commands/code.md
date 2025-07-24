---
description: Implement code based on the specified spec document
argument-hint: Spec document number (e.g., 003)
---

- .claude/plan: !`ls -p .claude/plan`

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
   git checkout -b feat/[feature-name]
   ```
4. Implement based on the spec document
5. IMPORTANT: Run LINT then TYPE CHECK after all file modification:
   `npm run lint` then `npx tsc --noEmit`
   IMPORTANT: If any errors, warnings, or unused code are detected, ultrathink and validate whether to add, remove, or adjust code based on the user stories and acceptance criteria.
   Every fix must be intentional and aligned with the intended functionality and requirements based on the spec document
6. After you write the all code based on spec document and pass the lint and typescript check. STOP and don't output anything to save token.

IMPORTANT: Ultrathink
