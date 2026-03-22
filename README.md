# Squad

Your AI agent starts every session from scratch. It forgets your conventions, rediscovers your codebase, and makes the same mistakes. Squad gives it persistent context, structured roles, and specialists that work independently and report back.

## Quick Start

```bash
git clone <repo-url> ~/Projects/squad
ln -s ~/Projects/squad ~/.squad
ln -s ~/Projects/squad/skill ~/.claude/skills/squad
```

```bash
/squad engineer              # seed + engineer DNA
/squad engineer/triage       # + triage specialist
/squad engineer/review       # + review specialist
```

On first run in a project, squad scaffolds `.squad/` with project-specific files (`config.md`, `style.md`, `context.md`, `intel.md`).

## What It Looks Like

You hit a bug. You run `/squad engineer/triage`. Squad loads your universal rules, engineering principles, and the triage protocol. The specialist creates a bug file tracking the investigation, spawns two agents (a skeptic and a pragmatist) to validate its hypothesis, and hands back a root cause with confidence level and a proposed fix. You apply the fix. Learnings get saved to `.squad/intel.md` so the next session already knows.

## How It Works

Context loads in layers, each adding specificity: **seed** (universal rules) → **DNA** (function principles) → **specialist** (focused protocol) → **project files** (local context) → **tools** (optional capabilities). A directory is a function if it contains `dna.md`.

Specialists run as isolated subagents. The parent agent builds the context stack, spawns the subagent, and validates its output. Subagents do not delegate further.

## Extending

Add a specialist to an existing function at `~/.squad/engineer/custom/deploy.md` and invoke it as `/squad engineer/deploy`. Create a new function by adding a directory with a `dna.md` to `~/.squad/custom/`. Custom paths are gitignored.

A minimal custom function:

**`~/.squad/custom/writer/dna.md`**
```markdown
# Writer DNA

- Write for clarity. Every sentence earns its place.
- Match the voice defined in `.squad/style.md`.
- Never publish without explicit user approval.
```

**`~/.squad/custom/writer/editor.md`**
```markdown
# Editor

Revision specialist. Reviews a draft and returns structured feedback.

## Input
- **Draft** — the text to review
- **Audience** — who will read it

## Protocol
1. Read the full draft before noting anything.
2. Flag unclear passages with a specific rewrite suggestion.
3. Check for consistency with `.squad/style.md`.

## Return Format
1. **Verdict** — ready / needs revision
2. **Issues** — list with location, problem, and suggested fix
```

Invoke with `/squad writer/editor`.
