---
name: OpenClaw groq plugin
description: How to install the groq provider plugin; it is not in the default local dist build.
---

# OpenClaw Groq Plugin

## Key facts
- Source lives in `extensions/groq/` — TypeScript, reads `GROQ_API_KEY` env var.
- The local `pnpm build:docker` run does NOT include groq in `dist/extensions/` by default.
- Railway Dockerfile build args (`OPENCLAW_EXTENSIONS=discord,telegram,groq`) DO include it.

## Installing for Replit (local)
```bash
OPENCLAW_CONFIG_PATH=./openclaw.json OPENCLAW_STATE_DIR=~/.openclaw \
  node openclaw.mjs plugins install @openclaw/groq-provider
```
OpenClaw detects the bundled source in `extensions/groq/` and uses it directly — no npm download needed.

## Verifying
```bash
node openclaw.mjs models list --provider groq
# groq/llama-3.3-70b-versatile should appear with tag "default"
```

**Why:** The local build script does not honour `OPENCLAW_EXTENSIONS` env var the same way the Dockerfile does. Plugin install is the correct path for adding providers to an existing local dist.
