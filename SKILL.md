---
name: openclaw-selfdocs
description: "OpenClaw self-documentation skill. Contains comprehensive knowledge of OpenClaw CLI commands, tools, configuration, and usage patterns. Use when: (1) unsure about OpenClaw CLI command syntax, (2) need configuration reference for openclaw.json, (3) need tool usage examples, (4) troubleshooting OpenClaw behavior, (5) setting up channels or gateway. This skill version matches OpenClaw v2026.2.26."
---

# OpenClaw Self-Docs

This skill contains the complete knowledge base for OpenClaw.
Use it as a reference when you're unsure about commands, configuration, or tool usage.

> **Skill Version**: 1.0.0  
> **Compatible OpenClaw Version**: v2026.2.26  
> **Last Updated**: 2026-03-02

See [references/VERSION.md](references/VERSION.md) for version history and update checklist.

## Quick Reference

### Essential CLI Commands

```bash
# Gateway management
openclaw gateway status              # Check gateway health
openclaw gateway start               # Start gateway service
openclaw gateway stop                # Stop gateway service
openclaw gateway restart             # Restart gateway

# Channel management
openclaw channels list               # List configured channels
openclaw channels add --channel <name>  # Add a channel
openclaw channels login              # Login to WhatsApp Web
openclaw channels status --probe     # Deep health check

# Configuration
openclaw config get <path>           # Get config value
openclaw config set <path> <value>   # Set config value
openclaw configure                   # Interactive wizard
openclaw doctor                      # Health checks + fixes

# Agent management
openclaw status                      # Show session health
openclaw sessions                    # List sessions
openclaw models list                 # List available models
openclaw models status               # Check model auth status

# Node management
openclaw nodes list                  # List paired nodes
openclaw nodes status                # Check node connections
openclaw pairing list                # List pending pairings
openclaw pairing approve <code>      # Approve pairing request
```

### Essential Tools

| Tool | Purpose | Key Actions |
|------|---------|-------------|
| `browser` | Web automation | status, start, stop, snapshot, act, navigate, click, type |
| `canvas` | Node Canvas control | present, hide, navigate, eval, snapshot, a2ui_push |
| `nodes` | Paired node control | status, describe, camera_snap, screen_record, notify, run |
| `message` | Cross-channel messaging | send, poll, react, thread-create, pin |
| `cron` | Scheduled jobs | status, list, add, run |
| `exec` | Shell commands | Run with yieldMs, background, timeout options |
| `process` | Background process mgmt | list, poll, log, kill |
| `web_search` | Brave Search | Search with count, freshness filters |
| `web_fetch` | URL content extraction | Fetch markdown/text with maxChars |
| `sessions_spawn` | Sub-agent spawning | runtime: "subagent" or "acp", mode: "run" or "session" |

## Reference Files

For detailed information, see:

- [CLI Reference](references/cli.md) - Complete CLI command reference
- [Tools Reference](references/tools.md) - Tool usage patterns and parameters
- [Configuration Reference](references/configuration.md) - openclaw.json field reference
- [Channels Reference](references/channels.md) - Channel-specific configuration
- [Concepts Reference](references/concepts.md) - Core concepts and architecture

## Common Patterns

### Browser Automation Flow
```
1. browser → status / start
2. snapshot (ai or aria)
3. act (click/type/press)
4. screenshot (visual confirmation)
```

### Node Media Capture Flow
```
1. nodes → status
2. describe on chosen node
3. camera_snap / screen_record
```

### Sub-Agent Spawning
```javascript
// One-shot mode
sessions_spawn({
  task: "analyze codebase",
  runtime: "subagent",
  mode: "run"
})

// Thread-bound persistent mode
sessions_spawn({
  task: "ongoing task",
  runtime: "acp",
  mode: "session",
  thread: true
})
```

## Configuration Quick Links

### Key Config Sections
- `gateway` - Gateway port, auth, bind settings
- `channels.<name>` - Channel-specific settings (whatsapp, telegram, discord, etc.)
- `agents.defaults` - Default agent settings, models, timeouts
- `agents.list` - Per-agent overrides and routing
- `tools` - Tool allow/deny policies and profiles
- `session` - Session scope, reset policies, maintenance

### Tool Profiles
| Profile | Includes |
|---------|----------|
| `minimal` | session_status only |
| `coding` | group:fs, group:runtime, group:sessions, group:memory, image |
| `messaging` | group:messaging + session tools |
| `full` | No restrictions |

## Safety Notes

- **Elevated mode**: Use `elevated: true` for host-level exec (requires `tools.elevated.enabled`)
- **Sandboxing**: Enable Docker sandboxing via `agents.defaults.sandbox.mode`
- **Channel allowlists**: Use `allowFrom` arrays to restrict inbound messages
- **Tool policies**: Use `tools.allow`/`tools.deny` to control tool access

## Troubleshooting

1. **Gateway not responding**: `openclaw gateway status --deep`
2. **Channel issues**: `openclaw channels status --probe`
3. **Config problems**: `openclaw doctor --fix`
4. **Model auth issues**: `openclaw models status --probe`

## Version Migration Notes

When OpenClaw upgrades beyond v2026.2.26:
1. Check `/docs` for new/changed commands
2. Update this skill's references/
3. Update version tags in SKILL.md
4. Test key commands against new version
