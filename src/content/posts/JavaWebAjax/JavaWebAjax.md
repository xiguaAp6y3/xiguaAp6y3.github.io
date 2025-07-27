---
title: JavaWeb Ajax 学习笔记
published: 2025-07-28
description: JavaWeb 项目中 Ajax 与 Axios 的基础用法与示例。
tags: [JavaWeb, Ajax, Axios, 学习笔记]
category: 学习笔记
draft: false
---

# JavaWeb Ajax 学习笔记

> 日期：2025年7月28日

---

## 目录

- [1. 什么是 Ajax](#1-什么是-ajax)
- [2. XML 简介](#2-xml-简介)
- [3. 同步与异步](#3-同步与异步)
- [4. Axios 简介与用法](#4-axios-简介与用法)
- [5. Axios 请求示例](#5-axios-请求示例)
- [6. 总结](#6-总结)

---

## 1. 什么是 Ajax

- **Ajax**（Asynchronous JavaScript And XML）：一种在无需重新加载整个页面的情况下，能够与服务器交换数据并更新部分网页内容的技术。
- **核心作用**：
  - 异步数据交换
  - 局部刷新页面
  - 提升用户体验
- **常见应用**：搜索联想、表单校验、动态内容加载等。

---

## 2. XML 简介

- **XML**（Extensible Markup Language）：可扩展标记语言，是一种常用的数据交换格式，也可用 JSON 替代。

---

## 3. 同步与异步

- **同步**：请求发送后，必须等待服务器响应，才能进行后续操作。
- **异步**：请求发送后，无需等待服务器响应，可以继续执行后续代码，响应到来时再处理。
- ![同步与异步示意图](./image.png) 

---

## 4. Axios 简介与用法

- **Axios**：基于 Promise 的 HTTP 客户端，对原生 Ajax 进行了封装，使用简单，支持各种请求方式。
- 官网：[axios-http.cn](https://axios-http.cn/)
- **基本用法**：
  ```js
  axios({
    method: 'GET',
    url: '请求路径',
    data: {},   // POST 请求体
    params: {}  // GET 请求参数
  }).then().catch();
  ```
- **简写方式**：
  ```js
  axios.get(url, config)
  axios.post(url, data, config)
  ```
- **请求方式别名**：`axios.get`、`axios.post`、`axios.put`、`axios.delete` 等

---

## 5. Axios 请求示例

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Axios 示例</title>
</head>
<body>
    <input type="button" value="GET 请求" id="forget">
    <input type="button" value="POST 请求" id="forpost">
    <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
    <script>
        // GET 请求
        document.querySelector("#forget").addEventListener("click", () => {
            axios.get("https://mock.apifox.cn/m1/3083103-0-default/emps/list")
                .then(response => console.log(response))
                .catch(error => console.log(error));
        });
        // POST 请求
        document.querySelector("#forpost").addEventListener("click", () => {
            axios.post("https://mock.apifox.cn/m1/3083103-0-default/emps/update", "id=1")
                .then(response => console.log(response))
                .catch(error => console.log(error));
        });
    </script>
</body>
</html>
```

---

## 6. 总结

- Ajax 让网页实现无刷新数据交互，提升用户体验。
- Axios 推荐用于现代前端开发，语法简洁，支持 Promise。
- 实际开发中建议优先使用 Axios 进行前后端通信。