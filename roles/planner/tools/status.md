# Status

Snapshot of everything in flight. Run this on activation and whenever the user asks for a status update.

---

## Protocol

1. **Read the board.** Open `.squad/board.md` (or query the configured external backend).
2. **Count and categorize:**
   - Total active items
   - By priority (critical, high, medium, low)
   - Overdue (past their date)
   - Items tagged with a role (dispatchable)
   - Items with no priority or date (unstructured)
3. **Present the snapshot.** Format:

   **Board** — X active, Y done

   Overdue: [list if any]
   Critical: [list if any]
   Up next: [top 3 by sequencing heuristic]

   Keep it tight. This is a glance, not a full readout. If the user wants detail, they'll ask.

4. **Flag anything worth calling out.** Items sitting for more than a week with no progress. Items that were marked critical but have no date. Patterns ("you've added 5 items this week and completed 1").

---

## Return Format

1. **Snapshot** — counts and categorized summary
2. **Flags** — anything worth the user's attention
3. **Recommendation** — what to tackle next (per sequencing heuristic in DNA)
