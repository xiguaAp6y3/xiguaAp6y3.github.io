---
title: Java AI Lambda与方法引用笔记
published: 2025-07-13
description: "JDK8 Lambda表达式、方法引用、构造器引用用法与示例"
tags: ["Java", "Lambda", "方法引用", "函数式接口"]
category: Java
draft: false
---

# Java AI Lambda λ 与方法引用笔记

> 日期：2025-07-13

## 目录

- [Lambda表达式](#lambda表达式)
- [函数式接口](#函数式接口)
- [方法引用](#方法引用)
  - [静态方法引用](#静态方法引用)
  - [实例方法引用](#实例方法引用)
  - [特定类型方法引用](#特定类型方法引用)
- [构造器引用](#构造器引用)
- [小结](#小结)

---

## Lambda表达式

JDK8新特性，简化函数式接口的匿名内部类写法。

```java
(参数列表) -> {
    方法体代码;
}
```

**示例：**

```java
@FunctionalInterface
interface swim {
    void swimming();
}

public class demo1 {
    public static void main(String[] args) {
        swim s1 = () -> {
            System.out.println("swimming");
        };
        s1.swimming();
    }
}
```

- Lambda表达式只能简化函数式接口的匿名内部类。

---

## 函数式接口

- 只有一个抽象方法的接口就是函数式接口。
- 用 `@FunctionalInterface` 注解标记。

**简化写法：**

- 参数类型可省略。
- 只有一个参数时，括号可省略。
- 只有一行代码时，大括号和分号可省略，若为return语句也要省略。

**示例：**

```java
@FunctionalInterface
interface swim {
    void swimming(int a);
}
@FunctionalInterface
interface got {
    int Got(int a);
}

public class demo1 {
    public static void main(String[] args) {
        swim s1 = a -> System.out.println(a + "swimming");
        s1.swimming(1); // 1swimming
        got s2 = a -> a + 2;
        int b = s2.Got(2);
        System.out.println(b); // 4
    }
}
```

---

## 方法引用

### 静态方法引用

- 语法：`类名::静态方法名`

**示例：**

```java
public class Student {
    // ...existing code...
    public static int cmp(Student x, Student y) {
        return x.getAge() - y.getAge();
    }
}

Student[] a = ...;
Arrays.sort(a, (x, y) -> Student.cmp(x, y)); // Lambda
Arrays.sort(a, Student::cmp); // 静态方法引用
```

---

### 实例方法引用

- 语法：`对象名::实例方法名`
- 用于Lambda表达式仅调用某对象的实例方法，且参数一致。

**示例：**

```java
public class Student {
    // ...existing code...
    public int cmpbyscore(Student x, Student y) {
        return Double.compare(x.getScore(), y.getScore());
    }
}

Student t = new Student();
Arrays.sort(a, (x, y) -> t.cmpbyscore(x, y));
Arrays.sort(a, t::cmpbyscore); // 实例方法引用
```

---

### 特定类型方法引用

- 语法：`特定类名::实例方法名`
- 用于Lambda表达式调用某类型的实例方法，且第一个参数为主调对象。

**示例：**

```java
String[] names = {"Alice", "Bob", "Charlie", "David", "Eve", "曹操", "as", "cS"};
Arrays.sort(names, (o1, o2) -> o1.compareToIgnoreCase(o2)); // Lambda
Arrays.sort(names, String::compareToIgnoreCase); // 方法引用
```

---

## 构造器引用

- 语法：`类名::new`
- 用于Lambda表达式只创建对象，且参数一致。

**示例：**

```java
public class Car {
    private String name;
    // ...构造方法...
}

public interface CarFactory {
    Car getCar(String name);
}

public class test3 {
    public static void main(String[] args) {
        CarFactory cf = new CarFactory() {
            @Override
            public Car getCar(String name) {
                return new Car(name);
            }
        };
        CarFactory cf2 = a -> new Car(a); // Lambda
        CarFactory cf3 = Car::new; // 构造器引用
        Car a1 = cf.getCar("BMW");
        Car a2 = new Car("BMW");
        Car a3 = cf3.getCar("BMW");
        Car a4 = cf2.getCar("BMW");
        System.out.println(a1);
        System.out.println(a2);
        System.out.println(a3);
        System.out.println(a4);
    }
}
```

---

## 小结

1. **什么是函数式编程？有什么好处？**  
   - 使用Lambda函数替代匿名内部类，让代码更简洁、可读性更好。

2. **Lambda表达式是什么？怎么写？**  
   - JDK8新增语法，代表函数，用于简化函数式接口的匿名内部类。

3. **什么是函数式接口？如何保证？**  
   - 只有一个抽象方法的接口。用`@FunctionalInterface`注解标记。

4. **方法引用与构造器引用**  
   - 方法引用可简化Lambda表达式，分为静态方法、实例方法、特定类型方法引用。
   - 构造器引用用于简化对象创建。