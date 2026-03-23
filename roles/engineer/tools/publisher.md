# Publisher

Commit and pull request protocol. When enabled, follow this protocol whenever committing or creating a PR.

---

## Input

When committing, gather:
- **Changed files** which files to consider for staging
- **Intent** what the changes accomplish
- **Scope** if the user specified "commit changes related to X", only X is in scope

If intent is missing, ask before proceeding.

---

## Commit Protocol

1. **Read `.squad/style.md`** for project-specific conventions (branch names, PR tags, changelog format). If not defined, ask the user for preferences.
2. **Lightweight style check** scan the diff for obvious violations against `.squad/style.md`. This is a quick pre-commit gate, not a full review. Flag violations but don't block.
3. **Group by theme** look for common themes in pending changes and propose separate themed commits. If 4+ files touch different concerns, split into separate commits. If not defined in style.md, use judgment and explain your grouping.
4. **Only stage what was asked for** never bundle in unrelated uncommitted work. If in doubt, ask.
5. **Respect ignore rules** if version control says a file is ignored, do not force-add it. Never use `-f` to override.
6. **If summary tool is active** follow the summary protocol for log entries. Do not generate log entries outside the summary protocol.
7. **Trigger Learn** committing is a task completion milestone. Hand off to Learn with what was attempted, what happened, and anything surprising.
8. **Propose the commit(s)** present grouped commits with messages for approval.

### Commit Message Format

- Capital letter to start
- No dashes in commit messages (rephrase with "since", "because", etc.)
- No co-author tags

**Description style:** Symptom. Cause. Fix (only if non-obvious from the title).
- Lead with what was wrong or what changed
- Name the root cause
- Keep concrete specifics (measurements, values, repro steps)
- Length matches difficulty. Simple fix = one sentence. Hard bug = two or three.
- Capture full scope, not just what landed. Discarded approaches and experiments count.

---

## PR Protocol

1. **Read `.squad/style.md` and `.squad/context.md`** for PR conventions (title format, description template). If not defined, ask the user.
2. **Review ALL commits on the branch** not just the latest. Understand the full scope of changes across the entire branch.
3. **Read the project log** (if summary tool is active) for context on what each commit solved
4. **Propose a title and description** return to the engineer for approval.

---

## Return Format

### For commits:
1. **Proposed commit(s)** grouped by theme, each with a message and the files it includes
2. **Style flags** anything in the diff that doesn't match `.squad/style.md` (non-blocking)

### For PRs:
1. **Proposed title** following `.squad/style.md` conventions
2. **Proposed description** covering the full scope of changes on the branch
