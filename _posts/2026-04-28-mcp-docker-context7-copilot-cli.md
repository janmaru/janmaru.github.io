---
title: "Wiring GitHub Copilot CLI to a Dockerised MCP server — the Context7 case"
date: 2026-04-28
author: Mauro Ghiani
category: AI
tags: [GitHubCopilot, MCP, Docker, DevTools, ContextSeven, CopilotCLI]
excerpt: "Turning Copilot CLI from a generic assistant into one that always has up-to-date library docs at hand is a matter of a few config files. The trick is called MCP."
---

> **Scope of this article.** The MCP concepts and the Docker setup are platform‑agnostic. The shell glue (wrapper, PATH precedence, profile/AutoRun) shown here is **Windows‑specific**. On macOS/Linux the same ideas apply with bash/zsh and `~/.profile` instead of PowerShell profile + cmd AutoRun.

Turning Copilot CLI from a *generic* assistant into one that always has up‑to‑date library docs at hand is a matter of a few config files. The trick is called **MCP**. In this article: what it is, why it pays off to run it in a Docker container, and how to wire it to Copilot CLI so it boots autonomously every time you open a terminal — even after a reboot.

The concrete example is **Context7**, an open‑source MCP server that indexes documentation for thousands of libraries and exposes it as a tool to the model.

---

## What MCP is, in two lines

**MCP — Model Context Protocol** is an open protocol (introduced by Anthropic, now adopted by OpenAI/GitHub Copilot and others) that standardises how an LLM talks to external tools, data and context. In practice, it's the *common format* between:

- an **MCP client** (the IDE or CLI — e.g. Copilot CLI, Claude Code, Cursor),
- an **MCP server** (a process that exposes *tools*, *resources*, *prompts*),
- the **model** which, through the client, can invoke those tools.

Without MCP every IDE would invent its own plugin format. With MCP, the same server works everywhere.

![Copilot CLI + MCP architecture](/assets/images/mcp-copilot-architecture.png)

---

## Why Docker

An MCP server is a process like any other: you could install it via `npm`, `pip`, `cargo`. But Docker buys you three practical wins:

1. **Dependency isolation** — no clashes with the rest of the machine.
2. **Idempotence** — image pinned to a digest, runs identically everywhere.
3. **Managed lifecycle** — `create / start / stop / rm` from one CLI.

The price is a small twist: you have to manage the container's state (does it exist? is it running?) before launching the client.

---

## The example: Context7

