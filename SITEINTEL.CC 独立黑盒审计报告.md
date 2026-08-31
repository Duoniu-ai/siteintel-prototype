# 《SITEINTEL.CC INDEPENDENT BLACK-BOX AUDIT REPORT》

**Audit Date:** 2026-08-31（UTC+8）；探测时间 2026-08-30 22:44–23:05 UTC
**Auditor:** 独立第三方（本审计未接受 SiteIntel 任何委托或报酬）
**Environment:** 新加坡出口节点 / Ubuntu 22.04 / 桌面级 Chrome（1920×1080）/ curl + Python + OpenSSL 辅助探测；请求经本地出口代理（X-VE-Vortex-IP 可见）
**Pages Tested:**
- `/`（首页，中/英）
- `/website/github.com`（报告页）
- `/website/example.com`、`/website/ratetest1.com`（空态）
- `/analyze/{id}`（分析进度页）
- `/technology/github-pages`（技术详情页）
- `/report`（近期报告）、`/docs/api`（API 文档）、`/bulk`、`/how-it-works`
- API：`POST /api/analyze`、`GET /api/analyze/{id}?token=`、`/api/analyze/{id}/stream`、`/api/me`、`/api/export`、`/api/v1/analyze`
- 对照探测域：github.com / github.io / netflix.com / stripe.com / wordpress.org / wikipedia.org / cloudflare.com / apple.com / bilibili.com / example.com / openai.com（真实响应头与 TLS 证书均为独立核验）

**Limitations:**
1. 指纹可信度以 11 个有独立真值核验的域名为样本，样本量有限；未对全部技术规则做穷尽测试。
2. 移动端为**代码级评估**（viewport meta、Tailwind 断点、汉堡菜单），本环境无法用真实手机视口渲染验证。
3. 未做登录态下的功能验证（原始检测记录、完整变化时间线、关联图谱、导出等付费/登录功能仅能观察到“登录后可见”的提示与门禁，无法验证其内部质量）。
4. 全程仅做非侵入式公开信息检查；未进行任何漏洞利用、绕过或破坏性测试。
5. 所有结论基于 2026-08-30/31 的快照，产品可能随时间变化。

---

## 0. 结论摘要（TL;DR）

SiteIntel 是一个**中文优先**、定位比“技术栈探测”更宽的网站情报工具：它的真实卖点不是指纹识别，而是**基于持续观测的变化检测 + 共享基础设施关系图 + 中文报告**。UI/UX 打磨程度明显高于同类小工具，分析流程的 10 步进度反馈做得很好。

但它的**核心数据可信度存在硬伤**：对 github.com 检测出“GitHub Pages（80%）”并给出“很可能”的强结论是**确凿的误报**；netflix.com 同时被检测出“GitHub Pages + Heroku”也是误报；wikipedia.org 的“Amazon CloudFront”高度可疑；bilibili.com / apple.com 则完全漏检。而 Evidence 体系对匿名用户**只展示标签、不展示原始证据值**，原始检测记录被登录墙挡住——宣称“带证据链”与匿名用户实际能看到的不一致。**结论：产品打磨与商业化包装（B+），底层指纹引擎（C–），证据透明性（C）。**

---

## 1. 产品定位

### 1.1 它是什么
首页 H1：“看懂任何一个网站”（英文版 “Understand any website”）。Meta 描述：“输入一个网站，了解它是什么、如何运行、最近发生了什么，以及公开信息中有哪些值得关注的变化。”

它把自己定位成**网站情报/洞察产品**，而非单纯的指纹识别器。首页四大价值主张：
1. **网站如何运行** —— 技术栈、网络服务商、公开配置
2. **最近发生了什么** —— 服务/证书/技术变化追踪
3. **有什么值得关注** —— 自动整理公开数据中的重点变化
4. **它与什么有关联** —— 共享基础设施/技术的关联图

### 1.2 核心用户
从文案（“想了解一个陌生网站 / 想观察网站有没有变化 / 想研究其他网站 / 想深入探索”）看，目标用户很宽：销售/BD（了解客户网站）、SEO/运营（观察竞品变化）、采购/风控（尽调）、安全研究者。结合 sitemap（`/cases/infrastructure-migration`、`/cases/search-traffic-drop`、`/guides/baidu-seo`、`/guides/website-migration`），它在主推**“迁移监测 / 变更告警”**场景——这是和 Wappalyzer 类工具最不同的卖点。

