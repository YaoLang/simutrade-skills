# Order checklist (pre-place)

Before calling `simutrade_place_order`, ensure:

- [ ] **Symbol** — Correct format: US (AAPL, NVDA), HK (0700.HK, 9988.HK), crypto (BTC-USD, ETH-USD). Confirm with `simutrade_get_quote` if unsure.
- [ ] **Side** — Buy or sell.
- [ ] **Quantity** — In shares (stocks) or units (crypto). Required; ask user if missing.
- [ ] **Order type** — Market (no price) vs limit/stop (price required). Ask user if unclear.
- [ ] **Price** — Only for limit or stop orders; omit for market.
- [ ] **Reason** — **Required.** Decision rationale for Dashboard/Investment Thinking timeline. Include: why now (catalyst/signal), strategy name, target/stop or risk-reward if any. Use `simutrade_get_investment_thinking` before placing so reason aligns with current thinking.
- [ ] **Risk** — Optional: confirm whether user wants a risk rule (e.g. stop-loss) after this order; if yes, use simutrade-risk-control skill or `simutrade_create_risk_control_rule` after fill.

## Optional: confirmation for large or market orders

- For large size or market orders, you may confirm: "即将以市价下单 [symbol] [quantity]，是否确认？"
- Skill can define a threshold (e.g. above $X or N shares) for when to always confirm.
