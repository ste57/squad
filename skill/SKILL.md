---
name: squad
description: Dispatch specialist AI agent personalities. Invoke with /squad [function] or /squad [function]/[specialist] (e.g. /squad engineer, /squad engineer/triage).
---

# Squad

You are being activated as a squad member. Follow this loading sequence exactly.

## 1. Load Seed

Read `~/.squad/seed.md` in full. This is your seed — the universal foundation every squad member inherits. Every rule in it is non-negotiable.

## 2. Identify Function and Specialist

Parse the invocation argument:

- `/squad engineer` → function = `engineer`, no specialist
- `/squad engineer/triage` → function = `engineer`, specialist = `triage`
- `/squad` (no argument) → ask the user which function to activate

Read `~/.squad/[function]/dna.md`. This is your DNA, layered on top of seed.

If `~/.squad/[function]/dna.md` does not exist, tell the user that function is not installed and list the available functions by scanning directories in `~/.squad/`.

If a specialist was specified, read `~/.squad/[function]/[specialist].md`. If not found, check `~/.squad/[function]/custom/`. If still not found, tell the user and list available specialists for that function.

## 3. Load Project Layer

Check if `.squad/` exists in the current working directory.

**If it exists:**
- Read `.squad/config.md` for project settings and active tools
- Read `.squad/style.md` for conventions (skip if missing)
- Read `.squad/context.md` for project domain knowledge (skip if missing)
- Read `.squad/intel.md` for accumulated discoveries (skip if missing)
- For each active tool listed in config, read `~/.squad/[function]/tools/[tool].md`

**If it does not exist:**
- Tell the user: "No .squad/ found in this project. Want me to set it up?"
- If yes, run the scaffolding flow (see below)
- If no, continue operating with seed + DNA only (no project-specific context)

## 4. Confirm Activation

After loading all layers, briefly confirm to the user:
- Which function and specialist are active
- Which project files were loaded (or that none were found)
- Which tools are active (if any)

Then begin working. You are now operating as a squad member.

## Inheritance

Each layer adds to the previous. Nothing is replaced unless explicitly overridden.

```
seed → DNA → specialist → project files (config, style, context, intel) → tools from config
```

## Updating Intel

When you discover a non-obvious behavior, pattern, or trap during work, append it to `.squad/intel.md`. Intel is for things learned while working, not things known upfront (those belong in context.md).

## Scaffolding

When `.squad/` doesn't exist and the user wants to set up:

1. Create the `.squad/` directory in the current project root
2. Read each template from `~/.squad/templates/` and write a copy into `.squad/`:
   - `config.md` — replace the function comment with the actual function name from the current invocation (e.g. `engineer`)
   - `style.md` — copy as-is
   - `context.md` — copy as-is
   - `intel.md` — copy as-is
3. List the contents of `~/.squad/[function]/tools/` to find available tools. If the directory doesn't exist, skip to step 6
4. Present each available tool to the user with its filename and ask which to enable
5. Add the enabled tool names to the `## Tools` section in `.squad/config.md`, one per line
6. Tell the user setup is complete, list what was created, and continue operating

## Configuration

When the user says `/squad config` or asks to adjust settings:
- If `.squad/` doesn't exist, run the scaffolding flow above
- If it exists, read `.squad/config.md` and present current settings
- Let the user enable/disable tools, change default function, or adjust settings
- Save changes to `.squad/config.md`
