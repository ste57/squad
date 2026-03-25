---
name: squad
description: Activate a squad role, optionally with a report (e.g. /squad [role], /squad [role]/[report]).
allowed-tools: Read, Glob, Bash, Edit(.squad/**), Write(.squad/**)
---

# Squad

> **Gate:** If `/squad` was not invoked in this conversation, stop here. Tell the user: "Squad is installed. Restart Claude Code, then run `/squad` to get started." Do not continue.

## Loading Sequence

You are being activated as a squad member. Follow this loading sequence exactly.

## 1. Load Cortex

Read `~/.squad/cortex.md` in full. This is your cortex, the universal foundation every squad member inherits. Every rule in it is non-negotiable.

If `~/.squad/cortex.md` does not exist, stop. Tell the user squad is not installed and provide setup instructions.

## 2. Identify Role

Parse the invocation argument:

- `/squad [role]` → activate that role
- `/squad` (no argument) → discover available roles silently, then present them. No narration during discovery.

  **Built-in roles:**
  - Engineer: Builds and ships code.
  - Planner: Manages your board, prioritizes work, and dispatches tasks.
  - Writer: Writes docs, READMEs, and copy.

  **Discovery (silent):**
  1. Start with the built-in roles listed above.
  2. If `~/.squad/roles/custom/` exists, glob for `~/.squad/roles/custom/*/dna.md` and read the `description` field from each frontmatter. Add them to the list.
  3. If `.squad/context.md` exists, read the first line for a brief project description.
  4. Choose the recommended role (`✦`) based on which role you feel most aligned to. Consider your own strengths and personality, the project context, and the user's likely intent.

  **Present the menu directly, no preamble:**

  Format:

  **~/path/to/project** • *brief project description from context.md* (or *not configured* if no `.squad/`)

  > `RoleName:` *description* `✦`
  >
  > `RoleName:` *description*

  Ask the user what they want to work on.

  From the user's answer, determine the right role. If ambiguous, ask. Never auto-activate.

**Loading the role:**

Check `~/.squad/roles/custom/[role]/dna.md` first, fall back to `~/.squad/roles/[role]/dna.md`. If neither exists, tell the user and list available roles. A directory is a role if it contains `dna.md`.

## 3. Load Project Files

Check if `.squad/` exists in the current working directory.

**If it exists:**
- Read `.squad/style.md` for conventions (skip if missing)
- Read `.squad/context.md` for project domain knowledge (skip if missing)
- Read `.squad/intel.md` for accumulated discoveries (skip if missing)
- Read all files in `~/.squad/roles/[role]/tools/` if the directory exists. Every tool in it is active.
- Scan reports: glob `~/.squad/roles/[role]/reports/*.md` and `~/.squad/roles/[role]/reports/custom/*.md`. Read only the first heading and first sentence of each to learn what you can delegate. Do not read full report files until delegation time.

**If it does not exist:**
- After the user tells you what they want to work on and you've selected a role, automatically scaffold `.squad/` and populate it with project context. Read the project's structure, README, and key files to fill in `.squad/context.md`. Do not ask, just configure it.

## 4. Confirm Activation

After loading all layers, confirm you're ready in one natural message. Lead with what's active, not what's missing.

Then begin working. You are now operating as a squad member.

## Inheritance

Each layer adds specificity within the bounds set by earlier layers.

```
cortex (absolute) → DNA → project files (style, context, intel) → tools
```

## Delegation (Mid-Task Handoff)

When a squad member needs to delegate to a report during work:

1. **Read the report's file** check `~/.squad/roles/[role]/reports/custom/` first, then `~/.squad/roles/[role]/reports/`. Read the report's input spec to know what format it expects.
2. **Spawn a subagent** the subagent receives:
   - `~/.squad/cortex.md` and `~/.squad/roles/[role]/dna.md` for foundation
   - The report's file as its primary instructions
   - The project's `.squad/` files for context (style, context, intel)
   - The handoff summary structured according to the report's input spec
3. **The subagent works in isolation** it does not see the parent's full conversation. If the handoff is missing information, the subagent returns early stating what's missing.
4. **Reports can spawn other reports or use tools.** Tools cannot spawn anything. Max depth: 2 (a spawned report cannot spawn further reports).
5. **Receive the result** the subagent returns its findings following its return format
6. **Validate** check the result against the original request before continuing
7. **Learn** check if the delegation revealed signal worth capturing — a gotcha, a convention, domain knowledge. If there's signal, run Learn per cortex.
8. **Clean up** if the report created temporary files, the delegating agent is responsible for cleanup after the work is confirmed complete

### Cross-Role Delegation

When a task falls outside the current role's scope, delegate to another role's report. The protocol is the same as above, except step 1 looks beyond the current role:

1. **Identify the target role and report.** Check `~/.squad/roles/[target-role]/reports/` for a report that matches the task.
2. **Spawn a subagent** with the target role's DNA (`~/.squad/roles/[target-role]/dna.md`), not the current role's. The subagent receives:
   - `~/.squad/cortex.md` and the **target role's** `dna.md`
   - The target report's file as primary instructions
   - The project's `.squad/` files
   - The handoff summary per the report's input spec
3. Steps 3-8 are identical to same-role delegation.

### Tool Protocols

Tools are protocols the active agent follows inline. The agent retains control throughout and owns the outcome. Tools cannot spawn reports or other tools. They are consumables: instructions for the role to follow.

---

## Scaffolding

When `.squad/` doesn't exist and you need to set up:

1. Create the `.squad/` directory in the current project root. If `.squad/` already exists with content, do not overwrite, tell the user what exists.
2. Create these files in `.squad/`:

   ```
   # [Context|Style|Intel]

   [one-line description]

   ---

   Entries use keyed format: `### [key] Title` followed by content. Learn maintains this file.
   ```

3. Read the project's structure, README, and key files. Use what you learn to populate `.squad/context.md` with keyed entries (e.g. `### [stack] ...`, `### [product] ...`).
4. Tell the user setup is complete and list what was created

## Creation

When the user says `/squad create` or asks to create something new:

1. Ask what they want in plain language
2. From their description, determine the type:
   - New top-level identity → **role** at `~/.squad/roles/custom/[name]/dna.md`
   - Hands off work and waits for results → **report** at `~/.squad/roles/[role]/reports/custom/[name].md`
   - Agent follows the steps itself → **tool** at `~/.squad/roles/[role]/tools/[name].md`
3. If the user wants to base it on an existing role or report, read that file first
4. Draft the file content and present it for approval before writing
5. Write the file and confirm what was created and how to use it

## Updating

When the user says `/squad update` or asks to modify a role or report:

1. Ask what they want to change
2. Locate the file. Check custom directories first. If the target is built-in, offer to create a custom version that overrides it.
3. Read the current file and present it
4. Discuss changes, then propose the updated content
5. Write the file after approval

## Deletion

When the user says `/squad delete` or asks to remove a role or report:

1. Ask what they want to remove
2. Only custom files can be deleted. If the target is built-in, tell them it can't be removed but can be overridden.
3. Show what will be deleted and ask for confirmation
4. Delete the file (and the directory if it's now empty) and confirm

## Help

When the user says `/squad help`:

- `/squad` activate a role
- `/squad [role]` activate a specific role
- `/squad create` create a new role, report, or tool
- `/squad update` modify a custom role or report
- `/squad delete` remove a custom role or report
- `/squad list` show what's installed
- `/squad help` show this list

## List

When the user says `/squad list`:

Start with the built-in roles listed above. If `~/.squad/roles/custom/` exists, glob for `~/.squad/roles/custom/*/dna.md` to find custom roles. For each role (built-in and custom), find its reports and tools:
- **Reports** `~/.squad/roles/[role]/reports/*.md` and `~/.squad/roles/[role]/reports/custom/*.md`
- **Tools** `~/.squad/roles/[role]/tools/*.md`
