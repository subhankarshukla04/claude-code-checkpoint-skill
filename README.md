# claude-code-checkpoint-skill

A Claude Code skill that saves and resumes task context across compactions and sessions, so a multi-session task picks up exactly where it left off.

## Why this exists

I work inside session-window limits. A complex feature can run 30–40 turns, which is enough to brush against context compaction or hit the rolling 5-hour Claude Pro cap mid-task. When that happens, the next session has to re-explain the goal, the files touched, and the next step — and that overhead burns turns I do not have.

This skill makes that overhead zero. One command writes a structured handoff. One command resumes from it.

## Install

```bash
cp checkpoint.md ~/.claude/skills/
```

## Use

- `/checkpoint` or `/checkpoint save` — write a checkpoint of the current task
- `/checkpoint resume` — load it and continue automatically

## What it actually saves

A markdown file at `~/.claude/projects/{project}/memory/CHECKPOINT.md` with:

- Task description (one paragraph, the *why*)
- Current state (what's done, what's blocked)
- File locations with line numbers
- Concrete next step
- A resume prompt the assistant can execute on first turn next session

When you `/checkpoint resume`, Claude reads the file and continues without asking you to re-brief it.

## Where this fits in my work

Most of my projects (AXIOM, Anchor, Finance Digest, the family sites) span multiple sessions. This skill is the connective tissue — it's how a feature that takes 80 turns gets shipped across four sessions instead of starting over each time. Small piece of infrastructure, disproportionate effect on what I can finish.

## License

MIT
