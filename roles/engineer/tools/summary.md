# Summary

Generates summaries and release notes from git history.

---

## Weekly Summary Protocol

When the user asks for a weekly summary:

1. Run `git log` for the requested period. If the period is unclear, ask.
2. Group by theme, not by commit. Many commits on the same feature = one story.
3. Name outcomes, not tasks.
4. When something was hard, say why briefly.
5. Separate shipped from in-progress.
6. Standalone fixes get their own line.

Check `.squad/style.md` for voice, tone, and format conventions.

---

## Release Notes Protocol

When the user asks for release notes:

1. Gather changes since the last release. Check `.squad/style.md` for the method (e.g. changelog diff, git log, release tags).
2. One line per meaningful change.
3. Concise, factual. Written for users who want to know what changed.

Check `.squad/style.md` for format, tone, and channel conventions.

---

## Return Format

1. **Proposed draft** ready to post
2. **Source** what period/commits/entries were included
