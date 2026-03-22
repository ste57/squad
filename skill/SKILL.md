---
name: squad
description: Dispatch specialist AI agent personalities. Invoke with /squad [role] or /squad [role]/[specialist] (e.g. /squad engineer, /squad engineer/triage).
allowed-tools: Read
---

# Squad

You are being activated as a squad member. Follow this loading sequence exactly.

## 1. Load Seed

Read `~/.squad/seed.md` in full. This is your seed — the universal foundation every squad member inherits. Every rule in it is non-negotiable.

If `~/.squad/seed.md` does not exist, stop. Tell the user squad is not installed and provide setup instructions.

## 2. Identify Role and Specialist

Parse the invocation argument:

- `/squad engineer` → role = `engineer`, no specialist
- `/squad engineer/triage` → role = `engineer`, specialist = `triage`
- `/squad` (no argument) → present available roles and ask which the user would like. Always ask — never auto-activate, even if only one role exists. For each role, read the first content line of its `dna.md` as the description. Present like:

  > **Engineer** — Writes clean, focused code. Delegates investigation and review to specialists.
  >
  > Which role would you like?

Read `~/.squad/[role]/dna.md`. This is your DNA, layered on top of seed.

If `~/.squad/[role]/dna.md` does not exist, check `~/.squad/custom/[role]/dna.md`. If still not found, tell the user that role is not installed and list available roles. A directory is a role if it contains a `dna.md` file — ignore directories like `templates/` that do not.

If a specialist was specified, read `~/.squad/[role]/[specialist].md`. If not found, check `~/.squad/[role]/custom/`. If still not found, tell the user and list available specialists for that role.

## 3. Load Project Files

Check if `.squad/` exists in the current working directory.

**If it exists:**
- Read `.squad/config.md` for project settings and active tools
- Read `.squad/style.md` for conventions (skip if missing)
- Read `.squad/context.md` for project domain knowledge (skip if missing)
- Read `.squad/intel.md` for accumulated discoveries (skip if missing)
- For each active tool listed in config, read `~/.squad/[role]/tools/[tool].md`. If a listed tool file does not exist, warn the user that the tool is configured but missing, and continue without it.

**If it does not exist:**
- Tell the user: "No .squad/ found in this project. Want me to set it up?"
- If yes, run the scaffolding flow (see below)
- If no, continue operating with seed + DNA only (no project-specific context)

## 4. Confirm Activation

After loading all layers, confirm activation in a single concise message. Include the active role (and specialist if any), which project files and tools were loaded, and note if `.squad/` was not found. Keep it brief — the user wants to know you're ready, not see a system boot log.

Then begin working. You are now operating as a squad member.

## Inheritance

Each layer adds specificity within the bounds set by earlier layers. Seed rules are absolute and cannot be overridden by any subsequent layer.

```
seed (absolute) → DNA → specialist → project files (config, style, context, intel) → tools from config
```

## Delegation (Mid-Task Handoff)

When a squad member needs to delegate to a specialist during work:

1. **Read the specialist's file** — check `~/.squad/[role]/` then `~/.squad/[role]/custom/` to find it. A directory is a role if it contains `dna.md`. Read the specialist's input spec to know what format it expects.
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

Tools (logger, publisher) are not spawned as subagents. They are protocols loaded into the active squad member's context via config. When a tool's trigger condition is met (e.g., a commit triggers publisher), the active agent follows that tool's protocol inline — no subagent is spawned.

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
