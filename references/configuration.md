# Configuration Reference (v2026.2.26)

Key configuration sections for `~/.openclaw/openclaw.json`.

## Gateway

```json5
{
  gateway: {
    mode: "local",              // local | remote
    port: 18789,
    bind: "loopback",           // loopback | lan | tailnet | auto | custom
    auth: {
      mode: "token",            // none | token | password | trusted-proxy
      token: "your-token",
      // password: "your-password",
      allowTailscale: true,
      rateLimit: {
        maxAttempts: 10,
        windowMs: 60000,
        lockoutMs: 300000,
        exemptLoopback: true,
      }
    },
    tailscale: {
      mode: "off",              // off | serve | funnel
      resetOnExit: false,
    },
    controlUi: {
      enabled: true,
      basePath: "/openclaw",
      // allowedOrigins: ["https://control.example.com"],
    }
  }
}
```

## Channels

### WhatsApp
```json5
{
  channels: {
    whatsapp: {
      dmPolicy: "pairing",      // pairing | allowlist | open | disabled
      allowFrom: ["+15555550123"],
      groups: {
        "*": { requireMention: true }
      },
      textChunkLimit: 4000,
      mediaMaxMb: 50,
    }
  }
}
```

### Telegram
```json5
{
  channels: {
    telegram: {
      enabled: true,
      botToken: "your-bot-token",
      dmPolicy: "pairing",
      allowFrom: ["tg:123456789"],
      groups: {
        "*": { requireMention: true },
        "-1001234567890": {
          allowFrom: ["@admin"],
          topics: {
            "99": { requireMention: false }
          }
        }
      },
      streaming: "partial",       // off | partial | block | progress
    }
  }
}
```

### Discord
```json5
{
  channels: {
    discord: {
      enabled: true,
      token: "your-bot-token",
      dmPolicy: "pairing",
      allowFrom: ["1234567890"],
      guilds: {
        "123456789012345678": {
          slug: "server-name",
          requireMention: false,
          channels: {
            general: { allow: true },
            help: { requireMention: true }
          }
        }
      },
      streaming: "off",
      threadBindings: {
        enabled: true,
        ttlHours: 24,
      }
    }
  }
}
```

### Slack
```json5
{
  channels: {
    slack: {
      enabled: true,
      botToken: "xoxb-...",
      appToken: "xapp-...",       // For Socket Mode
      dmPolicy: "pairing",
      channels: {
        C123: { allow: true, requireMention: true }
      },
      streaming: "partial",
    }
  }
}
```

### Multi-Account Pattern
```json5
{
  channels: {
    telegram: {
      accounts: {
        default: {
          name: "Primary bot",
          botToken: "123456:ABC..."
        },
        alerts: {
          name: "Alerts bot",
          botToken: "987654:XYZ..."
        }
      }
    }
  }
}
```

## Agents

### Defaults
```json5
{
  agents: {
    defaults: {
      workspace: "~/.openclaw/workspace",
      model: {
        primary: "anthropic/claude-opus-4-6",
        fallbacks: ["minimax/MiniMax-M2.1"]
      },
      imageModel: {
        primary: "openrouter/qwen/qwen-2.5-vl-72b-instruct:free"
      },
      timeoutSeconds: 600,
      maxConcurrent: 3,
      thinkingDefault: "low",
      verboseDefault: "off",
      
      // Heartbeat
      heartbeat: {
        every: "30m",
        prompt: "Read HEARTBEAT.md if it exists..."
      },
      
      // Compaction
      compaction: {
        mode: "safeguard",
        reserveTokensFloor: 24000
      },
      
      // Sandbox
      sandbox: {
        mode: "non-main",         // off | non-main | all
        scope: "agent",           // session | agent | shared
        workspaceAccess: "none",  // none | ro | rw
      }
    }
  }
}
```

### Per-Agent Overrides
```json5
{
  agents: {
    list: [
      {
        id: "main",
        default: true,
        name: "Main Agent",
        workspace: "~/.openclaw/workspace",
        model: "anthropic/claude-opus-4-6",
        identity: {
          name: "Samantha",
          emoji: "🦥",
          avatar: "avatars/samantha.png"
        },
        groupChat: {
          mentionPatterns: ["@openclaw"]
        },
        tools: {
          profile: "coding",
          allow: ["browser"],
          deny: ["canvas"]
        },
        subagents: {
          allowAgents: ["*"]
        }
      }
    ]
  }
}
```

