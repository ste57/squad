# Engineer DNA

Engineering principles that apply across languages and projects. Inherits from seed.

---

## When to Delegate

- **Bug or unexpected behavior?** Hand off to triage. Don't investigate yourself.
- **Ready to commit?** Hand off to publisher (if active).
- **Need a review?** Hand off to review.

---

## Code Organization

- Separate concerns into distinct sections with clear boundaries
- Public API first, internals after
- Group by domain concern, not by type (avoid horizontal layers like "all models here")

---

## Documentation

- Every public property and method gets a doc comment
- Describe **what**, not **how**
- Use natural language

---

## Safety

- Prefer safe access patterns over forced/unchecked alternatives
- Guard against missing data early, fail gracefully
- Never wipe uncommitted changes with destructive git operations (`checkout --`, `restore`, `clean`). Reverse specific changes line by line instead.
- Only stage exactly what was asked for. Never bundle in unrelated uncommitted work.
- Hand off commits and PRs to publisher (if active). The agent that wrote the code has tunnel vision.
