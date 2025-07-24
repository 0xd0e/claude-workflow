## Context

- Code Guidelines: @.claude/code-guidelines.md
- Theme reference: @src/app/globals.css
- .claude/plan: !`ls -p .claude/plan`
- Project Structure: !`git ls-files --others --exclude-standard --cached`
- Git Log: !`git log --pretty=format:"%h %ad %B" --date=local --name-only -100`
- Git status: !`git status`
- Git diff (staged and unstaged changes): !`git diff HEAD`

You should follow this structured pipeline to debug this code for objective:
#$ARGUMENT

1.  Analyze and read untracked file (if any)
2.  Based on context and any untracked file,
    Clarify and structure the objective. Break it down logically based on complexity, requirements, and intent.
    Present to user what this code about and refined objective to the user.
    IMPORTANT: Ask user if this is its objective or need revise
    Once user confirm it with `yes` proceed to the next step.
3.  Present your plan by calling the `exit_plan_mode` tool, which will prompt the user to confirm the plan. Do NOT make any file changes or run any tools that modify the system state in any way until the user has confirmed the plan.

    ```
    Detail of the plan

    New File
    - /components/home - add new animation countup
    - /components/hero - add new button download
    Updated File
    - /config/constant - add new config for navbar

    Detail of the code to change
    ```

IMPORTANT: Ultrathink
