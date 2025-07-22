---
description: Create a spec document with gemini
---

# GEMINI Tools

Only use this tool if the user explicitly instructs you to use Gemini, then run `gemini -p <prompt>`
Instruct Gemini to return results in raw text format with comprehensive analysis.

## File Path Syntax

Use the `@` syntax to include files and directories in your Gemini prompts. All paths are relative to your current working directory when running the gemini command.

### Basic Examples:

**Single file analysis:**

```bash
gemini -p "@src/main.py Analyze this file's architecture and identify key patterns. Report back in raw text format with detailed findings."
```

**Multiple files:**

```bash
gemini -p "@package.json @src/index.js Compare dependencies with main entry point implementation. Report back in raw text format with detailed analysis."
```

**Entire directory:**

```bash
gemini -p "@src/ Analyze the codebase structure and identify common patterns. Report back in raw text format with comprehensive findings."
```

**Multiple directories:**

```bash
gemini -p "@src/ @tests/ Compare implementation patterns between source and test files. Report back in raw text format with detailed comparison."
```

**Current directory and subdirectories:**

```bash
gemini -p "@./ Provide comprehensive overview of entire project structure. Report back in raw text format with detailed breakdown."
```

**All files flag:**

```bash
gemini --all_files -p "Analyze entire codebase for security vulnerabilities. Report back in raw text format with detailed security assessment."
```

## Usage Patterns

### Code Analysis

- **Pattern Discovery**:

```bash
gemini -p "@src/ Find all instances of error handling patterns and report variations. Report back in raw text format with detailed examples and recommendations."
```

- **Architecture Review**:

```bash
gemini -p "@./ Analyze the overall architecture and identify potential improvements. Report back in raw text format with detailed architectural assessment."
```

- **Dependency Analysis**:

```bash
gemini -p "@package.json @yarn.lock Identify outdated or vulnerable dependencies. Report back in raw text format with detailed dependency report."
```

### Documentation Research

- **Library Documentation**:

```bash
gemini -p "Use context7 to get latest documentation for shadcn library. Report back in raw text format with detailed examples and usage patterns."
```

- **API Reference**:

```bash
gemini -p "Find context7 documentation for authentication patterns. Report back in raw text format with detailed API reference and implementation examples."
```

### Implementation Guidance

- **Best Practices**:

```bash
gemini -p "@src/components/ Identify React component patterns and suggest improvements. Report back in raw text format with detailed pattern analysis and recommendations."
```

- **Code Quality**:

```bash
gemini -p "@src/ @tests/ Analyze test coverage and identify missing test cases. Report back in raw text format with detailed testing assessment."
```

### Security Analysis

- **Vulnerability Assessment**:

```bash
gemini -p "@src/ @package.json Scan for security vulnerabilities and unsafe practices. Report back in raw text format with detailed security findings."
```

- **Access Control Review**:

```bash
gemini -p "@src/auth/ @src/middleware/ Review authentication and authorization implementation. Report back in raw text format with detailed security analysis."
```

### Performance Analysis

- **Performance Bottlenecks**:

```bash
gemini -p "@src/ Identify performance bottlenecks and optimization opportunities. Report back in raw text format with detailed performance analysis."
```

- **Bundle Analysis**:

```bash
gemini -p "@webpack.config.js @src/ Analyze bundle size and optimization potential. Report back in raw text format with detailed bundle assessment."
```

## Key Features

- **Direct File Inclusion**: The `@` syntax includes actual file contents in the prompt context
- **Relative Path Resolution**: All paths resolve relative to your current working directory
- **Read-Only Analysis**: No --yolo flag needed for safe code analysis
- **Comprehensive Coverage**: Use --all_files for complete project analysis

## Best Practices

1. **Always Include Raw Text Request**: Every prompt must end with "Report back in raw text format with detailed [analysis/findings/assessment]"
2. **Be Specific**: Include detailed prompts about what you're looking for
3. **Use Context7**: Always request comprehensive analysis in the raw text output
4. **Scope Appropriately**: Use specific directories/files rather than entire codebase when possible
5. **Combine Sources**: Mix configuration files with implementation for better context

## Gemini Model Usage

**Default Usage:**

```bash
gemini -p <prompt>
```

**Rate Limit Fallback:**
If you encounter rate limits with the default model, use this:

```bash
gemini -p <prompt> -m gemini-2.5-flash
```

**IMPORTANT:** Only use the `-m` flag when rate limited. Always try the default command first.
**IMPORTANT**: Always prompt gemini to report back to you in raw text format.

# Architect Gemini Workflow

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
2.  Analyze the context. Use `gemini` to identify relevant files, existing code patterns, integration points, and reusable components that align with the objective, emphasizing opportunities to eliminate duplication and follow DRY (Don't Repeat Yourself) principles. Then report back with detailed findings. Restrict the `gemini` to only read files and report it back to you in raw text format with detailed prompt.

3.  Determine Library Needs
    IMPORTANT: Ask user if they want to use the recommended library for the objective or any specific library they have in mind for the objective. Then you must:

    - Use `gemini -p <prompt>` to check latest docs of library. Use context7 in prompt. Prompt is detailed as possible and report it back to you in raw text format with detailed prompt.
    - Use that information to revise your spec document.

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
