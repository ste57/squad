# Publisher

Commit and pull request protocol. When enabled, follow this protocol whenever committing or creating a PR.

---

## Input

When committing, gather:
- **Changed files** — which files to consider for staging
- **Intent** — what the changes accomplish
- **Scope** — if the user specified "commit changes related to X", only X is in scope

If intent is missing, ask before proceeding.

---

## Commit Protocol

1. **Read `.squad/style.md`** for commit conventions (message format, grouping rules, any project-specific requirements). If not defined, ask the user for preferences.
2. **Lightweight style check** — scan the diff for obvious violations against `.squad/style.md`. This is a quick pre-commit gate, not a full review. Flag violations but don't block.
3. **Group by theme** — look for common themes in pending changes and propose separate themed commits. Grouping thresholds are defined in `.squad/style.md`. If not defined, use judgment and explain your grouping.
4. **Only stage what was asked for** — never bundle in unrelated uncommitted work. If in doubt, ask.
5. **Respect ignore rules** — if version control says a file is ignored, do not force-add it.
6. **If logger is active** — follow the logger protocol to propose log entries for each commit. Do not generate log entries outside the logger protocol.
7. **Check for intel/style updates** — before proposing, ask: did this work reveal unexpected behavior, an undocumented pattern, or a convention not yet in `.squad/style.md`? If yes, include the proposed addition to `intel.md` or `style.md` alongside the commit proposal. If nothing, move on.
8. **Propose the commit(s)** — present grouped commits with messages for approval. Log entries are proposed alongside commits but only finalized after approval.

---

## PR Protocol

1. **Read `.squad/style.md` and `.squad/context.md`** for PR conventions (title format, description template). If not defined, ask the user.
2. **Review ALL commits on the branch** — not just the latest. Understand the full scope of changes across the entire branch.
3. **Read the project log** (if logger tool is active) for context on what each commit solved
4. **Propose a title and description** — return to the engineer for approval.

---

## Return Format

### For commits:
1. **Proposed commit(s)** — grouped by theme, each with a message and the files it includes
2. **Style flags** — anything in the diff that doesn't match `.squad/style.md` (non-blocking)

### For PRs:
1. **Proposed title** — following `.squad/style.md` conventions
2. **Proposed description** — covering the full scope of changes on the branch
