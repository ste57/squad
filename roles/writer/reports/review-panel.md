# Review Panel

Review report that spawns a panel of reviewers with distinct perspectives to pressure-test a draft.

---

## Input

When you receive a handoff, expect:
- **Draft** the text being reviewed
- **Context** relevant project files (`.squad/style.md`, `.squad/context.md`) if available
- **Stakes** (optional) what this will be used for, who will read it

If the draft is missing, return early stating that a draft is needed.

---

## When to Use

After completing a draft or section that will be read by others. Not for every edit. Use when the stakes justify it or the user requests it.

---

## Panel

Spawn these five reviewer reports in parallel. Each receives the draft and returns findings independently.

- **Skeptic** pokes holes. Finds what's wrong, what's misleading, what won't work in practice. Asks the questions the reader will ask.
- **Creative director** challenges the draft to have a point of view. Flags generic writing, missing opinions, and process disguised as philosophy.
- **First-time reader** reads as someone who has never seen this before. Walks through line by line. Reports where they paused, re-read, or got lost.
- **UX writer** checks for ambiguity, contradictions, and instructions that could be interpreted multiple ways. Tests whether someone can follow the draft without guessing.
- **Framework architect** checks structural consistency. Does the draft match the conventions, terminology, and patterns established elsewhere in the project?

---

## Synthesis

After all reviewers return:
1. Group findings by theme, not by reviewer
2. Flag consensus issues (multiple reviewers flagged the same thing)
3. Present to the user with recommended changes
4. Don't apply changes without approval

---

## Return Format

When handing results back, provide:
1. **Consensus issues** things multiple reviewers flagged
2. **Individual findings** grouped by theme, with the reviewer perspective noted
3. **Recommended changes** specific edits to make, pending approval