### 1.3 10 秒理解度
**通过。** 首页第一屏 = 大标题 + 域名输入框 + 三个示例域名 + “开始分析”，第二屏四个价值块。普通用户 10 秒内能明白“输入域名 → 得到分析报告”。这是首页设计最成功的地方。

### 1.4 与 Wappalyzer / BuiltWith 的差异
| 维度 | SiteIntel | Wappalyzer | BuiltWith |
|---|---|---|---|
| 语言 | 中英双语（中文优先） | 英文 | 英文 |
| 变化检测/历史时间线 | **核心卖点** | 无/弱 | 弱（快照式） |
| 共享基础设施关系图 | **核心卖点** | 无 | 有（付费） |
| 技术指纹库规模 | 有限（technology 页面约几十个类别） | 数万条规则 | 数万条规则 |
| 免费报告 | 公开可看 | 浏览器插件免费 | 强付费墙 |
| 洞察/发现 | 自动生成“值得关注”发现 | 无 | 部分 |

差异化方向是对的（变化监测 + 关系图谱 + 中文），但**最底层的“技术指纹”能力反而是短板**（见第 4 节）。

### 1.5 定位是否清晰
**总体清晰但有野心过大的风险。** 一句“看懂任何一个网站”把至少 6 个子产品（website / technology / infrastructure / relationship / search intelligence + monitoring）装进一个域名，`/technology/*` 与 `/ip/*`、`/asn/*`、`/certificate/*`、`/organization/*` 大量 SEO 页面在摊薄“这是干嘛的”的认知。作为独立审计，我认为定位方向（情报而非指纹）是聪明的，但执行上指纹底座尚未撑起这个野心。

---

## 2. 实际用户路径（实测）

我用 github.com 完整走了一遍：

| 步骤 | 观察 | 评价 |
|---|---|---|
| 首页输入 `github.com` | 输入框占位 `example.com`，点击即编辑 | ✅ 清楚 |
| 点击“开始分析” | URL 跳到 `/analyze/{id}` | ✅ 一步到位，无中间确认 |
| 等待分析 | 显示 **1/10 → 10/10 步骤**：检查域名→检测DNS记录→解析IP与网络→检测SSL证书→访问网站→检测使用的技术→整理基础设施信息→对比历史变化→生成分析发现→生成分析报告 | ✅ **优秀**。可视化进度显著降低等待焦虑；浏览器内约 8 秒，API 全流程约 20 秒 |
| 查看结果 | “查看分析报告”按钮 → `/website/github.com` | ✅ 明确 |
| 查看技术栈 | “使用的技术”区块：GitHub Pages（80%）+ 证据信号 http_header / asset_url | ⚠️ 技术栈**只有 1 项**，且是误报（见第 4 节） |
| 查看置信度 | “80/100 中置信”“88% 较强”“55% 可能”等标签 | ⚠️ 同一页面混用“/100”“%”“中文词”三种置信度表达 |
| 查看证据 | “查看依据 (1)” 可展开 | ⚠️ 展开后证据仅为 `eventType + detectedAt` 标签，**看不到任何原始值** |
| 查看原始数据 | “原始检测记录 · 登录后查看完整数据与原始检测记录” | ⚠️ **登录墙**，匿名用户拿不到原始检测记录 |

**等待焦虑**：进度条分 10 步且有 SSE 实时推进，同类工具中少见，处理得好。
**空态/错误态**：对无报告域名（ratetest1.com）显示“还没有 {domain} 的分析结果”+ 预填输入框 + START ANALYSIS，干净可用；对非法域名 API 返回明确错误（见第 7 节）。
**易误解点**：报告页把误报的“GitHub Pages”以 80% 置信度和“很可能”结论直接展示，用户很容易当成事实带走。

---

## 3. UI / UX

### 3.1 首页
深色主题，信息层级清晰：H1 → 输入框 → 示例 → 四价值块。示例域名（cloudflare.com / github.com / openai.com）有引导作用。唯一小问题：四价值块的“查看完整分析 -”按钮实际跳到对应 SEO 页面，而非某个真实示例报告。