[Context7](https://context7.com) is an MCP server that returns up‑to‑date documentation snippets for 1000+ libraries. The thing that prevents answers like "according to my 2024 knowledge…" about a framework that just shipped 3.0.

Official Docker image:

```bash
docker pull mcp/context7
```

The server exposes HTTP on internal port `8080`. We map it to a local port — say `9876` — and Copilot CLI connects to `http://localhost:9876/mcp`.

---

## Solution architecture

The idea is to wrap the real `copilot` binary so the wrapper:

1. ensures the Context7 container is up (creates it if missing, restarts it if stopped, reuses it if running),
2. waits for the port to be reachable,
3. delegates the actual invocation to the original Copilot CLI binary.

![Copilot CLI wrapper sequence](/assets/images/mcp-copilot-sequence.png)

No race condition: the MCP connection only fires after the port answers.

---

## Step‑by‑step implementation (Windows)

> The same ideas apply to macOS/Linux: only the shell glue changes (bash instead of PowerShell, no `cmd.exe`, `~/.profile` instead of AutoRun).

### 1. Copilot CLI MCP config

Copilot CLI reads MCP servers from `~/.copilot/mcp-config.json`:

```json
{
  "mcpServers": {
    "context7": {
      "type": "http",
      "url": "http://localhost:9876/mcp"
    }
  }
}
```

And in `~/.copilot/settings.json` we whitelist the host:

```json
{
  "allowedUrls": [
    "http://localhost:9876"
  ]
}
```

### 2. Container guard script

`~/.copilot/start-context7.ps1` — idempotent create/start/reuse with TCP wait:

```powershell
$containerName = "context7-mcp"
$image    = "sha256:d7f6436f3581b18290df958e3ca924f7aa0b6e47b9c513b7c5aac0b7e7d1a949"
$hostPort = 9876

docker info *> $null
if ($LASTEXITCODE -ne 0) {
    Write-Host "Docker daemon not reachable." -ForegroundColor Red
    return
}

$allNames     = @(docker ps -a --format "{{.Names}}" 2>$null)
$runningNames = @(docker ps    --format "{{.Names}}" 2>$null)
$exists  = $allNames     -contains $containerName
$running = $runningNames -contains $containerName

if (-not $exists) {
    Write-Host "Creating context7 MCP server..." -ForegroundColor Yellow
    docker run -d --name $containerName -p "${hostPort}:8080" $image | Out-Null
}
elseif (-not $running) {
    Write-Host "Starting context7 MCP server..." -ForegroundColor Yellow
    docker start $containerName | Out-Null
}

# Wait for the port to answer
$deadline = (Get-Date).AddSeconds(15)
while ((Get-Date) -lt $deadline) {
    try {
        $tcp = [System.Net.Sockets.TcpClient]::new()
        $tcp.Connect('127.0.0.1', $hostPort)
        $tcp.Close()
        Write-Host "context7 ready on localhost:$hostPort" -ForegroundColor Green
        return
    } catch { Start-Sleep -Milliseconds 300 }
}
Write-Host "context7 did not respond in 15s" -ForegroundColor Red
```

> **Pitfall #1.** `docker ps --filter "name=^foo$"` with regex anchors is unreliable. Better to use `--format "{{.Names}}"` and compare on the shell side.

> **Pitfall #2.** Pinning the image as `mcp/context7@sha256:<id>` **does not work** if that SHA is the local *image ID* and not the registry *manifest digest*. Docker tries to pull it and fails with "manifest schema unsupported". Use plain `sha256:<id>` instead — it points directly to the image already present locally.

### 3. Wrapper that encapsulates `copilot`

`C:\Users\<me>\bin\copilot.cmd`:

```cmd
@echo off
pwsh -NoLogo -NoProfile -ExecutionPolicy Bypass -File "%USERPROFILE%\.copilot\start-context7.ps1"
call "C:\Program Files\nodejs\copilot.cmd" %*
```

### 4. Making the wrapper win on PATH (Windows‑specific)

On Windows the **System PATH** takes precedence over the **User PATH**. Putting `C:\Users\<me>\bin` in User PATH is **not enough**: the original `copilot.cmd` in `C:\Program Files\nodejs` is resolved first.

Solution without admin privileges: prepend `C:\Users\<me>\bin` to `$env:Path` **on every shell startup**.

**PowerShell** — `~/Documents/PowerShell/profile.ps1` (and the equivalent `WindowsPowerShell` for 5.1):

```powershell
$__userBin = "C:\Users\<me>\bin"
$__parts = $env:Path -split ';' |
    Where-Object { $_ -and ($_.TrimEnd('\') -ne $__userBin.TrimEnd('\')) }
$env:Path = (@($__userBin) + $__parts) -join ';'
```

**cmd** — `cmd-autorun.cmd` script:

```cmd
@echo off
set "PATH=C:\Users\<me>\bin;%PATH%"
```

Registered as AutoRun:

```powershell
Set-ItemProperty "HKCU:\Software\Microsoft\Command Processor" `
                 -Name AutoRun `
                 -Value "C:\Users\<me>\bin\cmd-autorun.cmd" -Type String
```

> **Pitfall #3.** The first version of the prepend had a check "if `C:\Users\<me>\bin` is already in PATH, do nothing". But `C:\Users\<me>\bin` *was* already in PATH (User scope) — just in the wrong position. The check skipped the prepend exactly when it mattered. Lesson: in cases like this, **don't optimise with `if not contains`** — strip the existing occurrences and prepend unconditionally.

---

## Verification

In a fresh terminal:

```cmd
where copilot
```

Should show the wrapper **at the top**:

```
C:\Users\<me>\bin\copilot.cmd
C:\Program Files\nodejs\copilot
C:\Program Files\nodejs\copilot.cmd
```

Then:

```cmd
docker stop context7-mcp
copilot
```

Expected output:

```
Starting context7 MCP server...
context7 ready on localhost:9876
● Environment loaded: 2 skills, 1 MCP server, 3 plugins
```

No more `Failed to connect to MCP server "context7"`.

---

## Takeaways

- **MCP is not magic, it's an interface.** An MCP server is a process speaking JSON‑RPC: over HTTP, stdio or WebSocket. Treat it as such.
- **Docker kills drift.** Pin the image to a digest and your tool behaves the same everywhere.
- **The critical integration point is PATH.** Most "it doesn't work but the file is there" issues on Windows boil down to which `.cmd` gets resolved first. `where` is your best friend.
- **Bad idempotence is worse than no idempotence.** An `if-already-present` check that doesn't account for *position* or *state* is a classic anti‑pattern.

And above all: **the model is only as good as the context you feed it**. A well‑configured MCP is the difference between a generic suggestion and an answer that quotes the current version of the library you're actually using.

---

*Got a favourite MCP server or a different setup? Curious to hear how you've integrated it into your workflow.*

#GitHubCopilot #MCP #Docker #DevTools #AI #Context7
