---
title: Java 泛型与集合框架笔记
published: 2025-07-14
description: "Java泛型机制、包装类、Collection集合框架详解"
tags: ["Java", "泛型", "集合框架", "Collection"]
category: Java
draft: false
---

# Java 泛型与集合框架笔记

> 日期：2025-07-14

## 目录

- [泛型](#泛型)
- [包装类](#包装类)
- [集合框架Collection](#集合框架collection)
- [Collection的遍历](#collection的遍历)
- [并发修改异常问题](#并发修改异常问题)
- [List接口的实现类](#list接口的实现类)

---

## 泛型

泛型只用于引用数据类型，如 `ArrayList<E>`。

### 泛型类

```java
public class ClassE<E> {
    E e;
}
```

### 泛型接口

```java
public interface InterfaceE<E> {
    public void setX(E x);
}

public class ClassE<E> implements InterfaceE<E> {
    E x;
    E y;
    public ClassE(E x, E y) {
        this.x = x;
        this.y = y;
    }
    @Override
    public void setX(E x) {
        // ...existing code...
    }
}
```

### 泛型方法

```java
public static <T, T2> void test(T t, T2 t2) {
    // ...existing code...
}
```

### 通配符上下限

```java
// 上限：Car及其子类
public static void go(ArrayList<? extends Car> cars) {
    // ...existing code...
}

// 下限：Car及其父类
public static void go(ArrayList<? super Car> cars) {
    // ...existing code...
}
```

---

## 包装类

包装类就是把基本类型的数据包装成对象的类型。

| 基本数据类型 | 对应的包装类（引用数据类型） |
| ------------ | ---------------------------- |
| byte         | Byte                         |
| short        | Short                        |
| int          | Integer                      |
| long         | Long                         |
| char         | Character                    |
| float        | Float                        |
| double       | Double                       |
| boolean      | Boolean                      |

### Integer赋值

```java
// 推荐写法
Integer it = Integer.valueOf(100);
// 不推荐（已过时）
Integer it = new Integer(100);

// 自动装箱
Integer it = 100;
// 自动拆箱
int i = it;
```

> 注意：Integer缓存了-128到127的数字，可以直接赋值，节省内存。

### 包装类功能

```java
// toString
int j = 23;
String rs1 = Integer.toString(j);
String rs2 = j + ""; // 也可以这样转换

// String转int
String str = "98";
int i1 = Integer.parseInt(str);
int i2 = Integer.valueOf(str);
```

---

## 集合框架Collection

```
Collection<E> (单列集合接口)
├── List<E> (可重复，有序，有索引)
│   ├── ArrayList
│   └── LinkedList
└── Set<E> (不可重复，无序，无索引)
    ├── HashSet
    ├── TreeSet
    └── LinkedHashSet
```

### Collection的方法

| 方法名                              | 说明               |
| ----------------------------------- | ------------------ |
| `public boolean add(E e)`           | 添加元素           |
| `public void clear()`               | 清空集合           |
| `public boolean remove(E e)`        | 删除元素           |
| `public boolean contains(Object o)` | 判断是否包含该元素 |
| `public boolean isEmpty()`          | 判断是否为空       |
| `public int size()`                 | 获取集合长度       |
| `public Object[] toArray()`         | 转换为数组         |

```java
Collection<E> list = new ArrayList<>();
// 转换为Object数组
Object[] array = list.toArray();
// 转换为特定类型数组（构造器引用）
E[] array = list.toArray(E[]::new);
```

---

## Collection的遍历

```java
Collection<String> names = new ArrayList<>();
names.add("张无忌");
names.add("玄冥二老");
names.add("宋青书");
names.add("殷素素");
```

### 1. 迭代器遍历

```java
Iterator<String> it = names.iterator();
while(it.hasNext()) {
    String name = it.next();
    System.out.println(name);
}
```

### 2. 增强for循环

```java
for(String name : names) {
    System.out.println(name);
}
```

### 3. List的索引遍历

```java
List<String> list = new ArrayList<>();
for(int i = 0; i < list.size(); i++) {
    System.out.println(list.get(i));
}
```

### 4. Lambda遍历

```java
names.forEach(s -> System.out.println(s));
```

---

## 并发修改异常问题

遍历集合的同时又存在增删集合元素的行为时可能出现业务异常。

### 错误方法

```java
// 增强for循环中删除（错误）
for(String item : list) {
    if (item.contains("枸杞")) {
        list.remove(item); // 抛出异常
    }
}

// 迭代器遍历中用集合删除（错误）
while(it.hasNext()) {
    String name = it.next();
    if(name.contains("枸杞")) {
        list.remove(name); // 抛出异常
    }
}
```

### 正确解决方案

```java
// 方案1：正向遍历，删除后索引减一
for(int i = 0; i < list.size(); i++) {
    String item = list.get(i);
    if (item.contains("枸杞")) {
        list.remove(i);
        i--; // 移除后，索引需要减一
    }
}

// 方案2：反向遍历
for(int i = list.size() - 1; i >= 0; i--) {
    String item = list.get(i);
    if (item.contains("枸杞")) {
        list.remove(i);
    }
}

// 方案3：使用removeIf
list.removeIf(item -> item.contains("枸杞"));

// 方案4：使用迭代器的remove方法
while(it.hasNext()) {
    String name = it.next();
    if(name.contains("枸杞")) {
        it.remove(); // 使用迭代器的remove
    }
}
```

---

## List接口的实现类

### ArrayList
- 基于数组实现
- 第一次扩容为10
- 满了之后至少扩容1.5倍
- 查询快，增删慢

> 详细功能参考：[Java集合ArrayList笔记](https://xiguaap6y3.github.io/posts/java1/)

### LinkedList
- 基于双链表实现
- 查询慢，增删快，占用内存多
- 可以设计队列、栈

**LinkedList特有方法：**

| 方法名称                    | 说明                             |
| --------------------------- | -------------------------------- |
| `public void addFirst(E e)` | 在该列表开头插入指定的元素       |
| `public void addLast(E e)`  | 将指定的元素追加到此列表的末尾   |
| `public E getFirst()`       | 返回此列表中的第一个元素         |
| `public E getLast()`        | 返回此列表中的最后一个元素       |
| `public E removeFirst()`    | 从此列表中删除并返回第一个元素   |
| `public E removeLast()`     | 从此列表中删除并返回最后一个元素 |

> 数据结构详解参考：[数据结构笔记](https://xiguaap6y3.github.io/posts/datastructure2/)


