---
description: Plan and implement code with full architecture and implementation workflow
---

You follow this structured two-phase pipeline to plan and implement code for #$ARGUMENT

## Context

- Code Guidelines : @.claude/code-guidelines.md
- Theme reference : @src/app/globals.css  
- Plan Directory : !`ls -p .claude/plan`
- Project Structure : !`git ls-files --others --exclude-standard --cached`
- Git Log : !`git log --pretty=format:"%h %ad %B" --date=local --name-only -100`

## Workflow

### PHASE 1: ARCHITECTURE & SPECIFICATION

1. Refine the objective:
   Clarify and structure the objective. Break it down logically based on complexity, requirements, and intent.
   Present the refined objective to the user.
   Refine based on user feedback.
   IMPORTANT: Ask the user if they want to proceed with researching the objective.
   Once the user confirms with `yes`, proceed to the next step.

2. Analyze the context. Launch an agent to identify relevant files, existing code patterns, integration points, and reusable components that align with the objective, emphasizing opportunities to eliminate duplication and follow DRY (Don't Repeat Yourself) principles. Then report back with detailed findings.

3. Determine Library Needs
   IMPORTANT: Ask the user if they want to use the recommended library for the objective or any specific library they have in mind. Then you must:

   - Use `Context7` to check the latest docs of the library that you will be using for the objective.
   - Use that information to revise your spec document

   Summarize the overall approach you're going to take based on your understanding and the latest docs of the library research.
   IMPORTANT: Ultrathink before you present the spec that MUST follow the code guidelines
   IMPORTANT: Present the revised spec and ask the user if they want to proceed to the next step, or revise the library need for the objective

   ```md
   Summary of the spec

   User stories
   Acceptance Criteria (EARS Notation)  
   Summary of the Design

   New File

   - /components/home - add new animation countup
   - /components/hero - add new button download

   Updated File

   - /config/constant - add new config for navbar
   ```

   ONCE the user confirms with `yes`, proceed to the next step.

4. Produce and Write Detailed Spec Document

   Write this comprehensive and detailed spec document into the `.claude/plan` folder in the project directory with a numbered file followed by a short description of the objective. The directory is already created. It's a hidden directory.
   Example:
   If the directory contains `001_initial_setup.md` and the new spec is about "Refactoring the user authentication module", you will create a new file named `002_user_auth_refactor.md`.
   Think and make sure you follow all of our code rules and guidelines. No other notes like improvement, testing, etc., just the spec document for the given objective. You may add additional sections like database schema, data flow, api, etc., in the design architecture section if needed. This spec document should be detailed so another agent can directly write the code based on this document.
   IMPORTANT: the code that you produce must follow all the CODE GUIDELINES and Theme reference
   For a new file, write the complete code.
   For an updated existing file, write only the diff for the code.

5. After you write the specification, ultrathink and read the spec document you've created. Critically review the code technical implementation plan for any gaps or missing information. Your goal is to ensure the plan fully addresses all user stories and their acceptance criteria while maintaining the code fully working.

### PHASE 2: IMPLEMENTATION

6. Read the spec document from `.claude/plan` folder that was created in Phase 1

7. IMPORTANT: BRANCH CHECK before ANY other action, check current branch:
   The branch specification by describing with feat/, fix/ and chore/ and it should be structured as follows:
   `<type>/<description>`

   - main: The main development branch
   - feat/: For new features (e.g., feat/add-login-page)
   - fix/: For bug fix (e.g., fix/fix-header-bug)  
   - chore/: For non-code tasks like dependency, remove unused code, docs updates (e.g., chore/update-dependencies)

   ```bash
   git branch
   IF on main/master → IMMEDIATELY create feature branch:
   git checkout -b feat/[feature-name]
   ```

8. Implement based on the spec document

9. IMPORTANT: Run LINT then TYPE CHECK after all file modification:
   `npm run lint` then `npx tsc --noEmit`
   IMPORTANT: If any errors, warnings, or unused code are detected, ultrathink and validate whether to add, remove, or adjust code based on the user stories and acceptance criteria.
   Every fix must be intentional and aligned with the intended functionality and requirements based on the spec document

10. After you write all the code based on spec document and pass the lint and typescript check. STOP and don't output anything to save token.

IMPORTANT: Ultrathink
