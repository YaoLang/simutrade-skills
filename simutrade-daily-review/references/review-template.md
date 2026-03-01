# Daily review template

Use this structure for 盘前 (pre-market) and 盘后 (post-market) outputs.

## 盘前 (Pre-market)

```markdown
# 盘前计划 — YYYY-MM-DD

## 今日关注
- [From investment thinking: key symbols/themes and planned actions]

## 当前持仓与风控
- [Summary from simutrade_list_positions and simutrade_list_risk_control_rules]

## 计划操作
- [Concrete actions aligned with 今日关注: add/reduce/exit, levels if any]

## 风控要点
- [Active rules to watch; any new rules to set before open]
```

## 盘后 (Post-market)

```markdown
# 盘后复盘 — YYYY-MM-DD

## 执行情况
- [From simutrade_list_orders status=filled and simutrade_list_positions: what was done, fills, PnL]

## 思路验证
- [Compare outcomes to investment thinking: what worked, what didn’t, why]

## 明日关注
- [What to watch or do next; optional link to macro/events if external search is used]

## 可选：复盘结论
- [Short summary to append to investment thinking via simutrade_update_investment_thinking]
```

## Data sources

- **Pre**: `simutrade_get_investment_thinking`, `simutrade_list_positions`, `simutrade_list_risk_control_rules`.
- **Post**: Same plus `simutrade_list_orders` (e.g. status=filled) and possibly `simutrade_get_market_history` for technical context. Optionally call `simutrade_update_investment_thinking` to save复盘结论.
