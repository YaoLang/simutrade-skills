# Simutrade Skills

Skills that teach an AI agent how to use [Simutrade](https://simutrade.ai) MCP for investment workflows: idea capture, trading, risk control, and daily review. Use with Claude (Claude.ai, Claude Code, or API) once the Simutrade MCP server is connected.

## Prerequisites

- Simutrade MCP server configured and connected (e.g. in Claude Code or Claude.ai).
- API key in MCP config, for example:

```json
{
  "mcpServers": {
    "simutrade": {
      "url": "https://api.simutrade.ai/v1/mcp/sse",
      "headers": {
        "Authorization": "Bearer YOUR_API_KEY"
      }
    }
  }
}
```

## Skills in this repo

| Skill | Purpose | Trigger phrases (examples) |
|-------|----------|-----------------------------|
| **simutrade-idea-capture** | Record and structure investment ideas and research notes in Investment Thinking | "记录投资想法", "写研报/笔记", "整理今天/本周思路", "复盘归纳" |
| **simutrade-trade-workflow** | Place orders, cancel orders, query quotes, portfolio, positions, orders | "下单", "买入/卖出", "撤单", "查持仓", "查委托", "报价" |
| **simutrade-risk-control** | Create and inspect risk rules (stop-loss, take-profit, trailing-stop, position limits) | "设止损/止盈", "风控", "仓位限制", "检查风控" |
| **simutrade-daily-review** | Pre-market plan and post-market recap | "盘前计划", "今日计划", "盘后复盘", "今日总结", "复盘" |

## Installation

### Claude.ai

1. Download this repo (clone or ZIP).
2. In Claude.ai go to **Settings → Capabilities → Skills**.
3. Click **Upload skill** and select **one** skill folder (e.g. `simutrade-idea-capture`). Repeat for each skill you want.
4. Enable the skills you uploaded.
5. Ensure the Simutrade MCP server is connected (e.g. in **Settings → Extensions** or your MCP client).

### Claude Code

1. Clone or copy this repo.
2. Copy the skill folders you want into your Claude Code skills directory (see Claude Code docs for the path).
3. Enable the skills and ensure Simutrade MCP is configured in your Claude Code MCP config.

### Using skills together

- **Idea → Trade → Risk → Review**: Use idea-capture to record theses, trade-workflow to execute and risk-control to set rules; use daily-review for 盘前/盘后.
- All skills assume **investment thinking is read before any trade or risk decision**; trade-workflow and risk-control state this explicitly.

## Skill layout (per skill)

- `SKILL.md` — Main instructions and when to use the skill (do not add README.md inside the skill folder).
- `references/` — Templates and checklists (e.g. idea-template.md, order-checklist.md, risk-rules.md, review-template.md).

## References

- [Anthropic: Building Skills for Claude](https://resources.anthropic.com/hubfs/The-Complete-Guide-to-Building-Skill-for-Claude.pdf)
- Simutrade API: [https://docs.simutrade.ai](https://docs.simutrade.ai)
# simutrade-skills
