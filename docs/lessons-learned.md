# Lessons Learned

## What worked well

### Telegram first

Telegram is a clean first channel because it avoids phone-linking complexity.

### Tailnet-only browser access

Tailscale Serve is a good fit for remote browser UI without exposing the gateway publicly.

### Local Whisper

Local voice transcription is viable on Apple Silicon and avoids recurring API cost.

### Separate bot per agent

Per-agent Telegram bots are a robust routing model when same-bot user routing is unreliable.

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
- use browser automation only when interaction is needed
- keep heartbeat cheap
- keep public documentation separate from live state
