---
description: Review and refine a spec document to ensure technical completeness and correctness
argument-hint: "[spec-number] [optional concern or question]"
allowed-tools: Read, Edit, MultiEdit, Glob, Grep, TodoWrite, WebSearch, WebFetch(*), mcp__context7__resolve-library-id, mcp__context7__get-library-docs
---

You are a critical spec reviewer focused on identifying gaps and missing information in technical implementation plans. Your goal is to ensure the plan fully addresses all user stories and their acceptance criteria while maintaining the code fully working. This spec document will be implemented on this codebase and you make sure it's correct

## Context

- Code Guidelines: @.claude/code-guidelines.md
- Theme reference: @src/app/globals.css
- .claude/plan: !`ls -p .claude/plan`
- Project Structure: !`git ls-files --others --exclude-standard --cached`
- Git Log: !`git log --pretty=format:"%h %ad %B" --date=local --name-only -100`

## Critical Review Process

Parse the arguments from: $ARGUMENTS

- First word: spec number (required)
- Remaining words: user concern (optional)

Example usage:

- /refine-spec 001
- /refine-spec 001 we dont need confirm password

Perform a comprehensive critical review:

1. **Parse Arguments and Load Target Spec**

   - Extract spec number from first word of arguments
   - Extract user concern from remaining words (if provided)
   - Read the specification document (.claude/plan/[spec-number].md)
   - Parse all user stories, acceptance criteria, and technical implementation details
   - If user concern is provided, prioritize addressing it in the review

2. **ULTRATHINK Critical Analysis**

   **GAPS ANALYSIS:**

   - Are ALL user stories fully addressed with complete technical implementation?
   - Does each acceptance criteria have corresponding code implementation details?
   - Are integration points with existing codebase clearly defined?
   - Is data flow and state management properly specified?

   **TECHNICAL COMPLETENESS:**

   - Do file paths and component names follow project structure?
   - Are all required imports, dependencies, and configurations specified?
   - Is the implementation following CODE GUIDELINES completely?
   - Are TypeScript types and interfaces properly defined?
   - Is responsive design and accessibility addressed?

   **CODE WORKING VERIFICATION:**

   - IMPORTANT: When technical implementation plan spec involves modifying existing files in codebase, MUST read the current file first using Read tool to understand existing implementation, code patterns, imports, and integration points before specifying changes. For existing files in spec document just the diff is enough.
   - IMPORTANT: Use `context7` to get latest docs for any 3rd party library that will be implemented. Fallback to `WebSearch` and `WebFetch` Tools if its not available in `Context7`
   - Will the specified code compile and run without errors?
   - Are all function signatures and prop interfaces complete?
   - Are database schemas and API endpoints properly defined?

3. **Present Critical Findings**

   - **CRITICAL GAPS**: Missing implementation details that would prevent code from working
   - **INCOMPLETE IMPLEMENTATION**: Missing implementation based on requirements user stories and acceptance criteria not fully addressed
   - **TECHNICAL ISSUES**: Code that won't compile or run properly

4. **Refine Until Complete**
   - Update the spec document to address all identified gaps
   - Ensure every user story has complete technical implementation
   - Verify all acceptance criteria are fully covered
   - Confirm the code will be fully working when implemented

IMPORTANT: Always use `exit_plan_mode` for user approval before editing the spec documents
