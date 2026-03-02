# OpenClaw Self-Docs Skill

<div align="center">

🦞 **The ultimate OpenClaw reference skill for AI agents**

[![OpenClaw Version](https://img.shields.io/badge/OpenClaw-v2026.2.26-orange)](https://github.com/openclaw/openclaw)
[![Skill Version](https://img.shields.io/badge/Skill-1.0.0-blue)](./references/VERSION.md)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

[English](README.md) | [中文](README.zh-CN.md)

</div>

## 📖 Overview

**OpenClaw Self-Docs** is a comprehensive self-documentation skill for [OpenClaw](https://github.com/openclaw/openclaw) - the multi-channel AI gateway. This skill embeds essential OpenClaw knowledge directly into your AI agent's context, enabling faster and more accurate assistance without external documentation lookups.

### Why Use This Skill?

- ⚡ **Instant Answers** - No web searches or doc lookups needed
- 🎯 **Accurate Syntax** - Exact command parameters and examples
- 📚 **Complete Coverage** - CLI, tools, config, and best practices
- 🔒 **Works Offline** - Self-contained reference
- 🔄 **Version Matched** - Synchronized with OpenClaw releases

## ✨ Features

### 📋 CLI Reference
Complete documentation for 50+ `openclaw` commands:
```bash
# Gateway management
openclaw gateway status --deep
openclaw gateway install --port 18789

# Channel operations
openclaw channels add --channel telegram --token $TOKEN
openclaw channels status --probe

# Agent management
openclaw agents add work --workspace ~/.openclaw/workspace-work
openclaw sessions --active 60
```

### 🛠️ Tool Documentation
Detailed parameters for all OpenClaw tools:
- `browser` - Web automation with Playwright
- `canvas` - Node Canvas control
- `nodes` - Paired device management
- `message` - Cross-channel messaging
- `cron` - Scheduled jobs
- `sessions_spawn` - Sub-agent orchestration
- And more...

### ⚙️ Configuration Guide
Complete schema for `~/.openclaw/openclaw.json`:
- Gateway settings (ports, auth, Tailscale)
- Channel configurations (WhatsApp, Telegram, Discord, Slack, etc.)
- Multi-agent routing
- Tool policies and sandboxing
- Session management

### 📱 Channel Guides
Platform-specific setup for:
- WhatsApp (Baileys Web)
- Telegram (Bot API)
- Discord (Bot + Socket Mode)
- Slack (Socket/HTTP modes)
- Signal
- iMessage (macOS)
- Google Chat
- Mattermost (plugin)

## 🚀 Installation

### Prerequisites
- OpenClaw v2026.2.26 or compatible version
- Skill system enabled in your OpenClaw setup

### Method 1: From ClawHub (Recommended)
```bash
npx clawhub install openclaw-selfdocs
```

### Method 2: Manual Installation
```bash
# Download the skill file
curl -L -o openclaw-selfdocs.skill https://github.com/arleyGuoLei/openclaw-selfdocs/releases/latest/download/openclaw-selfdocs.skill

# Install to OpenClaw
mkdir -p ~/.openclaw/skills
cp openclaw-selfdocs.skill ~/.openclaw/skills/

# Verify installation
openclaw skills info openclaw-selfdocs
```

### Method 3: From Source
```bash
# Clone the repository
git clone https://github.com/arleyGuoLei/openclaw-selfdocs.git
cd openclaw-selfdocs

# Package the skill
python3 /path/to/skill-creator/scripts/package_skill.py .

# Install
cp openclaw-selfdocs.skill ~/.openclaw/skills/
```

## 📚 Usage

Once installed, this skill automatically activates when you ask about:

- OpenClaw CLI commands
- Configuration options
- Tool usage patterns
- Channel setup
- Troubleshooting

### Example Queries

> "How do I check the gateway status?"

The skill provides:
```bash
openclaw gateway status [--deep] [--json] [--url <url>] [--token <token>]
```

> "What's the config for Telegram bot?"

The skill shows:
```json5
{
  channels: {
    telegram: {
      enabled: true,
      botToken: "${TELEGRAM_BOT_TOKEN}",
      dmPolicy: "pairing",
      groups: { "*": { requireMention: true } }
    }
  }
}
```

## 📁 Repository Structure

```
openclaw-selfdocs/
├── SKILL.md                    # Main skill file (entry point)
├── README.md                   # This file
├── LICENSE                     # MIT License
├── references/                 # Detailed reference docs
│   ├── VERSION.md             # Version history & changelog
│   ├── cli.md                 # CLI command reference
│   ├── tools.md               # Tool parameters & examples
│   ├── configuration.md       # Config schema reference
│   ├── channels.md            # Channel-specific guides
│   └── concepts.md            # Core concepts & architecture
└── openclaw-selfdocs.skill    # Packaged skill file (release)
```

## 🔧 For Skill Developers

### Building from Source

```bash
# Clone repository
git clone https://github.com/arleyGuoLei/openclaw-selfdocs.git
cd openclaw-selfdocs

# Find the skill-creator scripts
SKILL_CREATOR="$(npm root -g)/openclaw/skills/skill-creator/scripts"

# Package
python3 "$SKILL_CREATOR/package_skill.py" .

# The .skill file is created in current directory
```

### Project Structure Standards

This skill follows [OpenClaw Skill Guidelines](https://docs.openclaw.ai/tools/creating-skills):

- **SKILL.md** - Required. Contains YAML frontmatter with `name` and `description`, plus usage instructions
- **references/** - Optional. Detailed documentation loaded on-demand
- **No extraneous files** - No README in skill package (README is for repo only)

### Updating Content

1. Edit the relevant file in `references/`
2. Update `references/VERSION.md` with changes
3. Test the skill loads correctly
4. Submit a pull request

See [references/VERSION.md](./references/VERSION.md) for the full update checklist.

## 📝 Version Compatibility

| Skill Version | OpenClaw Version | Status |
|--------------|------------------|--------|
| 1.0.0 | v2026.2.26 | ✅ Current |

This skill is designed to be **forward-compatible** with patch releases. For new minor/major OpenClaw versions, check for skill updates.

## 🤝 Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-improvement`)
3. Make your changes
4. Test thoroughly
5. Commit with clear messages
6. Push and submit a PR

### Contribution Ideas

- 🌍 Add translations
- 📖 Improve documentation clarity
- 🔧 Add more configuration examples
- 🐛 Fix inaccuracies
- ✨ Document new OpenClaw features

## 📜 License

MIT License - see [LICENSE](LICENSE) file

## 🙏 Acknowledgments

- [OpenClaw](https://github.com/openclaw/openclaw) - The awesome AI gateway this skill documents
- [OpenClaw Community](https://discord.com/invite/clawd) - Help and inspiration
- [ClawHub](https://clawhub.com) - Skill registry and ecosystem

---

<div align="center">

**[⬆ Back to Top](#openclaw-self-docs-skill)**

Made with 🦞 for the OpenClaw community

</div>
