# SiteIntel 2.0 Prototype

唯一预览入口：`https://duoniu-ai.github.io/siteintel-prototype/`

本仓库是 SiteIntel 2.0 的独立 UI/UX 原型仓库，不连接正式 SiteIntel 数据库。

## 统一规则

- `main/index.html` 是唯一首页与唯一 Pages 入口。
- 不再使用 `prototype-vXX.html` 作为独立首页。
- 所有版本继续在 `index.html` 内演进。
- 所有数据均为模拟数据。
- 所有实体应当可以继续点击探索。

## 当前交互模型

`Website → Technology / IP / ASN / Provider / Country → Detail → Evidence / History / Relations`

`Finding → Evidence → Source → Observation → Timestamp → Confidence`

`Change → Before / After → Evidence → History`

## 国际化

目标 UI：中文、English、日本語、한국어；数据结构按全球国家、地区、时区设计。

## GitHub Pages

`.github/workflows/pages.yml` 使用 GitHub Actions 发布 `main` 根目录。

如果 Pages 尚未启用，需要在仓库 Settings → Pages 中选择 **GitHub Actions** 作为 Build and deployment source；之后每次推送 `main` 自动部署。
