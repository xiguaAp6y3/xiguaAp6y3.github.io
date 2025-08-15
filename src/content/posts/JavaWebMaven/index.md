---
title: JavaWeb Maven 学习笔记
published: 2025-08-16
description: JavaWeb 项目中 Maven 的依赖管理、项目构建、测试与生命周期等知识总结。
tags: [JavaWeb, Maven, JUnit, 学习笔记]
category: 学习笔记
draft: false
---

# JavaWeb Maven 学习笔记

> 日期：2025年8月16日

---

## 目录

- [1. Maven 作用](#1-maven-作用)
- [2. IDEA 集成 Maven](#2-idea-集成-maven)
- [3. Maven 仓库](#3-maven-仓库)
- [4. Maven 坐标](#4-maven-坐标)
- [5. 依赖管理与排除](#5-依赖管理与排除)
- [6. Maven 生命周期](#6-maven-生命周期)
- [7. 测试类型与方法](#7-测试类型与方法)
- [8. JUnit 单元测试](#8-junit-单元测试)
- [9. 依赖范围](#9-依赖范围)
- [10. 常见问题](#10-常见问题)

---

## 1. Maven 作用

- **依赖管理**：方便快捷地管理项目依赖的资源（如 commons-io、lombok 等第三方库）。
- **项目构建**：标准化、自动化的项目编译、测试、打包、发布流程。
- **统一项目结构**：提供标准统一的项目目录结构，兼容多种 IDE。

---

## 2. IDEA 集成 Maven

- IDEA 会自动识别 `pom.xml` 文件，并提供 Maven 工具窗口。
- `pom.xml` 配置了项目对象模型（POM）和依赖管理。
- 仓库用于存储和管理各种 jar 包。

---

## 3. Maven 仓库

- **本地仓库**：自己计算机上的一个目录。
- **远程仓库**：公司团队搭建的私有仓库。
- **中央仓库**：Maven 官方维护的全球唯一仓库 [https://repo1.maven.org/maven2/](https://repo1.maven.org/maven2/)
- **查找依赖顺序**：本地仓库 → 远程仓库 → 中央仓库

---

## 4. Maven 坐标

- 坐标是 jar 包的唯一标识。
- 主要字段：
  - `groupId`：组织名称（如：`com.itheima`）
  - `artifactId`：项目名称（如：`order-service`）
  - `version`：版本号（如：`6.1.4`，`SNAPSHOT` 表示开发版本，`RELEASE` 表示正式版本）

**示例：**
```xml
<dependency>
    <groupId>commons-io</groupId>
    <artifactId>commons-io</artifactId>
    <version>2.16.1</version>
</dependency>
```

---

## 5. 依赖管理与排除

- 在 `pom.xml` 的 `<dependencies>` 标签中添加依赖。
- 可通过 `<exclusions>` 标签排除不必要的依赖。

**示例：**
```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>7.0.0-M7</version>
    <exclusions>
        <exclusion>
            <groupId>io.micrometer</groupId>
            <artifactId>micrometer-observation</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

---

## 6. Maven 生命周期

- Maven 定义了三个内置生命周期：`clean`、`default`、`site`
  - `clean`：清理项目，删除生成文件
  - `default`：编译、测试、打包等
  - `site`：生成项目文档

**主要阶段：**
- `clean`：移除上一次构建生成的文件
- `compile`：编译项目源代码
- `test`：运行单元测试
- `package`：打包（如 jar、war）
- `install`：安装到本地仓库

**执行方式：**
- IDEA 右侧 Maven 工具栏双击对应生命周期
- 命令行执行，如 `mvn package`

---

## 7. 测试类型与方法

- **单元测试**（白盒）：验证代码逻辑，开发人员编写
- **集成测试**（灰盒）：验证模块协作，开发人员编写
- **系统测试**（黑盒）：验证系统功能，测试人员编写
- **验收测试**（黑盒）：验证业务需求，客户/需求方参与

**测试方法：**
- 白盒：了解内部结构，验证逻辑
- 黑盒：只关注功能表现
- 灰盒：结合内部结构和外部表现

---

## 8. JUnit 单元测试

- 推荐使用 JUnit 框架，测试代码与源代码分开，便于维护和自动化测试。
- 测试类命名规范：`XxxxxTest`
- 测试方法需声明为 `public void`
- 断言方法常用如下：

| 方法 | 描述 |
| --- | --- |
| `Assertions.assertEquals(exp, act)` | 检查相等 |
| `Assertions.assertNotEquals(unexp, act)` | 检查不相等 |
| `Assertions.assertNull(act)` | 检查为 null |
| `Assertions.assertNotNull(act)` | 检查不为 null |
| `Assertions.assertTrue(condition)` | 检查为 true |
| `Assertions.assertFalse(condition)` | 检查为 false |
| `Assertions.assertThrows(expType, exec)` | 检查抛出异常 |

**常见注解：**

| 注解 | 说明 |
| --- | --- |
| `@Test` | 单元测试方法 |
| `@ParameterizedTest` | 参数化测试 |
| `@ValueSource` | 参数来源 |
| `@DisplayName` | 显示名称 |
| `@BeforeEach` | 每个测试前执行 |
| `@AfterEach` | 每个测试后执行 |
| `@BeforeAll` | 所有测试前执行一次 |
| `@AfterAll` | 所有测试后执行一次 |

**参数化测试示例：**
```java
@ParameterizedTest
@ValueSource(strings = {"110101199001011234", "110101199001011235"})
public void testGetGender2(String idCard) {
    UserService userService = new UserService();
    String gender = userService.getGender(idCard);
    Assertions.assertEquals("男", gender);
}
```

---

## 9. 依赖范围

- 通过 `<scope>` 标签设置依赖的作用范围

| scope值 | 主程序 | 测试程序 | 打包 | 范例 |
| --- | --- | --- | --- | --- |
| compile（默认） | Y | Y | Y | log4j |
| test | -- | Y | -- | junit |
| provided | Y | Y | -- | servlet-api |
| runtime | -- | Y | Y | jdbc驱动 |

**示例：**
```xml
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter</artifactId>
    <version>5.9.3</version>
    <scope>test</scope>
</dependency>
```

---

## 10. 常见问题

- 如果网络不好，Maven 下载插件失败，可删除 `.lastupdated` 文件
- Windows 指令：`del /s *.lastupdated`

---