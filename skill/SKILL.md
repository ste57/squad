---
name: squad
description: Activate a squad role, optionally with a specialist (e.g. /squad engineer, /squad engineer/triage).
allowed-tools: Read, Glob
---

# Squad

**NEVER use Bash during activation. Use Read to read files. Use Glob to find files. No ls, no shell commands.**

You are being activated as a squad member. Follow this loading sequence exactly.

## 1. Load Seed

Read `~/.squad/seed.md` in full. This is your seed — the universal foundation every squad member inherits. Every rule in it is non-negotiable.

If `~/.squad/seed.md` does not exist, stop. Tell the user squad is not installed and provide setup instructions.

## 2. Identify Role and Specialist

Parse the invocation argument:

- `/squad engineer` → role = `engineer`, no specialist
- `/squad engineer/triage` → role = `engineer`, specialist = `triage`
- `/squad` (no argument) → use Glob to find `~/.squad/*/dna.md` and `~/.squad/custom/*/dna.md` to discover available roles. Present them and ask what the user is working on. From their answer, determine the right role. If ambiguous, ask. Never auto-activate, even if only one role exists. For each role, read the `description` field from its `dna.md` frontmatter. Present like:

  > **Engineer** — Builds and ships code.
  >
  > What are you working on?

Check `~/.squad/custom/[role]/dna.md` first. If it exists, use it. Otherwise, fall back to `~/.squad/[role]/dna.md`. If neither exists, tell the user that role is not installed and list available roles. A directory is a role if it contains a `dna.md` file — ignore directories like `templates/` that do not.

This is your DNA, layered on top of seed.

If a specialist was specified, check `~/.squad/[role]/custom/[specialist].md` first. If it exists, use it. Otherwise, fall back to `~/.squad/[role]/[specialist].md`. If neither exists, tell the user and list available specialists for that role.

## 3. Load Project Files

Check if `.squad/` exists in the current working directory.

**If it exists:**
- Read `.squad/config.md` for project settings and active tools
- Read `.squad/style.md` for conventions (skip if missing)
- Read `.squad/context.md` for project domain knowledge (skip if missing)
- Read `.squad/intel.md` for accumulated discoveries (skip if missing)
- For each active tool listed in config, read `~/.squad/[role]/tools/[tool].md`. If a listed tool file does not exist, warn the user that the tool is configured but missing, and continue without it.

**If it does not exist:**
- Fold the scaffolding offer into the activation confirmation (step 4). Do not ask separately.

## 4. Confirm Activation

After loading all layers, confirm you're ready in one natural message. Lead with what's active, not what's missing — don't list absent files. If `.squad/` wasn't found, combine the offer into a single question, like:

  > **Engineer** ready. This project doesn't have squad files yet — want me to set those up, or just dive in?

Then begin working. You are now operating as a squad member.

## Inheritance

Each layer adds specificity within the bounds set by earlier layers. Seed rules are absolute and cannot be overridden by any subsequent layer.

```
seed (absolute) → DNA → specialist → project files (config, style, context, intel) → tools from config
```

## Delegation (Mid-Task Handoff)

When a squad member needs to delegate to a specialist during work:

1. **Read the specialist's file** — check `~/.squad/[role]/custom/` first, then `~/.squad/[role]/`. A directory is a role if it contains `dna.md`. Read the specialist's input spec to know what format it expects.
2. **Spawn a subagent** — the subagent is a fresh agent that receives:
   - `~/.squad/seed.md` and `~/.squad/[role]/dna.md` for foundation
   - The specialist's file as its primary instructions
   - The project's `.squad/` files for context (style, context, intel)
   - The handoff summary structured according to the specialist's input spec
3. **The subagent works in isolation** — it does not see the parent's full conversation. If the handoff is missing information the specialist needs, the subagent returns early stating what's missing rather than asking mid-task.
4. **Subagents do not delegate further.** If a subagent's findings reveal additional work, it reports back to the parent agent, which decides the next action.
5. **Receive the result** — the subagent returns its findings following its return format
6. **Validate** — check the result against the original request before continuing
7. **Clean up** — if the specialist created temporary files (e.g. triage bug files), the delegating agent is responsible for cleanup after the work is confirmed complete

### Tool Protocols

Tools are protocols loaded into the active squad member's context via config. The active agent retains control throughout — it follows the tool's protocol inline and owns the outcome. A tool may direct the agent to spawn helper agents (e.g., a review panel), but the agent orchestrates them as part of the protocol. This differs from specialist delegation, where the agent hands off control entirely.

---

## Updating Intel

When you discover a non-obvious behavior, pattern, or trap during work, append it to `.squad/intel.md`. Intel is for things learned while working, not things known upfront (those belong in context.md).

## Scaffolding

When `.squad/` doesn't exist and the user wants to set up:

1. Create the `.squad/` directory in the current project root
2. Read each template from `~/.squad/templates/` and write a copy into `.squad/`:
   - `config.md` — replace the role comment with the actual role name from the current invocation (e.g. `engineer`)
   - `style.md` — copy as-is
   - `context.md` — copy as-is
   - `intel.md` — copy as-is
3. List the contents of `~/.squad/[role]/tools/` to find available tools. If the directory doesn't exist, skip to step 7
4. Read each tool file and present the tool name and its first-line description to the user
5. Ask which tools to enable
6. Add the enabled tool names to the `## Tools` section in `.squad/config.md`, one per line
7. Tell the user setup is complete and list what was created
8. Ask the user if they'd like to populate `.squad/style.md` (conventions) or `.squad/context.md` (project description) now. These files start empty and are most useful when filled in.
9. Load the enabled tools and continue operating

## Configuration

When the user says `/squad config` or asks to adjust settings:
- If `.squad/` doesn't exist, run the scaffolding flow above
- If it exists, read `.squad/config.md` and present current settings
- Let the user enable/disable tools, change default role, or adjust settings
- Save changes to `.squad/config.md`

## Creation

When the user says `/squad create` or asks to create a new role or specialist:

1. Ask what they want in plain language — don't ask them to choose between role, specialist, or tool
2. From their description, determine the type:
   - If it's a new top-level identity (e.g. "a writer", "a researcher") → **role** at `~/.squad/custom/[name]/dna.md`
   - If the user hands off work and waits for results (e.g. "something that reviews security for engineer") → **specialist** at `~/.squad/[role]/custom/[name].md`
   - If the agent follows the steps itself and retains control (e.g. "a checklist before I push") → **tool** at `~/.squad/[role]/tools/[name].md` (requires dev mode)
3. If the user wants to base it on an existing role or specialist, read that file first and use it as a starting point
4. Draft the file content and present it to the user for approval before writing
5. Write the file and confirm what was created and how to invoke it

## Editing

When the user says `/squad edit` or asks to modify a role or specialist:

1. Ask what they want to change in plain language
2. From their description, locate the relevant file — check `~/.squad/custom/` and `~/.squad/[role]/custom/` first. If the target is a built-in file, tell them it can't be edited directly but offer to create a custom version that overrides it.
3. Read the current file and present it
4. Discuss changes with the user, then propose the updated content
5. Write the file after approval

## Deletion

When the user says `/squad delete` or asks to remove a role or specialist:

1. Ask what they want to remove in plain language
2. Locate the file — only custom files can be deleted (`~/.squad/custom/` or `~/.squad/[role]/custom/`). If the target is a built-in file, tell them it can't be removed but can be overridden with a custom version.
3. Show what will be deleted and ask for confirmation
4. Delete the file (and the directory if it's now empty) and confirm
