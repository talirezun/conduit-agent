# Install Excel prompt (opencode only)

Claude Cowork / Code read and write Excel **natively** — skip this if you're on Claude. On **opencode**, this adds spreadsheet support.

```
Install the Excel MCP server (haris-musa/excel-mcp-server) for this opencode
project so the agent can read, create, and update .xlsx files.

Steps:
1. Make sure a Python runner (uv / uvx) is installed; if not, install it for me.
2. Register the Excel MCP in opencode.jsonc in THIS project folder (do not touch
   any Claude configuration file), using:
       {
         "mcpServers": {
           "excel": {
             "type": "stdio",
             "command": "uvx",
             "args": ["excel-mcp-server", "stdio"]
           }
         }
       }
3. Confirm the MCP is available, then create a test workbook at
   data/tracker.xlsx with a header row (Date, Task, Status, Evidence, Notes)
   to prove it works.
4. Record in AGENTS.md (Capabilities table) that the Excel MCP is installed.

After setup, summarise what you did in 3 bullet points.
```
