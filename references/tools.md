# Tools Reference (v2026.2.26)

Complete reference for OpenClaw agent tools.

## Tool Profiles

Profiles set base allowlists before `tools.allow`/`tools.deny`:

| Profile | Included Tools |
|---------|---------------|
| `minimal` | `session_status` only |
| `coding` | `group:fs`, `group:runtime`, `group:sessions`, `group:memory`, `image` |
| `messaging` | `group:messaging`, `sessions_list`, `sessions_history`, `sessions_send`, `session_status` |
| `full` | No restrictions |

## Tool Groups

| Group | Tools |
|-------|-------|
| `group:runtime` | `exec`, `process` |
| `group:fs` | `read`, `write`, `edit`, `apply_patch` |
| `group:sessions` | `sessions_list`, `sessions_history`, `sessions_send`, `sessions_spawn`, `session_status` |
| `group:memory` | `memory_search`, `memory_get` |
| `group:web` | `web_search`, `web_fetch` |
| `group:ui` | `browser`, `canvas` |
| `group:automation` | `cron`, `gateway` |
| `group:messaging` | `message` |
| `group:nodes` | `nodes` |
| `group:openclaw` | All built-in tools (excludes plugins) |

## Core Tools

### `exec`
Run shell commands.

**Parameters:**
- `command` (required): Shell command to run
- `yieldMs`: Auto-background after timeout (default: 10000)
- `background`: Immediate background execution
- `timeout`: Kill after seconds (default: 1800)
- `elevated`: Run on host (requires `tools.elevated.enabled`)
- `host`: `sandbox` | `gateway` | `node`
- `security`: `deny` | `allowlist` | `full`
- `ask`: `off` | `on-miss` | `always`
- `node`: Node id/name for `host=node`
- `pty`: Use TTY (for interactive UIs)

**Returns:** `status: "running"` with `sessionId` when backgrounded

### `process`
Manage background exec sessions.

**Actions:**
- `list`: List all sessions
- `poll`: Get new output and exit status
- `log`: Get session log with offset/limit
- `write`: Send input to session
- `kill`: Terminate session
- `clear`: Clear completed sessions
- `remove`: Remove session entry

### `web_search`
Search web using Brave Search API.

**Parameters:**
- `query` (required): Search query
- `count`: 1-10 results (default from config)
- `country`: 2-letter country code (default: US)
- `search_lang`: 2-letter language code
- `freshness`: `pd` | `pw` | `pm` | `py` or date range

**Config:** `tools.web.search.enabled`, requires `BRAVE_API_KEY`

### `web_fetch`
Fetch URL content as markdown/text.

**Parameters:**
- `url` (required): HTTP(S) URL
- `extractMode`: `markdown` | `text`
- `maxChars`: Truncate limit

**Config:** `tools.web.fetch.enabled`, `tools.web.fetch.maxCharsCap`

### `browser`
Control dedicated OpenClaw-managed browser.

**Management Actions:**
- `status`: Check browser status
- `start`: Start browser
- `stop`: Stop browser
- `tabs`: List open tabs
- `open`: Open new tab
- `focus`: Focus tab by targetId
- `close`: Close tab

**Profile Actions:**
- `profiles`: List all profiles
- `create-profile`: Create new profile
- `delete-profile`: Remove profile
- `reset-profile`: Kill orphan process

**Inspect Actions:**
- `snapshot`: Capture page structure (`--format aria|ai`)
- `screenshot`: Capture screenshot (`--full-page`, `--type png|jpeg`)

**Action Actions:**
- `act`: Perform UI action (click, type, press, hover, drag, select, fill, resize, wait, evaluate)
- `navigate`: Go to URL
- `console`: Get console logs
- `pdf`: Generate PDF
- `upload`: Upload files
- `dialog`: Handle dialogs (accept/dismiss)

**Common Parameters:**
- `profile`: Browser profile name (default: `browser.defaultProfile`)
- `target`: `sandbox` | `host` | `node`
- `node`: Specific node id/name
- `ref`: Element reference from snapshot

### `canvas`
Drive node Canvas (present, eval, snapshot, A2UI).

**Actions:**
- `present`: Show content
- `hide`: Hide canvas
- `navigate`: Navigate to URL
- `eval`: Evaluate JavaScript
- `snapshot`: Capture screenshot
- `a2ui_push`: Push A2UI content
- `a2ui_reset`: Reset A2UI

