# 📍 Digital Craftsmen — Personal AI Assistant

<p align="center">
    <img src="https://i.postimg.cc/zfpcXcKQ/Picsart-26-07-02-12-14-28-365.jpg" alt="Digital Craftsmen" width="500" />
</p>

<p align="center">
    <img src="https://i.postimg.cc/XvbnMkhr/Screenshot-20260620-110623-org-telegram-gold-Launch-Activity.jpg" alt="Developer Badge" width="200" />
</p>

<p align="center">
  <strong>🌟 Your Personal AI Assistant. Any OS. Any Platform. The Craftsmen Way.</strong>
</p>

<p align="center">
  <a href="https://github.com/0ll-ai/Digital-Craftsmen/actions/workflows/ci.yml?branch=main"><img src="https://img.shields.io/github/actions/workflow/status/0ll-ai/Digital-Craftsmen/ci.yml?branch=main" alt="Build Status"></a>
  <a href="https://github.com/0ll-ai/Digital-Craftsmen/releases"><img src="https://img.shields.io/github/v/release/0ll-ai/Digital-Craftsmen?include_prereleases&style=for-the-badge" alt="GitHub rel"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-blue.svg?style=for-the-badge" alt="MIT License"></a>
</p>

## 📱 Social Media & Support

<p align="center">
  <a href="https://www.instagram.com/1.0_v_?igsh=N2N5MXNwN3p4ZDY2">
    <img src="https://img.shields.io/badge/Instagram-E4405F?logo=instagram&logoColor=white" alt="Instagram">
  </a>
  
  <a href="https://t.me/Y9_S4">
    <img src="https://img.shields.io/badge/Telegram-26A5E4?logo=telegram&logoColor=white" alt="Telegram">
  </a>
  
  <a href="https://www.tiktok.com/@zix8ii?_r=1&_d=f3c01a6371bii9&sec_uid=">
    <img src="https://img.shields.io/badge/TikTok-000000?logo=tiktok&logoColor=white" alt="TikTok">
  </a>
  
  <a href="https://www.facebook.com/share/1HrfmU67VS/">
    <img src="https://img.shields.io/badge/Facebook-1877F2?logo=facebook&logoColor=white" alt="Facebook">
  </a>
  
  <a href="https://wa.link/lc6f5w">
    <img src="https://img.shields.io/badge/WhatsApp-25D366?logo=whatsapp&logoColor=white" alt="WhatsApp">
  </a>
  
  <a href="https://t.me/shaheen_ys">
    <img src="https://img.shields.io/badge/Support-0088cc?logo=telegram&logoColor=white" alt="Support">
  </a>
</p>

---

## About Digital Craftsmen

**Digital Craftsmen** is a powerful _personal AI assistant_ you run on your own devices. It answers you on the channels you already use, speaks and listens on macOS/iOS/Android, and renders a live[...]

If you want a personal, single-user assistant that feels local, fast, and always-on, this is it.

### Supported Channels
WhatsApp, Telegram, Slack, Discord, Google Chat, Signal, iMessage, IRC, Microsoft Teams, Matrix, Feishu, LINE, Mattermost, Nextcloud Talk, Nostr, Synology Chat, Tlon, Twitch, and more.

