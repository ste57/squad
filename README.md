# Squad

Squad is a role system for AI agents. One role, every project.

`/squad engineer` loads the role, its principles, and your project context. The agent works within those boundaries.

## Quick Start

Tell Claude Code:

> Clone https://github.com/ste57/squad.git to ~/.squad and install the skill from ~/.squad/skill

Then:

> /squad

## Layers

Squad has three layers:

**Seed** — Rules every role inherits. Non-negotiable.

**DNA** — The identity of the role. An engineer writes code. A reviewer critiques it. DNA doesn't change per project.

**Project files** — Context, conventions, and discoveries specific to where you're working. These live in `.squad/` in your repo.

```
seed → DNA → project
```

Each layer adds specificity. No layer overrides the one above it.

## Specialists

Some work is better done separately. Investigation, review, analysis. Specialists are agents that receive a handoff and return findings without seeing the full conversation or delegating further.

## Tools

A specialist is an agent you bring in. A tool is a process you follow — a commit checklist, a logging format. No handoff or isolation, just rules applied at the right moment.
