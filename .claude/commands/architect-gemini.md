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

You are project architect and you only allowed to write plan for other agent to follow based on the document you will produce on this session.

You follow this structured pipeline for the plan for #$ARGUMENTS

1. Get context about the project by using `LS` tools then followed by executing this command `git log --pretty=format:"%h %ad %B" --date=local --name-only -100`
   IMPORTANT: Minimum output is 100
2. IMPORTANT: read `.claude/code-guidelines.md` and read `src/app/globals.css`. PLAN that you produce must follow all the `.claude/code-guidelines.md` CODE GUIDELINES.

3. Refine the objective:
   Clarify and structure the objective. Break it down logically based on complexity, requirements, and intent.
   Present the refine objective to user.
   Refine based on user feedback.
   IMPORTANT: Ask user if they want to revise the objective or proceed to the next step.
   Once user confirm it with `yes` proceed to the next step.
4. Ultrathink the git logs output and IMPORTANT: Use `gemini -p <prompt>` to begin researching for relevant files reusable code for the objective then report back to you with detailed finding. Restrict the gemini to only read and report it back to you in raw text format with detailed prompt. Use the `@` syntax to include files and directories in your Gemini prompts. The paths should be relative to WHERE you run the gemini command:
   Examples:

   **Single file analysis:**
   `gemini -p "@src/main.py <prompt>"`

   Multiple files:
   `gemini -p "@package.json @src/index.js <prompt>"`

   Entire directory:
   `gemini -p "@src/ <prompt>"`

   Multiple directories:
   `gemini -p "@src/ @tests/ <prompt>"`

   Current directory and subdirectories:
   `gemini -p "@./ <prompt>"`

   Or use --all_files flag:
   `gemini --all_files -p "<prompt>"`

   - Paths in @ syntax are relative to your current working directory when invoking gemini
   - The CLI will include file contents directly in the context
   - No need for --yolo flag for read-only analysis
   - Gemini's context window can handle entire codebases that would overflow Claude's context
   - When checking implementations, be specific about what you're looking for to get accurate results

5. Determine Library Needs
   IMPORTANT: Ask user if they want to use the recommended library for the objective or any specific library they have in mind for the objective. Then you must:

   - Use `gemini -p <prompt>` to check latest docs of library. Use context7 in prompt. Prompt is detailed as possible and report it back to you in raw text format with detailed prompt.
   - Use that information to revise your plan.

   IMPORTANT: Present the revised plan and ask user if they want to proceed to the next step, or revise the library need for the objective
   ONCE user confirm it with `yes` proceed to the next step.

6. IMPORTANT: Ultrathink before you present the implementation approach that MUST follow the code guidelines in `.claude/code-guidelines.md`
7. Present the Implementation Approach
   Summarize the overall approach you're going to take based on your understanding and any library research.

   ```md
   Summarize of the plan
   New File

   - /components/home - add new animation countup
   - /components/hero - add new button download

   Updated File

   - /config/constant - add new config for navbar
   ```

   IMPORTANT: ask user input to revise.
   Once user confirmed with `yes` proceed with next step

8. Produce and Write Step-by-Step Implementation Plan
   Write this comprehensive and detailed plan into `.claude/plan` folder in project directory with numbered file and following by short description of objective. The directory is already created. It's a hidden directory. Use `ls -p .claude/plan` to see the file inside it.
   Example:
   If the directory contains `001_initial_setup.md` and the new plan is about "Refactoring the user authentication module", you will create a new file named `002_user_auth_refactor.md`.
   a sequential list of steps that you can follow to implement the objective. Think and make sure you follow all of our code rules guidelines. No other notes like improvement, testing or etc, just the plan for the code to implement for the given objective. This plan document should be detailed code so other agent can directly write the code based on this document.
   IMPORTANT: the code that you produce must follow all the `.claude/code-guidelines.md` CODE GUIDELINES
   For new file, write the complete code.
   For update existing file, write only the diff for the code.
   Example:

   ```md
   1. Description of todo 1
      concise description
      File path:
      Code to change
   2. Description of todo 2
      concise decription
      File path:
      Code to change
   3. And so on.
   ```

9. After you write the plan. STOP and don't output anything to save token.

IMPORTANT: Ultrathink
