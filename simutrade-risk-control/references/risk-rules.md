# Risk control rules reference

Rules are created with `simutrade_create_risk_control_rule`. You **must** provide `reason`. Use `simutrade_get_investment_thinking` before creating or updating rules so the reason aligns with current thinking.

## Rule types

- **stop_loss** — Trigger when price falls to a level or by a percentage.
- **take_profit** — Trigger when price reaches a target level or percentage.
- **trailing_stop** — Trigger when price retraces by a percentage (or amount) from a high.
- **position_limit** — Cap position size or exposure (implementation depends on MCP config schema).

## Config examples

Exact parameter names depend on Simutrade MCP; below are typical patterns.

### Percentage stop-loss (e.g. 5%)

```json
{ "type": "percentage", "percentage": 5 }
```

### Fixed price stop-loss

```json
{ "type": "price", "price": 180.50 }
```

### Percentage take-profit (e.g. 10%)

```json
{ "type": "percentage", "percentage": 10 }
```

### Trailing stop (e.g. 3% from high)

```json
{ "type": "percentage", "percentage": 3 }
```

### Position limit

Check MCP docs for `position_limit` config (e.g. max shares or max notional per symbol).

## Filters when listing

- **simutrade_list_risk_control_rules**: filter by `symbol`, `rule_type`, `is_active`, `order_id`.
- **simutrade_list_risk_control_executions**: filter by `rule_id`, `status`, `limit`.

## Update and delete

- **simutrade_update_risk_control_rule** — Pass `rule_id` and new parameters.
- **simutrade_delete_risk_control_rule** — Pass `rule_id` to remove permanently.

If the user says "改一下刚才那条风控" without `rule_id` or new params, ask: 请确认要修改的规则（rule_id 或描述）以及新的参数。
