# Diagrams

These diagrams are intentionally generic and public-safe.
They describe the system shape without exposing live host details, user identifiers, or secrets.

## 1. System Architecture

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

## 2. Agent Isolation Model

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

## 3. Watchers and Automation Flow

```mermaid
flowchart LR
    subgraph Events[Event-driven]
        WA[WhatsApp inbound]
        TG[Telegram inbound]
    end

    subgraph Polling[Polling]
        HB[Heartbeat]
        CRON[Cron Jobs]
        GM[Gmail checks]
        PR[Price checks]
    end

    WA --> GW[OpenClaw Gateway]
    TG --> GW
    HB --> GW
    CRON --> GW
    GM --> HB
    PR --> CRON

    GW --> AG[Target Agent]
    AG --> STATE[Local state files\nlast seen ids / last price / last alert]
    AG --> MEM[Durable memory note\nonly when worth keeping]
    AG --> ALERT[Telegram alert / digest]

    RULES[Rules:\n- event-driven for chats\n- heartbeat for cheap recurring checks\n- cron for exact timing\n- alert only on changes]
    RULES -.-> HB
    RULES -.-> CRON
```

## 4. Versioning Boundaries

```mermaid
flowchart TB
    subgraph LocalOnly[Local-only state]
        CFG[Live openclaw.json]
        CREDS[Credentials and tokens]
        SESS[Sessions and transcripts]
        MEDIA[Media / logs / device state]
        PRIV[Private memory / case notes]
    end

    subgraph PrivateRepo[Private repo]
        OPS[Ops docs]
        FULL[Full setup docs]
        SANHOOKS[Hooks and helper scripts]
        TEMPL[Sanitized templates]
    end

    subgraph PublicRepo[Public use case repo]
        ARCH[Architecture docs]
        REPL[Replication guide]
        LESSONS[Lessons learned]
        GUARD[Public-safe templates]
    end

    LocalOnly -. never publish .-> PublicRepo
    LocalOnly -. usually not versioned in GitHub .-> PrivateRepo
    PrivateRepo --> PublicRepo

    EXPORT[Manual sanitized export]
    PrivateRepo --> EXPORT --> PublicRepo
```
