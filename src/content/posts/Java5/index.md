---
title: Java 权限修饰符、代码块与抽象类/方法笔记
published: 2025-07-09
description: "面向对象进阶笔记3：权限修饰符、代码块、抽象类与抽象方法"
tags: ["Java", "权限修饰符", "抽象类", "代码块"]
category: Java
draft: false
---

# Java 权限修饰符、代码块与抽象类/方法笔记

> 日期：2025-07-09

## 目录

- [权限修饰符](#权限修饰符)
- [代码块](#代码块)
- [抽象类与抽象方法](#抽象类与抽象方法)
- [练习：抽象类的标准JavaBean](#练习抽象类的标准javabean)
- [总结](#总结)

---

## 权限修饰符

### 权限修饰符的分类

| 修饰符      | 同类 | 同包 | 子类 | 其他包 |
| ----------- | ---- | ---- | ---- | ------ |
| private     | √    | ×    | ×    | ×      |
| 默认(无)    | √    | √    | ×    | ×      |
| protected   | √    | √    | √    | ×      |
| public      | √    | √    | √    | √      |

- `private`：只能本类访问，子类和其他类不能直接访问，需要通过 getter/setter。
- 默认（无修饰符）：同包可访问。
- `protected`：同包和子类可访问（即使子类在不同包）。
- `public`：所有类都可访问。

**注意：**  
如果基类变量为 `private`，子类访问需通过 getter/setter。  
如果为默认，同包可直接访问。  
如果为 `protected`，不同包的子类也可访问。  
如果为 `public`，所有地方都可访问。

---

## 代码块

### 1. 局部代码块

```java
public class Test1 {
    public static void main(String[] args) {
        int a = 10;
        System.out.println(a);
    } // 这就是局部代码块
}
```
- 只能在方法内部使用。

### 2. 构造代码块

```java
public class Student {
    private String name;
    {
        System.out.println("开始创建对象了");
    } // 构造代码块
    Student(){}
    Student(String name) { this.name = name; }
}
```
- 优先于构造方法执行，每次创建对象都会执行。

### 3. 静态代码块

```java
public class Student {
    static {
        System.out.println("静态代码块执行了");
    }
}
```
- 随类加载而执行，只执行一次。

---

## 抽象类与抽象方法

- 关键字：`abstract`
- 抽象方法没有方法体，子类必须重写。
- 有抽象方法的类必须声明为抽象类。

### 示例

```java
public abstract class Object1 {
    public abstract void func();
}

public class Object2 extends Object1 {
    public void func() {
        System.out.println("2:f2");
    }
}
```

### 注意事项

- 抽象类不能实例化。
- 抽象类不一定有抽象方法，但有抽象方法的类一定是抽象类。
- 抽象类可以有构造方法（用于子类实例化时调用）。
- 抽象类的子类要么重写所有抽象方法，要么本身也是抽象类。

---

## 练习：抽象类的标准JavaBean

| 动物名称 | 属性     | 行为         |
| -------- | -------- | ------------ |
| 青蛙(frog) | 名字，年龄 | 吃虫子，喝水 |
| 狗(Dog)   | 名字，年龄 | 吃骨头，喝水 |
| 山羊(Sheep) | 名字，年龄 | 吃草，喝水   |

**Animal类：**

```java
package ToObjectAdvanced.Abstract.p1;

public abstract class Animal {
    private String name;
    private int age;

    public Animal() {}
    public Animal(String name, int age) {
        this.name = name;
        this.age = age;
    }
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public int getAge() { return age; }
    public void setAge(int age) { this.age = age; }
    public abstract void wt_do();
    public void drink() {
        System.out.println("im drinking");
    }
}
```

**frog类：**

```java
package ToObjectAdvanced.Abstract.p1;
public class frog extends Animal {
    public frog() {}
    public frog(String name, int age) { super(name, age); }
    @Override
    public void wt_do() {
        System.out.println("我会吃虫子");
    }
}
```

**Dog类：**

```java
package ToObjectAdvanced.Abstract.p1;

public class Dog extends Animal {
    public Dog() {}
    public Dog(String name, int age) { super(name, age); }
    @Override
    public void wt_do() {
        System.out.println("我会吃骨头");
    }
}
```

**Sheep类：**

```java
package ToObjectAdvanced.Abstract.p1;

public class Sheep extends Animal {
    public Sheep() {}
    public Sheep(String name, int age) { super(name, age); }
    @Override
    public void wt_do() {
        System.out.println("吃草");
    }
}
```

**Main类：**

```java
package ToObjectAdvanced.Abstract.p1;

public class Main {
    public static void main(String[] args) {
        Animal a = new Dog("旺财", 11);
        Animal b = new frog("96", 11);
        Animal c = new Sheep("肖恩", 11);
        a.wt_do();
        b.wt_do();
        c.wt_do();
        a.drink();
    }
}
```

---

## 总结

1. **抽象类的作用**
   - 抽取共性但无法确定方法体时，定义为抽象方法。
   - 强制子类按照格式重写。
   - 抽象方法所在类必须是抽象类。

2. **抽象类和抽象方法的格式**
   - `public abstract 返回值类型 方法名(参数列表);`
   - `public abstract class 类名 {}`

3. **继承抽象类注意事项**
   - 要么重写所有抽象方法
   - 要么子类也是抽象类