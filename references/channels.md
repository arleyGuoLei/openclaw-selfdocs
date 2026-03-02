# Channels Reference (v2026.2.26)

Channel-specific configuration and setup.

## WhatsApp

### Setup
```bash
openclaw channels login              # Interactive QR scan
```

### Configuration
```json5
{
  channels: {
    whatsapp: {
      dmPolicy: "pairing",
      allowFrom: ["+15555550123"],
      groups: {
        "*": { requireMention: true },
        "+15551234567": {           // Group ID (owner number)
          allowFrom: ["+15559876543"],
          requireMention: false
        }
      },
      textChunkLimit: 4000,
      chunkMode: "length",          // length | newline
      mediaMaxMb: 50,
      sendReadReceipts: true
    }
  }
}
```

### Notes
- Uses Baileys Web library
- Group IDs are owner phone numbers
- Requires phone connected to internet
- Multi-account via `accounts.{id}`

## Telegram

### Setup
```bash
# Get token from @BotFather
openclaw channels add --channel telegram --token "BOT_TOKEN"
```

### Configuration
```json5
{
  channels: {
    telegram: {
      enabled: true,
      botToken: "${TELEGRAM_BOT_TOKEN}",
      dmPolicy: "pairing",
      allowFrom: ["tg:123456789"],
      groups: {
        "*": { requireMention: true },
        "-1001234567890": {        // Channel/group ID
          topics: {
            "99": {                 // Topic ID
              requireMention: false,
              systemPrompt: "Stay on topic."
            }
          }
        }
      },
      customCommands: [
        { command: "backup", description: "Git backup" }
      ],
      historyLimit: 50,
      replyToMode: "first",         // off | first | all
      linkPreview: true,
      streaming: "partial",
      mediaMaxMb: 5
    }
  }
}
```

### Features
- Native commands support
- Topic support (supergroups)
- Streaming replies
- Custom command menu

## Discord

### Setup
```bash
# Create bot at https://discord.com/developers/applications
# Enable Message Content Intent
openclaw channels add --channel discord --token "BOT_TOKEN"
```

### Configuration
```json5
{
  channels: {
    discord: {
      enabled: true,
      token: "${DISCORD_BOT_TOKEN}",
      mediaMaxMb: 8,
      allowBots: false,
      dmPolicy: "pairing",
      allowFrom: ["1234567890"],
      guilds: {
        "123456789012345678": {
          slug: "my-server",
          requireMention: false,
          reactionNotifications: "own",
          users: ["9876543210"],
          channels: {
            general: { allow: true },
            help: {
              allow: true,
              requireMention: true,
              skills: ["docs"]
            }
          }
        }
      },
      historyLimit: 20,
      textChunkLimit: 2000,
      maxLinesPerMessage: 17,
      streaming: "off",
      threadBindings: {
        enabled: true,
        ttlHours: 24,
        spawnSubagentSessions: false
      },
      voice: {
        enabled: true,
        autoJoin: [{
          guildId: "1234567890",
          channelId: "2345678901"
        }],
        tts: {
          provider: "openai",
          openai: { voice: "alloy" }
        }
      }
    }
  }
}
```

### Features
- Guild and channel-specific config
- Thread bindings
- Voice channel support
- Component styling (v2 containers)
- Reaction notifications

## Slack

### Setup (Socket Mode)
```bash
# Create app at https://api.slack.com/apps
# Enable Socket Mode, add Bot Token Scopes
openclaw channels add --channel slack --token "xoxb-..." --app-token "xapp-..."
```

### Configuration
```json5
{
  channels: {
    slack: {
      enabled: true,
      botToken: "${SLACK_BOT_TOKEN}",
      appToken: "${SLACK_APP_TOKEN}",
      signingSecret: "${SLACK_SIGNING_SECRET}",  // For HTTP mode
      dmPolicy: "pairing",
      allowFrom: ["U123", "U456"],
      channels: {
        C123: { allow: true, requireMention: true },
        "#general": {
          allow: true,
          requireMention: true,
          users: ["U123"]
        }
      },
      historyLimit: 50,
      thread: {
        historyScope: "thread",    // thread | channel
        inheritParent: false
      },
      streaming: "partial",
      nativeStreaming: true,
      slashCommand: {
        enabled: true,
        name: "openclaw",
        ephemeral: true
      }
    }
  }
}
```

