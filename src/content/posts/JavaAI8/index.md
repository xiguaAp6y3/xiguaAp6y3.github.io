---
title: Java File类、递归与字符集笔记
published: 2025-07-18
description: "Java File类操作、递归算法、字符集编码解码详解"
tags: ["Java", "File", "递归", "字符集", "编码"]
category: Java
draft: false
---

# Java File类、递归与字符集笔记

> 日期：2025-07-18

## 目录

- [File类基础](#file类基础)
- [File类常用方法](#file类常用方法)
- [File类创建和删除功能](#file类创建和删除功能)
- [递归算法](#递归算法)
- [字符集与编码](#字符集与编码)

---

## File类基础

File类用于表示文件和目录路径名的抽象表示形式。

```java
File f1 = new File(pathname);
```

> 注意：路径分隔符要用 `/`，如果用 `\` 需要转义为 `\\`

详细API文档：[Java File API](https://doc.qzxdp.cn/jdk/17/zh/api/java.base/java/io/File.html)

---

## File类常用方法

### 判断和获取信息的方法

| 方法名称                        | 说明                                           |
| ------------------------------- | ---------------------------------------------- |
| `public boolean exists()`       | 判断文件路径是否存在，存在返回`true`           |
| `public boolean isFile()`       | 判断是否是文件，是文件返回`true`               |
| `public boolean isDirectory()`  | 判断是否是文件夹，是文件夹返回`true`           |
| `public String getName()`       | 获取文件的名称（包含后缀）                     |
| `public long length()`          | 获取文件的大小，返回字节个数                   |
| `public long lastModified()`    | 获取文件的最后修改时间                         |
| `public String getPath()`       | 获取创建文件对象时使用的路径                   |
| `public String getAbsolutePath()` | 获取绝对路径                                   |

### 获取目录内容的方法

```java
String[] list = (new File()).list();     // 获取文件名数组
File[] files = (new File()).listFiles(); // 获取文件对象数组
```

#### `public File[] listFiles()` 注意事项

- 当主调是文件，或者路径不存在时，返回`null`
- 当主调是空文件夹时，返回一个长度为`0`的数组
- 当主调是有内容的文件夹时，将里面所有一级文件和文件夹的路径放在`File`数组中返回
- 当主调是文件夹且里面有隐藏文件时，将里面所有文件和文件夹的路径放在`File`数组中返回
- 当主调是文件夹但没有权限访问时，返回`null`

---

## File类创建和删除功能

### 创建功能

| 方法名称                        | 说明                     |
| ------------------------------- | ------------------------ |
| `public boolean createNewFile()` | 创建一个新的空文件       |
| `public boolean mkdir()`        | 只能创建一级文件夹       |
| `public boolean mkdirs()`       | 可以创建多级文件夹       |

### 删除功能

| 方法名称                  | 说明                   |
| ------------------------- | ---------------------- |
| `public boolean delete()` | 删除文件、空文件夹     |

> **注意**：delete方法默认只能删除文件和空文件夹，删除后的文件不会进入回收站。

---

## 递归算法

递归是一种算法思想，指方法自己调用自己的编程技巧。

### 递归文件搜索示例

```java
package Recursion;
import java.io.File;
import java.util.Objects;

public class FileSearch {
    public static void main(String[] args) {
        // 测试文件搜索
        File dir = new File("D:\\");
        searchFile(dir, "test.txt");
    }
    
    // 搜索指定目录下的文件
    public static void searchFile(File file, String fileName) {
        // 判断是否是文件夹
        if (file.isDirectory()) {
            // 获取文件夹下的所有文件
            File[] files = file.listFiles();
            if (files != null) {
                for (File f : files) {
                    // 递归调用
                    searchFile(f, fileName);
                }
            }
        } else {
            // 判断是否是目标文件
            if (Objects.equals(file.getName(), fileName)) {
                System.out.println("找到文件: " + file.getAbsolutePath());
            }
        }
    }
}
```

---

## 字符集与编码

### ASCII字符集
- 最早的字符编码标准
- 使用7位二进制数表示128个字符
- 主要包含英文字母、数字和基本符号

### GBK字符集
- 汉字编码字符集
- 一个中文字符使用2个字节
- 兼容ASCII字符集
- 为了兼容，GBK规定16位的第一位是1，如果第一位是0则只读1个字节（对应ASCII）

### Unicode字符集（统一码，万国码）
Unicode是国际组织制定的，可以容纳世界上所有文字、符号的字符集。

#### Unicode的实现方式
- **UTF-32**：固定32位（4字节）
- **UTF-16**：16位或32位
- **UTF-8**：采用可变长编码方案，占1-4字节。通过前缀标记来识别对应语言

### 编码解码示例

```java
package Decode;
import java.util.Arrays;

public class Demo1 {
    public static void main(String[] args) throws Exception {
        // 测试编码解码
        String text = "1我爱你中国aaa";
        
        // 编码：字符串 -> 字节数组
        byte[] bytes = text.getBytes(); // 使用默认编码
        System.out.println("编码结果: " + Arrays.toString(bytes));
        
        // 解码：字节数组 -> 字符串
        String decoded = new String(bytes);
        System.out.println("解码结果: " + decoded);
        
        // 指定编码格式
        byte[] utf8Bytes = text.getBytes("UTF-8");
        byte[] gbkBytes = text.getBytes("GBK");
        
        System.out.println("UTF-8编码: " + Arrays.toString(utf8Bytes));
        System.out.println("GBK编码: " + Arrays.toString(gbkBytes));
    }
}
```

### 总结

1. **File类**：用于文件和目录的操作，提供创建、删除、查询等功能
2. **递归**：方法调用自身，适用于树形结构的遍历
3. **字符集**：
   - ASCII：英文字符集
   - GBK：中文字符集，兼容ASCII
   - Unicode/UTF-8：国际标准，支持全球所有语言
4. **编码解码**：字符串与字节数组之间的转换




