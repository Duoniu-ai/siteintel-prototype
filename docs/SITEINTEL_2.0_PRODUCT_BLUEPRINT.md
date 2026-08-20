# SiteIntel 2.0 产品蓝图

> 全球网站数据智能平台（Website Data Intelligence Platform）
>
> 本文用于提供给 Claude Code / AI Coding Agent，作为 SiteIntel 2.0 静态原型设计与实现的产品蓝图。
>
> **核心原则：本文定义“SiteIntel 2.0 是什么、解决什么问题、有哪些能力”，不规定具体 UI 风格。UI/UX、信息架构、桌面端/移动端布局由 Claude 根据本蓝图自行设计。**

---

## 一、产品定义

SiteIntel 2.0 是一个：

> **全球网站数据智能平台（Website Data Intelligence Platform）**

SiteIntel 不只是一个网站检测工具，也不是单纯的 WHOIS、DNS、IP、ASN、SEO、技术识别、安全检测或网站评分工具。

这些只是 SiteIntel 的数据来源和基础能力。

SiteIntel 真正要做的是：

> **围绕全球网站建立数据采集、实体识别、关系发现、历史追踪、证据关联和数据洞察能力。**

用户输入一个网站以后，SiteIntel 不只是告诉用户“这个网站怎么样”，而是让用户逐渐看到：

> **这个网站是什么、由什么组成、运行在哪里、使用了什么技术、连接了什么基础设施、属于什么网络、与哪些网站存在关系、这些数据过去发生过什么变化，以及这些数据之间有什么联系。**

---

## 二、核心思想

SiteIntel 的核心不是：

```text
网站
↓
评分
```

而是：

```text
网站
↓
数据
↓
实体
↓
关系
↓
证据
↓
历史
↓
分析
↓
洞察
```

原则：

> **真实数据优先于评分。**

如果数据可以直接展示真实参数，就不要用抽象分数代替。

例如不要：

```text
安全：92分
```

而是：

```text
TLS 1.3
HSTS 已启用
CSP 存在
X-Content-Type-Options 已启用
```

不要：

```text
性能：87分
```

而是：

```text
TTFB：420ms
LCP：2.1s
INP：180ms
CLS：0.04
页面大小：1.82MB
请求数量：68
```

---

## 三、核心数据实体

### 1. Website

网站是最核心实体，包括：

- Domain
- Registrable Domain
- Host
- URL
- 页面
- 状态
- HTTP / HTTPS
- HTTP/2 / HTTP/3
- 页面标题
- Meta
- SEO
- 技术
- IP
- ASN
- DNS
- TLS
- Cookie
- Header
- Script
- 图片
- 链接
- 国家/地区
- 服务商
- 历史变化

### 2. Technology

识别网站使用的技术，例如：

- React
- Vue
- Angular
- Next.js
- WordPress
- Laravel
- Nginx
- Apache
- Cloudflare
- Google Analytics
- Google Tag Manager
- Bootstrap
- Tailwind
- Redis
- MySQL
- PostgreSQL

技术实体包括：

- 技术名称
- 技术类别
- 厂商
- 版本
- 首次发现
- 最近发现
- 当前状态
- 检测位置
- 检测证据
- 使用该技术的网站数量
- 相关技术
- 历史变化

技术不是简单标签，而是可继续探索的实体：

```text
网站
↓
React
↓
React 技术实体
↓
使用 React 的网站
↓
其他网站
```

### 3. IP

支持 IPv4 / IPv6。

数据包括：

- IP 地址
- IP 版本
- ASN
- BGP Prefix
- ISP
- Organization
- Country
- Region
- City
- Latitude
- Longitude
- Reverse DNS
- DNS 关系
- 网站数量
- 首次发现
- 最近发现
- 历史变化

一个 IP 可以关联多个网站。

```text
Website
↓
IP
↓
其他 Website
```

### 4. ASN

包括：

- ASN
- 组织
- ISP
- 国家
- BGP Prefix
- IP 数量
- 网站数量
- 首次发现
- 最近发现
- 历史变化

关系：

```text
ASN
↓
IP
↓
Website
```

以及：

```text
Website
↓
ASN
↓
其他 Website
```

### 5. Provider / 服务商

包括 Cloudflare、AWS、Google Cloud、Azure、Alibaba Cloud、Tencent Cloud、DigitalOcean、Fastly、Akamai、OVH、Hetzner 等。

服务商可以关联：

- 网站
- IP
- ASN
- 国家
- 技术

形成：

```text
服务商
↓
IP
↓
网站
```

以及：

```text
服务商
↓
网站
↓
技术
```

### 6. 国家 / 地区

SiteIntel 是国际业务，从一开始支持全球国家和地区。

至少支持：

- 国家
- 地区
- 城市
- 国家代码
- 地区代码
- 经纬度

最终支持全球国家和地区，并作为独立实体：

```text
国家
↓
网站
↓
IP
↓
ASN
↓
服务商
↓
技术
```

---

## 四、DNS

支持：