### Features
- Socket Mode (recommended) or HTTP mode
- Thread session isolation
- Slash commands
- Native streaming API

## Signal

### Setup
Requires Signal CLI or native integration.

### Configuration
```json5
{
  channels: {
    signal: {
      reactionNotifications: "own",
      reactionAllowlist: ["+15551234567"],
      historyLimit: 50
    }
  }
}
```

## iMessage

### Setup (macOS only)
```bash
# Requires Full Disk Access for Messages DB
openclaw channels add --channel imessage
```

### Configuration
```json5
{
  channels: {
    imessage: {
      enabled: true,
      cliPath: "imsg",              // or SSH wrapper script
      dbPath: "~/Library/Messages/chat.db",
      remoteHost: "user@gateway-host",  // For SSH relay
      dmPolicy: "pairing",
      allowFrom: ["+15555550123", "user@example.com"],
      historyLimit: 50,
      includeAttachments: false,
      attachmentRoots: ["/Users/*/Library/Messages/Attachments"],
      mediaMaxMb: 16,
      service: "auto",              // auto | imessage | sms
      region: "US"
    }
  }
}
```

### SSH Wrapper Example
```bash
#!/usr/bin/env bash
exec ssh -T gateway-host imsg "$@"
```

### Notes
- Uses `imsg` CLI for DB access
- Supports remote relay via SSH
- Chat IDs: Use `imsg chats --limit 20` to list

## Google Chat

### Setup
```bash
# Create service account, download JSON key
openclaw channels add --channel googlechat --service-account-file "/path/to/key.json"
```

### Configuration
```json5
{
  channels: {
    googlechat: {
      enabled: true,
      serviceAccountFile: "/path/to/service-account.json",
      audienceType: "app-url",
      audience: "https://gateway.example.com/googlechat",
      webhookPath: "/googlechat",
      botUser: "users/1234567890",
      dm: {
        enabled: true,
        policy: "pairing",
        allowFrom: ["users/1234567890"]
      },
      groups: {
        "spaces/AAAA": { allow: true, requireMention: true }
      },
      typingIndicator: "message",
      mediaMaxMb: 20
    }
  }
}
```

## Mattermost

### Setup
```bash
openclaw plugins install @openclaw/mattermost
```

### Configuration
```json5
{
  channels: {
    mattermost: {
      enabled: true,
      botToken: "mm-token",
      baseUrl: "https://chat.example.com",
      dmPolicy: "pairing",
      chatmode: "oncall",           // oncall | onmessage | onchar
      oncharPrefixes: [">", "!"],
      textChunkLimit: 4000
    }
  }
}
```

### Chat Modes
- `oncall`: Respond on @-mention (default)
- `onmessage`: Every message
- `onchar`: Messages starting with trigger prefix

## MS Teams

### Setup
Azure Bot Framework registration required.

### Configuration
```json5
{
  channels: {
    msteams: {
      enabled: true,
      // Bot Framework credentials
    }
  }
}
```

## Common Patterns

### Response Prefix Per Channel
```json5
{
  channels: {
    whatsapp: { responsePrefix: "🦞" },
    telegram: { responsePrefix: "auto" }  // Uses [{identity.name}]
  }
}
```

### Model Override Per Channel
```json5
{
  channels: {
    modelByChannel: {
      discord: {
        "1234567890": "anthropic/claude-opus-4-6"
      }
    }
  }
}
```

### Config Writes Control
```json5
{
  channels: {
    telegram: { configWrites: false },  // Block /config commands
    slack: { configWrites: false }
  }
}
```

## Delivery Targets

| Channel | DM Target | Channel Target |
|---------|-----------|----------------|
| WhatsApp | `+15555550123` | N/A |
| Telegram | `tg:123456789` | `-1001234567890` |
| Discord | `user:1234567890` | `channel:1234567890` |
| Slack | `user:U123` | `channel:C123` or `#general` |
| Google Chat | `users/123` | `spaces/AAAA` |
| iMessage | `+15555550123` or `chat_id:123` | N/A |

## Environment Variables

| Channel | Variable |
|---------|----------|
| Telegram | `TELEGRAM_BOT_TOKEN` |
| Discord | `DISCORD_BOT_TOKEN` |
| Slack | `SLACK_BOT_TOKEN`, `SLACK_APP_TOKEN` |
| Google Chat | `GOOGLE_CHAT_SERVICE_ACCOUNT_FILE` |