### Multi-Agent Routing
```json5
{
  agents: {
    list: [
      { id: "home", default: true, workspace: "~/.openclaw/workspace-home" },
      { id: "work", workspace: "~/.openclaw/workspace-work" }
    ]
  },
  bindings: [
    { agentId: "home", match: { channel: "whatsapp", accountId: "personal" } },
    { agentId: "work", match: { channel: "whatsapp", accountId: "biz" } }
  ]
}
```

## Tools

```json5
{
  tools: {
    // Base profile
    profile: "coding",            // minimal | coding | messaging | full
    
    // Allow/deny (applied after profile)
    allow: ["browser"],
    deny: ["canvas"],
    
    // Per-provider restrictions
    byProvider: {
      "google-antigravity": { profile: "minimal" }
    },
    
    // Elevated mode
    elevated: {
      enabled: true,
      allowFrom: {
        whatsapp: ["+15555550123"]
      }
    },
    
    // Loop detection
    loopDetection: {
      enabled: true,
      warningThreshold: 10,
      criticalThreshold: 20
    },
    
    // Web tools
    web: {
      search: {
        enabled: true,
        maxResults: 5,
        cacheTtlMinutes: 15
      },
      fetch: {
        enabled: true,
        maxChars: 50000
      }
    },
    
    // Session visibility
    sessions: {
      visibility: "tree"          // self | tree | agent | all
    },
    
    // Sub-agent defaults
    subagents: {
      model: "minimax/MiniMax-M2.1",
      runTimeoutSeconds: 900
    }
  }
}
```

## Session

```json5
{
  session: {
    scope: "per-sender",
    dmScope: "main",              // main | per-peer | per-channel-peer | per-account-channel-peer
    reset: {
      mode: "daily",              // daily | idle
      atHour: 4,
      idleMinutes: 60
    },
    resetTriggers: ["/new", "/reset"],
    threadBindings: {
      enabled: true,
      ttlHours: 24
    },
    maintenance: {
      mode: "warn",               // warn | enforce
      pruneAfter: "30d",
      maxEntries: 500
    }
  }
}
```

## Messages

```json5
{
  messages: {
    responsePrefix: "🦞",         // or "auto" for [{identity.name}]
    ackReaction: "👀",
    ackReactionScope: "group-mentions",
    removeAckAfterReply: false,
    queue: {
      mode: "collect",            // steer | followup | collect | queue | interrupt
      debounceMs: 1000,
      cap: 20
    },
    inbound: {
      debounceMs: 2000
    },
    tts: {
      auto: "always",             // off | always | inbound | tagged
      provider: "elevenlabs"
    }
  }
}
```

## Browser

```json5
{
  browser: {
    enabled: true,
    defaultProfile: "chrome",
    evaluateEnabled: true,
    ssrfPolicy: {
      dangerouslyAllowPrivateNetwork: true
    },
    profiles: {
      openclaw: { cdpPort: 18800, color: "#FF4500" },
      work: { cdpPort: 18801, color: "#0066CC" },
      remote: { cdpUrl: "http://10.0.0.42:9222" }
    }
  }
}
```

## Skills & Plugins

```json5
{
  skills: {
    allowBundled: ["gemini", "peekaboo"],
    load: {
      extraDirs: ["~/Projects/skills"]
    },
    entries: {
      peekaboo: { enabled: true }
    }
  },
  plugins: {
    enabled: true,
    allow: ["voice-call"],
    load: {
      paths: ["~/Projects/extensions"]
    }
  }
}
```

## Cron

```json5
{
  cron: {
    enabled: true,
    maxConcurrentRuns: 2,
    sessionRetention: "24h",
    runLog: {
      maxBytes: "2mb",
      keepLines: 2000
    }
  }
}
```

## Logging

```json5
{
  logging: {
    level: "info",
    file: "/tmp/openclaw/openclaw.log",
    consoleLevel: "info",
    consoleStyle: "pretty",       // pretty | compact | json
    redactSensitive: "tools"
  }
}
```

## Environment Variables

Config can reference env vars with `${VAR_NAME}`:
```json5
{
  gateway: {
    auth: { token: "${OPENCLAW_GATEWAY_TOKEN}" }
  },
  channels: {
    telegram: { botToken: "${TELEGRAM_BOT_TOKEN}" }
  }
}
```

## Config Includes

Split config across files:
```json5
{
  gateway: { port: 18789 },
  agents: { $include: "./agents.json5" },
  channels: { $include: ["./whatsapp.json5", "./telegram.json5"] }
}
```

Rules:
- Paths relative to including file
- Must stay within top-level config directory
- Max 10 nested levels
- Array files deep-merged in order
