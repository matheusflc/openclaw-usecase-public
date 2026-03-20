# Architecture

## Goal

Run OpenClaw on a dedicated headless Mac mini with:

- local gateway
- Telegram as the first user-facing channel
- Tailscale and SSH for remote administration
- Screen Sharing / VNC as GUI fallback
- browser UI exposed only over the tailnet
- light automation and durable memory
- separate personal agents for different end users

## Core Layout

- one macOS machine
- one OpenClaw gateway
- one admin/operator agent
- one dedicated Telegram bot per personal agent
- local-only gateway bind
- tailnet-only browser access

## System Diagram

```mermaid
flowchart LR
    U1[Admin User] --> TG1[Telegram Bot: Admin]
    U2[End User A] --> TG2[Telegram Bot: User A]
    U3[End User B] --> TG3[Telegram Bot: User B]

    TG1 --> GW[OpenClaw Gateway\nloopback-only]
    TG2 --> GW
    TG3 --> GW

    AdminLaptop[Admin Devices] --> TS[Tailscale Tailnet]
    AdminPhone[Admin Phone] --> TS
    TS --> Serve[Tailscale Serve / HTTPS]
    TS --> SSH[SSH]
    Serve --> GW
    SSH --> Host[Headless Mac mini Host]
    GW --> Host

    GW --> A1[Admin Agent]
    GW --> A2[Personal Agent A]
    GW --> A3[Personal Agent B]

    A1 --> M1[Remote Model]
    A1 --> L1[Local Model for heartbeat / low-cost tasks]
    A2 --> M1
    A3 --> M1

    A1 --> INT[Optional Integrations\nGmail, Sheets, Home Assistant, Roku, Spotify]
    A2 --> INT
    A3 --> INT
```

## Why This Shape

### 1. Loopback-only gateway

The gateway stays bound to `127.0.0.1`.
Remote access is layered on top with Tailscale and SSH.
When shell-only access is insufficient, Screen Sharing / VNC can be used as a GUI fallback.

This keeps the control surface off the public internet.

### 2. Multi-bot Telegram instead of same-bot peer routing

A reliable pattern for per-agent Telegram isolation is:

- one bot for the admin agent
- one bot for each personal agent

This avoids depending on same-bot per-user DM routing behavior.

### 3. Shared machine, separate agent identities

Each personal agent gets:

- its own workspace
- its own session store
- its own memory files
- its own future watcher state

This is operational separation, not a hard security boundary.

## Agent Isolation Diagram

```mermaid
flowchart TB
    GW[One OpenClaw Gateway]

    GW --> BOT1[Telegram Account: admin]
    GW --> BOT2[Telegram Account: user-a]
    GW --> BOT3[Telegram Account: user-b]

    BOT1 --> AG1[Admin Agent]
    BOT2 --> AG2[Personal Agent A]
    BOT3 --> AG3[Personal Agent B]

    AG1 --> WS1[Workspace A]
    AG2 --> WS2[Workspace B]
    AG3 --> WS3[Workspace C]

    AG1 --> SS1[Session Store A]
    AG2 --> SS2[Session Store B]
    AG3 --> SS3[Session Store C]

    AG1 --> MEM1[Memory A]
    AG2 --> MEM2[Memory B]
    AG3 --> MEM3[Memory C]

    Shared[Shared Host Tools and Skills] --> AG1
    Shared --> AG2
    Shared --> AG3

    Note[Operational separation, not a hard security boundary]
    AG1 -.-> Note
    AG2 -.-> Note
    AG3 -.-> Note
```

### 4. Headless appliance model

The machine is treated like an appliance:

- automatic login
- no system sleep
- restart after power loss
- persistent tailnet access
- OpenClaw auto-recovers on reboot

## Trust Boundaries

### Shared

- host OS
- OpenClaw gateway
- installed tools and skills
- Tailscale / SSH admin plane

### Per-agent

- workspace files
- sessions
- memory
- chat bot/account entrypoint

### Not shared publicly

- credentials
- session transcripts
- private reminders
- user-specific notes

## Practical Limits

- macOS GUI automation is harder than shell control
- stable GUI permissioning prefers a signed app identity over a raw `node` runtime
- personal agents should not be treated as strongly isolated tenants on the same host
