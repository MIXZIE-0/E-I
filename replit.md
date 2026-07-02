# Digital Craftsmen — OpenClaw Gateway

Personal AI Assistant gateway server with Discord and Telegram channels. Deployed on Railway via Docker.

## Stack

- **Runtime:** Node.js 24 (via Docker)
- **Language:** TypeScript (built with tsdown)
- **Package manager:** pnpm
- **Entry point:** `openclaw.mjs` (compiled gateway binary)
- **Default port:** 18789 (overridden by `OPENCLAW_GATEWAY_PORT` or Railway's `PORT`)

## How to run (Railway)

Railway builds using the `Dockerfile` and starts with:

```
sh -c 'OPENCLAW_GATEWAY_PORT=${PORT:-18789} node openclaw.mjs gateway'
```

Railway's injected `PORT` env var is forwarded to the gateway via `OPENCLAW_GATEWAY_PORT`.

## Bundled extensions (build-time)

`railway.json` passes `OPENCLAW_EXTENSIONS=discord,telegram,groq` as a Docker build arg.
This bundles the Discord channel plugin, Telegram channel plugin, and Groq LLM provider into the image.

## Required environment variables (set in Railway)

| Variable | Purpose |
|---|---|
| `GROQ_API_KEY` | Groq LLM / transcription provider API key |
| `ANTHROPIC_API_KEY` | Anthropic Claude provider API key |
| `DISCORD_BOT_TOKEN` | Discord bot token (from Discord Developer Portal) |
| `TELEGRAM_BOT_TOKEN` | Telegram bot token (from @BotFather) |
| `OPENCLAW_GATEWAY_TOKEN` | Auth token clients use to connect to the gateway (generate with `openssl rand -hex 32`) |

## Optional environment variables

| Variable | Purpose |
|---|---|
| `OPENAI_API_KEY` | OpenAI provider |
| `OPENROUTER_API_KEY` | OpenRouter (multi-provider) |
| `GITHUB_TOKEN` | GitHub tool access |

## Channel configuration

Channels are configured in `.railway/config.json`.
- **Discord** — reads `DISCORD_BOT_TOKEN` via `${DISCORD_BOT_TOKEN}` substitution, `dmPolicy: open`
- **Telegram** — reads `TELEGRAM_BOT_TOKEN` via `${TELEGRAM_BOT_TOKEN}` substitution, `dmPolicy: open`

Both bots run concurrently as separate channel runtimes within the same gateway process.

## Health check endpoints

- `GET /healthz` — liveness
- `GET /readyz` — readiness

## Config file

`.railway/config.json` — OpenClaw gateway config (name, bind address, auth mode, channels, skills).
The port is **not** hardcoded here; it comes from `OPENCLAW_GATEWAY_PORT` / Railway `PORT`.

## User preferences

- Deploy on Railway using Dockerfile builder.
- Gateway auth mode: `token` — always set `OPENCLAW_GATEWAY_TOKEN` in Railway environment.
- Groq and Anthropic are the primary LLM providers.
- Discord and Telegram channels both run concurrently in the same gateway.
