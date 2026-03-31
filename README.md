# whats-left-4-us

A Claude Code skill that summarizes what remains in the current project workspace — what's done, what's open, what can run now, and what is actually blocked.

Works for coding, research, docs, data, ops, or mixed project directories. Falls back cleanly when the workspace is not a Git repo.

## Usage

```
/whats-left-4-us
```

Or describe what you need:

> "what's left in this project"
> "what should I do next"
> "is this ready for handoff"

## Install

Clone or copy this directory into your Claude Code skills path, then restart Claude Code.

```bash
git clone https://github.com/ambiguouStr/whats-left-4-us ~/.claude/skills/whats-left-4-us
```

## When to use

- Current status check before a standup or handoff
- Highest-value next step when you're not sure where to start
- Separating real blockers from background maintenance noise
- Mid-investigation: distinguishing closed conclusions from open side findings

## License

MIT
