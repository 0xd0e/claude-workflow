---
description: Create a spec document
---

You are a project architect and you are only allowed to write spec documents for other agents to follow based on the document you will produce on this session.

## Context

- Example Spec : @.claude/plan/000_example_spec.md
- Code Guidelines : @.claude/code-guidelines.md
- Theme reference : @src/app/globals.css
- Plan Directory : !`ls -p .claude/plan`
- Project Structure : !`git ls-files --others --exclude-standard --cached`
- Git Log : !`git log --pretty=format:"%h %ad %B" --date=local --name-only -100`

## Workflow

You follow this structured pipeline to create a spec document for #$ARGUMENTS

1.  Refine the objective:
    Clarify and structure the objective. Break it down logically based on complexity, requirements, and intent.
    Present the refined objective to the user.
    Refine based on user feedback.
    IMPORTANT: Ask the user if they want to proceed with researching the objective.
    Once the user confirms with `yes`, proceed to the next step.
2.  Analyze the context. Launch an agent to identify relevant files, existing code patterns, integration points, and reusable components that align with the objective, emphasizing opportunities to eliminate duplication and follow DRY (Don't Repeat Yourself) principles. Then report back with detailed findings.
3.  Determine Library Needs
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

4.  Produce and Write Detailed Spec Document

    Write this comprehensive and detailed spec document into the `.claude/plan` folder in the project directory with a numbered file followed by a short description of the objective. The directory is already created. It's a hidden directory.
    Example:
    If the directory contains `001_initial_setup.md` and the new spec is about "Refactoring the user authentication module", you will create a new file named `002_user_auth_refactor.md`.
    Think and make sure you follow all of our code rules and guidelines. No other notes like improvement, testing, etc., just the spec document for the given objective. You may add additional sections like database schema, data flow, api, etc., in the design architecture section if needed. This spec document should be detailed so another agent can directly write the code based on this document.
    IMPORTANT: the code that you produce must follow all the CODE GUIDELINES and Theme reference
    For a new file, write the complete code.
    For an updated existing file, write only the diff for the code.

5.  After you write the specification, ultrathink and read the spec document you've created. Critically review the code technical implementation plan for any gaps or missing information. Your goal is to ensure the plan fully addresses all user stories and their acceptance criteria while maintaining the code fully working.

IMPORTANT: Ultrathink
