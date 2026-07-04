# OpenClaw — Multi-Channel AI Gateway

OpenClaw is a personal AI gateway that connects Discord, Telegram, and 20+ other messaging channels to AI providers (OpenAI, Anthropic, Groq, Gemini, etc.). It runs as a persistent background service.

## Stack

- **Runtime:** Node.js 22 (required: ≥22.19.0)
- **Language:** TypeScript (monorepo)
- **Package Manager:** pnpm 11.2.2
- **Build Tool:** tsdown (rolldown-based)
- **Extensions/Channels:** discord, telegram, groq (bundled)

## How to Run on Replit

The **OpenClaw Gateway** workflow starts the service automatically. It:

1. Reads config from `openclaw.json` in the workspace root
2. Writes state to `~/.openclaw/`
3. Listens on port 8080

The workflow command:

```
OPENCLAW_CONFIG_PATH=/home/runner/workspace/openclaw.json \
OPENCLAW_STATE_DIR=/home/runner/.openclaw \
node openclaw.mjs gateway --port 8080 --allow-unconfigured --verbose
```

Health check endpoints (once running):

- `GET http://127.0.0.1:8080/healthz` → `{"ok":true,"status":"live"}`
- `GET http://127.0.0.1:8080/readyz` → readiness + uptime

## Build

The build was generated with reduced memory (3 GB heap cap) and DTS skipped — same flags as Docker:

```bash
OPENCLAW_TSDOWN_MAX_OLD_SPACE_MB=3072 OPENCLAW_RUN_NODE_SKIP_DTS_BUILD=1 node scripts/tsdown-build.mjs
node scripts/check-cli-bootstrap-imports.mjs
node scripts/runtime-postbuild.mjs
node scripts/build-stamp.mjs
node scripts/runtime-postbuild-stamp.mjs
pnpm plugins:assets:build
pnpm plugins:assets:copy
node --experimental-strip-types scripts/copy-hook-metadata.ts
node --experimental-strip-types scripts/copy-export-html-templates.ts
node --experimental-strip-types scripts/write-build-info.ts
node --experimental-strip-types scripts/write-cli-startup-metadata.ts
node --experimental-strip-types scripts/write-cli-compat.ts
```

Or combined:

```bash
OPENCLAW_TSDOWN_MAX_OLD_SPACE_MB=3072 OPENCLAW_RUN_NODE_SKIP_DTS_BUILD=1 pnpm build:docker
```

## Railway Deployment

Railway builds from `Dockerfile` (multi-stage, Node 24 + Bun). Config:

- **`railway.json`** — Railway build + deploy settings (active)
- **`.railway.toml`** — local override (start command variant)
- **`.railway/config.json`** — OpenClaw channel config read by the container

Railway build args: `OPENCLAW_EXTENSIONS=discord,telegram,groq`

Start command (in `railway.json`):

```
OPENCLAW_CONFIG_PATH=/app/.railway/config.json OPENCLAW_GATEWAY_PORT=${PORT:-18789} node openclaw.mjs gateway
```

## Key Config Files

| File                   | Purpose                                                |
| ---------------------- | ------------------------------------------------------ |
| `openclaw.json`        | Replit runtime config (Discord + Telegram channels)    |
| `.railway/config.json` | Railway container config (Discord + Telegram channels) |
| `railway.json`         | Railway build + deploy settings                        |
| `.railway.toml`        | Local Railway override (start command)                 |

## Environment Secrets (Replit)

| Secret                         | Used for                                  |
| ------------------------------ | ----------------------------------------- |
| `OPENCLAW_GATEWAY_TOKEN`       | Gateway auth token                        |
| `OPENCLAW_CONFIG_PATH`         | Config file path (overridden in workflow) |
| `OPENCLAW_STATE_DIR`           | State directory (overridden in workflow)  |
| `DISCORD_BOT_TOKEN`            | Discord channel auth                      |
| `TELEGRAM_BOT_TOKEN`           | Telegram channel auth                     |
| `OPENAI_API_KEY`               | OpenAI provider                           |
| `ANTHROPIC_API_KEY`            | Anthropic provider                        |
| `GROQ_API_KEY`                 | Groq provider                             |
| `GOOGLE_GENERATIVE_AI_API_KEY` | Gemini provider                           |
| `OPEN_ROUTER_API_KEY`          | OpenRouter provider                       |
| `OPENAI_LIKE_API_KEY`          | OpenAI-compatible provider                |

## Notes

- `dist/` is in `.gitignore` — Railway builds from source via Dockerfile; Replit uses the locally-built dist
- Node.js 22 module was installed (upgraded from 20); pnpm 11.2.2 installed via npm
- Control UI assets (web UI) require `pnpm ui:build` — currently skipped (not needed for headless gateway)
- The `channels.discord.allowFrom: ["*"]` setting is required when `dmPolicy: "open"`

## User Preferences

- Autonomous operation preferred — continue working until all issues resolved
- Push all changes to GitHub (MIXZIE-0/E-I) using GITHUB_TOKEN
- Railway deployment is the production target
