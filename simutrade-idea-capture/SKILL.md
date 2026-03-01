---
name: simutrade-idea-capture
description: Consolidates and records investment ideas, research notes, and post-trade takeaways using Simutrade Investment Thinking. Use when the user says "record my idea", "write research notes", "organize today's/week's thinking", "summarize复盘", or uploads research/screenshots. Requires Simutrade MCP.
compatibility: Simutrade MCP must be connected.
metadata:
  mcp-server: simutrade
  version: 1.0.0
---

# Simutrade Idea Capture

## Instructions

### Step 1: Get current thinking

Call `simutrade_get_investment_thinking` to load the account’s current investment thinking document. Use it as the base to merge or append new content.

### Step 2: Confirm what to record

- If the user provided concrete content (pasted text, uploaded file, or clear idea in chat), extract: symbol/theme, logic, catalyst/signal, risks, and optional target/stop.
- If the user only said something like "record my idea" or "organize today’s thinking" **without** specific content or attachments, you **must ask**:
  - "请提供要记录的标的/来源，或粘贴/上传研报、笔记内容。"

### Step 3: Structure and merge

- Format the new idea(s) using the structure in [references/idea-template.md](references/idea-template.md) (title, logic, catalyst, risks, date).
- Merge into the existing `content` from Step 1 (append or insert by section). Do not drop existing content unless the user asks to replace it.

### Step 4: Update thinking

Call `simutrade_update_investment_thinking` with the **full** merged `content` string (required parameter). This updates the account’s Investment Thinking document and keeps the Dashboard/timeline in sync.

## When to consult the user

- User says "记录一下思路" or "整理今天想法" but **did not** provide specific content or attachments → Ask for 标的/来源 or paste/upload of research or notes.
- User only shared a link → Ask them to paste the relevant text or a short summary (MCP has no URL-fetch; do not claim to fetch the link).

## When to use information outside MCP

- Only use text or files the user has **already provided** in the conversation. Do not fetch external URLs; if they share a link, ask for pasted content or a summary.

## Examples

**Example 1: User pastes a research snippet**

User: "把这段记进投资思路：NVDA 数据中心需求持续，H200 放量，目标 950，止损 820。"

Actions:

1. `simutrade_get_investment_thinking`
2. Add a block per idea-template: symbol NVDA, logic (data center + H200), catalyst/signal (demand), risks (implied: demand slowdown), target 950, stop 820, date today
3. Merge into existing content and call `simutrade_update_investment_thinking` with full content

Result: Investment thinking document updated; idea visible in Dashboard/timeline.

**Example 2: User asks to organize without content**

User: "帮我整理一下今天的投资想法。"

Actions:

1. `simutrade_get_investment_thinking` to see current state
2. **Ask**: "请提供要记录的标的/来源，或粘贴/上传研报、笔记内容。"
3. After user provides content, continue with Step 3–4 above

## Troubleshooting

| Issue | Cause | Action |
|-------|--------|--------|
| Update fails or "MCP error" | MCP disconnected or auth | Suggest user check Simutrade MCP connection and API key; offer to save content locally for later paste |
| User only gave a URL | No URL fetch in MCP | Ask user to paste the article body or a short summary |
| Content too long / truncation | Platform limit on `content` size | Suggest splitting into multiple idea blocks or summarizing the oldest sections before merging |

## Additional resources

- Idea structure and examples: [references/idea-template.md](references/idea-template.md)
