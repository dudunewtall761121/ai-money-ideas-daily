# 检索手册（渠道、查询式与限流对策）

## 渠道清单

| 渠道 | 入口 | 用途 |
|------|------|------|
| WebSearch | 查询式见下 | 中英文全面覆盖 |
| Hacker News | Algolia API：`https://hn.algolia.com/api/v1/search_by_date?query=AI%20revenue%20MRR&tags=story&hitsPerPage=15` | 独立开发者收入自述（Show HN） |
| Product Hunt | `https://www.producthunt.com/leaderboard/weekly/2026/<周数>`（周数≈当月第几周） | Agent/工具类新品与变现信号 |
| Unite.AI Funding | `https://www.unite.ai/series/funding/` | 每日 AI 融资流水（日期明确） |
| Value Add VC | `https://valueaddvc.com/blog/`（搜 `AI Funding Rounds <YYYY年MM月>`） | 月度融资 Top 榜单，含原始来源链接 |
| Crunchbase News | `https://news.crunchbase.com/venture/` | 大额轮次周汇总 |
| NeoDrop 独立开发者周报 | `https://neodrop.ai/channel/j4cgnS0HVME` | 每周 $10K MRR 拆解案例 |
| 36氪 / 娱乐资本论 | WebSearch `36氪 AI 变现 赚钱 收入 <年月>` | 中文变现案例、平台激励、商单价格 |
| 新浪财经 / 证券时报 | WebSearch `<公司> 融资 <金额>` | 中文融资互证 |
| 头条/搜狐自媒体 | WebSearch `AI 漫剧 接单 收入 <月>` 等 | 副业实操样本与价格分层 |
| 网信办 / 中国政府网 | `https://www.cac.gov.cn/wxzw/wxfb/A093702index_1.htm` | 备案公告、AI 监管新规 |

## 查询式模板（中英各一轮）

- `AI 变现 副业 真实收入 案例 <YYYY年M月>`
- `AI side hustle indie hacker MRR revenue <Month Year>`
- `AI startup funding announced <Month Day Year>`（用具体日期逼近 48 小时窗口）
- `AI 智能体 Agent 创业 变现 融资 <YYYY年M月>`
- `Product Hunt AI 产品发布 爆款 收入 <YYYY>`
- `小红书/闲鱼 AI 副业 月入 <YYYY>`
- `AI 政策 监管 生成式人工智能 <YYYY年M月> 最新`
- `<头部公司> 收入 ARR <Month Year>`

## 限流与失败对策

- WebSearch 并行请求易触发 `Failed to call search service`：每批最多 2 个查询，失败后改单发或切换到 WebFetch 抓固定 URL。
- WebFetch 对 TechCrunch 分类页、Crunchbase 首页可能返回空：改抓具体文章 URL 或换 Unite.AI / Value Add VC 聚合页。
- HN 首页直接抓取可能失败：一律走 Algolia API。

## 清洗规则

- 垃圾站注入特征（直接剔除）：正文夹带「棋牌/博彩/app官网下载」等词、金额单位失真（如「8553 亿美元 B 轮」）、来源域名与内容不符。
- 时效外旧闻：变现事件发生超过 14 天且无热度延续证据的不收录；背景性数据（如 Photo AI 收入）可收录但标注披露时间。
- 同一事件多来源：合并为一条，链接并列（融资类至少 2 源）。
