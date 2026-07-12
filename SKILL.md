---
name: ai-pm-daily-brief
description: AI product manager daily intelligence workflow. Use when the user wants to collect, filter, summarize, or archive AI industry/product news for AI PM work; design or run a daily AI news SOP; integrate Follow Builders, Hot Info Crawler, X/Twitter, blogs, podcasts, papers, Reddit, YouTube, or Chinese community sources; generate an Obsidian/Markdown/Notion-ready AI product manager daily brief; compare traditional manual news collection with an AI-upgraded workflow.
---

# AI PM Daily Brief

Use this skill to turn scattered AI industry information into a product-manager-ready daily brief. The goal is not to summarize everything. The goal is to identify signals that help an AI product manager make better product, competitor, roadmap, UX, growth, and business decisions.

## Core Principle

Separate the workflow into three layers:

1. **Collection layer**: gather raw public information from configured sources.
2. **Intelligence layer**: deduplicate, classify, score, and interpret each item from an AI PM perspective.
3. **Delivery layer**: write a concise daily brief to chat, Obsidian, Notion, email, Slack, or another destination requested by the user.

Prefer deterministic collectors and source feeds over ad hoc browsing. Use LLM reasoning for judgment and synthesis, not for inventing facts.

## Reference Files

- Read `references/source-model.md` when setting up or changing source lists, integrating Follow Builders or Hot Info Crawler, or explaining the collection architecture.
- Read `references/digest-template.md` when generating a daily brief, designing scoring rules, or changing the output format.

## Daily Run SOP

### 1. Load User Settings

Look for a user config at `~/.ai-pm-daily-brief/config.json`. If it does not exist, infer reasonable defaults and ask only for missing information that blocks execution.

Recommended defaults:

```json
{
  "language": "zh",
  "timezone": "Asia/Shanghai",
  "frequency": "daily",
  "delivery": {
    "method": "obsidian",
    "folderPath": "/Users/wangxiaozi/Downloads/昭昭的知识库/每日资讯"
  },
  "focus": [
    "AI product launches",
    "competitor updates",
    "model capability changes",
    "agent workflows",
    "AI product UX",
    "commercialization and pricing",
    "user/community feedback"
  ]
}
```

If the user has an existing destination such as Obsidian, preserve it.

### 2. Collect Raw Inputs

Use the best available collectors in this priority order:

1. **Follow Builders feed** if installed or reachable. Use it for high-signal AI builders, AI podcasts, and official AI blogs.
2. **Hot Info Crawler** if configured. Use it for broad scanning across papers, X, YouTube, Reddit, Jike, and user-defined topics.
3. **User-provided links/files** if the user gives specific sources.
4. **Web browsing** only when the user asks for live/latest info or when no configured collector can answer the request. When browsing, prefer primary sources.

Do not mix unsupported assumptions into the raw source pool. Every retained item must have a source URL or an explicit file reference.

### 3. Normalize Items

Normalize every collected item into this shape before synthesis:

```json
{
  "source": "x | blog | podcast | youtube | reddit | paper | jike | github | other",
  "collector": "follow-builders | hot-info-crawler | web | user",
  "title": "string",
  "author": "string",
  "url": "string",
  "publishedAt": "ISO date or null",
  "content": "raw text, transcript, excerpt, or factual summary",
  "metrics": {
    "likes": 0,
    "comments": 0,
    "shares": 0
  }
}
```

Skip items without enough text to evaluate and without a source URL.

### 4. Deduplicate

Deduplicate by URL, tweet/status ID, YouTube video ID, podcast GUID, article slug, GitHub repo/release ID, or paper ID. If multiple sources discuss the same event, merge them into one item and preserve all useful source links.

### 5. Score for AI PM Value

Score items from an AI product manager perspective, not from a general tech-news perspective. Favor items that affect:

- Product strategy or roadmap
- Competitor positioning
- Model capability or cost structure
- AI UX and interaction patterns
- Agent/workflow automation
- User demand, community pain, or behavior changes
- Pricing, packaging, monetization, distribution, or growth
- Enterprise adoption, trust, safety, compliance, or operations

Use `references/digest-template.md` for the full scoring rubric.

### 6. Classify

Classify each retained item into one primary category:

- 今日必看
- 产品发布与竞品动态
- 模型与技术趋势
- Agent / Workflow
- 用户反馈与社区洞察
- 商业化与增长
- 深度内容: 播客 / 视频 / 长文 / 论文
- 暂存观察

### 7. Write the Daily Brief

Generate the brief in the user's preferred language. For Chinese output, use natural simplified Chinese and keep common AI terms in English when Chinese PMs would normally use them: AI, LLM, API, RAG, prompt, agent, workflow, eval, benchmark, pricing, GTM.

For each important item, include:

- 发生了什么
- 为什么重要
- 对 AI 产品经理的启发
- 可跟进问题
- 原文链接

Keep the brief scannable. Avoid long generic summaries. If a detail is not in the sources, do not invent it.

### 8. Deliver and Archive

If delivery is Obsidian or Markdown, write one file per day:

`AI PM Daily Brief - YYYY-MM-DD.md`

Use the user's timezone for the date. If the file already exists for the same date, update or overwrite only when the user asked for one daily canonical note.

If no important items are found, still create a short note saying there were no high-signal updates today, unless the user prefers silence.

## Follow Builders Integration

When using Follow Builders:

1. Prefer the installed skill path `/Users/wangxiaozi/.codex/skills/follow-builders` if present.
2. Run or inspect its `scripts/prepare-digest.js` workflow to obtain central feed data.
3. Treat `feed-x.json`, `feed-podcasts.json`, and `feed-blogs.json` as high-signal source feeds.
4. Use its content as raw material, then re-score and reframe for AI PM value. Do not simply reuse a generic builders digest if the user requested AI PM intelligence.

## Hot Info Crawler Integration

When using Hot Info Crawler:

1. Treat it as the broad scanning layer.
2. Use its configured topics, platforms, and watched accounts to discover candidate items.
3. Do not let broad popularity dominate the final brief. Re-rank everything using the AI PM scoring rubric.
4. Merge overlapping Hot Info Crawler items with Follow Builders items before writing.

## Quality Rules

- Every included item needs a source link.
- Prefer primary sources over commentary.
- Separate facts from interpretation.
- Mention uncertainty when the source is incomplete.
- Avoid influencer-style hype and generic "AI is moving fast" language.
- Do not include mundane social posts unless they reveal a product, strategy, market, or user-behavior signal.
- Keep an audit trail: source URL, collector, category, and date should be recoverable from the final note.
