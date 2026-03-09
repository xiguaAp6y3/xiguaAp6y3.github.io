---
title: 苍穹外卖 Day1 学习笔记
published: 2026-03-09
description: "关于项目模块划分（sky-common、sky-pojo、sky-server）说明以及 Git 版本管理的学习笔记"
tags: ["Java", "Spring Boot", "项目实践", "Git"]
category: Java
draft: true
---

# 苍穹外卖 Day1 学习笔记

## 1. 项目模块说明

### 1.1 `sky-common` 模块
存放一些公共类，供其他模块使用。

### 1.2 `sky-pojo` 模块
提供各层传输或展示所需要的数据模型对象：

| 名称 | 说明 |
| ---- | ---- |
| **Entity** | 实体，通常和数据库中的表对应 |
| **DTO** | 数据传输对象，通常用于程序中各层之间传递数据 |
| **VO** | 视图对象，为前端展示数据提供的对象 |
| **POJO** | 普通 Java 对象，只有属性和对应的 getter 和 setter |

### 1.3 `sky-server` 模块
子模块中存放的是后端核心代码，包括：
- 配置文件
- 配置类
- 拦截器
- Controller
- Service
- Mapper
- 启动类等

---

## 2. Git 项目管理
- 使用 Git 进行版本控制与项目管理
- 设置 `.gitignore` 文件，忽略 `target` 目录等不需要上传的编译生成文件
- 设置本地仓库，并共享项目到 GitHub