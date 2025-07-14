---
title: Java 接口笔记
published: 2025-07-10
description: "面向对象进阶4：接口、接口特性、接口与类关系、接口新特性、适配器模式"
tags: ["Java", "接口", "OOP"]
category: Java
draft: false
---

# Java 接口笔记

> 日期：2025-07-10

## 目录

- [接口的概念](#接口的概念)
- [接口实现案例](#接口实现案例)
- [接口中成员的特点](#接口中成员的特点)
- [接口和类之间的关系](#接口和类之间的关系)
- [编写带有接口和抽象类的标准JavaBean类](#编写带有接口和抽象类的标准javabean类)
- [接口新特性（JDK8/9）](#接口新特性jdk89)
- [适配器设计模式](#适配器设计模式)
- [总结](#总结)

---

## 接口的概念

- 接口就是一种规则，是对行为的抽象。
- 当需要给多个类同时定义规则，就要有接口。

![接口示意图](./image.png)

---

## 接口实现案例

![案例UML](./image-1.png)

**Animal类：**

```java
package Interface;

public abstract class Animal {
    private String name;
    private int age;
    // ...getter/setter/toString...
    public abstract void eat();
}
```

**Frog类：**

```java
package Interface;

public class Frog extends Animal implements Swim {
    @Override
    public void eat() { System.out.println("吃虫子"); }
    @Override
    public void swim() { System.out.println("我会蛙泳"); }
    // ...构造方法...
}
```

**Dog类：**

```java
package Interface;

public class Dog extends Animal implements Swim {
    @Override
    public void eat() { System.out.println("吃骨头"); }
    @Override
    public void swim() { System.out.println("我会狗刨"); }
    // ...构造方法...
}
```

**Rabbit类：**

```java
package Interface;

public class Rabbit extends Animal {
    @Override
    public void eat() { System.out.println("吃胡萝卜"); }
    // ...构造方法...
}
```

**Swim接口：**

```java
package Interface;

public interface Swim {
    public abstract void swim();
}
```

**Test类：**

```java
package Interface;

public class Test {
    public static void main(String[] args) {
        Frog a = new Frog("96", 14);
        System.out.println(a);
        a.eat();
        a.swim();
        Dog b = new Dog("旺财", 14);
        System.out.println(b);
        b.eat();
        b.swim();
        Rabbit c = new Rabbit("土", 14);
        System.out.println(c);
        c.eat();
    }
}
```

---

## 接口中成员的特点

- **成员变量**
  - 只能是常量，默认修饰符：`public static final`
- **构造方法**
  - 没有
- **成员方法**
  - 只能是抽象方法，默认修饰符：`public abstract`
- **JDK7以前**：接口中只能定义抽象方法。

---

## 接口和类之间的关系

- **类和类的关系**  
  继承关系，只能单继承，不能多继承，但可以多层继承。
- **类和接口的关系**  
  实现关系，可以单实现，也可以多实现，还可以在继承一个类的同时实现多个接口。
- **接口和接口的关系**  
  继承关系，可以单继承，也可以多继承。

类实现子接口意味着要重写该接口的所有方法。

---

## 编写带有接口和抽象类的标准JavaBean类

**需求分析：**

- 乒乓球运动员：姓名，年龄，学打乒乓球，说英语
- 篮球运动员：姓名，年龄，学打篮球
- 乒乓球教练：姓名，年龄，教打乒乓球，说英语
- 篮球教练：姓名，年龄，教打篮球

**Person类：**

```java
package Interface.P1;

public abstract class Person {
    private String name;
    private int age;
    // ...getter/setter/toString...
}
```

**Player/Coach抽象类：**

```java
package Interface.P1;

public abstract class Player extends Person {
    public abstract void Learning();
    // ...构造方法...
}

public abstract class Coach extends Person {
    public abstract void Teaching();
    // ...构造方法...
}
```

**SpeakEnglish接口：**

```java
package Interface.P1;

public interface SpeakEnglish {
    public void speakEnglish();
}
```

---

## 接口新特性（JDK8/9）

### JDK8新特性

#### 1. 默认方法

- 格式：`public default 返回值类型 方法名(参数列表) { }`
- 注意事项：
  1. 默认方法不是抽象方法，不强制重写，重写时去掉 `default` 关键字
  2. `public` 可省略，`default` 不能省略
  3. 多接口冲突时，子类必须重写

```java
public default void method() { System.out.println("1"); }
```

#### 2. 静态方法

- 格式：`public static 返回值类型 方法名(参数列表) { }`
- 注意事项：
  - 无需重写，也不能被重写
  - 只能通过接口名调用
  - `public` 可省略，`static` 不能省略

```java
public interface Any {
    public static void show() { System.out.println("show"); }
}

public class Any1 implements Any {
    public static void show() { System.out.println("Im showing"); }
}
// show方法并非重写，只是重名
// 静态方法不会被添加到虚方法表
```

### JDK9新特性

#### 私有方法

- 只能在接口内部使用，用于代码复用

```java
public interface InterA {
    private void show3() { System.out.println("only"); }
    
    public default void show1() {
        System.out.println("...");
        show3();
        System.out.println("...");
    }
    
    public default void show2() {
        System.out.println("...");
        show3();
        System.out.println("...");
    }
}
```

---

## 适配器设计模式

### 使用场景
当一个接口中抽象方法过多，但只需要使用其中一部分时，可以使用适配器设计模式。

### 实现步骤
1. 编写中间类 `XXXAdapter`，实现对应的接口
2. 对接口中的抽象方法进行空实现
3. 让真正的实现类继承中间类，并重写需要用的方法
4. 中间适配器类用 `abstract` 修饰，避免被直接实例化

---

## 总结

1. **接口的作用**  
   接口代表规则，是行为的抽象。想要让哪个类拥有一个行为，就让这个类实现对应的接口。

2. **接口多态**  
   当一个方法的参数是接口时，可以传递接口所有实现类的对象，这种方式称为接口多态。

3. **接口新特性**  
   - JDK8：默认方法、静态方法
   - JDK9：私有方法

4. **适配器模式**  
   用于简化接口实现，避免实现不需要的方法。