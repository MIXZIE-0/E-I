---
name: OpenClaw build setup
description: How to build and run the OpenClaw monorepo on Replit — Node version, pnpm install, heap limits, and what breaks.
---

## Rules

1. Node.js **22** required (`engines: >=22.19.0`). Node 20 (old default) causes a startup error.
2. The nix-provided pnpm 10 sees `packageManager: pnpm@11.2.2` in package.json and tries to self-upgrade via `pnpm add pnpm@11.2.2 ...`, which SIGABRTs in this sandbox (worker-thread creation fails) and loops forever. Do NOT try to install pnpm 11 — instead add `manage-package-manager-versions=false` to `~/.npmrc` AND the project `.npmrc` (both are needed; pnpm exec spawned from scripts reads project npmrc). This lets nix pnpm 10 run pnpm/tsdown normally.
3. `corepack enable` fails on NixOS (read-only store). Use nix-provided pnpm directly with the npmrc fix above.
4. Background/detached shell processes (`nohup ... & disown`) get killed the instant a bash tool call returns in this sandbox — they do NOT survive across tool calls. For any build/command that needs >120s, run it via a temporary Replit workflow (`configureWorkflow`) instead, then poll `getWorkflowStatus` and `removeWorkflow` when done — workflows are supervised independently and do survive.
5. `dist/` is in `.gitignore` — Railway builds from Dockerfile; Replit needs a local build.
6. `railway.json` `buildArgs.OPENCLAW_EXTENSIONS` is a **Docker build-time** allowlist consumed by `scripts/prune-docker-plugin-dist.mjs` — it physically deletes any provider plugin dir NOT listed, from the built image. It is NOT the same as the `channels`/`agents` config in `openclaw.json`/`.railway/config.json` (which is read at runtime). If a provider (e.g. openai, anthropic, openrouter, google) is missing from this build-arg list, the Railway container will throw "Missing API key for the selected provider" even though the env var secret is correctly set — because the plugin code itself was pruned from the image, not because the key is missing. Any new provider added to the model/fallback chain must also be added here.
7. `git commit`/`git add` are blocked for the main agent in this sandbox (destructive-git-ops policy) — the platform auto-commits a checkpoint at the end of each turn. To sync to GitHub, `git push` still works but needs the token embedded in the URL (`git -c credential.helper= push https://$GITHUB_TOKEN@github.com/<org>/<repo> main`) since no credential helper is configured — a bare `git push origin main` over https hangs forever waiting for a password prompt.

## Build command (Replit)

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

**Why:** Default tsdown heap is 12 GB, which OOM-kills on an 8 GB Replit VM. 3 GB cap works. DTS skip matches the Docker build flag.

**How to apply:** Any time the gateway fails to start with "missing dist/entry.js", or after a container restart.

## Run command (Replit workflow)

```
OPENCLAW_CONFIG_PATH=/home/runner/workspace/openclaw.json OPENCLAW_STATE_DIR=/home/runner/.openclaw node openclaw.mjs gateway --port 8080 --allow-unconfigured --verbose
```

`/app/state` (set in secrets) is read-only on Replit — override both path secrets inline in the workflow command.
