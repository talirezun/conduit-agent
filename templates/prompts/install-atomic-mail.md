# Install Atomic Mail prompt

Gives your agent its own mailbox — no human webmail login; the agent holds the credentials and is the only one who uses it.

The MCP command is `npx -y @atomicmail/mcp` on both harnesses; only the config **format** differs (see [MCPs — the two config formats](../../docs/06-mcps.md)). **Copy the prompt for your harness.**

## Track A — opencode

```
Please set up Atomic Mail for this agent so it has its own agentic mailbox.
This is an agent-owned mailbox — there is no human inbox login; you (the agent)
hold the credentials and are the only one who accesses it. Work only in the
project-level opencode.jsonc in this folder — never touch any Claude config file.

Steps:
1. Ask me what username I want for the agent's mailbox (e.g. a short handle based
   on my project, like "petra-agent").
2. Register the mailbox by running:
       npx --package=@atomicmail/agent-skill atomicmail register --username "<the
       username I give you>"
   Capture the account it returns and store any credentials securely in the tool's
   own credential store / an environment variable — NEVER in AGENTS.md or any file
   in this project.
3. Register the Atomic Mail MCP in opencode.jsonc using opencode's format EXACTLY
   (top-level "mcp", "type": "local", "command" as one array):
       {
         "mcp": {
           "atomicmail": {
             "type": "local",
             "command": ["npx", "-y", "@atomicmail/mcp"],
             "enabled": true
           }
         }
       }
   If opencode.jsonc already has an "mcp" block, merge this "atomicmail" entry in.
4. Verify it works: run a JMAP "Mailbox/get" request (or send yourself a short test
   message and read it back) and confirm the mailbox is live.
5. Record in AGENTS.md (Capabilities + Communication): the mailbox username/address
   you registered, and that Atomic Mail is installed. Do NOT record credentials.
6. Remind me of the email rule: you never send an email without my explicit
   "confirm send", and you always show me the draft first.

After setup, summarise what you did in 5 bullet points.
```

## Track B — Claude (Cowork / Claude Code)

```
Please set up Atomic Mail for this agent so it has its own agentic mailbox.
This is an agent-owned mailbox — there is no human inbox login; you (the agent)
hold the credentials and are the only one who accesses it. Work only in
claude_desktop_config.json (system location) — never touch opencode.jsonc.

Steps:
1. Ask me what username I want for the agent's mailbox (e.g. a short handle based
   on my project, like "petra-agent").
2. Register the mailbox by running:
       npx --package=@atomicmail/agent-skill atomicmail register --username "<the
       username I give you>"
   Capture the account it returns and store any credentials securely in the tool's
   own credential store / an environment variable — NEVER in AGENTS.md or any file
   in this project.
3. Register the Atomic Mail MCP under the "mcpServers" key in
   claude_desktop_config.json (Claude's format: separate "command" and "args"),
   WITHOUT deleting any MCP already there:
       {
         "mcpServers": {
           "atomicmail": {
             "command": "npx",
             "args": ["-y", "@atomicmail/mcp"]
           }
         }
       }
   Then remind me to fully quit and restart Claude Desktop so the MCP loads.
4. Verify it works: run a JMAP "Mailbox/get" request (or send yourself a short test
   message and read it back) and confirm the mailbox is live.
5. Record in AGENTS.md (Capabilities + Communication): the mailbox username/address
   you registered, and that Atomic Mail is installed. Do NOT record credentials.
6. Remind me of the email rule: you never send an email without my explicit
   "confirm send", and you always show me the draft first.

After setup, summarise what you did in 5 bullet points.
```

> ⚠️ The agent **never** sends mail without your explicit "confirm send." Keep that rule in your mandate.
