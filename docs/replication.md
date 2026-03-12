# Replication Guide

## 1. Install Base Runtime

```bash
npm install -g openclaw@latest
```

Run onboarding:

```bash
openclaw onboard --install-daemon
```

Recommended onboarding choices:

- local gateway
- loopback bind
- token auth
- Tailscale exposure off during onboarding
- Telegram as the first channel
- skip skills and hooks initially

## 2. Remote Access

Enable:

- SSH
- Tailscale

Recommended model:

- keep OpenClaw local-only
- use Tailscale Serve for browser UI
- use SSH for admin shell access

## 3. Telegram

Start with Telegram because it is operationally simple.

Recommended defaults:

- DM-only
- pairing enabled
- group messages disabled initially
- streaming off

## 4. Audio

Use local Whisper for Telegram voice notes.

Recommended path:

- `whisper-cli`
- local small model
- wrapper script that converts `.ogg`/Opus to `.wav` before transcription

This avoids API transcription cost and works well for short voice notes.

## 5. Memory and Automation

Recommended early setup:

- durable memory backend enabled
- one daily brief
- one lightweight heartbeat
- no aggressive autonomous remediation

## 6. Local Models

Recommended split:

- strong remote model for tool-heavy operator work
- cheap local model for heartbeat and background checks

On a 16 GB Apple Silicon host, small/modest Ollama models are the pragmatic choice.

## 7. Personal Agents

Recommended pattern:

- admin/operator agent
- one separate personal agent per user
- one dedicated Telegram bot per personal agent

Let each user define:

- agent name
- emoji
- style
- how the agent addresses them

## 8. Smart-home / Daily-use Extensions

Good candidates:

- Home Assistant
- Spotify control
- Roku control
- camera integration
- Gmail / Sheets later

Defer account-heavy integrations until the base system is stable.