### 3.2 Header
`首页 / 网站分析 / 探索 / 登录` + `中文 | EN` 切换。**注意：** “探索”导航实际落到 `/report`（近期报告列表），命名与落地内容不符；“网站分析”无独立落地页（`/analyze` 返回 404）。导航信息架构有轻微名不副实。

### 3.3 搜索/输入区
输入 + 示例 + 主 CTA 突出，验证通过。可改进：无输入建议/历史；占位 `example.com` 对中文用户暗示性一般。

### 3.4 分类/导航
Footer 有：网站分析/探索/工具/工作原理/使用指南/案例/报告库/API 文档/批量分析。信息完整，但 `/technology/*`、`/ip/*` 等海量 SEO 页在导航中不可达（靠 sitemap），说明这些是 SEO 抓收录的“内容页”而非产品功能入口。

### 3.5 分析过程
**本项目最佳体验。** 10 步进度 + 每步完成态 + “分析另一个网站”按钮 + “查看分析报告”主 CTA。实测无卡死、无无谓刷新。

### 3.6 分析结果页
报告按 `01 网站概览 → 02 它如何运行 → 03 历史变化 → 04 值得关注 → 05 它与什么有关联 → 06 深入数据` 编号分区，层级专业、间距一致，状态徽章（在线/HTTP 200）、mono 等宽数值、置信度标签齐全。**问题**：页面非常长（6 大区 + 每个区若干子卡），首屏只看到概览，技术栈要滚动很深；深数据区（搜索情报/发现溯源/原始检测记录）折叠且大部分需登录。

### 3.7 Evidence / Claims
每个“值得关注”发现带“查看依据 (n)”，点开是 `eventType + 时间戳` 标签；“为什么这样判断”文案存在。**但匿名用户看不到证据的原始值**（HTTP 头原文、前后对比、检测到 URL），只有标签。UI 上“查看依据”按钮暗示可验证，实际给的是空壳标签——这是 Evidence 体系的核心问题（见第 5 节）。

### 3.8 移动端（代码级）
- viewport meta 正确（`width=device-width, initial-scale=1`）；
- 存在汉堡菜单（`aria-label="Open menu"`）；
- 响应式断点以 `sm:` 为主（26 处），`md:`/`lg:` 很少（3/4 处）——**桌面优先设计**；
- 报告页 6 大区长表格在手机上会非常长，未见到移动端专用的精简视图。
- **局限**：无法在本环境用真实手机视口验证，仅能判为“具备基础响应式、未针对移动深度优化”。

### 3.9 中英文切换
实测完整：中文版 “看懂任何一个网站 / 开始分析 / 网站分析 / 探索”，英文版 “Understand any website / Start analysis / Analyze / Explore”。翻译完整、无残留中英混杂。**这是国内工具里少见的完成度。**

### 3.10 一致性 / 专业感
设计系统统一（同一套 Token：panel/line/accent/mono），视觉专业，接近成熟 B2B 情报产品的观感。少数粗糙点：`服务商·35` 这类标签对用户无解释（35 是什么？）；置信度同页混用三种表达（80/100、88%、中置信）。

---

## 4. 指纹分析可信度（重点）

### 4.1 实测样本（每项均有独立真值核验）

| 域名 | SiteIntel 检测结果 | 独立核验的真值 | 判定 |
|---|---|---|---|
| github.com | **GitHub Pages**（hosting, 80%） | `server: github.com`（GitHub 主站 Rails 应用，自有边缘）；TLS issuer Sectigo（该项检测正确） | ❌ **误报** |
| netflix.com | **GitHub Pages**(80%) + **Heroku**(80%) + AWS hosting(80%) | AWS us-west-2（`via: 2 i-... (us-west-2)`）；Netflix 自建/ AWS，绝无 GitHub Pages 或 Heroku | ❌ **双误报**（AWS 托管正确） |
| wikipedia.org | **Amazon CloudFront**（cdn, 85%） | 自有 DNS `199.16.x.x`，Wikimedia 自建 Varnish CDN，非 CloudFront | ⚠️ **高度可疑** |
| stripe.com | Next.js（77%）+ Nginx（80%） | `server: nginx` 实测一致 | ✅ 正确 |
| wordpress.org | WordPress（78%）+ Nginx + GA/GTM/Fonts | WordPress + nginx 属实 | ✅ 正确 |
| example.com | Cloudflare（95%）+ Cloudflare DNS（98%） | Cloudflare DNS 实测一致 | ✅ 正确 |
| cloudflare.com | Cloudflare（95%）+ Astro（46%）+ Zendesk | 自建 CDN + Zendesk 支持属实 | ✅ 基本正确 |
| github.io | **GitHub Pages**（80%）+ jQuery（72%） | 真 GitHub Pages | ✅ 正确（证明规则在真 GH Pages 上能命中） |
| apple.com | **空**（0 项技术） | `server: Apple`，自有 CDN，React 站点 | ❌ 漏检 |
| bilibili.com | **空**（0 项技术） | `server: Tengine`（阿里系），Vue | ❌ 漏检 |
| openai.com | PARTIAL（部分完成） | — | ⚠️ 部分态 |

