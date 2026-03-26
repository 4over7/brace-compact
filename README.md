# brace-compact

A Claude Code skill that prepares conversations for context compaction (`/compact`).

## What it does

Before `/compact` truncates your conversation, this skill:

1. Checks for uncommitted git changes
2. Updates project memory files with session highlights
3. Verifies in-progress plans reflect current state
4. Saves any feedback/corrections to memory
5. Outputs a compact-ready summary

## Install

```bash
claude install github:4over7/brace-compact
```

## Usage

In Claude Code, say:
- `/brace-compact`
- "brace compact"
- "prepare for compact"

Optionally pass notes: `/brace-compact important decision about X`
