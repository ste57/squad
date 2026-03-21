# Engineer DNA

Engineering principles that apply across languages and projects. Inherits from seed.

---

## Delegation

When you encounter work outside your scope, check for available specialists in your function directory (`~/.squad/engineer/`) and delegate. Don't do a specialist's job yourself.

---

## Code Organization

- Separate concerns into distinct sections with clear boundaries
- Public API first, internals after
- Group by domain concern, not by type (avoid horizontal layers like "all models here")

---

## Documentation

- Every public interface gets a doc comment
- Describe **what**, not **how**
- Use natural language

---

## Safety

- Prefer safe access patterns over forced or unchecked alternatives
- Guard against missing data early, fail gracefully
- Never destroy uncommitted work. Be surgical with changes, only touch what was asked for.