### 4.2 误报根因（已定位）
1. **“GitHub Pages”规则过度泛化**：技术详情页 `/technology/github-pages` 明确写着检测方式 = `HTTP header` + `Asset URL`。实测 **github.com 与 microsoft.github.io 都返回 `server: GitHub.com`**，且都从 `github.githubassets.com` 出资源。任何“Server 含 GitHub.com”的规则都会把 github.com 主站、乃至任何 GitHub 自家域名误标为“GitHub Pages”。github.com 被给到 80% 置信度并输出结论“**这是一个很可能使用 GitHub Pages 的网站**”。
2. **分类自相矛盾**：`/technology/github-pages` 的分布页显示“使用 GitHub Pages”的 66 个站点中，有 13 个在 **Pantheon（AS54113）**、9 个在 **A10 ROW（AS16509）** 网络上——GitHub Pages 不可能跑在 Pantheon 的 ASN 上。这说明该“技术类别”把多种不相关信号揉在了一起。
3. **DNS 服务商识别是弱启发式**：对未知 nameserver（如 `ns1.wordpress.org`、`ns3.dnsv5.com`）直接**把 nameserver 域名当服务商名**，置信度 70%。已知服务商（Cloudflare DNS 98%、Route 53 98%、NS1 90%）能正确识别，但覆盖有限。

### 4.3 Observed / Detected / Inferred / Unknown 分类评估
SiteIntel **没有在 UI 中区分这四类**，这是可信度问题的结构性原因：

| 分类 | 定义 | SiteIntel 现状 |
|---|---|---|
| Observed | 直接观察到的事实 | TLS issuer、HTTP 状态码、DNS NS 记录 —— 这些可靠 ✅ |
| Detected | 规则直接命中 | “GitHub Pages / Heroku / CloudFront” —— 规则过宽导致误报 ❌ |
| Inferred | 多特征推断 | hosting 判断、DNS 服务商 —— 弱启发式（nameserver 当服务商）⚠️ |
| Unknown | 无法确认 | 被 CDN 隐藏的源站（“Hidden behind CDN” 90%）—— 这个反而诚实 ✅ |

**关键判断：“检测到某技术” ≠ “该技术真实存在”。** 用户看到的只是规则命中，且 UI 用“很可能 / 较强 / 80%”等强语言包装规则命中，没有把“这是 Observed 还是 Inferred”告诉用户。

---

## 5. Evidence / Confidence

### 5.1 现状盘点
- 技术项：`name + category + confidence(0-100) + evidence[]`，evidence 为 `{source, detectedAt, signal}`，signal 只有 `http_header / asset_url / cookie / html_pattern` 这类**类型标签**，无规则值、无原始内容。
- 发现项（值得关注）：`title + summary + confidence + importance + evidence[]`，evidence 为 `{eventType, detectedAt}`，**同样无任何原始数据**。示例：github.com 的“同轮多事实复合变化（88%，high）”证据只有 `{"eventType":"infrastructure_migration","detectedAt":"..."}`。
- “为什么这样判断”的文案模板存在。
- **原始检测记录（真正的证据）被登录墙挡住**：“原始检测记录 · 登录后查看完整数据与原始检测记录”“登录后查看完整变化时间线”。

