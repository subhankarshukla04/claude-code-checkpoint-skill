# Claude Code Checkpoint Skill

A custom skill for [Claude Code](https://claude.ai/code) that saves and resumes task context across compactions or sessions.

## Installation

Copy `checkpoint.md` to your Claude Code skills directory:

```bash
cp checkpoint.md ~/.claude/skills/
```

## Usage

- `/checkpoint` or `/checkpoint save` — Save current task state
- `/checkpoint resume` — Load saved state and continue automatically

## How It Works

When you save a checkpoint, Claude creates a structured markdown file at `~/.claude/projects/{project}/memory/CHECKPOINT.md` containing:

- Task description
- Current state
- File locations with line numbers
- Next steps
- A resume prompt that Claude can execute immediately

When you resume, Claude reads the checkpoint and continues exactly where you left off — no re-explaining needed.

## Why Use This

- **Context compaction**: Save before Claude compresses your conversation
- **Session breaks**: Pick up tomorrow where you left off today
- **Complex tasks**: Checkpoint mid-task when you need to step away

## License

MIT