- A
- AAAA
- CNAME
- MX
- NS
- TXT
- SOA
- CAA
- PTR
- SRV
- 其他记录

每条 DNS 记录保留：

- 类型
- 名称
- 值
- TTL
- 查询时间
- 数据来源
- 历史变化

例如：

```text
example.com
↓
A
↓
104.18.32.10
```

---

## 五、TLS / SSL

采集：

- TLS 版本
- 证书
- 证书链
- 签发机构
- 签发时间
- 到期时间
- SAN
- 指纹
- 公钥信息
- 加密套件
- HTTP/2
- HTTP/3

并支持历史变化，例如：

```text
证书 A
↓
证书 B
```

---

## 六、HTTP 数据

采集：

- 状态码
- Response Headers
- Request Headers
- Content-Type
- Content-Length
- Cache-Control
- Server
- Location
- ETag
- Last-Modified
- Set-Cookie
- CSP
- HSTS
- X-Frame-Options
- X-Content-Type-Options
- Referrer-Policy

这些都是原始数据。

---

## 七、SEO

### 页面

- Title
- Description
- Keywords
- Canonical
- Robots
- H1 / H2 / H3
- 图片
- ALT
- 内链
- 外链

### 网站

- robots.txt
- sitemap.xml
- Sitemap 数量
- Canonical 结构
- Hreflang
- Schema
- Open Graph
- Twitter Card

### 可索引性

分析：

- 是否允许抓取
- 是否允许索引
- robots 规则
- canonical
- noindex
- redirect
- soft 404
- 状态码

SEO 是数据分析能力之一，不要变成“SEO 评分工具”。

---

## 八、性能

支持：

- DNS 时间
- TCP 连接
- TLS 握手
- TTFB
- FCP
- LCP
- INP
- CLS
- 页面大小
- HTML 大小
- CSS 大小
- JS 大小
- 图片大小
- 请求数量
- 资源数量

这些都是具体参数。

---

## 九、安全

采集：

- TLS
- HSTS
- CSP
- CORS
- Cookie 安全属性
- Security Headers
- Mixed Content
- Redirect
- 证书
- DNS 安全相关信息

目标：

> **展示事实、发现问题、提供证据。**

而不是简单给网站一个“安全分”。

---

## 十、网站关系

这是 SiteIntel 2.0 的核心能力之一。

例如：

```text
网站 A
↓
IP
↓
ASN
↓
其他网站
```

或者：

```text
网站 A
↓
Cloudflare
↓
其他网站
```

或者：

```text
网站 A
↓
React
↓
其他 React 网站
```

或者：

```text
网站 A
↓
同一 IP
↓
网站 B
网站 C
网站 D
```

最终逐渐形成：

> **全球网站关系网络。**

---

## 十一、历史数据

不能只保存当前状态，应逐渐保存：

- 首次发现
- 最近发现
- IP 变化
- ASN 变化
- DNS 变化
- 技术变化
- 技术版本变化
- TLS 变化
- 页面变化
- Header 变化
- SEO 变化
- 国家变化
- 服务商变化

例如：

```text
2026-08-01
发现 React

2026-08-08
IP 变化

2026-08-12
新增 Cloudflare

2026-08-18
React 版本变化

2026-08-20
TLS 证书更新
```

这样 SiteIntel 才具有真正的“情报”属性。

---

## 十二、证据体系

任何重要识别结果都应该能够回答：

> **你为什么知道？**

例如：

```text
React
↓
HTML
↓
Script
↓
React 相关代码
↓
检测时间
```

或者：

```text
网站
↓
DNS
↓
IP
↓
ASN
```

证据可以来自：

- HTML
- HTTP Headers
- DNS
- TLS
- JavaScript
- Cookie
- URL
- 页面内容
- 网络数据
- 外部数据源

最终形成：

> **Evidence Chain / 证据链**

---

## 十三、数据来源

可以整合多个公开数据源，例如：

- DNS
- RDAP
- WHOIS
- BGP
- ASN
- IP Geo
- Cloudflare Radar
- Globalping
- 网站 HTTP 探测
- 技术指纹
- 自有发现系统

这些只是数据来源，最终统一进入 SiteIntel 数据模型。

用户看到的是：

> **SiteIntel 的统一数据。**

---

## 十四、探索系统

核心理念：

> **任何重要实体都应该可以继续探索。**

例如：

```text
example.com
↓
IP
↓
ASN
↓
服务商
↓
国家
↓
其他网站
```

或者：

```text
example.com
↓
技术
↓
React
↓
其他使用 React 的网站
```

或者：

```text
example.com
↓
DNS
↓
IP
↓
其他 DNS 关系
```

用户应该能够不断向下探索。

---

## 十五、搜索

搜索未来不应该只支持域名，应逐步支持：

- 网站
- Domain
- URL
- IP
- IPv6
- ASN
- 技术
- 服务商
- 国家
- 城市

例如：

`104.18.32.10` → IP 实体

`AS13335` → ASN 实体

`React` → 技术实体

---

## 十六、对比

支持网站之间的数据对比：

```text
网站 A
vs
网站 B
```

