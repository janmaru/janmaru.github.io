---
title: "Copilot CLI + Dockerised MCP Server: The Setup That Cost Me a Morning"
date: 2026-04-28
author: Mauro Ghiani
category: AI
tags: [GitHubCopilot, MCP, Docker, DevTools, ContextSeven, CopilotCLI]
excerpt: "Wiring Context7 MCP into Copilot CLI looked like four config files. It turned into three traps: docker filter regex, manifest digest vs local image ID, and Windows PATH precedence."
---

![Copilot CLI + MCP architecture](/assets/images/mcp-copilot-architecture.png)

> Note: the MCP and Docker bits are platform-agnostic. The shell glue (PATH precedence, profile, AutoRun) is **Windows-specific**.

I wanted to wire **Context7** (an MCP server with up-to-date docs for 1000+ libraries) into **GitHub Copilot CLI**, with the container booting autonomously every time the terminal opens.

On paper: 4 config files. In reality: 3 noteworthy traps.

### Trap 1 — `docker ps` Filter With Regex Anchors

`docker ps --filter "name=^foo$"` with regex anchors is unreliable. When the container is stopped it returns empty, the script enters the "create" branch instead of "start", `docker run` fails because of the duplicate name — and we silence it with `*> $null`. Fix: `--format "{{.Names}}"` and compare on the shell side.

### Trap 2 — `sha256` Digest vs Local Image ID

`mcp/context7@sha256:<id>` does not work when that SHA is the **local image ID** and not the **registry manifest digest**. Docker tries to pull it and fails with "manifest schema unsupported". Correct reference: plain `sha256:<id>`, which points to the image already present locally.

### Trap 3 — Windows PATH Precedence

On Windows the System PATH takes precedence over the User PATH. Dropping the wrapper in `C:\Users\<me>\bin` is not enough: the original nodejs `copilot.cmd` wins. Fix: prepend the user bin to `$env:Path` on every shell startup (PS profile + cmd AutoRun). And here's the final twist: a check "if already present, do nothing" *skips the prepend exactly when it matters* because `bin` already is in User PATH — just in the wrong position. Lesson: strip existing occurrences and prepend *every time*.

### The Pattern That Worked

The pattern that came out is straightforward: a `copilot.cmd` wrapper that (a) guarantees the container with idempotent create/start/reuse, (b) waits for the TCP port, (c) delegates to the real binary. No race condition with the MCP client.

![Copilot CLI wrapper sequence](/assets/images/mcp-copilot-sequence.png)

### Takeaway

**MCP is not magic, it's JSON-RPC**. Docker gives you isolation and digest pinning. The critical integration point is PATH — `where copilot` is your best friend.
