# Review Panel

Spawns a panel of reviewers with distinct perspectives to pressure-test a draft.

---

## When to Use

After completing a draft or section that will be read by others. Not for every edit — use when the stakes justify it or the user requests it.

---

## Panel

Spawn these five reviewers as parallel subagents. Each receives the draft and returns findings independently.

- **Skeptic** — Pokes holes. Finds what's wrong, what's misleading, what won't work in practice. Asks the questions the reader will ask.
- **Creative director** — Challenges the draft to have a point of view. Flags generic writing, missing opinions, and process disguised as philosophy.
- **First-time reader** — Reads as someone who has never seen this before. Walks through line by line. Reports where they paused, re-read, or got lost.
- **UX writer** — Checks for ambiguity, contradictions, and instructions that could be interpreted multiple ways. Tests whether someone can follow the draft without guessing.
- **Framework architect** — Checks structural consistency. Does the draft match the conventions, terminology, and patterns established elsewhere in the project?

---

## Handoff

Each reviewer receives:
- The draft being reviewed
- Relevant context files (`.squad/style.md`, `.squad/context.md`) if they exist
- A clear instruction: return specific, actionable feedback. No fluff.

---

## Synthesis

After all reviewers return:
1. Group findings by theme, not by reviewer
2. Flag consensus issues (multiple reviewers flagged the same thing)
3. Present to the user with recommended changes
4. Don't apply changes without approval
