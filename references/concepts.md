# Concepts Reference (v2026.2.26)

Core OpenClaw concepts and architecture.

## Architecture Overview

```
Chat Apps → Gateway → Pi Agent (RPC mode)
               ↓
         Web Control UI
               ↓
         iOS/Android Nodes
```

- **Gateway**: Single WebSocket server for all channels and routing
- **Pi Agent**: Coding agent with tool streaming
- **Control UI**: Browser dashboard for chat, config, sessions
- **Nodes**: Paired iOS/Android devices with Canvas support

## Session Scoping

### DM Scope Options
| Mode | Behavior |
|------|----------|
| `main` | All DMs share main session |
| `per-peer` | Isolate by sender across channels |
| `per-channel-peer` | Isolate per channel + sender |
| `per-account-channel-peer` | Isolate per account + channel + sender |

### Session Reset Modes
| Mode | Description |
|------|-------------|
| `daily` | Reset at specified hour |
| `idle` | Reset after idle minutes |

Reset triggers: `/new`, `/reset`

## Channel Policies

### DM Policies
| Policy | Behavior |
|--------|----------|
| `pairing` (default) | Unknown senders get pairing code |
| `allowlist` | Only `allowFrom` senders |
| `open` | Allow all (requires `allowFrom: ["*"]`) |
| `disabled` | Ignore all DMs |

### Group Policies
| Policy | Behavior |
|--------|----------|
| `allowlist` (default) | Only configured groups |
| `open` | Bypass group allowlists |
| `disabled` | Block all groups |

### Mention Gating
Group messages default to require mention:
- **Metadata mentions**: Native @-mentions
- **Text patterns**: Regex in `mentionPatterns`

## Multi-Agent Routing

Binding match order (first wins):
1. `match.peer` (specific sender)
2. `match.guildId` / `match.teamId`
3. `match.accountId` (exact)
4. `match.accountId: "*"` (channel-wide)
5. Default agent

## Tool Policy Precedence

1. Base profile (`tools.profile`)
2. Provider restrictions (`tools.byProvider`)
3. Global allow/deny (`tools.allow`, `tools.deny`)
4. Agent-specific overrides

## Streaming Modes

| Mode | Description |
|------|-------------|
| `off` | Single message |
| `partial` | Edit-as-we-go |
| `block` | Chunk into blocks |
| `progress` | Show progress indicators |

## Queue Modes

| Mode | Behavior |
|------|----------|
| `collect` | Batch rapid messages |
| `steer` | Interrupt + replace |
| `followup` | Queue after current |
| `queue` | Strict FIFO |
| `interrupt` | Cancel + replace |

## Sandbox Modes

| Mode | Description |
|------|-------------|
| `off` | No sandboxing |
| `non-main` | Sandbox non-main sessions |
| `all` | Sandbox all sessions |

### Sandbox Scope
| Scope | Container Strategy |
|-------|-------------------|
| `session` | Per-session container |
| `agent` | One container per agent |
| `shared` | Shared container |

## Heartbeat

Periodic agent runs:
- Default interval: 30m
- Runs full agent turn
- Can deliver to channel or run silently
- Per-agent configuration

## Compaction

Context reduction when approaching limits:
- `mode: "safeguard"` - Chunked summarization
- `memoryFlush` - Store memories before compaction

## Context Pruning

Old tool result removal:
- `mode: "cache-ttl"` - TTL-based pruning
- Soft-trim: Keep head + tail, insert `...`
- Hard-clear: Replace with placeholder

## Model Aliases

Built-in aliases (when model in catalog):
| Alias | Model |
|-------|-------|
| `opus` | `anthropic/claude-opus-4-6` |
| `sonnet` | `anthropic/claude-sonnet-4-5` |
| `gpt` | `openai/gpt-5.2` |
| `gpt-mini` | `openai/gpt-5-mini` |
| `gemini` | `google/gemini-3-pro-preview` |
| `gemini-flash` | `google/gemini-3-flash-preview` |

## File Locations

| File | Location |
|------|----------|
| Main config | `~/.openclaw/openclaw.json` |
| State | `~/.openclaw/state/` |
| Workspace | `~/.openclaw/workspace/` |
| Sessions | `~/.openclaw/agents/{agentId}/sessions/` |
| Credentials | `~/.openclaw/credentials/` |
| Extensions | `~/.openclaw/extensions/` |
| Logs | `/tmp/openclaw/` |

## Environment Variables

| Variable | Purpose |
|----------|---------|
| `OPENCLAW_CONFIG_PATH` | Override config path |
| `OPENCLAW_STATE_DIR` | Override state directory |
| `OPENCLAW_AGENT_DIR` | Override agent directory |
| `OPENCLAW_GATEWAY_PORT` | Gateway port |
| `OPENCLAW_GATEWAY_TOKEN` | Gateway auth token |
| `OPENCLAW_GATEWAY_PASSWORD` | Gateway auth password |
| `OPENCLAW_SKIP_GMAIL_WATCHER` | Disable Gmail watcher |
| `OPENCLAW_SKIP_CANVAS_HOST` | Disable Canvas host |
| `BRAVE_API_KEY` | Brave Search API |

## Chat Commands

| Command | Description |
|---------|-------------|
| `/status` | Quick diagnostics |
| `/config` | Read/write config |
| `/debug` | Runtime overrides |
| `/restart` | Restart gateway |
| `/model` | Switch model |
| `/thinking` | Set thinking level |
| `/verbose` | Toggle verbose mode |
| `/elevated` | Toggle elevated mode |
| `/new` | Reset session |
| `/tts` | Toggle text-to-speech |

## Security Checklist

- [ ] Set gateway auth (token/password)
- [ ] Configure channel allowlists
- [ ] Enable pairing for unknown senders
- [ ] Set up DM policies appropriately
- [ ] Configure group mention requirements
- [ ] Review tool policies
- [ ] Enable sandboxing if needed
- [ ] Set up elevated mode allowlists
- [ ] Run `openclaw security audit`

## Common Patterns

### Development Mode
```bash
openclaw gateway --dev              # Uses ~/.openclaw-dev + port 19001
```

### Profile Isolation
```bash
openclaw --profile work gateway     # Uses ~/.openclaw-work
```

### Remote Gateway
```json5
{
  gateway: {
    mode: "remote",
    remote: {
      url: "ws://gateway.tailnet:18789",
      token: "your-token"
    }
  }
}
```

### Tailscale Access
```json5
{
  gateway: {
    bind: "tailnet",
    tailscale: {
      mode: "serve"               // or "funnel" for public
    }
  }
}
```
