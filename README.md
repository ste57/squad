<h1 align="center">squad</h1>

<p align="center">
  A role system for AI agents.<br>
  One role, every project.
</p>

<br>

---

<h2 align="center">Install</h2>

Tell Claude Code:

```
Clone https://github.com/ste57/squad.git to ~/.squad and install the skill from ~/.squad/skill
```

Then run:

```
/squad
```

---

<h2 align="center">Layers</h2>

1. `seed` — Universal rules every role inherits. Non-negotiable.
2. `DNA` — The identity of the role. An engineer writes code. A reviewer critiques it. DNA doesn't change per project.
3. `project` — Context, conventions, and discoveries specific to where you're working. Lives in `.squad/` in your repo.

Each layer adds specificity. None overrides the one above it.

---

<h2 align="center">Custom Roles</h2>

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
