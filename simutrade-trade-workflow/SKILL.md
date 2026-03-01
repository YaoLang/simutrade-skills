---
name: simutrade-trade-workflow
description: Executes stock/HK/crypto buy and sell orders, cancels orders, and queries quotes, portfolio, positions, and orders via Simutrade MCP. Use when the user says "place order", "buy/sell", "cancel order", "check positions", "check orders", "quote", or provides symbol and quantity/price. Always read investment thinking before any trade or risk decision.
compatibility: Simutrade MCP must be connected.
metadata:
  mcp-server: simutrade
  version: 1.0.0
---

# Simutrade Trade Workflow

## Required order of operations

If the user intent involves **placing an order** or **changing risk**: call `simutrade_get_investment_thinking` **first**, then proceed with quote/order/cancel. This keeps decisions aligned with the account’s thinking and visible in the Dashboard/timeline.

## Tools

- **simutrade_get_investment_thinking** — Call before any order or risk decision.
- **simutrade_get_quote** — Real-time quote for one symbol. Use before placing an order to confirm price and symbol validity. Symbol format: US (AAPL, NVDA), HK (0700.HK, 9988.HK), crypto (BTC-USD, ETH-USD). Returns price, change, volume, high/low.
- **simutrade_place_order** — Place buy/sell. **You must provide `reason`** (decision rationale; shown in Dashboard and Investment Thinking timeline). Include: why now (signals/catalyst), strategy name, target/stop or risk-reward if any. Omit `price` for market orders; include `price` for limit or stop orders. Quantity in shares (stocks) or units (crypto).
- **simutrade_get_portfolio** — Portfolio summary (total value, cash, PnL) plus all positions in one call. Use when you need both summary and holdings; for positions only use `simutrade_list_positions`.
- **simutrade_list_positions** — Current holdings: symbol, quantity, avg cost, market value, unrealized PnL. Use before selling or setting position-level risk rules.
- **simutrade_list_orders** — Recent orders. Optional filters: `status` (pending | filled | cancelled), `limit` (default 10). For a single order use `simutrade_get_order`.
- **simutrade_get_order** — One order by ID. Returns full order including decisionReason, fill price/quantity if filled, status. Use after place_order to confirm or for复盘.
- **simutrade_cancel_order** — Cancel a pending order by ID. Only orders with status `pending` can be cancelled.

## Place-order flow

1. Call `simutrade_get_investment_thinking`.
2. Parse symbol, quantity, and order type (market vs limit/stop). If **quantity** is missing (e.g. user said "buy Apple"), **ask**: "请确认数量（股数或约等于多少股）。"
3. If order type is limit or stop and **price** is missing, **ask**: "请确认：市价单还是限价/止损单？若限价/止损，目标价是多少？"
4. Call `simutrade_get_quote` for the symbol to confirm current price and symbol validity.
5. Compose `reason` (required) from current thinking and user intent.
6. Run through [references/order-checklist.md](references/order-checklist.md) and call `simutrade_place_order`.
7. Optionally call `simutrade_get_order` with the returned order ID to confirm result.

## Query flows

- **Portfolio + positions**: `simutrade_get_portfolio`.
- **Positions only**: `simutrade_list_positions`.
- **Order list**: `simutrade_list_orders` (use `status` and `limit` as needed).
- **Single order**: `simutrade_get_order` with order ID.

## Cancel flow

- If user says "撤单" but does not give order ID: call `simutrade_list_orders` with `status: pending`, then ask user to choose which order to cancel, or ask for the order ID.
- Call `simutrade_cancel_order` with the chosen order ID.

## When to consult the user

- User says "买苹果" / "加仓 NVDA" etc. but **no quantity** → Ask for quantity (shares or approximate amount in shares).
- User did not specify market vs limit/stop, or limit/stop but **no price** → Ask: 市价单还是限价/止损单？若限价/止损，目标价是多少？
- **Optional**: For large or market orders, confirm: "即将以市价下单 [symbol] [quantity]，是否确认？"
- User says "撤单" but **no order ID** → List pending orders and ask which to cancel, or ask for order ID.

## When to use information outside MCP

- If the environment supports Web Search and the user asks "why did it drop today" or "any news", you may search for news or research to enrich `reason`. Otherwise rely only on `simutrade_get_quote`, `simutrade_get_market_history`, and investment thinking.

## Troubleshooting

| Issue | Cause | Action |
|-------|--------|--------|
| Order rejected / invalid | Bad symbol, quantity, or price | Re-check symbol format; confirm quantity and price with user; retry or suggest limit order |
| "Connection" or MCP error | MCP or auth | Suggest user check Simutrade MCP connection and API key |
| Cancel fails | Order already filled or cancelled | Confirm with `simutrade_get_order`; only pending orders can be cancelled |

## Additional resources

- Pre-order checklist: [references/order-checklist.md](references/order-checklist.md)
