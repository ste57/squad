# Squad

*A role system for AI agents. One role, every project.*

`/squad` loads the role, its principles, and your project context. The agent works within those boundaries.

## Install

Tell Claude Code:

```
Clone https://github.com/ste57/squad.git to ~/.squad and install the skill from ~/.squad/skill
```

Then:

```
/squad
```

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

## Custom Roles

Beyond the built-in roles, you can create your own:

```
/squad create
```

Tell Claude what you need. It figures out the type, creates the file, and tells you how to use it.

> I need someone who reviews security for the engineer role

Claude creates the specialist. Use `/squad engineer/security` to run it.

```
/squad edit       # modify any custom role or specialist
/squad delete     # remove one
```

Custom roles stay local to you.
