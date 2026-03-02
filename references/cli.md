# CLI Reference (v2026.2.26)

Complete reference for `openclaw` CLI commands.

## Global Flags
- `--dev`: Isolate state under `~/.openclaw-dev`, shift ports
- `--profile <name>`: Isolate state under `~/.openclaw-<name>`
- `--no-color`: Disable ANSI colors
- `--update`: Shorthand for `openclaw update`
- `-V, --version, -v`: Print version

## Command Reference

### Setup & Onboarding
```bash
openclaw setup [--workspace <dir>] [--wizard] [--non-interactive] [--mode local|remote]
openclaw onboard [--install-daemon] [--mode local|remote] [--flow quickstart|advanced]
openclaw configure                    # Interactive config wizard
```

### Gateway Management
```bash
openclaw gateway status [--deep] [--json] [--url <url>] [--token <token>]
openclaw gateway start
openclaw gateway stop
openclaw gateway restart
openclaw gateway install [--port <port>] [--runtime node|bun] [--token <token>]
openclaw gateway uninstall
openclaw gateway run [--port <port>] [--bind loopback|lan|tailnet|auto|custom]
                       [--token <token>] [--auth token|password] [--tailscale off|serve|funnel]
```

### Channel Management
```bash
openclaw channels list [--json] [--no-usage]
openclaw channels status [--probe]
openclaw channels logs [--channel <name|all>] [--lines <n>] [--json]
openclaw channels add --channel <name> [--account <id>] [--name <label>]
openclaw channels remove --channel <name> [--account <id>] [--delete]
openclaw channels login [--channel whatsapp|web] [--account <id>] [--verbose]
openclaw channels logout --channel <name> [--account <id>]
```

Supported channels: `whatsapp`, `telegram`, `discord`, `googlechat`, `slack`, `mattermost`, `signal`, `imessage`, `msteams`

### Configuration
```bash
openclaw config get <path>            # Dot/bracket path, e.g., "gateway.port"
openclaw config set <path> <value>    # JSON5 or raw string
openclaw config unset <path>
```

### Agent & Session Management
```bash
openclaw status [--deep] [--usage] [--json] [--all]
openclaw health [--json] [--timeout <ms>]
openclaw sessions [--json] [--active <minutes>] [--store <path>]
openclaw agent --message <text> [--to <dest>] [--session-id <id>] [--local] [--deliver]
openclaw agents list [--json] [--bindings]
openclaw agents add [name] [--workspace <dir>] [--model <id>] [--bind <channel[:accountId]>]
openclaw agents delete <id> [--force]
```

### Model Management
```bash
openclaw models list [--all] [--local] [--provider <name>] [--json]
openclaw models status [--json] [--check] [--probe]
openclaw models set <model>           # Set primary model
openclaw models set-image <model>     # Set image model
openclaw models aliases list|add|remove
openclaw models fallbacks list|add|remove|clear
openclaw models scan [--set-default] [--yes]
openclaw models auth add|setup-token|paste-token
```

### Browser Control
```bash
openclaw browser status [--browser-profile <name>]
openclaw browser start [--browser-profile <name>]
openclaw browser stop [--browser-profile <name>]
openclaw browser tabs
openclaw browser open <url> [--browser-profile <name>]
openclaw browser focus <targetId>
openclaw browser close [targetId]
openclaw browser snapshot [--format aria|ai] [--target-id <id>]
openclaw browser screenshot [targetId] [--full-page] [--type png|jpeg]
openclaw browser navigate <url> [--target-id <id>]
openclaw browser click <ref> [--double] [--button left|right|middle]
openclaw browser type <ref> <text> [--submit] [--slowly]
openclaw browser press <key>
openclaw browser profiles
openclaw browser create-profile --name <name> [--color <hex>] [--cdp-url <url>]
openclaw browser delete-profile --name <name>
```

### Node Management
```bash
openclaw nodes status [--connected] [--last-connected <duration>]
openclaw nodes list [--connected] [--last-connected <duration>]
openclaw nodes describe --node <id|name|ip>
openclaw nodes pending
openclaw nodes approve <requestId>
openclaw nodes reject <requestId>
openclaw nodes camera list --node <id>
openclaw nodes camera snap --node <id> [--facing front|back|both]
openclaw nodes screen record --node <id> [--duration <ms|10s|1m>]
openclaw nodes notify --node <id> --title <text> --body <text>
openclaw nodes run --node <id> <command...>
```

### Skills & Plugins
```bash
openclaw skills list [--eligible] [--json] [-v]
openclaw skills info <name>
openclaw skills check
openclaw plugins list [--json]
openclaw plugins info <id>
openclaw plugins install <path|.tgz|npm-spec>
openclaw plugins enable|disable <id>
openclaw plugins doctor
```

### Cron Jobs
```bash
openclaw cron status [--json]
openclaw cron list [--all] [--json]
openclaw cron add --name <name> (--at <time> | --every <duration> | --cron <expr>)
                  (--system-event <text> | --message <text>)
openclaw cron edit <id>
openclaw cron rm <id>
openclaw cron enable|disable <id>
openclaw cron run <id> [--force]
openclaw cron runs --id <id> [--limit <n>]
```

### Memory & Logs
```bash
openclaw memory status
openclaw memory index                   # Reindex memory files
openclaw memory search "<query>"        # Semantic search
openclaw logs [--follow] [--limit <n>] [--plain] [--json]
```

### Security & Maintenance
```bash
openclaw security audit [--deep] [--fix]
openclaw secrets reload
openclaw secrets audit
openclaw secrets configure
openclaw doctor [--deep] [--fix] [--yes] [--non-interactive]
openclaw reset [--scope config|config+creds+sessions|full] [--yes]
openclaw uninstall [--service] [--state] [--workspace] [--all] [--yes]
openclaw update
```

### Messaging
```bash
openclaw message send --target <dest> --message <text> [--channel <name>]
openclaw message poll --channel <name> --target <dest> --poll-question <q> --poll-option <o1> --poll-option <o2>
openclaw message react --channel <name> --message-id <id> --emoji <emoji>
```

### Utilities
```bash
openclaw tui                          # Terminal UI
openclaw dashboard                    # Open browser dashboard
openclaw docs [query...]              # Search docs
openclaw completion <shell>           # Shell completion
openclaw dns setup [--apply]          # DNS discovery setup
```

## Multi-Instance Flags
Use these for isolated gateway instances:
- `OPENCLAW_CONFIG_PATH=~/.openclaw/a.json`
- `OPENCLAW_STATE_DIR=~/.openclaw-a`
- `--dev` → uses `~/.openclaw-dev` + port `19001`
- `--profile <name>` → uses `~/.openclaw-<name>`
