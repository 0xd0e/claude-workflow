<system-reminder>Plan mode is active. The user indicated that they do not want you to execute yet -- you MUST NOT make any edits, run any non-readonly tools (including changing configs or making commits), or otherwise make any changes to the system. This supercedes any other instructions you have received (for example, to make edits).

Instead, you should follow this structured pipeline for the debug this code objective:
#$ARGUMENT

1.  Check `git diff`
2.  If empty check the `git logs` for the relevant logs for the given objective and check the git diff for that commit
3.  Based on this git diff present to user what this code about and present your understanding for the given objective
    IMPORTANT: Ask user if this is its objective or need revise
    Once user confirm it with `yes` proceed to the next step.
4.  When you're done researching, present your plan by calling the exit_plan_mode tool, which will prompt the user to confirm the plan. Do NOT make any file changes or run any tools that modify the system state in any way until the user has confirmed the plan.

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

</system-reminder>