### 5.2 结论
- **证据不透明**：匿名用户只能看到“证据存在（1 条）”和“signal 类型”，看不到证据内容。所谓“全部带证据链”与匿名可见不一致。
- **置信度语义未定义**：高/较强/可能/弱 四档与 0-100 分数并存，但没有说明阈值与“强证据/弱证据”的定义。用户不知道 80% 意味着什么。
- **低置信度容易被当真**：astro 46% 这种低置信度项与 95% 项同列表展示，没有“仅供参考”的视觉弱化；而误报项（GitHub Pages 80%）反而用了“很可能”的强结论措辞。
- **证据不足却下强结论**：github.com 的“很可能使用 GitHub Pages”是规则误命中 + 强措辞的组合，是“Evidence→Claim→Confidence→Conclusion”链条断裂的典型。

---

## 6. 技术架构黑盒推断

### Confirmed（实测确认）
- **前端**：Next.js（App Router，RSC/Server Components）——响应头 `vary: rsc, next-router-state-tree...`、`/_next/static/chunks/*`、turbopack 构建产物。SSR 首屏 + 客户端水合。
- **CSS/UI**：Tailwind CSS（`text-4xl sm:text-5xl` 等原子类）+ 自定义设计 Token（`text-accent/bg-panel-2` 等）。
- **边缘/托管**：Cloudflare（`server: cloudflare`、`cf-ray`、NEL `report-to`、robots.txt 为 Cloudflare-managed content signals）；源站为 nginx（部分路径 `server: nginx`）。**同一站点不同路径分别暴露 `server: nginx` 与 `server: cloudflare`，存在 nginx 源 + Cloudflare 前置的双层结构。**
- **分析任务模式**：异步任务 + 进度拉取 + **SSE 推送**（`GET /api/analyze/{id}/stream`，事件 `event: progress / data: {...}`）。任务分 10 步：validate → dns → ip → ssl → http → technology → infrastructure → snapshot(历史) → findings → report。
- **认证**：匿名一次性 `streamToken`；`GET /api/analyze/{id}?token=`；登录用户走会话；`/api/me` 返回 `{"user":null}`（匿名）；付费 API `/api/v1/*` 需 `X-API-Key`，密钥服务端只存哈希。
- **授权上下文**：图/关系视图使用 **HS256 签名 JWT（gctx）**，10 分钟过期（`iat/exp` 差 600s），payload 含 `mode:graph`、entry/current 实体 key、jti——短时签名令牌做深链授权。
- **导出**：`/api/export`（POST，405 于 GET），有“今日导出次数已达上限”的每日限额。
- **实体 ID**：investigationId 形如 `cmtge...`（CUID 风格），说明有稳定 ID 生成与存储。

### Likely（较可能）
- 后端语言：Next.js API routes + 支持 SSE 的运行时（Node 系可能性高）；分析执行器可能是独立 worker。
- 数据存储：关系型/文档库存域名快照与历史（变化检测需要时序快照）；`/technology/*` 的“66 个观测站点/首次观测/末次观测”说明有聚合统计存储。
- 反爬/限流：Cloudflare 层存在（观察到快速轮询时 403），但 `POST /api/analyze` 连续 6 次未触发限流。

### Possible（可能）
- 任务队列（Redis/队列）做异步编排；对象存储存报告 JSON。

### Unknown
- 指纹规则引擎实现方式（规则文件/DB）、检测是否复用开源规则库（Wappalyzer 等）——从“GitHub Pages”误报模式看，规则质量不高，但无法确认来源。

---

## 7. 安全与可靠性（仅非侵入式）

| 项 | 结果 |
|---|---|
| HTTPS/TLS | ✅ TLS 1.3，TLS_AES_256_GCM_SHA384，证书有效 |
| HSTS | ✅ `max-age=31536000`（**未带 includeSubDomains/preload**） |
| CSP | ❌ **缺失**（首页与 API 均无） |
| X-Frame-Options / X-Content-Type-Options / Referrer-Policy / Permissions-Policy | ❌ **全部缺失** |
| Cookie | 匿名访问**无 Set-Cookie**（隐私友好，会话用 token） |
| CORS | ✅ 收紧：OPTIONS 返回 204 但**无 Access-Control-Allow-Origin** |
| 输入校验 | ✅ 良好：IP/乱码/空值/非法 JSON 均返回明确错误（`Not a valid domain`/`Invalid JSON body`/`Missing domain`） |
| 错误信息泄露 | ✅ 无堆栈/无内部路径泄露 |
| API 暴露 | `POST /api/analyze` **匿名可用**（产品设计如此），但**未观察到限流**（连续 6 次全 200）→ 存在被滥用/刷任务的面（成本面） |
| 信息泄露 | 报告 URL `/website/{domain}` 公开（产品设计），聚合统计公开；未发现隐私数据泄露 |
| 观察到的保护 | 快速轮询状态接口时出现过 403（Cloudflare/应用层 bot 防护存在但阈值较松） |

