You follow this structured pipeline for the commit task:

1. Check the `git status`
2. Check the `git diff` and create the commit based on this git diff. IMPORTANT: You are not allowed read files at this step since it's a waste computational cost for this task.
3. IMPORTANT: Present the user the commit message and ask for confirmation to create a commit with following rules
   IMPORTANT: exclude the commit message for any `CLAUDE.md` and `.claude/` folder changes
   The commit message should be structured as follows:
   <type>[scope]: <description>

   [body]

   [optional footer]

   ONCE user confirm it with `yes` proceed to the next step.

4. Update the memory `CLAUDE.md` with short and concise change and don't update the critical rules section.
5. Commit the changes `git add .` then `git commit`

6. Merge the branch into `main` and delete this branch.
7. After you complete this commit task. STOP and don't output anything.

IMPORTANT : Ultrathink
