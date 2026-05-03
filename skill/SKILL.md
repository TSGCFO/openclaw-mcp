---
name: openclaw-mcp
description: Comprehensive OpenClaw 🦞 expertise built from the official docs at docs.openclaw.ai. OpenClaw is a self-hosted gateway that connects chat apps (Discord, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo, WebChat, and more) to AI coding agents like Pi/Codex/Claude. Use when installing, operating, or scripting OpenClaw — Gateway daemon, agents/providers/runtimes, channels, tools/plugins, MCP, sessions, secrets/SecretRef, hooks, automation, OpenAI-compatible endpoints, troubleshooting (`openclaw doctor`/`logs`), and the full `openclaw` CLI.
---

# Openclaw-Mcp Skill

Comprehensive OpenClaw 🦞 expertise built from the official docs at docs.openclaw.ai. OpenClaw is a self-hosted gateway that connects chat apps (Discord, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo, WebChat, and more) to AI coding agents like Pi/Codex/Claude. Use when installing, operating, or scripting OpenClaw — Gateway daemon, agents/providers/runtimes, channels, tools/plugins, MCP, sessions, secrets/SecretRef, hooks, automation, OpenAI-compatible endpoints, troubleshooting (`openclaw doctor`/`logs`), and the full `openclaw` CLI.

## When to Use This Skill

Use this skill when you need to:
- understand openclaw-mcp features, APIs, and workflows
- find concrete code examples before implementing or debugging
- navigate the official documentation quickly through categorized references

## Quick Reference

### High-Signal Examples

**Example 1** (bash):
```bash
openclaw onboard --anthropic-api-key "$ANTHROPIC_API_KEY"
```

**Example 2** (bash):
```bash
PLUGIN_SRC=./path/to/local/zalouser-plugin
openclaw plugins install "$PLUGIN_SRC"
cd "$PLUGIN_SRC" && pnpm install
```

**Example 3** (bash):
```bash
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice deepseek-api-key \
  --deepseek-api-key "$DEEPSEEK_API_KEY" \
  --skip-health \
  --accept-risk
```

**Example 4** (bash):
```bash
openclaw onboard --mistral-api-key "$MISTRAL_API_KEY"
```

**Example 5** (bash):
```bash
openclaw onboard --opencode-go-api-key "$OPENCODE_API_KEY"
```

### Key Usage Notes

**Pattern 1:** Documentation IndexFetch the complete documentation index at: https://docs.openclaw.ai/llms.txtUse this file to discover all available pages before...

```
npm install -g openclaw@latest
```

**Pattern 2:** Documentation IndexFetch the complete documentation index at: https://docs.openclaw.ai/llms.txtUse this file to discover all available pages before...

```
{
  agents: {
    defaults: {
      musicGenerationModel: {
        primary: "google/lyria-3-clip-preview",
      },
    },
  },
}
```

**Pattern 3:** Documentation IndexFetch the complete documentation index at: https://docs.openclaw.ai/llms.txtUse this file to discover all available pages before...

```
openclaw pairing list telegram
openclaw pairing list --channel telegram --account work
openclaw pairing list telegram --json

openclaw pairing approve <code>
openclaw pairing approve telegram <code>
openclaw pairing approve --channel telegram --account work <code> --notify
```

**Pattern 4:** Documentation IndexFetch the complete documentation index at: https://docs.openclaw.ai/llms.txtUse this file to discover all available pages before...

```
openclaw gateway --port 18789
```

**Pattern 5:** Documentation IndexFetch the complete documentation index at: https://docs.openclaw.ai/llms.txtUse this file to discover all available pages before...

```
openclaw plugins install @openclaw/matrix
```

**Pattern 6:** Documentation IndexFetch the complete documentation index at: https://docs.openclaw.ai/llms.txtUse this file to discover all available pages before...

```
openclaw webhooks gmail setup --account you@example.com
openclaw webhooks gmail run
```

**Pattern 7:** Documentation IndexFetch the complete documentation index at: https://docs.openclaw.ai/llms.txtUse this file to discover all available pages before...

```
{
  models: {
    // Optional. Default: true. Requires a Gateway restart when changed.
    pricing: { enabled: false },
  },
}
```

**Pattern 8:** Documentation IndexFetch the complete documentation index at: https://docs.openclaw.ai/llms.txtUse this file to discover all available pages before...

```
agents.defaults.model.primary
```

## Reference Files

This skill includes comprehensive documentation in `references/`:

- **agents.md** - Agents documentation
- **automation.md** - Automation documentation
- **channels.md** - Channels documentation
- **cli.md** - Cli documentation
- **concepts.md** - Concepts documentation
- **gateway_ops.md** - Gateway Ops documentation
- **getting_started.md** - Getting Started documentation
- **help.md** - Help documentation
- **nodes.md** - Nodes documentation
- **other.md** - Other documentation
- **platforms.md** - Platforms documentation
- **providers_models.md** - Providers Models documentation
- **reference.md** - Reference documentation
- **tools_plugins.md** - Tools Plugins documentation
- **web_console.md** - Web Console documentation

Use `view` to read specific reference files when detailed information is needed.

## Working with This Skill

### Start Here
Start with the getting_started or tutorials reference files for foundational concepts.

### For Specific Features
Use the appropriate category reference file (api, guides, etc.) for detailed information.

### For Code Examples
Use the high-signal examples above first, then open the matching reference file for full context.

## Notes

- This skill was automatically generated from official documentation
- Reference files preserve the structure and examples from source docs
- Code examples include language detection for better syntax highlighting
- Quick reference entries are filtered to avoid low-signal placeholders and inline tokens

## Updating

To refresh this skill with updated documentation:
1. Re-run the scraper with the same configuration
2. The skill will be rebuilt with the latest information
