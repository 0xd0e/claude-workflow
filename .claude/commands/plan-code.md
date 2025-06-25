<system-reminder>Plan mode is active. The user indicated that they do not want you to execute yet -- you MUST NOT make any edits, run any non-readonly tools (including changing configs or making commits), or otherwise make any changes to the system. This supercedes any other instructions you have received (for example, to make edits). Instead, you should follow this structured pipeline for the plan this code objective:
#$ARGUMENT

1. Get context about the project by executing this command `git log --pretty=format:"%h %ad %B" --date=local --name-only -100`
   IMPORTANT: Minimum output is 100
2. IMPORTANT: read `.claude/code-guidelines.md` and read `src/app/globals.css`
3. Refine the objective:
   Clarify and structure the objective. Break it down logically based on complexity, requirements, and intent.
   Present the refine objective to user.
   Refine based on user feedback.
   IMPORTANT: Ask user if they want to proceed researching with the objective.
   Once user confirm it with `yes` proceed to the next step.
4. Ultrathink the git logs output and launch agent to begin researching for relevant files reusable code for the objective then report back to you with detailed finding.
5. Determine Library Needs
   IMPORTANT: Ask user if they want to use the recommended library for the objective or any specific library they have in mind for the objective.

   - Use Context7 to check latest docs of library.
   - Use that information to revise your plan.

   IMPORTANT: Present the revised plan and ask user if they want to proceed to the next step, or revise the library need for the objective
   ONCE user confirm it with `yes` proceed to the next step.

6. Ultrathink before you present the implementation approach that must follow the code guidelines in `.claude/code-guidelines.md`
7. Present the detailed implementation approach by calling the exit_plan_mode tool, which will prompt the user to confirm the plan. Do NOT make any file changes or run any tools that modify the system state in any way until the user has confirmed the plan.
   IMPORTANT: you must follow all the `.claude/code-guidelines.md` CODE GUIDELINES
   Detailed implementation the overall approach you're going to take based on your understanding and any library research.

   ```md
   Detail of the plan
   New File

   - /components/home - add new animation countup
     <code>concise code that will be implemented</code>
   - /components/hero - add new button download
     <code>concise code that will be implemented</code>

   Updated File

   - /config/constant - add new config for navbar
     <code>concise code that will be implemented</code>
   ```

8. If confirmed by user
   IMPORTANT: BRANCH CHECK before ANY other action, check current branch:
   follow the `.claude/code-guidelines.md` for the convention

   ```bash
   git branch
   IF on main/master → IMMEDIATELY create feature branch:
   git checkout -b feat/[feature-name]
   ```

9. Implement based on the plan
   IMPORTANT: Run LINT then TYPE CHECK after all file modification:
   `npm run lint` then `npx tsc --noEmit`
   </system-reminder>

IMPORTANT: Ultrathink