**Parameters:**
- `url`: URL or path for present/navigate
- `javaScript`: JS code for eval
- `target`: Target for present
- `x`, `y`, `width`, `height`: Position/dimensions

### `nodes`
Discover and control paired nodes.

**Discovery:**
- `status`: List connected nodes
- `describe`: Get node details
- `pending`: List pending pairings
- `approve`: Approve pairing request
- `reject`: Reject pairing request

**Media:**
- `camera_snap`: Take photo (`--facing front|back|both`)
- `camera_clip`: Record video (`--duration <ms|10s|1m>`)
- `screen_record`: Record screen

**Actions:**
- `notify`: Send notification (`--title`, `--body`, `--sound`, `--priority`)
- `run`: Run command (`--cwd`, `--env`, `--command-timeout-ms`)
- `location_get`: Get location (`--accuracy coarse|balanced|precise`)

### `message`
Send messages across channels.

**Actions:**
- `send`: Send text/media
- `poll`: Create poll
- `react`: Add reaction
- `reactions`: List reactions
- `read`: Read message
- `edit`: Edit message
- `delete`: Delete message
- `pin`/`unpin`: Pin management
- `thread-create`/`thread-list`/`thread-reply`: Thread operations

**Parameters:**
- `target`: Recipient/channel
- `message`: Message text
- `channel`: Channel name override
- `media`: Media URL/path

### `cron`
Manage Gateway cron jobs.

**Actions:**
- `status`: Show cron status
- `list`: List jobs
- `add`: Create job (requires `--name`, `--at`/`--every`/`--cron`, payload)
- `update`: Patch job fields
- `remove`: Delete job
- `run`: Trigger job now
- `runs`: Show run history

### `gateway`
Restart or update Gateway.

**Actions:**
- `restart`: In-place restart (`--delay-ms`)
- `config.get`: Get config
- `config.apply`: Apply config changes
- `config.patch`: Merge partial update
- `update.run`: Run update + restart

### Session Tools

#### `sessions_list`
List sessions.
- `kinds`: Filter by kind
- `limit`: Max results
- `activeMinutes`: Activity filter
- `messageLimit`: Include last N messages

#### `sessions_history`
Get session transcript.
- `sessionKey` (required): Session to inspect
- `limit`: Message limit
- `includeTools`: Include tool messages

#### `sessions_send`
Send message to another session.
- `sessionKey` (required): Target session
- `message` (required): Message to send
- `timeoutSeconds`: Wait for completion (0 = fire-and-forget)

#### `sessions_spawn`
Spawn sub-agent.
- `task` (required): Task description
- `runtime`: `subagent` | `acp`
- `mode`: `run` | `session`
- `thread`: Thread-bound (requires `mode: session`)
- `agentId`: Target agent
- `model`: Override model
- `thinking`: Thinking level
- `runTimeoutSeconds`: Timeout (0 = no timeout)
- `cleanup`: `delete` | `keep`

#### `session_status`
Show session status.
- `sessionKey`: Target session (default: current)
- `model`: Set model override (`default` to clear)

### `image`
Analyze image with vision model.

**Parameters:**
- `image`: Path or URL
- `prompt`: Custom prompt
- `model`: Override model
- `maxBytesMb`: Size limit

**Requires:** `agents.defaults.imageModel` configured

### `agents_list`
List spawnable agent ids.
- Result restricted by `agents.list[].subagents.allowAgents`
- `["*"]` = allow any

### `memory_search` / `memory_get`
Search and read MEMORY.md files.

## Tool Configuration

### Global Tool Policy
```json5
{
  tools: {
    profile: "coding",
    allow: ["browser"],
    deny: ["canvas"],
  }
}
```

### Per-Agent Override
```json5
{
  agents: {
    list: [{
      id: "support",
      tools: {
        profile: "messaging",
        allow: ["slack"]
      }
    }]
  }
}
```

### Per-Provider Restriction
```json5
{
  tools: {
    profile: "coding",
    byProvider: {
      "google-antigravity": { profile: "minimal" }
    }
  }
}
```

### Elevated Mode
```json5
{
  tools: {
    elevated: {
      enabled: true,
      allowFrom: {
        whatsapp: ["+15555550123"]
      }
    }
  }
}
```
