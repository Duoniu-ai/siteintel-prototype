# SiteIntel 2.0 UI Prototype — Master Plan

> 独立原型仓库：`Duoniu-ai/siteintel-prototype`
> 正式项目：`Duoniu-ai/siteintel`（本仓库不得修改正式项目代码）

## 1. 原型目标

本仓库用于完整规划、设计、制作和验证 SiteIntel 2.0 的 UI/UX。第一阶段全部使用静态 HTML/CSS/JS + Mock Data，不连接生产 API、不修改数据库。

设计参考采用三类产品：

- **LMSpeed**：视觉语言、信息密度、颜色、字体层级、卡片与布局节奏。
- **BuiltWith**：网站技术画像、技术情报、关系与数据深度表达。
- **Wappalyzer**：Technology Stack、Lookup、分类、搜索和产品化信息结构。

最终目标不是复制任何参考站，而是形成 SiteIntel 自己的 Website Intelligence 产品体系。

## 2. 核心产品原则

1. 首页首先解决“分析一个网站”，而不是展示后台统计。
2. 所有页面围绕 Website、Technology、IP、ASN、Entity、Evidence、Intelligence 展开。
3. 数据只是基础，页面最终要表达关系、证据和洞察。
4. 保持专业的数据产品感，但避免传统后台系统的臃肿感。
5. Desktop 与 Mobile 同时设计。
6. 每个页面先定义信息架构，再定义组件，再写 Mock 数据，再制作视觉稿。
7. 不产生无意义的页面；每一个路由必须有明确用户任务。

## 3. 页面地图

### P0 — 核心产品页面

| ID | 页面 | 路由示例 | 目的 |
|---|---|---|---|
| P0-01 | 首页 | `/` | 输入域名并理解 SiteIntel 能做什么 |
| P0-02 | 网站分析结果 | `/website/example.com` | 网站完整 Intelligence 总览 |
| P0-03 | 技术栈 | `/website/example.com/technologies` | 技术分类、版本、证据 |
| P0-04 | 技术详情 | `/technology/wordpress` | 技术本身及使用网站关系 |
| P0-05 | IP 详情 | `/ip/1.2.3.4` | IP、ASN、托管及关联网站 |
| P0-06 | ASN 详情 | `/asn/AS13335` | ASN 网络画像及关联实体 |
| P0-07 | 搜索 | `/search?q=...` | 全局 Website/Technology/IP/ASN 搜索 |

### P1 — Intelligence / 深度分析页面

| ID | 页面 | 路由示例 | 目的 |
|---|---|---|---|
| P1-01 | Website Intelligence | `/website/example.com/intelligence` | 从数据生成可读洞察 |
| P1-02 | Evidence | `/website/example.com/evidence` | 展示结论对应证据链 |
| P1-03 | Relations | `/website/example.com/relations` | Website ↔ IP ↔ ASN ↔ Technology 关系 |
| P1-04 | SEO | `/website/example.com/seo` | SEO/robots/sitemap/页面结构画像 |
| P1-05 | Security | `/website/example.com/security` | 安全信号与暴露面概览；原型阶段不做攻击功能 |
| P1-06 | Infrastructure | `/website/example.com/infrastructure` | DNS/CDN/Hosting/Network 结构 |
| P1-07 | Technology Market | `/technology` | 技术目录、分类、趋势与市场信息 |
| P1-08 | IP Websites | `/ip/1.2.3.4/websites` | 查看一个 IP 关联的网站 |
| P1-09 | ASN Websites | `/asn/AS13335/websites` | 查看 ASN 关联的网站 |

### P2 — 产品辅助页面

| ID | 页面 | 路由示例 | 目的 |
|---|---|---|---|
| P2-01 | Compare | `/compare` | 两个网站的技术/基础设施对比 |
| P2-02 | Reports | `/reports` | 已保存分析报告/示例报告 |
| P2-03 | About / Methodology | `/methodology` | 解释数据来源、证据和分析方法 |
| P2-04 | Pricing | `/pricing` | 后续商业化预留 |
| P2-05 | 404 | `/404` | 正常产品错误状态 |

## 4. 每个页面的制作顺序

每个页面严格经过：

**需求定义 → 用户任务 → 信息架构 → Wireframe → 组件 → Mock Data → Desktop → Mobile → 交互 → Review → 固化**

未完成上一阶段，不直接堆视觉代码。

## 5. P0 页面设计要求

### P0-01 首页

