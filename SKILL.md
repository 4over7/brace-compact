---
name: brace-compact
description: Prepares the conversation for context compaction (/compact) by persisting all important session knowledge to memory files, checking uncommitted changes, and verifying plan status. Use when the user says "brace compact", "prepare for compact", or before running /compact.
argument-hint: "[optional notes to save]"
---

# Prepare for Context Compaction

Ensure no important context is lost before `/compact` truncates the conversation.

## Steps

Execute all steps, then output the summary. If `$ARGUMENTS` is provided, include it as a session note in memory.

### 1. Check uncommitted changes

```bash
git status --short
```

- If uncommitted changes exist, warn: **"You have N uncommitted files. Consider committing before compact."**
- If clean, note it.

### 2. Update memory

Locate the project's memory index file (`MEMORY.md` in the memory directory). Review the current session and update if any of these changed:

- **Version number** — if bumped during this session
- **New features / architecture** — significant code additions or refactors
- **New files / directories** — important structural changes
- **Decisions made** — technical decisions, rejected approaches, why
- **Pending work** — anything started but not finished
- **New bugs discovered** — known issues not yet fixed
- **Test count** — if tests were added or removed

Be concise — memory stores pointers, not full details. Do not duplicate what git history already records.

### 3. Check in-progress plans

If an active plan file exists, verify it reflects current progress. Mark completed steps. If the plan is fully done, note it for cleanup.

### 4. Save feedback memories

If the user gave corrections or preferences during this session (e.g., "don't do X", "always do Y", "that approach worked well"), save to appropriate feedback memory files so future sessions respect them.

### 5. Output summary

```
🔒 Compact preparation complete

📝 Memory: [updated / no changes needed]
📋 Plan: [updated / none active / completed]
💾 Uncommitted: [none / N files — consider committing]
🔄 Session highlights:
  - [key bullet points of what was done]

Ready for /compact.
```
