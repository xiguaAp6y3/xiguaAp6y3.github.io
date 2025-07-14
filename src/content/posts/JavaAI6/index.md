---
title: Java 集合框架(三) Map 笔记
published: 2025-07-15
description: "Java Map集合详解：HashMap、LinkedHashMap、TreeMap底层原理与使用"
tags: ["Java", "集合框架", "Map", "HashMap", "TreeMap"]
category: Java
draft: false
---

# Java 集合框架(三) Map 笔记

> 日期：2025-07-15

## 目录

- [Map集合概述](#map集合概述)
- [Map集合体系特点](#map集合体系特点)
- [Map集合常用方法](#map集合常用方法)
- [Map集合遍历方式](#map集合遍历方式)
- [Map集合案例](#map集合案例)
- [底层实现原理](#底层实现原理)

---

## Map集合概述

- Map属于**双列集合**，存储键值对
- 键唯一，值可重复
- 适用于存储一一对应的数据，如购物车商品与数量

```java
Map<K,V> 接口
├── HashMap
├── LinkedHashMap  
└── TreeMap
```

---

## Map集合体系特点

> 注意：Map系列集合的特点都是由键决定的，值只是附属品

- **HashMap**：无序、不重复、无索引（使用最多）
- **LinkedHashMap**：有序、不重复、无索引
- **TreeMap**：按照大小默认升序排序、不重复、无索引

### 示例对比

```java
package Collection.Map;
import java.util.*;

public class Demo1 {
    public static void main(String[] args) {
        // HashMap - 无序
        HashMap<String,Integer> map = new HashMap<>();
        map.put("张三", 18);
        map.put("李四", 20);
        map.put("王五", 22);
        map.put("赵六", 25);
        map.put("张三", 19); // 键重复，覆盖旧值
        System.out.println(map); // {李四=20, 张三=19, 王五=22, 赵六=25}
        
        // LinkedHashMap - 有序（保持插入顺序）
        LinkedHashMap<String,Integer> linkedMap = new LinkedHashMap<>();
        linkedMap.put("张三", 18);
        linkedMap.put("李四", 20);
        linkedMap.put("王五", 22);
        linkedMap.put("赵六", 25);
        linkedMap.put("张三", 19);
        System.out.println(linkedMap); // {张三=19, 李四=20, 王五=22, 赵六=25}
        
        // TreeMap - 排序
        TreeMap<Integer,Integer> treeMap = new TreeMap<>();
        treeMap.put(10, 100);
        treeMap.put(2, 200);
        treeMap.put(5, 300);
        treeMap.put(4, 400);
        treeMap.put(3, 500);
        System.out.println(treeMap); // {2=200, 3=500, 4=400, 5=300, 10=100}
    }
}
```

---

## Map集合常用方法

> Map是双列集合的祖宗，它的功能是全部双列集合都可以继承使用的

| 方法名称                                   | 说明                   |
| ------------------------------------------ | ---------------------- |
| `public V put(K key, V value)`            | 添加元素               |
| `public int size()`                        | 获取集合的大小         |
| `public void clear()`                      | 清空集合               |
| `public boolean isEmpty()`                 | 判断集合是否为空       |
| `public V get(Object key)`                 | 根据键获取对应值       |
| `public V remove(Object key)`              | 根据键删除整个元素     |
| `public boolean containsKey(Object key)`   | 判断是否包含某个键     |
| `public boolean containsValue(Object value)` | 判断是否包含某个值     |
| `public Set<K> keySet()`                   | 获取全部键的集合       |
| `public Collection<V> values()`            | 获取Map集合的全部值    |

### 示例代码

```java
package Collection.Map;
import java.util.*;

public class Demo2 {
    public static void main(String[] args) {
        Map<Integer, Integer> treeMap = new TreeMap<>();
        treeMap.put(1, 100);
        treeMap.put(2, 200);
        treeMap.put(3, 300);
        
        System.out.println(treeMap.get(1)); // 100
        treeMap.remove(2);
        System.out.println(treeMap.get(2)); // null
        System.out.println(treeMap); // {1=100, 3=300}
        System.out.println(treeMap.containsKey(3)); // true
        System.out.println(treeMap.containsValue(100)); // true
        System.out.println(treeMap.isEmpty()); // false
        System.out.println(treeMap.size()); // 2
        
        Set<Integer> keys = treeMap.keySet(); // 获取所有的键
        Collection<Integer> values = treeMap.values(); // 获取所有值
    }
}
```

---

## Map集合遍历方式

### 方式1：键找值遍历

```java
for(Integer key : keys) {
    Integer value = treeMap.get(key);
    System.out.println("Key: " + key + ", Value: " + value);
}
```

### 方式2：键值对遍历

```java
for(Map.Entry<Integer,Integer> entry : treeMap.entrySet()) {
    entry.setValue(entry.getValue() + 100);
}
System.out.println(treeMap); // {1=200, 3=400}
```

### 方式3：Lambda表达式遍历

```java
treeMap.forEach((k,v) -> System.out.println(k + "=" + v));
```

---

## Map集合案例

### 统计投票信息

**需求：** 某个班级80名学生，现在需要组织秋游活动，班长提供了四个景点依次是（A、B、C、D），每个学生只能选择一个景点，请统计出最终哪个景点想去的人数最多。

```java
import java.util.Map;
import java.util.Scanner;
import java.util.TreeMap;

public class VoteCount {
    public static void main(String[] args) {
        Map<String, Integer> choice = new TreeMap<>();
        Scanner sc = new Scanner(System.in);
        int studentCount = 80;
        
        for(int i = 0; i < studentCount; i++) {
            String vote = sc.next();
            if(choice.containsKey(vote)) {
                choice.put(vote, choice.get(vote) + 1);
            } else {
                choice.put(vote, 1);
            }
        }
        
        Integer[] maxCount = {0};
        String[] winner = {""};
        choice.forEach((k, v) -> {
            if(v > maxCount[0]) {
                maxCount[0] = v;
                winner[0] = k;
            }
        });
        
        System.out.println(choice);
        System.out.println("最受欢迎景点: " + winner[0] + "，票数: " + maxCount[0]);
    }
}
```

---

## 底层实现原理

### HashMap
- 基于哈希表实现
- 底层：数组 + 链表 + 红黑树

### LinkedHashMap
- 基于哈希表实现
- 额外维护双端链表保持插入顺序

### TreeMap
- 基于红黑树实现
- 如果键是自定义类，需要：
  - 让类实现 `Comparable` 接口，或者
  - 在创建 `TreeMap` 时提供 `Comparator` 比较器对象

### 总结

- **HashMap**：适用于不关心顺序的键值对存储
- **LinkedHashMap**：适用于需要保持插入顺序的键值对存储
- **TreeMap**：适用于需要按键排序的键值对存储