- 主标题：理解任何网站 / Understand any website
- 主输入框：URL / Domain
- CTA：Analyze
- 示例网站快捷入口
- 产品能力分区
- Website Intelligence / Technology / Infrastructure / SEO / Security / Relations
- 最新分析或示例数据
- 不显示传统后台 Dashboard 的 KPI 墙

### P0-02 Website Analysis

顶部：Domain、状态、最后更新时间、重新分析。

核心区：
- Overview
- Technology Stack
- IP / ASN
- DNS / CDN / Hosting
- SEO
- Security signals
- Intelligence summary
- Evidence preview

必须让用户第一眼知道：**这个网站是什么、用了什么、在哪里、和谁有关、有什么值得注意。**

### P0-03 Technology Stack

- Technology categories
- Technology name/version
- Confidence
- Evidence
- First/last observed（Mock）
- 分类筛选
- 搜索
- 点击进入 Technology Detail

### P0-04 Technology Detail

- 技术简介
- 分类
- 当前检测信息
- 使用该技术的网站数量（Mock）
- Technology relationships
- Popular websites
- 地区/行业示例
- 变化趋势（Mock）

### P0-05 IP Detail

- IP 基础信息
- ASN
- Organization
- Location
- Hosting/CDN
- Reverse relationships
- Associated websites
- Evidence

### P0-06 ASN Detail

- ASN 基础信息
- Organization
- Country/Region
- Prefix/Network 概览（Mock）
- Associated IPs
- Associated websites
- Technology signals

### P0-07 Search

统一搜索 Website / Technology / IP / ASN。

搜索结果必须明确 Entity Type，避免不同实体混在一起。

## 6. 视觉系统

建立统一 Design Tokens：

- Font family
- Font sizes
- Heading scale
- Font weights
- Backgrounds
- Surface/card
- Borders
- Text hierarchy
- Accent
- Success / Warning / Danger
- Radius
- Shadow
- Spacing
- Container width
- Table density
- Mobile breakpoints

参考 LMSpeed 的视觉节奏，但不得直接复制品牌资产。

## 7. 统一组件

首批组件：

- Global Header
- Mobile Navigation
- Search Bar
- Domain Analyzer
- Breadcrumb
- Page Header
- Entity Badge
- Status Badge
- Metric Card
- Technology Card
- Technology Table
- Entity Table
- Evidence Card
- Intelligence Card
- Relationship Graph Placeholder
- Timeline
- Filter Bar
- Tabs
- Pagination
- Empty State
- Loading State
- Error State
- Footer

## 8. Mock 数据规范

Mock 数据必须看起来像真实 SiteIntel 数据，但明确属于原型数据。

至少准备：

- 3 个 Website
- 10+ Technology
- 5 个 IP
- 3 个 ASN
- Website ↔ Technology 关系
- Website ↔ IP 关系
- IP ↔ ASN 关系
- Evidence
- Intelligence findings
- SEO signals
- Security signals

所有页面共享 Mock 数据源，避免每个页面各写一套互相矛盾的数据。

## 9. 验收标准

### 页面层

- 页面有明确任务
- 信息层级清晰
- Desktop 正常
- Mobile 正常
- 页面之间可点击
- Mock 数据一致

### 产品层

- 用户不会把 SiteIntel 误认为普通 WHOIS/DNS 工具
- Website 是核心入口
- Technology/IP/ASN 是实体，不是孤立工具
- Intelligence 和 Evidence 有明确位置
- 不出现大量无意义 KPI

### 工程层

- 静态原型不依赖生产数据库
- 不写生产密钥
- 不修改 `Duoniu-ai/siteintel`
- CSS/JS 可复用
- 页面文件按路由组织
- Mock Data 独立

## 10. 开发顺序

**Phase A：Foundation**
- Design Tokens
- Global Layout
- Header/Nav
- Mock Data
- Base Components

**Phase B：P0 Core**
- Home
- Search
- Website Overview
- Technology Stack
- Technology Detail
- IP Detail
- ASN Detail

**Phase C：P1 Intelligence**
- Intelligence
- Evidence
- Relations
- SEO
- Security
- Infrastructure
- Entity relationship pages

**Phase D：P2 Product**
- Compare
- Reports
- Methodology
- Pricing
- 404

**Phase E：Polish**
- Mobile
- Empty/loading/error states
- Accessibility
- visual consistency
- interaction review

## 11. 当前状态

本文件是原型开发的总规划基线。后续每制作一个页面，应在对应页面文档中记录：目标、用户任务、信息架构、组件、Mock 数据、桌面布局、移动布局、交互和验收标准。
