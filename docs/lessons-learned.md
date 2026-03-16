# Lessons Learned

## What worked well

### Telegram first

Telegram is a clean first channel because it avoids phone-linking complexity.

### Tailnet-only browser access

Tailscale Serve is a good fit for remote browser UI without exposing the gateway publicly.

### Local Whisper

Local voice transcription is viable on Apple Silicon and avoids recurring API cost.

### Keep AGENTS.md short

Short startup instructions work better than dumping policy into one huge boot file.
Keep the startup sequence at the top, move personality/tool detail to separate files, and keep durable rules in `LEARNINGS.md`.

### Separate bot per agent

Per-agent Telegram bots are a robust routing model when same-bot user routing is unreliable.

### Use cron for reminders

For alarms, reminders, and delayed follow-ups, one-shot cron jobs are better than leaving a `sleep` process hanging in the background.
They are easier to audit, less likely to interfere with the active chat, and scale better once multiple users start asking for reminders.

### Prefer gws for Google Workspace

For Gmail, Sheets, Drive, Docs, and Calendar, `gws` is the cleaner path for agent use than older single-purpose Google CLIs.

### Use goplaces for local life queries

For pharmacies, restaurants, clinics, and business lookup, `goplaces` is a better first tool than generic search or browser automation.
It gives structured results and reduces noisy scraping.

## What was less clean

### GUI control on macOS

Shell control is easy.
GUI control is not.

Raw `node` processes are poor long-term macOS privacy/TCC targets.
If stable GUI automation matters, use a stable app identity.

### Personal-agent Git commits

Allowing personal agents to use Git for reminders and preference changes creates low-quality history and mixes private memory with engineering state.

Use Git for:

- infrastructure
- hooks
- scripts
- docs
- templates

Do not use Git for:

- reminders
- user preferences
- case notes
- conversation-derived memory

### Bigger local models are not automatically better

On modest hardware, a newer or larger local model may be slower or less operationally useful than a smaller, stable one.
Benchmark on the real host.

## Operational heuristics

- keep the gateway local-only
- prefer search before browser automation for general web lookup
- prefer `goplaces` before generic web search for place/business lookup
- use browser automation only when interaction is needed
- keep heartbeat cheap
- keep public documentation separate from live state
