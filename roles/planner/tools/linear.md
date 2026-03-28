# Linear

Connect and use Linear as the board backend via MCP.

---

## Setup

When the user wants to use Linear as their board:

1. **Check if Linear MCP is connected.** Search available tools for `linear`. If Linear tools exist, skip to step 3.
2. **Connect the MCP server.** Tell the user to run:
   ```
   ! claude mcp add --transport http linear-server https://mcp.linear.app/mcp
   ```
   The `!` prefix runs it in the current session. They will need to authenticate via the browser. Once connected, Linear tools become available.
3. **Record in project context.** Run Learn to add a `[board]` entry in `.squad/context.md` noting Linear as the board backend.
4. **Do not create `.squad/board.md`.** When Linear is the backend, the local board file is not used.

## Usage

When Linear is connected, use Linear MCP tools instead of reading/writing `.squad/board.md`. Map board operations to Linear:

- **Add item** → create a Linear issue
- **Complete item** → update the issue status to done
- **Status/briefing** → query Linear for active issues
- **History** → query Linear for completed issues

Ask the user which team/project to use on first interaction. Record it in `.squad/context.md` via Learn.

## Priority Mapping

| Board    | Linear   |
|----------|----------|
| critical | Urgent   |
| high     | High     |
| medium   | Medium   |
| low      | Low      |