### Quick Links
- **[Website](https://github.com/0ll-ai/Digital-Craftsmen)** · **[Documentation](https://docs.openclaw.ai)** · **[Vision](VISION.md)** · **[Third-party notices](THIRD_PARTY_NOTICES.md)**

---

## 🚀 Getting Started

**New to Digital Craftsmen?** Start here: [Getting Started Guide](https://docs.openclaw.ai/start/getting-started)

### Recommended Setup

```bash
# Run the onboard command for guided setup
digital-craftsmen onboard
```

Digital Craftsmen Onboard guides you step-by-step through:
- Setting up the gateway
- Configuring your workspace
- Connecting your channels
- Enabling AI skills

✅ **Works on:** macOS, Linux, and Windows  
✅ **Package Managers:** npm, pnpm, or bun

**Windows Desktop Users:** Start with the native [Windows Hub](https://docs.openclaw.ai/platforms/windows) companion app for setup, tray status, chat, node mode, and local MCP mode.

---

## 📋 Requirements

- **Runtime:** Node.js 24 (recommended) or Node 22.19+
- **Package Manager:** npm, pnpm, or bun
- **Memory:** Minimum 2GB RAM
- **Disk Space:** ~500MB for installation and dependencies

---

## 🔧 Installation

### Option 1: From npm (Recommended)

```bash
npm install -g digital-craftsmen
digital-craftsmen onboard
```

### Option 2: From Source

```bash
git clone https://github.com/0ll-ai/Digital-Craftsmen.git
cd Digital-Craftsmen
pnpm install
pnpm build
node dist/index.js
```

### Option 3: Docker

```bash
docker build -t digital-craftsmen .
docker run -it -v ~/.openclaw:/app/.openclaw digital-craftsmen
```

---

## 🚢 Railway Deployment

### Environment Variables

Copy these to your Railway project settings:

```env
# Required - Gateway Authentication
OPENCLAW_GATEWAY_TOKEN=your_secure_token_here

# Required - AI Model Provider (choose at least one)
OPENAI_API_KEY=sk-...
# or
ANTHROPIC_API_KEY=sk-ant-...
# or
GEMINI_API_KEY=...

# Optional - Communication Channels
TELEGRAM_BOT_TOKEN=...
DISCORD_BOT_TOKEN=...
SLACK_BOT_TOKEN=xoxb-...

# Optional - Tools & Services
BRAVE_API_KEY=...
ELEVENLABS_API_KEY=...

# Deployment Settings
NODE_ENV=production
NODE_OPTIONS=--max-old-space-size=2048
```

### Build & Start Commands

- **Build Command:** `pnpm build`
- **Start Command:** `node dist/index.js`

### Railway Configuration

1. Create a new Railway project
2. Connect your GitHub repository
3. Set environment variables in Railway dashboard
4. Deploy from the main branch

---

## 🛠️ Configuration

### Gateway Auth Token

Generate a secure token:

```bash
openssl rand -hex 32
```

Then set it as `OPENCLAW_GATEWAY_TOKEN` environment variable.

### AI Model Providers

You must configure at least one AI model provider:

- **OpenAI:** Set `OPENAI_API_KEY`
- **Anthropic (Claude):** Set `ANTHROPIC_API_KEY`
- **Google Gemini:** Set `GEMINI_API_KEY`
- **OpenRouter:** Set `OPENROUTER_API_KEY`

### Channel Configuration

Each channel requires specific setup. See [Channel Documentation](https://docs.openclaw.ai/channels) for detailed instructions.

---

## 📝 Configuration File

Create `.openclaw/openclaw.json` to customize settings:

```json
{
  "gateway": {
    "name": "My AI Assistant",
    "port": 8787,
    "auth": {
      "mode": "token"
    }
  },
  "skills": [],
  "channels": []
}
```

---

## 🧪 Development

### Build

```bash
pnpm build
```

### Watch Mode

```bash
pnpm build:watch
```

### Run Tests

```bash
pnpm test
```

### Lint

```bash
pnpm lint
```

### Format Code

```bash
pnpm format
```

---

## 📦 Project Structure

```
digital-craftsmen/
├── src/                    # Source code
├── packages/               # Workspace packages
├── extensions/             # Channel & feature extensions
├── apps/                   # Platform-specific apps (iOS, Android, Windows)
├── docs/                   # Documentation
├── scripts/                # Build and utility scripts
├── Dockerfile              # Docker configuration
├── package.json            # Project metadata
├── pnpm-workspace.yaml     # Workspace configuration
└── README.md              # This file
```

---

## 🤝 Contributing

We welcome contributions! Please read our [Contributing Guide](CONTRIBUTING.md) for details on our code of conduct and the process for submitting pull requests.

### Development Process

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature`
3. Commit your changes: `git commit -am 'Add feature'`
4. Push to the branch: `git push origin feature/your-feature`
5. Submit a pull request

---

## 📄 License

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

Special thanks to all contributors and the open-source community for making this project possible.

### Technology Stack

- **Runtime:** Node.js
- **Language:** TypeScript
- **Package Manager:** pnpm
- **Build Tool:** Tsdown
- **Testing:** Vitest
- **Container:** Docker

---

## 🔗 Resources

- **GitHub Repository:** https://github.com/0ll-ai/Digital-Craftsmen
- **Issues & Bug Reports:** https://github.com/0ll-ai/Digital-Craftsmen/issues
- **Documentation:** https://docs.openclaw.ai
- **Community Discussions:** https://github.com/0ll-ai/Digital-Craftsmen/discussions

---

## 📞 Support

Need help? Reach out to us:

- **GitHub Issues:** [Report a bug](https://github.com/0ll-ai/Digital-Craftsmen/issues/new)
- **Telegram:** [@shaheen_ys](https://t.me/shaheen_ys)
- **WhatsApp:** [Support Group](https://wa.link/lc6f5w)
- **Email:** Check GitHub profile for contact info

---

<p align="center">
  <strong>Made with ❤️ by the Digital Craftsmen Team</strong>
</p>

<p align="center">
  <sub>Last updated: July 2, 2026</sub>
</p>
