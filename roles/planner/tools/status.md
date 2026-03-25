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
3. **Present the snapshot.** Use box-drawing tables for all output. Never use markdown tables. Output inside a code block.

   **Layout:**

   The header bar and data table share the same column grid. The header uses heavy lines (┏━┳━┓), the data uses light lines (│). This keeps everything aligned.

   ```
   ┏━━━┳━━━━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━┳━━━━━━━━━━━━━━━━━━━━━━┓
   ┃   ┃  SECTION NAME             ┃          ┃   X active · Y high  ┃
   ┣━━━╋━━━━━━━━━━━━━━━━━━━━━━━━━━╋━━━━━━━━━━╋━━━━━━━━━━━━━━━━━━━━━━┫
   ┃ # ┃ Item                     ┃ Priority ┃ Notes                ┃
   ┣━━━╋━━━━━━━━━━━━━━━━━━━━━━━━━━╋━━━━━━━━━━╋━━━━━━━━━━━━━━━━━━━━━━┫
   │ 1 │ High task                │ ● high   │                      │
   │ 2 │ High task                │ ● high   │ some note            │
   ├───┼──────────────────────────┼──────────┼──────────────────────┤
   │ 3 │ Med task                 │ ○ med    │                      │
   └───┴──────────────────────────┴──────────┴──────────────────────┘

   ▶ Hit first: Item → Item → Item
   ```

   **Rules:**
   - Header bar + column headers use heavy lines (┏ ━ ┳ ┓ ┃ ┣ ╋ ┫)
   - Data rows use light lines (│ ├ ┼ ┤ └ ┴ ┘)
   - Header bar spans the same column grid as data rows — never a separate box
   - Section name in header bar is UPPERCASE
   - Summary stats (X active · Y high) right-aligned in the Notes column
   - Priority indicators: `● high`, `○ med`, `◌ low`, `◆ critical`
   - NO row separators between items in the same priority group
   - Row separator (├───┼...┤) ONLY between priority groups
   - Done items: prefix with `x ` (e.g. `x Streakers`). Do NOT use Unicode strikethrough — it breaks column alignment.
   - One table per section (Personal, project subtasks, Done, etc.)
   - Footer: `▶ Hit first: Item → Item → Item`

   Keep it tight. This is a glance, not a full readout. If the user wants detail, they'll ask.

4. **Flag anything worth calling out.** Items sitting for more than a week with no progress. Items that were marked critical but have no date. Patterns ("you've added 5 items this week and completed 1").

---

## Return Format

1. **Snapshot** — counts and categorized summary
2. **Flags** — anything worth the user's attention
3. **Recommendation** — what to tackle next (per sequencing heuristic in DNA)
