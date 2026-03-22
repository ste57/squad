---
name: squad
description: Activate a squad role, optionally with a specialist (e.g. /squad engineer, /squad engineer/triage).
allowed-tools: Read, Glob, Bash
---

# Squad

## First-Time Install

If you are reading this because you just installed squad (not because `/squad` was invoked), tell the user to restart Claude Code and run `/squad` to activate their first role. Do not proceed with the loading sequence below.

## Loading Sequence

You are being activated as a squad member. Follow this loading sequence exactly.

## 1. Load Seed

Read `~/.squad/seed.md` in full. This is your seed — the universal foundation every squad member inherits. Every rule in it is non-negotiable.

If `~/.squad/seed.md` does not exist, stop. Tell the user squad is not installed and provide setup instructions.

## 2. Identify Role and Specialist

Parse the invocation argument:

- `/squad engineer` → role = `engineer`, no specialist
- `/squad engineer/triage` → role = `engineer`, specialist = `triage`
- `/squad` (no argument) → discover available roles silently, then present them. No narration during discovery.

  **Discovery (silent):**
  1. Glob for `~/.squad/*/dna.md` and `~/.squad/custom/*/dna.md`
  2. Read `.squad/config.md` in the current project (if it exists) to infer which role best fits. Only read files inside `.squad/`.
  3. For each role, read the `description` field from its `dna.md` frontmatter.
  4. If `.squad/context.md` exists, read the first line for a brief project description.

  **Present the menu directly — no preamble:**

  Format:

  **~/path/to/project** • *brief project description from context.md* (or *not configured* if no `.squad/`)

  > `RoleName:` *description* `✦`
  >
  > `RoleName:` *description*

  Ask the user what they want to work on.

  From the user's answer, determine the right role. If ambiguous, ask. Never auto-activate.

**Loading the role:**

Check `~/.squad/custom/[role]/dna.md` first, fall back to `~/.squad/[role]/dna.md`. If neither exists, tell the user and list available roles. A directory is a role if it contains `dna.md`.

If `~/.squad/[role]/traits.md` exists, read it. Traits are personal preferences for this role — they layer on top of DNA and carry across projects.

If a specialist was specified, check `~/.squad/[role]/custom/[specialist].md` first, fall back to `~/.squad/[role]/[specialist].md`. If neither exists, tell the user and list available specialists.

## 3. Load Project Files

Check if `.squad/` exists in the current working directory.

**If it exists:**
- Read `.squad/config.md` for project settings and active tools
- Read `.squad/style.md` for conventions (skip if missing)
- Read `.squad/context.md` for project domain knowledge (skip if missing)
- Read `.squad/intel.md` for accumulated discoveries (skip if missing)
- For each active tool listed in config, read `~/.squad/[role]/tools/[tool].md`. If a listed tool file does not exist, warn the user and continue without it.

**If it does not exist:**
- After the user tells you what they want to work on and you've selected a role, automatically scaffold `.squad/` and populate it with project context. Read the project's structure, README, and key files to fill in `.squad/context.md`. Do not ask — just configure it.

## 4. Confirm Activation

After loading all layers, confirm you're ready in one natural message. Lead with what's active, not what's missing.

Then begin working. You are now operating as a squad member.

## Inheritance

Each layer adds specificity within the bounds set by earlier layers.

```
seed (absolute) → DNA → specialist → project files (config, style, context, intel) → tools from config
```

## Delegation (Mid-Task Handoff)

When a squad member needs to delegate to a specialist during work:

1. **Read the specialist's file** — check `~/.squad/[role]/custom/` first, then `~/.squad/[role]/`. Read the specialist's input spec to know what format it expects.
2. **Spawn a subagent** — the subagent receives:
   - `~/.squad/seed.md` and `~/.squad/[role]/dna.md` for foundation
   - The specialist's file as its primary instructions
   - The project's `.squad/` files for context (style, context, intel)
   - The handoff summary structured according to the specialist's input spec
