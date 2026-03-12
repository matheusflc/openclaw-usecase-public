# Versioning Boundaries

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

## Put In Git

- setup docs
- operations docs
- hooks
- helper scripts
- sanitized templates
- watcher templates
- automation playbooks

## Keep Local Only

- live `openclaw.json`
- credentials
- Telegram state
- devices
- logs
- media
- session transcripts
- durable user memory with private facts
- private legal, personal, or financial notes

## Public Export Rule

A public repo should describe the system, not mirror the live system.

If a file contains any of these, it stays out:

- a secret
- a stable user identifier
- a private transcript
- a host-specific operational detail that is unnecessary for replication
