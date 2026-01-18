---
description: "Start a specification interview for a feature"
argument-hint: "FEATURE_NAME [--context FILE] [--output-dir DIR] [--max-questions N]"
allowed-tools: ["Bash(${CLAUDE_PLUGIN_ROOT}/scripts/setup-lisa.sh:*)", "AskUserQuestion", "Read", "Write", "Glob", "Grep"]
hide-from-slash-command-tool: "true"
---

# Lisa Plan Command

Execute the setup script to initialize the specification interview:

```!
"${CLAUDE_PLUGIN_ROOT}/scripts/setup-lisa.sh" $ARGUMENTS
```

You are now conducting a comprehensive specification interview. Follow the instructions provided by the setup script exactly.

## INTERVIEW PHASE RULES
1. EVERY question must use AskUserQuestion - plain text questions won't work
2. Ask NON-OBVIOUS questions (not "what should it do?" but "how should X handle Y?")
3. Continue until user says "done" or "finalize"
4. Update the draft spec file regularly using the Write tool

## FINALIZATION CONSTRAINTS - CRITICAL

When the user says "done", "finalize", "finished", or similar:

### ALLOWED ACTIONS (Read and Write ONLY):
- Read any files needed to compile the final spec
- Write the final spec to {output-dir}/{slug}.md
- Write the JSON spec to {output-dir}/{slug}.json
- Write the progress file to {output-dir}/{slug}-progress.txt
- Delete the state file at .claude/lisa-{slug}.md using Write with empty content

### FORBIDDEN ACTIONS (DO NOT USE):
- NO Bash tool calls - do not run any commands
- NO Edit tool calls - do not modify existing code
- NO Task tool calls - do not launch subagents
- NO implementation of any kind - you are ONLY writing spec documents

### FINALIZATION SEQUENCE:
1. Write the final markdown spec file
2. Write the JSON spec file
3. Write the progress file with all phases marked as [PENDING]
4. Delete the state file (.claude/lisa-{slug}.md)
5. Output <promise>SPEC COMPLETE</promise>
6. STOP IMMEDIATELY - do not continue with any other actions

### CRITICAL: SPEC COMPLETE MEANS STOP
After outputting <promise>SPEC COMPLETE</promise>, you MUST stop. Do not:
- Offer to implement the feature
- Suggest next steps for implementation
- Make any code changes
- Run any commands

The spec is the deliverable. Implementation is a separate task.
