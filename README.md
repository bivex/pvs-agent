# pvs-studio-agent

A custom specialized agent and skill for Claude Code to execute static application security testing (SAST) and general static code analysis using the [PVS-Studio](https://pvs-studio.com/) analyzer for C and C++ projects.

## Installation

To install this in your project:
1. Copy the `.claude/agents/pvs-studio.md` and `.claude/skills/pvs-studio/SKILL.md` files into your project's `.claude/` directory, or drop them directly into `~/.claude/` for global use across all your repositories.
2. Ensure you have the `pvs-studio`, `pvs-studio-analyzer`, and `plog-converter` command line utilities installed on your system.

## Usage Examples

Once installed, you can trigger the analysis flow explicitly or contextually.

### Using the Slash Command (Skill)

Trigger the analysis flow explicitly using the slash command. The skill will fork a subagent to run the analysis without cluttering your main context window.

```text
/pvs-studio
```

You can pass specific instructions/arguments to guide the analysis:

```text
/pvs-studio prioritize security (OP) warnings and memory leaks
```

```text
/pvs-studio only check the files I modified recently
```

### Contextual Usage (Agent)

Claude will seamlessly utilize the `pvs-studio` subagent if you state that you want to run static analysis or check your C/C++ code for issues.

```text
> Check my code for any security vulnerabilities using PVS-Studio.
```

```text
> Can you run static analysis and fix the array out-of-bounds errors?
```

## How It Works

When invoked, the agent autonomously performs the following steps:
1. Validates the presence of the `pvs-studio-analyzer` tools.
2. Identifies the build system (Make, CMake, etc.) and generates a `compile_commands.json` (e.g., via `cmake`) or traces the compiler runs (`pvs-studio-analyzer trace`).
3. Runs the analyzer to produce `pvs-studio.log`.
4. Converts the log using `plog-converter` into easily readable formats like JSON or Markdown.
5. Reviews the high (GA:1, OP:1) and medium (GA:2, OP:2) certainty issues natively.
6. Presents the critical findings with suggested code fixes inside your Claude Code session.