比较：

- 技术
- IP
- ASN
- 服务商
- 国家
- DNS
- TLS
- SEO
- 性能
- HTTP
- 历史变化

重点是：

> **展示数据差异，而不是简单给两个网站评分。**

---

## 十七、报告

未来可生成网站数据报告，包括：

- 网站概览
- 基础设施
- 技术
- DNS
- TLS
- SEO
- 性能
- 安全
- 历史
- 关联实体
- 证据

报告最终回答：

> **这个网站到底是什么？**

> **它运行在哪里？**

> **它使用什么？**

> **它和谁有关？**

> **过去发生过什么？**

> **我们有什么证据？**

---

## 十八、国际化

SiteIntel 是国际业务，从产品架构开始支持多语言。

第一阶段：

- 中文
- English

未来可以继续：

- 日语
- 韩语
- 西班牙语
- 法语
- 德语
- 葡萄牙语
- 等

默认产品 UI：

> 中文

但数据模型不能依赖中文。国家、城市、技术、服务商等实体应具有国际化能力。

---

## 十九、桌面 / 手机

必须支持：

- 桌面
- 平板
- 手机

不是简单缩小页面，而应根据设备改变信息布局。

桌面强调：

> **信息密度 + 多实体同时观察**

手机强调：

> **单实体深入探索 + 连续下钻**

---

## 二十、产品价值

SiteIntel 最终不是告诉用户：

> “这个网站是 82 分。”

而是告诉用户：

> **“这个网站是什么。”**

并让用户继续看到：

- 它使用什么技术
- 它运行在哪些 IP
- 它属于哪个 ASN
- 它使用什么服务商
- 它位于哪个国家
- 它有哪些 DNS 数据
- 它的 TLS 是什么
- 它有哪些 SEO 数据
- 它有哪些性能参数
- 它和哪些网站存在关系
- 它过去发生过什么变化
- 这些判断有什么证据

最终：

> **用户可以从一个网站进入整个数据网络。**

---

## 二十一、未来方向

SiteIntel 2.0 是“天网”体系中的核心基础。

整体关系：

```text
天网
│
└── SiteIntel
      │
      ├── Website
      ├── Technology
      ├── IP
      ├── ASN
      ├── DNS
      ├── TLS
      ├── Provider
      ├── Country
      ├── Evidence
      ├── History
      └── Relationships
```

SiteIntel 现在是：

> **天网的早期种子和数据底座。**

未来可以继续向：

- 网站网络
- 技术网络
- IP 网络
- 基础设施网络
- 企业网络
- 国家网络
- 互联网关系网络

扩展。

---

## 二十二、给 Claude / AI Coding Agent 的任务

以上内容是：

> **SiteIntel 2.0 产品蓝图。**

请不要按照传统 SaaS Dashboard、SEO 工具、网站评分工具的思路设计 UI。

不要把本文理解为具体 UI 设计稿。

本文只定义：

- 产品定位
- 产品价值
- 数据体系
- 核心实体
- 数据关系
- 功能能力
- 探索逻辑
- 国际化方向
- 响应式产品要求
- 长期产品方向

**UI/UX、视觉语言、信息架构、页面布局、桌面端设计、移动端设计，由你根据这份产品蓝图自行设计。**

请自行完成：

1. SiteIntel 2.0 信息架构设计
2. UI/UX 设计
3. 桌面端布局
4. 平板布局
5. 手机布局
6. 网站探索体验
7. 实体下钻体验
8. 关系网络展示
9. 数据详情展示
10. 历史数据展示
11. 证据链展示
12. 中文 / English 国际化
13. 一致的模拟数据体系
14. 可实际点击探索的静态原型

---

## 二十三、原型数据要求

当前是静态原型，因此允许使用模拟数据。

但模拟数据必须：

- 看起来真实
- 逻辑一致
- 页面之间一致
- 实体关系一致
- 历史数据一致
- IP / ASN / Provider / Country 对应一致
- Technology 与 Website 对应一致

例如同一个 `example.com` 在不同页面出现时，必须保持相同的：

- IP
- ASN
- Provider
- Country
- Technology
- DNS
- TLS

禁止不同页面出现互相矛盾的数据。

---

## 二十四、评分原则

SiteIntel 2.0 不以评分作为产品核心。

禁止把页面设计成：

```text
安全 91
SEO 82
性能 76
技术 92
综合 88
```

优先展示：

> **真实参数、原始事实、实体关系、证据和历史。**

---

## 二十五、最终目标

SiteIntel 2.0 的最终体验应该是：

> 用户输入一个网站。

然后可以逐渐探索：

```text
网站
↓
基础数据
↓
技术
↓
IP
↓
ASN
↓
服务商
↓
国家
↓
DNS
↓
TLS
↓
SEO
↓
性能
↓
安全
↓
历史
↓
证据
↓
关联网站
↓
关联实体
↓
整个网站数据网络
```

最终形成：

> **一个可以探索全球网站、基础设施、技术、实体、关系、历史和证据的互联网数据智能平台。**
