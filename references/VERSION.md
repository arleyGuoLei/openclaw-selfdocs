# Version History

## Current Version

- **Skill Version**: 1.0.0
- **Compatible OpenClaw Version**: v2026.2.26
- **Release Date**: 2026-03-02

---

## About This Skill

This is a self-documentation skill for OpenClaw. It provides comprehensive reference materials that help the AI agent (and users) understand how to use OpenClaw effectively.

### Why This Skill Exists

OpenClaw is a powerful multi-channel AI gateway with extensive configuration options, CLI commands, and tools. Rather than relying on web searches or documentation lookups, this skill embeds the essential knowledge directly into the agent's context, enabling:

- **Faster responses** - No need to search docs
- **Accurate syntax** - Exact command parameters and examples
- **Best practices** - Curated configuration patterns
- **Offline capability** - Works without internet

---

## What's Documented

### CLI Commands
Complete reference for all `openclaw` CLI commands including:
- Gateway management (start, stop, status, install)
- Channel management (WhatsApp, Telegram, Discord, Slack, etc.)
- Configuration (get, set, wizard)
- Agent and session management
- Model management
- Browser and node control
- Cron jobs and automation

### Tools
Detailed parameters and usage patterns for:
- `exec`, `process` - Shell execution
- `browser` - Web automation
- `canvas` - Node Canvas control
- `nodes` - Paired device management
- `message` - Cross-channel messaging
- `cron` - Scheduled jobs
- `sessions_spawn` - Sub-agent spawning
- `web_search`, `web_fetch` - Web tools

### Configuration
Complete schema documentation for `~/.openclaw/openclaw.json`:
- Gateway settings
- Channel configurations (all supported platforms)
- Agent defaults and per-agent overrides
- Tool policies and profiles
- Session management
- Security settings

### Core Concepts
- Session scoping and routing
- Multi-agent architecture
- Sandbox modes
- Streaming and queue modes
- Security model

---

## Version Compatibility

| Skill Version | OpenClaw Version | Status |
|--------------|------------------|--------|
| 1.0.0 | v2026.2.26 | ✅ Current |

### Compatibility Notes

This skill is designed to be **forward-compatible** for patch releases. However, when OpenClaw releases new minor or major versions with breaking changes, this skill should be updated.

---

## Update Checklist

When OpenClaw releases a new version:

1. **Review Documentation**
   - [ ] Check `/docs/cli` for new or changed commands
   - [ ] Check `/docs/tools` for new tools or parameter changes
   - [ ] Check `/docs/gateway/configuration-reference.md` for new config fields
   - [ ] Check `/docs/channels` for new channel features

2. **Update Content**
   - [ ] Update `references/cli.md` if commands changed
   - [ ] Update `references/tools.md` if tools changed
   - [ ] Update `references/configuration.md` if config schema changed
   - [ ] Update `references/channels.md` if channel options changed

3. **Update Version Info**
   - [ ] Update `SKILL.md` header (Compatible OpenClaw Version)
   - [ ] Update `references/VERSION.md` (this file)
   - [ ] Add entry to Version History section below
   - [ ] Update version table above

4. **Test**
   - [ ] Verify skill loads without errors
   - [ ] Test key command references
   - [ ] Run `openclaw skills check`

5. **Release**
   - [ ] Tag release on GitHub
   - [ ] Update README if needed
   - [ ] Publish to ClawHub (optional)

---

## Version History

### 1.0.0 (2026-03-02)
**Compatible with OpenClaw v2026.2.26**

Initial release with comprehensive documentation:
- CLI reference (50+ commands)
- Tool reference (15+ tools)
- Configuration reference (all major sections)
- Channel guides (8 platforms)
- Core concepts and architecture

**Key Features Documented:**
- Pi agent as primary coding agent
- Multi-channel gateway
- Plugin system
- Node pairing (iOS/Android)
- Canvas and A2UI
- Browser automation
- Cron jobs
- Multi-agent routing
- Docker sandboxing

---

## Contributing

To contribute to this skill:

1. Fork the repository
2. Make your changes to the appropriate reference file
3. Update version info if needed
4. Submit a pull request

### Contribution Guidelines

- Keep content concise and accurate
- Use clear code examples
- Follow existing formatting
- Test commands before documenting

---

## License

This skill is released under the MIT License, matching OpenClaw's license.
