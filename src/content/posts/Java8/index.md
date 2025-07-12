---
title: 阶段性项目：拼图小游戏开发笔记
published: 2025-07-13
description: "Java Swing拼图小游戏开发要点与代码结构"
tags: ["Java", "Swing", "JFrame", "项目实践"]
category: Java
draft: true
---

# 阶段性项目：拼图小游戏开发笔记

> 日期：2025-07-13

## 目录

- [JFrame窗口基础](#jframe窗口基础)
- [自定义窗口类](#自定义窗口类)
- [主函数调用](#主函数调用)
- [菜单栏相关类](#菜单栏相关类)

---

## JFrame窗口基础

- `JFrame` 相当于一个JavaBean类，可以用来创建UI窗口。

```java
JFrame a = new JFrame();
a.setSize(603, 680); // 创建一个宽603，高680的窗口
a.setVisible(true);  // 窗口默认不可见，需设置为可见
a.show();            // 已过时的方法，也可以展示窗口
```

> 以上代码如果都写在 `main` 方法中会很繁琐。

---

## 自定义窗口类

可以通过继承 `JFrame`，将窗口相关设置封装到一个类中：

```java
public class aJFrame extends JFrame {
    aJFrame() {
        this.setSize(111, 1111);
        this.setTitle("拼图1.0"); // 显示的标题
        this.setAlwaysOnTop(true); // 置于顶层
        this.setVisible(true); // 可见
        this.setLocationRelativeTo(null); // 显示位置(null默认中间)
        this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE); // 推荐写法
        this.setDefaultCloseOperation(3); // 关闭窗口程序结束
    }
}
```

---

## 主函数调用

只需一行代码即可展示窗口：

```java
new aJFrame();
```

---

## 菜单栏相关类

- `JMenuBar`
- `JMenu`
- `JMenuItem`

> 具体用法可参考 Swing 官方文档或后续补充。