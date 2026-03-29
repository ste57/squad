# Linear

Use Linear as the board backend via MCP. When Linear is configured, all board operations go through Linear MCP tools — the local `.squad/board.md` file is not used.

---

## Setup

### What the agent does

1. Check if Linear MCP tools are available (search available tools for `linear`).
2. If connected, record `[board] Linear MCP` in `.squad/context.md` via Learn.
3. Ask the user which team/project to use. Record the answer in `.squad/context.md`.

### What the human must do

1. **Connect the MCP server** (one-time). The agent cannot do this — it requires browser auth:
   ```
   ! claude mcp add --transport http linear-server https://mcp.linear.app/mcp
   ```
2. **Authenticate** when prompted in the browser.

The agent should not proceed with Linear operations until tools are confirmed available.

---

## Authentication

### What the agent does

1. Before any Linear operation, verify connection by checking if Linear MCP tools respond.
2. If tools fail or are missing, tell the human what to do (see below).

### What the human must do

If the agent reports Linear is disconnected or needs auth, run:
```
! claude mcp list
```
This triggers the OAuth flow in the browser. The agent re-checks after the human confirms they've authenticated.

---

## Usage

Map board operations to Linear MCP tools:

| Board operation | Linear action |
|-----------------|---------------|
| Add item        | Create issue  |
| Complete item   | Update issue status to done |
| Status/briefing | Query active issues |
| History         | Query completed issues |

### What the agent does

- All reads and writes go through Linear MCP tools — never touch `.squad/board.md`.
- On first interaction, ask which team/project to use and record it via Learn.
- Apply the priority mapping below when creating or reading issues.

### What the human must do

- Answer which team/project to use when asked.
- Nothing else — all Linear operations are handled by the agent once connected.

---

## Priority Mapping

| Board    | Linear |
|----------|--------|
| critical | Urgent |
| high     | High   |
| medium   | Medium |
| low      | Low    |
