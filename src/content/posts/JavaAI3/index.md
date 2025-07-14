---
title: Java 异常 笔记
published: 2025-07-14
description: "Java异常处理机制、自定义异常、异常处理策略详解"
tags: ["Java", "异常处理", "Exception", "泛型", "集合框架"]
category: Java
draft: false
---

# Java 异常 笔记

> 日期：2025-07-14

## 目录

- [异常概述](#异常概述)
- [异常的基本处理](#异常的基本处理)
- [自定义异常](#自定义异常)
- [异常的处理方案](#异常的处理方案)

---

## 异常概述

Java异常（Exception）分为两大类：

### 1. RuntimeException（运行时异常）
- 数组越界异常
- 除零操作异常  
- 空指针异常（如：`String a = null;` 但求 `a.length()`）

### 2. 编译时异常
- 日期解析异常
- 文件导入异常

可以使用 `try catch printStackTrace()` 来分析异常。

---

## 异常的基本处理

### 抛出异常（throws）

在方法上使用 `throws` 关键字，将方法内部出现的异常抛出给调用者处理。

```java
方法 throws 异常1, 异常2, 异常3... {
   ...
}
```

### 捕获异常（try...catch）

直接捕获程序出现的异常：

```java
try {
   // 监视可能出现异常的代码！
} catch(异常类型1 变量) {
   // 处理异常
} catch(异常类型2 变量) {
   // 处理异常
} ...
```

**示例代码：**

```java
package Exception;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.InputStream;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

public class Main {
    public static void main(String[] args) {
        String time = "2025-7-14 12:00:00";
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        Date date = new Date();
        
        try {
            date = sdf.parse(time);
        } catch (ParseException e) {
            e.printStackTrace();
        }
        
        try {
            InputStream ips = new FileInputStream("C:/Users/Gua3/Desktop/2.png");
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
        
        System.out.println(date);
    }
}
```

### 异常的作用

1. 方便定位bug
2. 方便通知方法执行问题

```java
public static int div(int a, int b) throws Exception {
    if(b == 0) {
        System.out.println("error");
        throw new Exception("ERROR");
    }
    return a / b;
}
```

---

## 自定义异常

### 自定义编译时异常

```java
package Exception;

public class AgeExpection extends Exception {
    public AgeExpection(String message) {
        super(message);
    }
}

public static void saveAge(int age) throws AgeExpection {
    if(age < 18) {
        throw new AgeExpection("年龄不能小于18岁");
    }
    System.out.println("年龄保存成功");
}
```

### 自定义运行时异常

```java
package Exception;

public class AgeException extends RuntimeException {
    public AgeException(String message) {
        super(message);
    }
}

public static void saveAge(int age) {
    if(age < 18) {
        throw new AgeException("年龄不能小于18岁");
    }
    System.out.println("年龄保存成功");
}

public static void main(String[] args) {
    saveAge(16); // 可以不写TryCatch 但是也会报错
    // 如果问题很严重可以定义为Exception子类
    // 不太严重就可以定义RuntimeException子类
    // 但是编译时异常现在过于麻烦，正在淘汰它
}
```

---

## 异常的处理方案

### 方案1：异常层层向上抛出
抛给最外层然后来显示异常。

### 方案2：最外层捕获异常后，尝试重新修复

```java
while(true) {
    try {
        any1();
        System.out.println("OK");
        break;
    } catch(Exception e) {
        e.printStackTrace();
        System.out.println("别搞，再来一遍");
    }
}
```

### 总结

- **编译时异常**：必须处理，但使用繁琐，正在被淘汰
- **运行时异常**：可选处理，更加灵活
- **异常处理策略**：根据问题严重程度选择合适的异常类型和处理方式
