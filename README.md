# brace-compact

Save all session context before ending a Claude Code conversation. Seamless recovery next time.

在 Claude Code 会话结束前，自动保存所有工作上下文到文件，确保下次无缝恢复。

> Pairs with [`/resume`](https://github.com/4over7/resume) for seamless context recovery.

## Problem

Claude Code loses conversation context when you:

- Run `/compact` to free up the context window
- Exit the session (restart Claude Code, load new hooks/settings)
- Session times out or disconnects
- Switch to a different project
- Call it a day and come back tomorrow

Important context — debugging conclusions, architecture decisions, environment state — gets lost. `brace-compact` saves it to files before the session ends. Next time, `/resume` brings it all back.

Claude Code 会在以下场景丢失上下文：`/compact` 压缩对话、退出会话、超时断连、切换项目、隔天继续。调试结论、架构决策、环境状态等重要信息都会丢失。`brace-compact` 在会话结束前保存到文件，下次用 `/resume` 恢复。

## Install

```bash
claude install github:4over7/brace-compact
```

> Also install the companion: `claude install github:4over7/resume`

## Usage

Run before ending your session:

```
/brace-compact
```

With session notes:

```
/brace-compact migrating database to new schema, paused at step 3
```

## Workflows

**Compact:**
```
Working...
  → /brace-compact          # save context
  → /compact                 # compress conversation
  → /resume                  # restore context
  → Continue working
```

**Exit & restart:**
```
Working...
  → /brace-compact          # save context
  → exit                     # quit Claude Code
  → claude                   # restart
  → /resume                  # restore context
  → Continue working
```

**Next day:**
```
Working...
  → /brace-compact paused at database migration step 3
  → exit
  ... next morning ...
  → claude
  → /resume
  → Continue from step 3
```

## What it saves | 保存内容

| File | Content |
|------|---------|
| `.claude/resume.md` | Structured checkpoint: task state, critical context, next steps, environment |
| `MEMORY.md` | Updated project memory: decisions, debugging conclusions, pending work |
| Feedback memories | User corrections saved for future sessions |

## Companion: [resume](https://github.com/4over7/resume)

`brace-compact` saves. [`resume`](https://github.com/4over7/resume) restores. Install both:

```bash
claude install github:4over7/brace-compact
claude install github:4over7/resume
```

## Design

Follows [Anthropic's skill design guidelines](https://docs.anthropic.com/en/docs/claude-code/skills):

- `disable-model-invocation: true` — only invoked manually
- `allowed-tools: Read, Write, Edit, Bash, Glob, Grep`
- Portable: relative paths, works in any project
- Self-contained: `resume.md` + `MEMORY.md` is enough to continue
