# OpenClaw Headless Mac Mini Use Case

Public-safe reference repository for an always-on OpenClaw deployment on a headless macOS machine.

This repository is intentionally sanitized.

It documents:

- architecture decisions
- setup choices
- remote access model
- Telegram multi-bot routing
- local voice transcription
- local model strategy
- watcher patterns
- versioning boundaries

It does not contain:

- live secrets
- real hostnames, IPs, or user identifiers
- transcripts
- private agent memory
- operational credentials
- real user workspaces

## Repository Structure

- `docs/architecture.md`: deployment design and trust boundaries
- `docs/replication.md`: step-by-step sanitized setup guide
- `docs/lessons-learned.md`: practical pitfalls and tradeoffs
- `docs/versioning-boundaries.md`: what belongs in Git vs local-only state
- `templates/config/openclaw.template.json`: placeholder config template
- `templates/workspaces/AGENT-GUARDRAILS.md`: baseline guardrails for personal agents

## Intended Audience

People who want to replicate a similar OpenClaw setup without copying live operational state.

## Publication Rule

If this repo is ever published, keep it as documentation and templates only.
Do not turn it into a mirror of live `~/.openclaw` state.
