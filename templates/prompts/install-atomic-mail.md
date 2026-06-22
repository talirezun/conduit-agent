# Install Atomic Mail prompt

Gives your agent its own mailbox — no human webmail login; the agent holds the credentials and is the only one who uses it.

```
Please set up Atomic Mail for this agent so it has its own agentic mailbox.
This is an agent-owned mailbox — there is no human inbox login; you (the agent)
hold the credentials and are the only one who accesses it.

Steps:
1. Ask me what username I want for the agent's mailbox (e.g. a short handle based
   on my project, like "petra-agent").
2. Register the mailbox by running:
       npx --package=@atomicmail/agent-skill atomicmail register --username "<the
       username I give you>"
   Capture the account it returns and store any credentials securely in the tool's
   own credential store / an environment variable — NEVER in AGENTS.md or any file
   in this project.
3. Register the Atomic Mail MCP in the config file for the harness you are in:
   - opencode -> opencode.jsonc in this project folder
   - Claude Cowork / Code -> claude_desktop_config.json, then remind me to fully
     restart Claude Desktop
   using:
       {
         "mcpServers": {
           "atomicmail": {
             "command": "npx",
             "args": ["-y", "@atomicmail/mcp"]
           }
         }
       }
4. Verify it works: run a JMAP "Mailbox/get" request (or send yourself a short test
   message and read it back) and confirm the mailbox is live.
5. Record in AGENTS.md (Capabilities + Communication): the mailbox username/address
   you registered, and that Atomic Mail is installed. Do NOT record credentials.
6. Remind me of the email rule: you never send an email without my explicit
   "confirm send", and you always show me the draft first.

After setup, summarise what you did in 5 bullet points.
```

> ⚠️ The agent **never** sends mail without your explicit "confirm send." Keep that rule in your mandate.
