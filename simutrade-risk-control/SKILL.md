---
name: simutrade-risk-control
description: Creates and inspects risk control rules (stop-loss, take-profit, trailing-stop, position limits) via Simutrade MCP. Use when the user says "set stop-loss/take-profit", "risk control", "position limit", "trailing stop", "check risk rules", or "risk execution history". Always read investment thinking before creating or changing rules.
compatibility: Simutrade MCP must be connected.
metadata:
  mcp-server: simutrade
  version: 1.0.0
---

# Simutrade Risk Control

## Required order of operations

Before **creating or updating** any risk rule: call `simutrade_get_investment_thinking` so the rule’s `reason` stays consistent with the account’s thinking.

## Tools

- **simutrade_get_investment_thinking** — Call before making risk decisions.
- **simutrade_create_risk_control_rule** — Create a rule. Types: stop_loss, take_profit, trailing_stop, position_limit. **You must provide `reason`.** Pass `config` (e.g. type: percentage, percentage: 5 for 5% stop).
- **simutrade_list_risk_control_rules** — List rules. Filter by symbol, rule_type, is_active, order_id.
- **simutrade_list_risk_control_executions** — List when rules were triggered. Filter by rule_id, status, limit.
- **simutrade_update_risk_control_rule** — Update an existing rule by rule_id.
- **simutrade_delete_risk_control_rule** — Permanently delete a rule by rule_id.

## Create/update flow

1. Call `simutrade_get_investment_thinking`.
2. Understand scope: per-symbol, per-order, or global. If the user said "设个止损" or "加个止盈" but **did not give symbol or level**, **ask**: "请确认：为哪只标的？止损/止盈比例（如 5%）还是固定价格？"
3. Confirm parameters (price or percentage, config shape) from user or context. See [references/risk-rules.md](references/risk-rules.md) for config examples.
4. Call `simutrade_create_risk_control_rule` or `simutrade_update_risk_control_rule` with `reason` and config.
5. Call `simutrade_list_risk_control_rules` to confirm the new or updated rule.

## Inspect flow

- **Current rules**: `simutrade_list_risk_control_rules` — present as a short list; note any conflicts or gaps.
- **Trigger history**: `simutrade_list_risk_control_executions` — show when rules fired, filter by rule_id/status/limit as needed.

## When to consult the user

- User says "设个止损" / "加个止盈" but **no symbol or level** → Ask: 为哪只标的？止损/止盈比例（如 5%）还是固定价格？
- User says "改一下刚才那条风控" but **no rule_id or new params** → Ask: 请确认要修改的规则（rule_id 或描述）以及新的参数。

## When to use information outside MCP

- All rule data and execution history come from MCP. No external lookup required for this skill.

## Troubleshooting

| Issue | Cause | Action |
|-------|--------|--------|
| Invalid config / MCP error | Bad parameters or unsupported config | Describe constraints and give an acceptable config example (see risk-rules.md) |
| Rule not found on update/delete | Wrong rule_id or already deleted | List rules with `simutrade_list_risk_control_rules` and ask user to pick rule_id |

## Additional resources

- Rule types and config examples: [references/risk-rules.md](references/risk-rules.md)
