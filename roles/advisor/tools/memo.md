# Memo

Working document for active advisory work. Create a memo when the problem is complex enough that the thinking needs to be captured, iterated on, and shared.

---

## Protocol

1. **Create the file.** Place it in `memos/` at the project root. Name it after the topic: `memos/[topic].md`. Create the directory if it doesn't exist. Keep the name short and descriptive.
2. **Start with the frame.** Write what you understand so far: the problem, who it's for, what's at stake, open questions.
3. **Update as you work.** As you run recon, get research back, or hear from the critic, fold findings into the memo. Don't append — rewrite sections to reflect your current best thinking.
4. **Converge on a recommendation.** The memo should move from open questions toward a clear position. By the end, it reads as: here's the situation, here's what matters, here's what I'd do and why.

---

## Structure

The memo follows the advisor's three moves:

```
# [Topic]

## Situation
What's going on. Context, constraints, stakeholders.

## What Matters
Core vs noise. The essential trade-offs.

## Risks
What could go wrong. Assumptions being made.

## Recommendation
What to do and why. What you'd deprioritize.
```

---

## Keeping Memos Current

When new information surfaces — from reports, the user, or recon — route it correctly:

- **Project knowledge** (reusable facts that outlive this scenario) → Learn captures it in intel. The memo doesn't store project knowledge.
- **Scenario analysis** (what a fact means for this specific problem) → goes in the memo.
- **Both** — a discovery can be project knowledge and relevant to the scenario. Learn captures the fact, the memo captures the implication.

After routing, check active memos. If new information changes the situation, trade-offs, or risks for an active memo, update it immediately. Don't wait for the user to ask.

This is the memo's core value: it stays current. A memo that doesn't reflect the latest understanding is worse than no memo — it's misleading.

---

## Rules

- One active memo per topic. Don't create multiple files for the same problem.
- Rewrite, don't append. The memo reflects current thinking, not a changelog.
- Keep it readable by someone who wasn't in the conversation.
- Delete the memo when the work is done and the decision is made, or move it to `.squad/` if it contains intel worth preserving.