3. **The subagent works in isolation** — it does not see the parent's full conversation. If the handoff is missing information, the subagent returns early stating what's missing.
4. **Subagents do not delegate further.** They report back to the parent agent, which decides the next action.
5. **Receive the result** — the subagent returns its findings following its return format
6. **Validate** — check the result against the original request before continuing
7. **Clean up** — if the specialist created temporary files, the delegating agent is responsible for cleanup after the work is confirmed complete

### Tool Protocols

Tools are protocols the active agent follows inline. The agent retains control throughout and owns the outcome. A tool may direct the agent to spawn helper agents (e.g., a review panel), but the agent orchestrates them. This differs from specialist delegation, where the agent hands off control entirely.

---

## Updating Intel

When you discover a non-obvious behavior, pattern, or trap during work, append it to `.squad/intel.md`. Intel is for things learned while working, not things known upfront (those belong in context.md).

## Scaffolding

When `.squad/` doesn't exist and you need to set up:

1. Create the `.squad/` directory in the current project root. If `.squad/` already exists with content, do not overwrite — tell the user and suggest `/squad config` instead.
2. Read each template from `~/.squad/templates/` and write a copy into `.squad/`:
   - `config.md` — replace the role comment with the actual role name
   - `style.md` — copy as-is
   - `context.md` — copy as-is
   - `intel.md` — copy as-is
3. Read the project's structure, README, and key files. Use what you learn to populate `.squad/context.md` with a brief description of the project, its tech stack, and any relevant domain context.
4. List the contents of `~/.squad/[role]/tools/` to find available tools. If the directory doesn't exist, skip to step 7.
5. Read each tool file. Enable tools that are relevant to the project based on what you learned in step 3.
6. Add the enabled tool names to the `## Tools` section in `.squad/config.md`, one per line
7. Tell the user setup is complete and list what was created
8. Load the enabled tools and continue operating

## Configuration

When the user says `/squad config` or asks to adjust settings:
- If `.squad/` doesn't exist, run the scaffolding flow above
- If it exists, read `.squad/config.md` and present current settings
- Let the user enable/disable tools, change default role, or adjust settings
- Save changes to `.squad/config.md`

## Creation

When the user says `/squad create` or asks to create something new:

1. Ask what they want in plain language — don't ask them to choose between role, specialist, or tool
2. From their description, determine the type:
   - New top-level identity (e.g. "a writer") → **role** at `~/.squad/custom/[name]/dna.md`
   - Hands off work and waits for results (e.g. "something that reviews security") → **specialist** at `~/.squad/[role]/custom/[name].md`
   - Agent follows the steps itself (e.g. "a checklist before I push") → **tool** at `~/.squad/[role]/tools/[name].md`
3. If the user wants to base it on an existing role or specialist, read that file first
4. Draft the file content and present it for approval before writing
5. Write the file and confirm what was created and how to use it

## Updating

When the user says `/squad update` or asks to modify a role or specialist:

1. Ask what they want to change
2. Locate the file — check `~/.squad/custom/` and `~/.squad/[role]/custom/` first. If the target is built-in, offer to create a custom version that overrides it.
3. Read the current file and present it
4. Discuss changes, then propose the updated content
5. Write the file after approval

## Deletion

When the user says `/squad delete` or asks to remove a role or specialist:

1. Ask what they want to remove
2. Locate the file — only custom files can be deleted. If the target is built-in, tell them it can't be removed but can be overridden.
3. Show what will be deleted and ask for confirmation
4. Delete the file (and the directory if it's now empty) and confirm

## Help

When the user says `/squad help`:

- `/squad` — activate a role
- `/squad [role]` — activate a specific role
- `/squad [role]/[specialist]` — activate with a specialist
- `/squad create` — create a new role or specialist
- `/squad update` — modify a custom role or specialist
- `/squad delete` — remove a custom role or specialist
- `/squad config` — adjust project settings
- `/squad list` — show what's installed
- `/squad help` — show this list

## List

When the user says `/squad list`:

Glob for available roles at `~/.squad/*/dna.md` and `~/.squad/custom/*/dna.md`. For each role, find its specialists and tools:
- **Specialists** — `~/.squad/[role]/*.md` and `~/.squad/[role]/custom/*.md` (exclude `dna.md`)
- **Tools** — `~/.squad/[role]/tools/*.md`
