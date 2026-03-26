# brace-compact

A Claude Code skill that prepares conversations for context compaction (`/compact`) by persisting all session knowledge to checkpoint files.

## Problem

When you run `/compact`, Claude Code compresses the conversation history. Important context — debugging conclusions, architecture decisions, environment state — can be lost. `brace-compact` saves this context to files that survive compaction.

## Install

```bash
claude install github:4over7/brace-compact
```

## Usage

Before running `/compact`:

```
/brace-compact
```

Or with session notes:

```
/brace-compact migrating database to new schema, paused at step 3
```

Then run `/compact`. After compaction, use the companion skill `/resume` to restore context.

## What it saves

| File | Content |
|------|---------|
| `.claude/resume.md` | Structured checkpoint: task state, critical context, next steps, environment |
| `MEMORY.md` | Updated project memory with decisions, debugging conclusions, pending work |
| Feedback memories | User corrections saved for future sessions |

## Companion: resume

Install the resume skill to complete the cycle:

```bash
claude install github:4over7/resume
```

After `/compact`, say "resume" or run `/resume` to restore saved context.

## Design

Follows [Anthropic's skill design guidelines](https://docs.anthropic.com/en/docs/claude-code/skills):

- `disable-model-invocation: true` — only invoked manually (has side effects: writes files)
- `allowed-tools: Read, Write, Edit, Bash, Glob, Grep` — restricted tool set
- Portable: uses relative paths (`.claude/resume.md`), works in any project
- Checkpoint is self-contained: reading ONLY `resume.md` + `MEMORY.md` is enough to continue
