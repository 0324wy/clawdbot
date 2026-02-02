# Clawdbot: Your Personal AI Assistant

## What is Clawdbot?

Clawdbot is a **personal AI assistant** that runs on your own device. It connects to messaging apps you already use (WhatsApp, Telegram, Slack, Discord, etc.) and responds as you.

```
You ──► WhatsApp ──► Clawdbot ──► AI processes ──► Response back to you
```

---

## The Core: Pi Agent Engine

The actual AI thinking happens in **`@mariozechner/pi-coding-agent`** - an external library that handles the core agent loop:

```
Think → Call Tool → Get Result → Think Again → Repeat
```

**Example:**
```
User: "Create a file hello.txt"

1. Agent thinks: "I need to write a file"
2. Agent calls: write("hello.txt", "Hello World")
3. Tool executes: File created
4. Tool returns: Success
5. Agent thinks: "Done!" → Responds to user
```

---

## Tools System

Clawdbot gives the agent **capabilities** to take actions:

| Category | Tools |
|----------|-------|
| Files | read, write, edit |
| Shell | exec (run commands) |
| Browser | control Chrome/Chromium |
| Canvas | visual workspace |
| Camera | take photos, record video |
| Web | search, fetch web pages |
| Messages | send to channels |
| Sessions | list, send, spawn agents |
| And more... |

The pi-coding-agent has basic tools built-in. Clawdbot **extends** them with many more.

---

## Auth System

Clawdbot manages **API keys** for LLM providers:

- Anthropic (Claude)
- OpenAI (GPT-4)
- Google (Gemini)
- GitHub Copilot
- AWS Bedrock
- And more...

**Features:**
- Multiple accounts per provider
- Automatic failover if one hits rate limit
- OAuth token refresh

---

## Sandbox (Security)

**Sandbox = Docker container isolation** for agent sessions.

```
┌─────────────────────────────────────────┐
│           Your Computer                  │
│                                         │
│  ┌───────────────────────────────────┐  │
│  │    Docker Container (Sandbox)     │  │
│  │    - Isolated filesystem          │  │
│  │    - Isolated network             │  │
│  │    - Agent runs here              │  │
│  └───────────────────────────────────┘  │
│                                         │
│  Your files stay safe!                  │
└─────────────────────────────────────────┘
```

**When is sandbox used?**

| Session Type | Sandbox? | Access to Your Files? |
|--------------|----------|----------------------|
| Main session | ❌ No | ✅ Yes |
| Group chat | ✅ Yes | ❌ No |
| Subagent | ✅ Yes | ❌ No |

Use **main session** for coding tasks that need to modify your files. Use sandboxed sessions for background/untrusted tasks.

---

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                       Clawdbot                               │
│  - Channels (WhatsApp, Telegram, Slack, etc.)               │
│  - Extended tools (browser, camera, canvas, web, etc.)      │
│  - Auth management (API keys)                               │
│  - Sandbox (Docker isolation)                               │
│  - UI (CLI, macOS app, mobile)                              │
└─────────────────────────────────────────────────────────────┘
                              ▲
                              │ Uses
                              ▼
┌─────────────────────────────────────────────────────────────┐
│              @mariozechner/pi-coding-agent                   │
│                                                             │
│  - The thinking loop                                        │
│  - Conversation history                                     │
│  - LLM streaming                                            │
│  - Built-in tools (read/write/edit/bash)                    │
└─────────────────────────────────────────────────────────────┘
```

---

## Quick Start

```bash
# Install
npm install -g clawdbot@latest

# Set up
clawdbot onboard --install-daemon

# Use
clawdbot message send --message "Hello, help me write some code"
```

---

## Summary

- **Clawdbot** = Infrastructure (channels, tools, auth, UI)
- **pi-coding-agent** = Core AI engine (the thinking loop)
- **Tools** = What the agent can do (extended by Clawdbot)
- **Auth** = API key management for LLMs
- **Sandbox** = Docker container for isolation (security)

Clawdbot wraps the pi-coding-agent with powerful tools and channels, making it a complete personal AI assistant that works with your messaging apps and can take real actions on your computer.

**Website**: https://clawdbot.com
**Docs**: https://docs.clawd.bot
