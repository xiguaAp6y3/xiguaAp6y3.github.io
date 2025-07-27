---
title: JavaWeb Vue 学习笔记
published: 2025-07-28
description: JavaWeb 项目中 Vue 的基础用法与表单表格综合示例。
tags: [JavaWeb, Vue, 学习笔记]
category: 学习笔记
draft: false
---

# JavaWeb Vue 学习笔记

> 日期：2025年7月28日

---

## 目录

1. [Vue 快速入门](#vue-快速入门)
2. [常用指令](#常用指令)
3. [表单与表格综合示例](#表单与表格综合示例)
4. [总结](#总结)

---

## Vue 快速入门

- 参考：[Vue 官方快速上手](https://cn.vuejs.org/guide/quick-start.html)

### createApp 初始化

- 导入 Vue
- `data()` 返回响应式数据对象
- `methods` 定义组件方法
- `this` 在 methods 中指向当前 Vue 实例
- `.mount("#app")` 挂载到 DOM

**示例代码：**

```html
<!-- Vue3 最简用法 -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue Demo</title>
</head>
<body>
    <div id="app">
        <h1>{{ message }}</h1>
    </div>
    <script type="module">
      import { createApp } from 'https://unpkg.com/vue@3/dist/vue.esm-browser.js';
      createApp({
        data() {
            return {
                message: "HELLO VUE",
                count: 100
            }
        }
      }).mount("#app");
    </script>
</body>
</html>
```

---

## 常用指令

- `v-for`：循环渲染列表，如 `v-for="(item, index) in items" :key="index"`
- `v-bind`（`:`）：绑定属性，如 `:src="imgUrl"`
- `v-on`（`@`）：监听事件，如 `@click="handleClick"`
- `v-model`：表单双向绑定
- `v-if`：条件渲染

---

## 表单与表格综合示例

**要点：**
- 使用 `v-model` 实现表单双向绑定
- 使用 `v-for` 渲染表格
- 使用 `v-if` 条件渲染
- 数据驱动视图，模型变化自动更新页面

**示例代码：**

```html
<!DOCTYPE html>
<html lang="zh_cn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue 表单与表格</title>
    <style>
        body { max-width: 600px; margin: auto; }
        #thetable { margin: auto; width: 100%; border: 1px solid #ccc; font-size: 1.2em; }
        tr,th,td { border: 1px solid; text-align: center; }
    </style>
</head>
<body>
    <div id="app">
        <h1>{{ message }}</h1>
        <form class="form-row">
            <label>姓名：<input type="text" v-model="searchForm.name" placeholder="请输入姓名"></label>
            <label>性别：
                <select v-model="searchForm.sex">
                    <option value="">请选择性别</option>
                    <option value="1">男</option>
                    <option value="2">女</option>
                </select>
            </label>
            <label>职位：
                <select v-model="searchForm.job">
                    <option value="">请选择职位</option>
                    <option value="1">一</option>
                    <option value="2">二</option>
                    <option value="3">三</option>
                </select>
            </label>
            <button type="button" @click="search">搜索</button>
            <button type="button" @click="clr">清空</button>
        </form>
        <table id="thetable">
            <thead>
                <tr>
                    <th>index</th>
                    <th>id</th>
                    <th>name</th>
                    <th>sex</th>
                    <th>image</th>
                    <th>job</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(item,index) in emplist" :key="item.id">
                    <td>{{ index+1 }}</td>
                    <td>{{ item.id }}</td>
                    <td>{{ item.name }}</td>
                    <td>{{ item.sex==1 ? "🚹" : "🚺" }}</td>
                    <td><img :src="item.image" :alt="item.name" width="30px"></td>
                    <td>
                        <span v-if="item.job==1">一</span>
                        <span v-else-if="item.job==2">二</span>
                        <span v-else-if="item.job==3">三</span>
                        <span v-else>und</span>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
    <script type="module">
      import { createApp } from 'https://unpkg.com/vue@3/dist/vue.esm-browser.js';
      createApp({
        data() {
            return {
                message: "HELLO VUE",
                emplist: [
                    { id: 1, name: "gua3", sex: 1, image: "../img/1.jpg", job: 1 },
                    { id: 2, name: "gua4", sex: 2, image: "../img/2.jpg", job: 5 }
                ],
                searchForm: { name: '', sex: '', job: '' }
            }
        },
        methods: {
            search() {
                // 控制台输出搜索条件
                console.log(this.searchForm);
            },
            clr() {
                this.searchForm = { name: '', sex: '', job: '' };
            }
        }
      }).mount("#app");
    </script>
</body>
</html>
```

---

## 总结

- Vue3 推荐使用 `createApp` 初始化应用。
- 常用指令包括 `v-for`、`v-bind`、`v-on`、`v-model`、`v-if`。
- 表单与表格结合可实现数据驱动的动态页面。
- 建议多实践，结合官方文档深入理解 Vue 的响应式和组件化思想。