# Investment Idea Template

Use this structure when appending or updating content via `simutrade_update_investment_thinking`. Keeps Dashboard and Investment Thinking timeline consistent.

## Single idea block

```markdown
### [Symbol or Theme] — [Short Title]
- **Date**: YYYY-MM-DD
- **Logic**: [1–2 sentence thesis; why this idea]
- **Catalyst / Signal**: [What triggered or supports the idea: earnings, macro, technical, etc.]
- **Risks**: [Key downside or invalidation]
- **Optional**: Target / stop or position size note
```

## Example

```markdown
### AAPL — Earnings momentum and services growth
- **Date**: 2025-03-01
- **Logic**: Services revenue and installed base support multiple; iPhone refresh cycle in H2.
- **Catalyst / Signal**: Last earnings beat; guidance raised; 1d breakout above 200 MA.
- **Risks**: China demand, regulatory; break below 180 invalidates.
- **Optional**: Add on pullback to 185; stop 178.
```

## Batch update (multiple ideas)

When merging several ideas into the full document:

1. Call `simutrade_get_investment_thinking` to get current `content`.
2. Append or insert new blocks using the template above; preserve existing sections unless user asks to replace.
3. Call `simutrade_update_investment_thinking` with the full merged `content` string.
