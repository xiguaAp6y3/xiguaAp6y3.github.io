---
title: Java 图形化编程笔记
published: 2025-07-14
description: "Java Swing组件与事件处理实战教程"
tags: ["Java", "Swing", "GUI", "事件处理"]
category: Java
draft: true
---

# Java 图形化编程笔记

> 日期：2025-07-14

## 目录

- [Swing组件概述](#swing组件概述)
- [常用Swing组件](#常用swing组件)
- [登录界面实现](#登录界面实现)
- [事件处理](#事件处理)
- [键盘监听器](#键盘监听器)

---

## Swing组件概述

- **AWT编程**：已淘汰的图形界面技术
- **Swing组件**：目前主流的Java桌面应用开发技术

---

## 常用Swing组件

- **JFrame**: 窗口
- **JPanel**: 用于组织其他组件的容器
- **JButton**: 按钮组件
- **JTextField**: 输入框
- **JTable**: 表格

---

## 登录界面实现

```java
package GUI;
import javax.swing.*;

public class Test {
    public static void main(String[] args) {
        // 创建一个窗口（JFrame），标题为"登录窗口"
        JFrame frame = new JFrame("登录窗口");
        // 设置关闭窗口时退出程序
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        // 设置窗口大小为300x200像素
        frame.setSize(300, 200);

        // 创建面板（JPanel），用于放置控件
        JPanel panel = new JPanel();

        // 创建用户名标签和文本框
        JLabel userLabel = new JLabel("用户名:");
        JTextField userText = new JTextField(20);

        // 创建密码标签和密码输入框
        JLabel passwordLabel = new JLabel("密码:");
        JPasswordField passwordText = new JPasswordField(20);

        // 创建登录按钮
        JButton loginButton = new JButton("登录");

        // 将控件添加到面板
        panel.add(userLabel);
        panel.add(userText);
        panel.add(passwordLabel);
        panel.add(passwordText);
        panel.add(loginButton);

        // 将面板添加到窗口
        frame.add(panel);

        // 窗口居中显示
        frame.setLocationRelativeTo(null);

        // 设置窗口可见
        frame.setVisible(true);

        // 为登录按钮添加点击事件监听器
        loginButton.addActionListener(e -> {
            // 获取输入的用户名和密码
            String username = userText.getText();
            String password = new String(passwordText.getPassword());

            // 判断用户名和密码是否正确
            if ("admin".equals(username) && "123456".equals(password)) {
                // 弹出登录成功提示框
                JOptionPane.showMessageDialog(frame, "登录成功");
            } else {
                // 弹出登录失败提示框
                JOptionPane.showMessageDialog(frame, "登录失败");
            }
        });
    }
}
```

---

## 事件处理

- 上面的按钮监听器就是一种监听对象
- 事件处理是GUI编程的核心机制

---

## 键盘监听器

使用 `KeyListener` 监听键盘事件：

```java
package GUI;

public class test2 {
    public static void main(String[] args) {
        // 需求：监听键盘上下左右
        // 按下键盘的上下左右键，打印出对应的方向
        
        // 创建一个窗口
        javax.swing.JFrame frame = new javax.swing.JFrame("Key Listener Demo");
        frame.setDefaultCloseOperation(javax.swing.JFrame.EXIT_ON_CLOSE);
        frame.setSize(300, 200);
        // 设置窗口可见
        frame.setVisible(true);
        
        // 添加键盘监听器
        frame.addKeyListener(new java.awt.event.KeyAdapter() {
            @Override
            public void keyPressed(java.awt.event.KeyEvent e) {
                // 获取按下的键
                int keyCode = e.getKeyCode();
                // 判断按下的键
                switch (keyCode) {
                    case java.awt.event.KeyEvent.VK_UP:
                        System.out.println("上");
                        break;
                    case java.awt.event.KeyEvent.VK_DOWN:
                        System.out.println("下");
                        break;
                    case java.awt.event.KeyEvent.VK_LEFT:
                        System.out.println("左");
                        break;
                    case java.awt.event.KeyEvent.VK_RIGHT:
                        System.out.println("右");
                        break;
                    default:
                        System.out.println("其他键");
                }
            }
        });
    }
}
```

### 要点总结

1. **窗口创建**：使用 `JFrame` 创建主窗口
2. **组件布局**：使用 `JPanel` 作为容器组织其他组件
3. **事件监听**：通过监听器响应用户操作
4. **键盘事件**：使用 `KeyListener` 处理键盘输入