**一句话**：传输与输入校验扎实、CORS 收紧、无隐私泄露；但对一个“安全/情报”向产品，**安全响应头（CSP/XFO/CTO）大面积缺失**且**匿名分析接口无可见限流**，属明显的产品级短板。

---

## 8. 最终产品评价

### 8.1 评分（10 分制）

| 维度 | 得分 | 一句话理由 |
|---|---|---|
| A. 产品定位 | **7.0** | 差异化方向对（变化监测+关系图+中文），但“看懂任何网站”野心大于底座能力 |
| B. UX | **7.5** | 路径顺、10 步进度反馈是亮点、空/错态干净；深数据被登录墙削弱 |
| C. UI | **8.0** | 深色设计系统统一、层级专业、中英完整；小瑕疵：`服务商·35` 等费解标签、置信度表达混用 |
| D. 技术可信度 | **5.0** | github.com/netflix 误报确凿、bilibili/apple 漏检、DNS 弱启发式、无 Observed/Detected/Inferred 区分 |
| E. Evidence 体系 | **5.5** | 有置信度+证据标签+“为什么判断”，但证据无原始值、原始记录登录墙、强结论配弱证据 |
| F. 移动端 | **6.5** | 基础响应式+汉堡菜单，但桌面优先，长报告未做移动精简（代码级评估） |
| G. 产品成熟度 | **6.5** | 前端/API/商业化框架成熟度高，但观测历史仅 16 天、指纹引擎年轻、功能门禁多 |

### 8.2 回答问题

**1. 现在最大的优势是什么？**
产品形态与体验领先于同体量竞品：10 步可视化分析流程、清晰的中英双语报告、变化检测 + 关系图谱的差异化定位、整洁的 API（异步 + SSE + 哈希密钥）。它在“把网站情报做成一个能持续监控、能讲故事的产品”这件事上，方向和执行都比“又一个指纹检测器”高一个段位。

**2. 最大的问题是什么？**
**指纹引擎的可信度。** 最底层的检测结果出现系统性误报（GitHub Pages 规则把任何 GitHub 域名都标成 GitHub Pages；netflix 被标 GitHub Pages + Heroku）和漏检（apple/bilibili 空结果），而整个产品叙事建立在这些结果之上。地基不牢，上面的变化检测/关系图/洞察都跟着失真——**关系的“共享基础设施关联”很可能是建立在误报标签上的**（github.com 的 10 个“关联域名”全是 GitHub Pages 站点，即由误报驱动）。

**3. 最容易误导用户的地方是什么？**
把“规则命中”包装成事实：误报的 GitHub Pages 配 80% 置信度 + “这是一个**很可能**使用 GitHub Pages 的网站”的强结论；把 github.com 与一堆不相干的 github.io 站点列为“关联域名”；把仅 16 天观测窗口内的变化标为 high 重要性“值得关注”。用户会把这些当权威情报带走。

**4. 最值得优先解决的 5 个问题？**
1. **修复 GitHub Pages / Heroku / CloudFront 等过度泛化规则**，至少区分“GitHub 托管”与“GitHub Pages 产品”，并回测 github.com/netflix 误报；
2. **公开原始证据**：在报告中直接展示 HTTP 头原文、命中 URL、前后快照对比，而不是 `{eventType, detectedAt}` 标签；
3. **引入 Observed / Detected / Inferred 分类标注**，并在结论措辞上做等级约束（Inferred 不得用“很可能/确定”这类强词）；
4. **补齐安全响应头（CSP、X-Frame-Options、X-Content-Type-Options）并为匿名分析接口加可见限流**；
5. **统一置信度表达**（分数/百分比/中文档位三选一），并公开置信度定义与阈值。

