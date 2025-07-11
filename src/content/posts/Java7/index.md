---
title: Java 内部类 笔记
published: 2025-07-11
description: "Java 内部类（成员、静态、局部、匿名）用法与示例"
tags: ["Java", "内部类", "OOP"]
category: Java
draft: false
---

# Java 内部类笔记

> 日期：2025-07-11  
> 完成于：2025-07-11

## 目录

- [什么是内部类](#什么是内部类)
- [什么时候用到内部类](#什么时候用到内部类)
- [内部类的分类](#内部类的分类)
  - [成员内部类](#成员内部类)
  - [静态内部类](#静态内部类)
  - [局部内部类](#局部内部类)
  - [匿名内部类](#匿名内部类)
- [总结](#总结)

---

## 什么是内部类

写在一个类里面的类就叫做**内部类**。

---

## 什么时候用到内部类

当 B 类表示的事物是 A 类的一部分，且 B 单独存在没有意义时，适合用内部类。  
如：汽车的`发动机`，ArrayList的`迭代器`，人的`心脏`等。

---

## 内部类的分类

### 1. 成员内部类

如 Car 中的 Engine 类：

```java
package Inner;

public class Car {
    String name;
    String ID;
    String CarColor;
    int CarAge;
    Engine e;

    public Car(){}
    public Car(String name, String ID, String carColor, int carAge, String engineName, int engineAge) {
        this.name = name;
        this.ID = ID;
        CarColor = carColor;
        CarAge = carAge;
        e = new Engine(engineName, engineAge);
    }
    public void show() {
        System.out.println("Car->" + name + "->ID" + ID + "->" + CarColor + CarAge + "ENG" + e);
    }
    public class Engine {
        String EngineName;
        int EngineAge;
        Engine(){}
        public Engine(String engineName, int engineAge) {
            EngineName = engineName;
            EngineAge = engineAge;
        }
        @Override
        public String toString() {
            return "Engine{" +
                    "EngineName='" + EngineName + '\'' +
                    ", EngineAge=" + EngineAge +
                    '}';
        }
    }
}
```

- 可以用 `private`、`protected` 等修饰成员内部类。
- 获取成员内部类对象的两种方式：
  1. 外部类方法创建：
     ```java
     public Engine getInstance() {
         return new Engine();
     }
     ```
  2. 直接创建：
     ```java
     Car.Engine e = new Car().new Engine();
     ```
- JDK16 新特性：成员内部类中可以定义静态变量。

#### 作用域演示

```java
public class Outer {
    private int a = 10;
    class Inner {
        private int a = 20;
        public void show() {
            int a = 30;
            // System.out.println(Outer.this.a); // 10
            // System.out.println(this.a); // 20
            // System.out.println(a); // 30
        }
    }
}
```

---

### 2. 静态内部类

```java
public class Car {
    // ...existing code...
    public static class Engine {
        public static void show() {
            System.out.println("show");
        }
        public void show2() {
            System.out.println("show");
        }
    }
}
```

- 直接创建静态内部类对象：
  ```java
  Car.Engine a = new Car.Engine();
  ```
- 调用静态方法：
  ```java
  Car.Engine.show();
  ```
- 调用非静态方法：
  ```java
  Car.Engine a = new Car.Engine();
  a.show2();
  ```

---

### 3. 局部内部类

- 定义在方法内部，类似于方法的局部变量（不能用 public/protected/private 修饰，但可用 final）。
- 外界无法直接使用，需要在方法内部创建对象并使用。
- 可以访问外部类成员和方法内的局部变量。

---

### 4. 匿名内部类

- 本质：没有名字的内部类，常用于只用一次的场景。
- 格式：
  ```java
  new 类名或接口名() {
      // 重写方法
  };
  ```
- 示例：

```java
package Inner.Hide;

public interface Swim {
    public abstract void Swim();
}

public abstract class Animal implements Swim {
    @Override
    public abstract void Swim();
}

public class Test {
    public static void main(String[] Args) {
        method(
            new Animal() {
                @Override
                public void Swim() {
                    System.out.println("狗会狗刨泳");
                }
            }
        );
    }
    public static void method(Animal a) {
        a.Swim();
    }
}
```

- 接口多态示例：

```java
Swim s = new Swim() {
    public void Swim() {
        System.out.println("重写");
    }
};
s.Swim();
```

---

## 总结

1. **什么是匿名内部类？**  
   隐藏了名字的内部类，可以写在成员位置，也可以写在局部位置。

2. **匿名内部类的格式？**
   ```java
   new 类名或接口名() {
       重写方法;
   };
   ```

3. **格式细节**  
   包含继承/实现、方法重写、对象创建。整体是类的子类对象或接口的实现类对象。

4. **使用场景**  
   当方法参数是接口或类时，如果实现类只用一次，可用匿名内部类简化代码。


