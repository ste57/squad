# Squad

A layered agent framework for dispatching AI specialists with structured roles, project memory, and delegation.

Squad organizes AI behavior into inheritable layers so every session starts with the right context, follows the right protocols, and hands off to the right specialist when needed.

## Quick Start

```bash
# Clone and install
git clone <repo-url> ~/Projects/squad
ln -s ~/Projects/squad ~/.squad

# Link the skill (Claude Code)
ln -s ~/Projects/squad/skill ~/.claude/skills/squad

# Use it
/squad engineer
```

On first run in a project, squad offers to scaffold `.squad/` with project-specific files.

## How It Works

Squad loads context in layers. Each layer adds specificity. Earlier layers set the rules, later layers operate within them.

```
seed → DNA → specialist → project files → tools
```

| Layer | Lives in | Purpose |
|-------|----------|---------|
| **Seed** | `~/.squad/seed.md` | Universal foundation. Applies to every function, every project. Non-negotiable. |
| **DNA** | `~/.squad/engineer/dna.md` | Function-level principles. Language-agnostic, project-agnostic. |
| **Specialist** | `~/.squad/engineer/triage.md` | Focused protocol for a specific activity. Loaded on demand. |
| **Project files** | `.squad/` in your project | Style, context, intel, config. Project-specific, mutable. |
| **Tools** | `~/.squad/engineer/tools/` | Optional capabilities (logger, publisher). Enabled per project. |

## Invocation

```bash
/squad engineer              # Load seed + engineer DNA
/squad engineer/triage       # Load seed + engineer DNA + triage specialist
/squad engineer/review       # Load seed + engineer DNA + review specialist
```

## Project Files

When you run `/squad` in a project for the first time, it offers to create `.squad/` with four files:

| File | Contains |
|------|----------|
| `config.md` | Active function, enabled tools, project settings |
| `style.md` | Conventions for how work is done in this project |
| `context.md` | What this project is. Domain knowledge. Things known before starting. |
| `intel.md` | Things learned while working. Gotchas, patterns, traps. Updated by the squad. |

## Delegation

Specialists work in isolation via subagents. The flow:

1. The active agent reads the specialist's input spec
2. Spawns a subagent with seed + DNA + specialist + project files
3. The subagent works in isolation and returns structured results
4. The parent agent validates and continues

Subagents do not delegate further. Tools run inline, not as subagents.

## What's Included

### Engineer Function

| File | Role |
|------|------|
| `engineer/dna.md` | Code organization, documentation, safety principles |
| `engineer/triage.md` | Bug investigation. Dual-agent validation (skeptic + pragmatist). Bug file tracking. |
| `engineer/review.md` | Code review. Independent evaluation with structured findings. |
| `engineer/tools/logger.md` | Daily log entries, weekly summaries, release notes |
| `engineer/tools/publisher.md` | Commit grouping, style checking, PR generation |

## Extending

### Custom Specialists

Add specialists to an existing function:

```
~/.squad/engineer/custom/deploy.md
```

Invoked as `/squad engineer/deploy`. Custom directories are gitignored so upstream updates don't overwrite them.

### Custom Functions

Create an entirely new function:

```
~/.squad/custom/writer/
├── dna.md
├── editor.md
└── tools/
    └── formatter.md
```

Invoked as `/squad writer`. A directory is a function if it contains `dna.md`.

## Dev Mode

If `~/.squad/dev.md` exists, the system unlocks editing of system files. This is for the framework maintainer. `dev.md` is gitignored and contains:

- Layer placement rules and litmus tests
- Terminology standards
- Architectural invariants
- A changelog of all system file edits

Without `dev.md`, system files are read-only during operation.

## Structure

```
~/.squad/
├── seed.md                      # Universal foundation
├── engineer/
│   ├── dna.md                   # Engineering principles
│   ├── triage.md                # Investigation specialist
│   ├── review.md                # Code review specialist
│   └── tools/
│       ├── logger.md            # Daily log protocol
│       └── publisher.md         # Commit/PR protocol
├── custom/                      # User-created functions (gitignored)
├── templates/                   # Scaffolding templates
│   ├── config.md
│   ├── style.md
│   ├── context.md
│   └── intel.md
└── skill/
    └── SKILL.md                 # The /squad entry point
```
