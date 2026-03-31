---
name: whats-left-4-us
description: Summarize what remains in the current project workspace, identify the highest-value next steps, and surface only blocking maintenance items. Use when the user asks what's left, what remains, current status, handoff readiness, or what to do next in a coding, research, docs, data, ops, or mixed project workspace.
---

# What's Left

## Priority

- Evidence-first: use live workspace signals, not assumptions about structure.
- Execution-first: runnable experiments, tests, and validations outrank doc hygiene.
- Maintenance second: surface document or index issues only if they block execution, review, or handoff.
- If signals conflict, name the stronger source.

---

## Workspace signals

### Repo type
!`[ -d .git ] && echo "git repo" || echo "not a git repo"`

### Git status
!`git status --short 2>/dev/null`

### Recent commits
!`git log --oneline -15 2>/dev/null`

### Staged and unstaged changes
!`git diff --name-only HEAD 2>/dev/null; git ls-files --others --exclude-standard 2>/dev/null`

### Workspace structure
!`python3 - <<'PY'
from pathlib import Path
root = Path(".")
for p in sorted(root.iterdir()):
    if p.name.startswith('.'):
        continue
    print(p.name + ("/" if p.is_dir() else ""))
    if p.is_dir():
        for sub in sorted(p.iterdir()):
            if sub.name.startswith('.'):
                continue
            print("  " + sub.name + ("/" if sub.is_dir() else ""))
PY`

### Orientation docs
!`python3 - <<'PY'
from pathlib import Path
names = {'AGENTS.md','README.md','README.rst','README.txt','OVERVIEW.md','STATUS.md','HANDOFF.md'}
root = Path(".")
found = []
for p in root.rglob("*"):
    if any(part.startswith('.') for part in p.relative_to(root).parts):
        continue
    if p.name in names and len(p.relative_to(root).parts) <= 2:
        found.append(p)
for f in sorted(found, key=lambda p: len(p.parts)):
    print(f"=== {f} ===")
    print(f.read_text(errors="ignore"))
    print()
PY`

### Backlog and open items
!`python3 - <<'PY'
from pathlib import Path
names = {'TODO','TODO.md','BACKLOG.md','BACKLOG','ROADMAP.md'}
root = Path(".")
for p in root.rglob("*"):
    if any(part.startswith('.') for part in p.relative_to(root).parts):
        continue
    if p.name in names and p.is_file():
        print(f"=== {p} ===")
        print(p.read_text(errors="ignore"))
        print()
PY`

### Recently modified docs (git)
!`git diff --name-only HEAD 2>/dev/null | grep -i '\.\(md\|rst\|txt\)$'; git ls-files --others --exclude-standard 2>/dev/null | grep -i '\.\(md\|rst\|txt\)$'`

### TODO / FIXME markers in code
!`git grep -n "TODO\|FIXME\|HACK\|XXX" 2>/dev/null || grep -rn --binary-files=without-match "TODO\|FIXME\|HACK\|XXX" . 2>/dev/null | grep -v "^\./\."`

---

Based on the above workspace signals, produce a concise structured status. Respond in the user's language unless the workspace instructions require otherwise.

### What's done / new conclusions
- Only list material changes or closed conclusions.
- If there is no fresh movement, say so plainly.

### What remains
- Group by workstream when needed: code, validation, docs, ops, research, release, data.
- Each line should say what is still open and why it matters.

### Actionable now
- Only include actions that can be executed now.
- For each item, state:
  - what to do
  - what it validates or unlocks
  - the main files, scripts, or artifacts involved

### Waiting on external conditions
- Only list real blockers: missing data, missing access, future market window, upstream dependency, pending user decision.

### Maintenance (blocking only)
- Only include maintenance that affects execution, reviewability, handoff quality, or traceability.
- If nothing is blocking, say so plainly.

End with one short sentence naming the highest-value next move.
