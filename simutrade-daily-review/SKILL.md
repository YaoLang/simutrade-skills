---
name: simutrade-daily-review
description: Pre-market plan and post-market review using Simutrade MCP. Use when the user says "盘前计划", "今日计划", "盘后复盘", "今日总结", "复盘". Coordinates investment thinking, positions, orders, and risk rules into a structured plan or summary.
compatibility: Simutrade MCP must be connected.
metadata:
  mcp-server: simutrade
  version: 1.0.0
---

# Simutrade Daily Review

## Tool order (avoid duplicate calls)

1. **simutrade_get_investment_thinking** — Today’s ideas and planned symbols.
2. **simutrade_get_portfolio** or **simutrade_list_positions** — Current holdings and summary.
3. **simutrade_list_orders** — Recent or today’s orders (e.g. status=filled for executions).
4. **simutrade_list_risk_control_rules** — Active risk rules.
5. **Optional**: **simutrade_get_market_history** — K-line data for technical context (interval: 1m, 5m, 15m, 1h, 1d; limit default 30).
6. **Post-only**: **simutrade_update_investment_thinking** — Write back复盘结论 if the user wants it saved.

## Pre-market (盘前)

1. Call `simutrade_get_investment_thinking` → today’s focus and planned symbols.
2. Call `simutrade_list_positions` and `simutrade_list_risk_control_rules` → holdings and risk snapshot.
3. Produce the plan using [references/review-template.md](references/review-template.md): 今日关注、计划操作、风控要点. Reuse data from step 1–2; no extra MCP calls for the same data.

## Post-market (盘后)

1. Call `simutrade_list_orders` (e.g. status=filled) and `simutrade_list_positions` to see what changed.
2. Call `simutrade_get_investment_thinking` to compare execution vs. plan.
3. Produce the summary using the template: 执行情况、思路验证、明日关注.
4. **Optional**: If the user wants to save复盘结论, merge a short recap into the current content and call `simutrade_update_investment_thinking`.

## When to consult the user

- User says "帮我做盘前计划" but investment thinking is empty and there’s no other context → **Optional**: "是否先补充今日关注标的或操作思路？我可以先拉持仓与风控，您再说要关注什么。" to avoid a very generic plan.

## When to use information outside MCP

- For "why did it move" or "tomorrow’s catalysts": if the environment has Web Search, you may look up news or macro/events. Otherwise use only `simutrade_get_market_history` for technical context and ask the user to add fundamental/event context if needed.

## Troubleshooting

- If one of the MCP calls fails, still output the sections you can fill from other calls and clearly note what’s missing and suggest retrying (e.g. "持仓拉取失败，请稍后重试或检查 MCP 连接"). Pass data between steps (e.g. symbol list, positions) to avoid redundant calls.

## Additional resources

- Plan and summary structure: [references/review-template.md](references/review-template.md)
