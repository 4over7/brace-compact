# brace-compact

在 Claude Code 会话结束前，自动保存所有工作上下文到文件，确保下次无缝恢复。

> **Part of the compact-resume cycle** — pairs with [`/resume`](https://github.com/4over7/resume) to give Claude Code seamless context recovery.

## Problem

Claude Code 的会话上下文会在以下场景丢失：

- **`/compact`** — 压缩对话历史，释放上下文窗口
- **退出会话** — 需要重启 Claude Code（如加载新 hook、更新配置）
- **会话超时** — 长时间不操作，连接断开
- **切换项目** — 离开当前项目去做其他事
- **当天结束** — 下班前保存进度，第二天继续

这些场景都会导致 AI 丢失重要上下文：调试结论、架构决策、环境状态、待办事项。`brace-compact` 在会话结束前把这些保存到文件，下次用 `/resume` 恢复。

## Install

```bash
claude install github:4over7/brace-compact
```

> Also install the companion skill: `claude install github:4over7/resume`

## Usage

在以下任何时候运行：

```
/brace-compact
```

Or with session notes:

```
/brace-compact migrating database to new schema, paused at step 3
```

## Workflows

**Compact（压缩对话）：**
```
Working...
  → /brace-compact          # save context
  → /compact                 # compress conversation
  → /resume                  # restore context
  → Continue working
```

**Exit（退出会话重启）：**
```
Working...
  → /brace-compact          # save context
  → exit                     # quit Claude Code
  → claude                   # restart
  → /resume                  # restore context
  → Continue working
```

**End of day（跨天恢复）：**
```
Working...
  → /brace-compact 做到了数据库迁移第3步
  → exit
  ... next morning ...
  → claude
  → /resume
  → Continue from step 3
```

## What it saves

| File | Content |
|------|---------|
| `.claude/resume.md` | Structured checkpoint: task state, critical context, next steps, environment |
| `MEMORY.md` | Updated project memory with decisions, debugging conclusions, pending work |
| Feedback memories | User corrections saved for future sessions |

## Companion: [resume](https://github.com/4over7/resume)

`brace-compact` saves context. [`resume`](https://github.com/4over7/resume) restores it.

Install both to complete the cycle:

```bash
claude install github:4over7/brace-compact
claude install github:4over7/resume
```

After exiting or compacting, say "resume" or run `/resume` to restore saved context.

## Design

Follows [Anthropic's skill design guidelines](https://docs.anthropic.com/en/docs/claude-code/skills):

- `disable-model-invocation: true` — only invoked manually (has side effects: writes files)
- `allowed-tools: Read, Write, Edit, Bash, Glob, Grep` — restricted tool set
- Portable: uses relative paths (`.claude/resume.md`), works in any project
- Checkpoint is self-contained: reading ONLY `resume.md` + `MEMORY.md` is enough to continue
