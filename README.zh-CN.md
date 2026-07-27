# XiGuaAp6y3 Blog

这是一个基于 [Astro](https://astro.build) 和 Tailwind CSS 构建的个人博客，用于整理 Java、数据结构、JavaWeb、人工智能等学习笔记。

## 功能

- 响应式博客布局，支持桌面端和移动端
- 亮色、暗色模式和主题色切换
- 文章归档、分类、标签和站内搜索
- Markdown、数学公式、代码高亮和图片查看
- 固定顶部导航栏和文章目录
- 基于 GitHub Discussions 的 Giscus 评论
- RSS、站点地图和静态页面部署

## 快速开始

项目需要 Node.js 和 pnpm。

```bash
pnpm install
pnpm dev
```

开发服务器默认运行在 `http://localhost:4321`。

常用命令：

| 命令 | 作用 |
| --- | --- |
| `pnpm dev` | 启动开发服务器 |
| `pnpm build` | 构建生产文件到 `dist/` |
| `pnpm preview` | 预览生产构建结果 |
| `pnpm type-check` | 执行 TypeScript 检查 |
| `pnpm new-post <name>` | 创建新文章 |
| `pnpm format` | 格式化源码 |
| `pnpm lint` | 检查并修复源码格式 |

## 项目配置

网站基本信息、导航栏、个人资料、主题和许可证配置位于：

```text
src/config.ts
```

修改 `siteConfig`、`navBarConfig` 和 `profileConfig` 即可更新网站内容和导航。

## 创建文章

文章放在 `src/content/posts/` 下，每篇文章使用一个目录和 `index.md` 文件：

```text
src/content/posts/example/index.md
```

文章必须包含以下 frontmatter：

```yaml
---
title: 示例文章
published: 2026-07-27
description: 文章简介
image: ./cover.jpg
tags: [Java, 学习笔记]
category: Java
draft: false
---
```

将 `draft` 设置为 `true` 的文章不会在生产环境中显示。

## Giscus 评论

评论使用 GitHub Discussions，不需要 OAuth Secret。仓库需要公开，并在 GitHub 仓库中开启 Discussions，然后通过 [giscus.app](https://giscus.app/zh-CN) 获取配置。

复制 `.env.example` 为 `.env`，按需填写：

```env
PUBLIC_GISCUS_REPO=xiguaAp6y3/xiguaAp6y3.github.io
PUBLIC_GISCUS_REPO_ID=R_kgDOOOOaXQ
PUBLIC_GISCUS_CATEGORY=Announcements
PUBLIC_GISCUS_CATEGORY_ID=DIC_kwDOOOOaXc4DCDoK
```

Giscus 使用页面 URL 将文章与对应的 Discussion 关联。仓库 ID 和分类 ID 可以公开，不属于敏感信息。

## 目录结构

```text
src/
├─ components/       页面组件
├─ content/posts/    博客文章
├─ layouts/          页面布局
├─ pages/            Astro 路由
├─ plugins/          Markdown 插件
├─ styles/           全局样式
└─ config.ts         网站配置
public/              静态资源
astro.config.mjs     Astro 配置
```

## 部署

执行 `pnpm build` 生成静态文件，然后将 `dist/` 部署到 GitHub Pages、Vercel、Netlify 等静态托管平台。部署 GitHub Pages 时，请确保构建环境可以读取 `.env` 中的 Giscus 配置。

## 许可证

博客内容仅用于学习交流。项目基于开源博客模板构建，具体许可信息以仓库中的 `LICENSE` 文件为准。
