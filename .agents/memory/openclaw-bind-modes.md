---
name: OpenClaw bind modes
description: Valid gateway --bind CLI values and what they mean; 0.0.0.0 is rejected.
---

# OpenClaw Gateway Bind Modes

Valid values for `--bind` flag and `gateway.bind` config key:
- `loopback` — 127.0.0.1 only (default for local use)
- `lan` — 0.0.0.0 (all interfaces; use for Railway/container deployments)
- `auto` — picks based on environment (container detection)
- `custom` — uses `gateway.bindHost` config value
- `tailnet` — binds to Tailscale interface

**Why:** `src/cli/gateway-cli/run.ts` line ~713 enforces `VALID_BIND_MODES` and calls `defaultRuntime.error()` on anything else — the process exits before binding.

**How to apply:** Railway and any container deployment must use `--bind lan` (or omit the flag and set `"bind": "lan"` in config). Never use `--bind 0.0.0.0`.
