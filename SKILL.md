---
name: brace-compact
description: Prepares the conversation for context compaction (/compact) by persisting all important session knowledge to memory files, checking uncommitted changes, and verifying plan status. Use when the user says "brace compact", "prepare for compact", or before running /compact.
argument-hint: "[optional notes to save]"
---

# Prepare for Context Compaction

Ensure no important context is lost before `/compact` truncates the conversation.

## Steps

Execute all steps, then output the summary.

### 1. Check uncommitted changes

```bash
git status --short
```

- If uncommitted changes exist, warn: **"You have N uncommitted files. Consider committing before compact."**
- If clean, note it.

### 2. Check running background tasks

Look for any active background tasks, running processes, or pending operations that would be lost after compact. Examples:
- Background shell commands (compilation, install, deploy)
- Ongoing SSH sessions or tunnels
- Temporary test servers that need to be stopped or made permanent

If any exist, list them and suggest how to handle them (wait, stop, or note for later).

### 3. Update memory

Locate the project's memory index file (`MEMORY.md` in the memory directory). Review the current session and update if any of these changed:

- **New features / architecture** — significant code additions or refactors
- **New files / directories** — important structural changes
- **Decisions made** — technical decisions, rejected approaches, and WHY (include debugging conclusions like "X doesn't work because Y, use Z instead")
- **Pending work** — anything started but not finished, with enough detail to resume
- **New bugs discovered** — known issues not yet fixed
- **Environment state** — server processes started, config changes made, temporary workarounds in place
- **Version number** — if bumped during this session

Be concise — memory stores pointers, not full details. Do not duplicate what git history already records. But DO save non-obvious technical conclusions that aren't in code or git.

### 4. Save session note

If `$ARGUMENTS` is provided, save it as a memory file:
- File: `memory/session_note_YYYY-MM-DD.md` (use today's date)
- Type: project
- Content: the arguments text, plus any relevant context from the conversation

### 5. Check in-progress plans

If an active plan file exists, verify it reflects current progress. Mark completed steps. If the plan is fully done, note it for cleanup.

### 6. Save feedback memories

If the user gave corrections or preferences during this session (e.g., "don't do X", "always do Y", "that approach worked well"), save to appropriate feedback memory files so future sessions respect them.

### 7. Output summary

```
🔒 Compact preparation complete

📝 Memory: [updated / no changes needed]
📋 Plan: [updated / none active / completed]
💾 Uncommitted: [none / N files — consider committing]
⚙️ Background: [none / list of running tasks]
🔄 Session highlights:
  - [key bullet points of what was done]
📌 Resume after compact:
  - [what to do next, with enough context to continue]

Ready for /compact.
```