**5. 如果你是产品负责人，下一阶段会做什么？**
先止血可信度：做一轮基于“已知真值站点集”的指纹回归测试（Wappalyzer 规则集可对照），把误报率压下来并公开准确率指标——这是情报产品的立身之本。随后把“变化检测 + 关系图”两个差异化卖点做深（例如：变更订阅提醒、真实的关系解释文案），而不是继续扩 SEO 页面。最后补齐安全头与限流，再考虑付费墙的分层（把原始证据留在免费层以建立信任）。

**6. 与 Wappalyzer / BuiltWith 的核心差异？**
不在指纹识别（此处它是劣势），而在**三个叠加的产品层**：① 随时间的变化检测与“值得关注”发现（Wappalyzer 无）；② 跨站点共享基础设施关系图（BuiltWith 需付费且无图叙事）；③ 面向中文用户的完整本地化与报告化表达。也就是说，它卖的是“情报产品”，Wappalyzer/BuiltWith 卖的是“技术查询工具”。

**7. 是否已具备独立产品竞争力？**
**具备形态、尚不具备数据竞争力。** 前端、API、商业化框架、文案包装已经达到可卖的状态；但如果对手（Wappalyzer Pro / BuiltWith / 中文的同类如 站长工具类）把变化监测补上，而 SiteIntel 的误报问题不解决，差异化会被快速抹平。当前竞争力建立在“体验好 + 中文 + 免费”，而非“数据准”。

**8. 哪些地方需要重新设计，而不是继续修补？**
- **指纹规则引擎**：不是补一条规则，而是重做成“可解释、分证据等级、带回归测试”的引擎（这是全局根因）；
- **证据/报告的数据模型**：把“证据”从标签升级为可展开、可导出、带原始值的结构化证据链（UI 和 API 都要动）；
- **移动端报告体验**：长报告需要移动专用的信息架构（tab 化分区），不是堆响应式断点；
- **导航信息架构**：“探索 → /report”名不副实、/analyze 404、大量 SEO 页不可达，需要一次真正的 IA 梳理；
- **可信度叙事**：首页“看懂任何一个网站”的承诺需要配一个“准确性/覆盖率”的诚实面板，否则承诺与体验落差会反噬品牌。

---

## 附录：关键可观察证据清单

- 首页 H1：`看懂任何一个网站`；EN：`Understand any website`
- 分析流程 10 步文案（检查域名/检测DNS记录/解析IP与网络/检测SSL证书/访问网站/检测使用的技术/整理基础设施信息/对比历史变化/生成分析发现/生成分析报告）
- github.com 误报链路：报告页“GitHub Pages 80%”→ 证据 `signal: http_header / asset_url` → 结论“这是一个**很可能**使用 GitHub Pages 的网站” → 关联域名 10 个（opendifferentialprivacy.github.io、microsoft.github.io…）均标“共享基础设施·服务商35”
- 独立真值：github.com `server: github.com` + TLS issuer `Sectigo Public Server Authentication CA DV E36`（TLS 检测正确）；microsoft.github.io `server: GitHub.com`
- netflix.com 误报：GitHub Pages 80% + Heroku 80%；真值 `via: 2 i-... (us-west-2)`（AWS）
- wikipedia.org：Amazon CloudFront 85%；真值 DNS `199.16.158.12`（Wikimedia 自有 Varnish CDN）
- apple.com / bilibili.com：技术栈空；真值 `server: Apple` / `server: Tengine`
- `/technology/github-pages`：Detection=HTTP header + Asset URL；Adoption 66 站点（2026-08-14 首次观测）；Distribution AS54113 Pantheon ×13、AS16509 A10 ×9（分类内部矛盾）
- API：`POST /api/analyze` 匿名可用（连续 6 次 200 无限流）；`GET /api/analyze/{id}?token=`；SSE `.../stream`；`/api/me`→`{"user":null}`；`/api/v1` 需 X-API-Key；`/api/export` 405
- 授权：gctx JWT（HS256，10 分钟过期，mode=graph）
- 安全头：仅 HSTS（max-age=31536000，无 includeSubDomains）；无 CSP/XFO/CTO/Referrer-Policy；TLS1.3
- robots.txt：Cloudflare-managed Content Signals（search=yes, ai-train=no, use=reference）

*报告完*
