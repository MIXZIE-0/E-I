---
name: OpenClaw channel config field names
description: Correct JSON field names for Discord and Telegram channels in openclaw.json / .railway/config.json — easy to get wrong.
---

## Rules

- **Discord** channel token field: `"token"` (not `"botToken"`)
- **Telegram** channel token field: `"botToken"` (not `"token"`)
- When `channels.discord.dmPolicy = "open"`, must also set `"allowFrom": ["*"]` or all DMs are silently dropped.

**Why:** The gateway validates configs against plugin schemas at startup. Wrong field names produce "invalid config: must not have additional properties" errors. Verified from `extensions/discord/src/accounts.ts:33` (`channelKeys: ["token"]`) and `extensions/telegram/src/account-selection.ts:104` (`telegram.botToken`).

**How to apply:** Any time editing `openclaw.json`, `.railway/config.json`, or any channel config block.

## Working config shape

```json
{
  "channels": {
    "discord": {
      "token": "${DISCORD_BOT_TOKEN}",
      "dmPolicy": "open",
      "allowFrom": ["*"]
    },
    "telegram": {
      "botToken": "${TELEGRAM_BOT_TOKEN}"
    }
  }
}
```

## Telegram polling conflict on restart

When the gateway restarts, the previous Telegram polling session may still be alive for ~60s. The log shows "terminated by other getUpdates request" — this is normal and auto-resolves. The gateway retries with exponential backoff (~32s).
