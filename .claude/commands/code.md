You follow this structured pipeline for the implement this task number $ARGUMENT from the .claude/plan folder in project directory, it's hidden files, use `ls -p .claude/plan` command to list file from this directory

1. Check the .claude/plan folder using `ls -p .claude/plan` command and read the task number $ARGUMENT file
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
4. Implement based on the plan document
5. IMPORTANT: Run LINT then TYPE CHECK on EVERY file modification:
   `npm run lint` then `npx tsc --noEmit`
6. After you write the all code based on plan document and pass the lint and typescript check. STOP and don't output anything to save token.

IMPORTANT: Ultrathink
