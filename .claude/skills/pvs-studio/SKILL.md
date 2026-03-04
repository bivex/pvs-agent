---
name: pvs-studio
description: Run PVS-Studio static analysis on the project to find and fix bugs, security issues, and undefined behavior. Use proactively when code quality or security needs to be checked.
context: fork
agent: pvs-studio
---

Run PVS-Studio static code analysis on the current project. $ARGUMENTS

1. Assess the target project and determine the build system (Make, CMake, compdb, etc.).
2. Set up the environment to run PVS-Studio. If using CMake, generate `compile_commands.json` (`cmake -DCMAKE_EXPORT_COMPILE_COMMANDS=On .`). If using Make, intercept compilation (`pvs-studio-analyzer trace -- make`).
3. Run the analysis using `pvs-studio-analyzer analyze -o pvs-studio.log -j4`.
4. Convert the output using `plog-converter`. Use `-t json` or `-t markdown` so you can easily read the results. For example: `plog-converter -a GA:1,2;OP:1,2 -t json -r . -o pvs-report.json pvs-studio.log`.
5. Review the converted analysis results (e.g., `pvs-report.json`) and summarize the critical bugs. Look specially for High (Level 1) and Medium (Level 2) certainty issues in General Analysis (GA) and Security (OP).
6. Present a clear summary of findings to the user.
7. Suggest or implement code fixes for the critical issues found by the analyzer.
