---
name: ai_daily_briefing
description: "每日 AI 效率日报 - 自动扫描全球 AI 动态，筛选高价值信息生成个人情报日报"
metadata:
  builtin_skill_version: "1.1"
  copaw:
    emoji: "📊"
    requires: {}
---

# AI Daily Briefing

当用户请求"AI 日报"、"AI 动态"、"今日 AI 新闻"时，使用以下数据源和工具生成个人情报日报。

## 数据源

| 优先级 | 来源 | URL |
|--------|------|-----|
| 1 | GitHub Trending | https://github.com/trending?since=daily |
| 2 | Hugging Face Papers | https://huggingface.co/papers |
| 3 | OpenAI Blog | https://openai.com/blog/rss.xml |
| 4 | Anthropic News | https://www.anthropic.com/news |
| 5 | Meta AI Blog | https://ai.meta.com/blog/ |
| 6 | Google AI Blog | https://ai.googleblog.com/ |
| 7 | Product Hunt | https://www.producthunt.com/ |

## 约束

| 项目 | 要求 |
|------|------|
| 时间范围 | 过去 24 小时 |
| 屏蔽内容 | 股市波动、纯融资新闻、惊悚标题、重复浅层报道 |
| 通过标准 | 基座模型/技术突破、立刻可用的工具/插件/开源项目 |
| 丢弃标准 | 纯观点输出、无 Demo/无代码的"PPT 产品" |
| 核心原则 | 冷静、客观、反炒作。只讲事实和价值。 |

## How to Use

1. **发现信息**：用 `web_fetch` 抓取数据源表格中的 URL，获取最新 AI 动态
2. **去重筛选**：同一事件只保留最原始/最官方的 1 条来源
3. **分类整理**：
   - **S 级**：可验证 + 能立刻行动（1-2 条）
   - **B 级**：可用但不急（3-6 条）
   - **行业新闻**：只需知晓（1-3 条）
4. **输出报告**：按下方模板格式输出，保存为 `.md` 文件

## 输出模板

```markdown
# AI 效率日报 | YYYY-MM-DD

## 🚨 核心关注（S 级）

| 项目 | 内容 |
|------|------|
| 名称 | [项目名] ⭐ [stars] |
| 来源 | [来源] |
| 价值 | [省了什么/多了什么能力] |
| 用法 | [今天就能做的 1 个动作] |
| 链接 | [链接] |

## 🛠 工具箱更新（B 级）

| 工具 | ⭐ | 价值 | 可靠性 | 链接 |
|------|---|------|--------|------|
| [名] | [stars] | [价值] | [S/A/B/C] | [链接] |

## 📉 行业新闻

| 热点 | 摘要 |
|------|------|
| [公司/事件] | [一句话] |

## ✅ 今日行动

1. [动作] → [命令/链接]

## 💡 Insight

[一句话总结]
```

## 抓取规则

- 只抓取白名单域名：github.com, huggingface.co, openai.com, anthropic.com, ai.meta.com, ai.googleblog.com, producthunt.com
- 禁止抓取短链、未知域名、私网地址
- S 级条目必须能验证，否则降级或丢弃
- 每个来源最多抓 1-2 页
- 禁止启用浏览器自动化

## 故障处理

| 情况 | 处理 |
|------|------|
| 无法联网 | 顶部标注"**未联网**" |
| 白名单来源抓取失败 | S 级留空，Insight 提示检查网络 |

## 输出原则

- 读起来像给自己看的工作备忘，不要写论文
- 宁缺毋滥
- 每条都要有"对你有什么用"
- 最后给可执行的下一步