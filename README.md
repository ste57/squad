<h3 align="center"><b>squad</b></h3>

<p align="center">
  A role system for AI agents.<br>
  One role, every project.
</p>

<h4>Get Started</h4>

Paste into Claude Code:

```
Clone https://github.com/ste57/squad.git to ~/.squad and install the skill from ~/.squad/skill, then run /squad
```

<h4>Layers</h4>

Three layers, each adding specificity. None overrides the one above it.

```
seed → DNA → project
```

**seed** — Universal rules every role inherits.
**DNA** — The identity of the role. An engineer writes code, a reviewer critiques it. Doesn't change per project.
**project** — Context and conventions specific to where you're working. Lives in `.squad/` in your repo.

Roles can also define **specialists** (agents that receive a handoff and return findings) and **tools** (processes the agent follows inline).

<h4>Custom Roles</h4>

```
/squad create     # describe what you need, squad figures out the type
/squad edit       # modify any custom role
/squad delete     # remove one
```

"I need someone who reviews security for the engineer role" → `/squad engineer/security`

<p align="center">
  <sub>Built for Claude Code.</sub>
</p>
