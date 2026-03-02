# OpenClaw 自文档技能

<div align="center">

🦞 **终极 OpenClaw 参考技能 - 让 AI 秒变专家**

[![OpenClaw 版本](https://img.shields.io/badge/OpenClaw-v2026.2.26-orange)](https://github.com/openclaw/openclaw)
[![技能版本](https://img.shields.io/badge/技能-1.0.0-blue)](./references/VERSION.md)
[![License](https://img.shields.io/badge/许可-MIT-green.svg)](LICENSE)

[English](./README.md) | **中文**

</div>

## 📖 简介

**OpenClaw Self-Docs** 是为 [OpenClaw](https://github.com/openclaw/openclaw) 打造的全能自文档技能。OpenClaw 是多频道 AI 网关，而这个技能将所有核心知识直接嵌入 AI 智能体的上下文，让它瞬间成为 OpenClaw 专家——无需查文档，无需搜索。

### 为什么需要这个技能？

- ⚡ **即时响应** - 告别"让我查一下文档"
- 🎯 **精确语法** - 命令参数、示例代码一应俱全
- 📚 **全面覆盖** - CLI、工具、配置、最佳实践
- 🔒 **离线可用** - 无需网络连接
- 🔄 **版本匹配** - 与 OpenClaw 版本同步更新

## ✨ 功能亮点

### 📋 CLI 速查
50+ 个 `openclaw` 命令完整文档：
```bash
# 网关管理
openclaw gateway status --deep
openclaw gateway install --port 18789

# 频道操作
openclaw channels add --channel telegram --token $TOKEN
openclaw channels status --probe

# 代理管理
openclaw agents add work --workspace ~/.openclaw/workspace-work
openclaw sessions --active 60
```

### 🛠️ 工具文档
所有 OpenClaw 工具的详细参数：
- `browser` - 基于 Playwright 的网页自动化
- `canvas` - Node Canvas 控制
- `nodes` - 配对设备管理
- `message` - 跨频道消息
- `cron` - 定时任务
- `sessions_spawn` - 子代理编排
- 更多...

### ⚙️ 配置指南
完整的 `~/.openclaw/openclaw.json` 配置手册：
- 网关设置（端口、认证、Tailscale）
- 多频道配置（WhatsApp、Telegram、Discord、Slack 等）
- 多代理路由
- 工具策略与沙盒
- 会话管理

### 📱 频道指南
各平台专属配置教程：
- WhatsApp（Baileys Web）
- Telegram（Bot API）
- Discord（Bot + Socket Mode）
- Slack（Socket/HTTP 模式）
- Signal
- iMessage（macOS）
- Google Chat
- Mattermost（插件）

## 🚀 安装

### 前置要求
- OpenClaw v2026.2.26 或兼容版本
- 技能系统已启用

### 方法 1：通过 ClawHub（推荐）
```bash
npx clawhub install openclaw-selfdocs
```

### 方法 2：手动安装
```bash
# 下载技能文件
curl -L -o openclaw-selfdocs.skill https://github.com/arleyGuoLei/openclaw-selfdocs/releases/latest/download/openclaw-selfdocs.skill

# 安装到 OpenClaw
mkdir -p ~/.openclaw/skills
cp openclaw-selfdocs.skill ~/.openclaw/skills/

# 验证安装
openclaw skills info openclaw-selfdocs
```

### 方法 3：从源码安装
```bash
# 克隆仓库
git clone https://github.com/arleyGuoLei/openclaw-selfdocs.git
cd openclaw-selfdocs

# 打包技能
python3 /path/to/skill-creator/scripts/package_skill.py .

# 安装
cp openclaw-selfdocs.skill ~/.openclaw/skills/
```

## 📚 使用方式

安装完成后，技能会在以下场景自动激活：

- 询问 OpenClaw CLI 命令
- 查询配置选项
- 了解工具使用方法
- 设置频道
- 排查问题

### 示例对话

> "怎么检查网关状态？"

技能会提供：
```bash
openclaw gateway status [--deep] [--json] [--url <url>] [--token <token>]
```

> "Telegram 机器人的配置怎么写？"

技能会展示：
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

## 📁 文档结构

```
openclaw-selfdocs/
├── SKILL.md                    # 技能主文件（入口）
├── README.md                   # 英文文档
├── README.zh-CN.md            # 📘 中文文档（本文件）
├── LICENSE                     # MIT 许可证
├── references/                 # 英文参考资料
│   ├── VERSION.md             # 版本历史
│   ├── cli.md                 # CLI 命令参考
│   ├── tools.md               # 工具参数
│   ├── configuration.md       # 配置参考
│   ├── channels.md            # 频道指南
│   └── concepts.md            # 核心概念
└── references/zh-CN/          # 📘 中文参考资料
    ├── VERSION.md
    ├── cli.md
    ├── tools.md
    ├── configuration.md
    ├── channels.md
    └── concepts.md
```

### 语言选择

| 文档 | 语言 | 路径 |
|------|------|------|
| 主页 | English | [README.md](./README.md) |
| 主页 | 中文 | [README.zh-CN.md](./README.zh-CN.md) |
| CLI 参考 | English | [references/cli.md](./references/cli.md) |
| CLI 参考 | 中文 | [references/zh-CN/cli.md](./references/zh-CN/cli.md) |
| 工具文档 | English | [references/tools.md](./references/tools.md) |
| 工具文档 | 中文 | [references/zh-CN/tools.md](./references/zh-CN/tools.md) |
| 配置参考 | English | [references/configuration.md](./references/configuration.md) |
| 配置参考 | 中文 | [references/zh-CN/configuration.md](./references/zh-CN/configuration.md) |

## 🔧 开发者指南

### 从源码构建

```bash
# 克隆仓库
git clone https://github.com/arleyGuoLei/openclaw-selfdocs.git
cd openclaw-selfdocs

# 找到 skill-creator 脚本
SKILL_CREATOR="$(npm root -g)/openclaw/skills/skill-creator/scripts"

# 打包
python3 "$SKILL_CREATOR/package_skill.py" .

# 生成的 .skill 文件就在当前目录
```

### 项目规范

本技能遵循 [OpenClaw 技能规范](https://docs.openclaw.ai/tools/creating-skills)：

- **SKILL.md** - 必需。YAML 前置信息 + 使用说明
- **references/** - 可选。按需加载的详细文档
- **无冗余文件** - 技能包内不含 README（README 仅用于仓库展示）

### 更新内容

1. 编辑 `references/` 或 `references/zh-CN/` 下的文件
2. 更新 `references/VERSION.md`
3. 测试技能正常加载
4. 提交 Pull Request

详见 [references/VERSION.md](./references/VERSION.md) 的完整更新清单。

## 📝 版本兼容性

| 技能版本 | OpenClaw 版本 | 状态 |
|----------|---------------|------|
| 1.0.0 | v2026.2.26 | ✅ 当前版本 |

本技能设计与 **补丁版本前向兼容**。OpenClaw 发布新的 minor/major 版本时，请检查技能更新。

## 🤝 贡献

欢迎贡献！请遵循以下流程：

1. Fork 本仓库
2. 创建功能分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改
4. 充分测试
5. 推送到分支
6. 提交 Pull Request

### 贡献方向

- 🌍 改进翻译质量
- 📖 补充更多示例
- 🔧 修正错误
- ✨ 文档新功能

## 📜 许可

MIT 许可证 - 详见 [LICENSE](LICENSE) 文件

## 🙏 致谢

- [OpenClaw](https://github.com/openclaw/openclaw) - 这个技能所文档化的优秀 AI 网关
- [OpenClaw 社区](https://discord.com/invite/clawd) - 帮助与灵感来源
- [ClawHub](https://clawhub.com) - 技能注册中心与生态

---

<div align="center">

**[⬆ 回到顶部](#openclaw-自文档技能)**

为 OpenClaw 社区用 🦞 打造

</div>
