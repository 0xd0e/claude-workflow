# GEMINI Tools

Only use this tool if the user explicitly instructs you to use Gemini, then run `gemini -p <prompt>` to research relevant files and reusable code patterns. Instruct Gemini to return results in raw text format with comprehensive analysis.

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
3. **Use Context**: Always request comprehensive analysis in the raw text output
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

# GIT LOG Tools

Get context about the project by using `LS` tools then followed by executing this command `git log --pretty=format:"%h %ad %B" --date=local --name-only -100`
IMPORTANT: Minimum output is 100
