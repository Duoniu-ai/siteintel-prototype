# SiteIntel 2.0 Prototype — v0.2 Interaction Specification

## 本轮目标

把原型从“页面可跳转”提升到“实体可追踪、证据可验证、历史可回溯、关系可继续探索”。

## 1. Website 深层结构

`/website/:domain`

Tabs：
- Overview
- Technologies
- Infrastructure
- DNS
- TLS
- Security
- SEO
- Performance
- Geography
- Changes
- Evidence
- Relations
- History

每个 Tab 内的实体都必须可点击。

### Infrastructure

IP → IP Detail → ASN → Provider → Country → Datacenter → 关联 Websites。

### Technology

Technology → Technology Detail → Categories → Related Technologies → Websites → Countries → Adoption Trend → Evidence。

### DNS

Record → Record Detail → IP → ASN → Provider → Websites。

### Security

Finding → Finding Detail → Severity → Evidence → Observation → First Seen / Last Seen → Related Change。

### SEO

Finding → URL → Evidence → Recommendation → Related Page。

### Performance

Metric → Measurement → Region → Device → Time → Historical Trend。

## 2. Evidence Chain

所有重要结论必须能够展开：

`Finding → Evidence → Source → Observation → Timestamp → Confidence`

Evidence 类型：HTTP Header / HTML / DNS / TLS / WHOIS-RDAP / Certificate / Screenshot / Network / Historical Observation。

## 3. History / Changes

统一时间轴：

`Change → Entity → Before → After → Evidence → Time → Confidence`

支持：
- Technology added/removed
- IP changed
- ASN changed
- Provider changed
- DNS changed
- TLS certificate changed
- Security finding appeared/resolved
- SEO finding appeared/resolved
- Performance regression/improvement

## 4. Relationship Explorer

关系图支持：
- 点击节点
- 点击边
- 展开邻居
- 聚焦实体
- 返回上一层
- 路径追踪
- 关系类型筛选
- 时间范围筛选
- Confidence 筛选

核心路径：

`Website → Technology → Website`
`Website → IP → ASN → Provider`
`Website → Country → Provider`
`Website → Finding → Evidence`
`Website → Change → History`

## 5. Finding Detail

Finding 页面必须显示：
- Finding ID
- 类型
- Severity
- Score
- Status
- First Seen
- Last Seen
- Affected Entity
- Evidence Count
- Evidence Chain
- Recommendation
- Related Changes
- Related Websites

## 6. Global Search

Search 必须支持：
- Domain
- URL
- IP
- IPv6
- ASN
- Technology
- Provider
- Country
- Finding
- Certificate

结果显示 Entity Type、核心指标、最近更新时间，并允许直接进入 Detail。

## 7. International

第一版语言：中文、English、日本語、한국어。

国家维度必须支持全球国家，不以中国单一市场作为产品结构基础。

## 8. 移动端

移动端必须保持：
- Global Search
- Bottom Navigation
- Entity Detail
- Tabs 横向滚动
- Relationship Graph 可缩放
- Cards 优先
- Tables 横向滚动

## 9. 设计验收规则

任何显示数字都要考虑是否可点击；任何实体都必须能进入 Detail；任何 Finding 必须能追溯 Evidence；任何 Change 必须能进入 History；任何关系必须能继续 Explore。

## 10. 下一轮实现顺序

1. Website Tabs 真正可切换
2. Evidence Detail Drawer
3. Change Detail Drawer
4. Finding Detail
5. Relationship Explorer
6. IP/ASN/Provider 深层 Detail
7. Technology 深层 Detail
8. Country 深层 Detail
9. Search 结果 Detail
10. 国际化 UI
11. Mobile 深度交互
12. 全链路 Mock 数据验收
