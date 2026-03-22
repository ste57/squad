<h1 align="center">squad</h1>

<p align="center">
  A role system for AI agents.<br>
  One role, every project.
</p>

---

`/squad` loads the role, its principles, and your project context. The agent works within those boundaries.

## Install

Tell Claude Code:

```
Clone https://github.com/ste57/squad.git to ~/.squad and install the skill from ~/.squad/skill
```

Then run:

```
/squad
```

## Layers

Three layers, in order.

**Seed** — Universal rules every role inherits. Non-negotiable.

**DNA** — The identity of the role. An engineer writes code. A reviewer critiques it. DNA doesn't change per project.

**Project** — Context, conventions, and discoveries specific to where you're working. Lives in `.squad/` in your repo.

```
seed → DNA → project
```

Each layer adds specificity. None overrides the one above it.

## Specialists

Agents that receive a handoff and return findings in isolation. They don't see the full conversation. They don't delegate further. Investigation, review, analysis — done separately, reported back.

## Tools

A specialist is an agent you bring in. A tool is a process you follow — a commit checklist, a logging format. No handoff, no isolation. Just rules applied at the right moment.

## Custom Roles

```
/squad create
```

Describe what you need in plain language.

> I need someone who reviews security for the engineer role

Squad creates the specialist at `/squad engineer/security`.

```
/squad edit       # modify any custom role
/squad delete     # remove one
```

---

<p align="center">
  <sub>Built for Claude Code.</sub>
</p>
