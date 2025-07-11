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

**各具体类：**

```java
package Interface.P1;

public class PingPongPlayer extends Player implements SpeakEnglish {
    @Override
    public void Learning() { System.out.println("学习打乒乓球"); }
    @Override
    public void speakEnglish() { System.out.println("说英语"); }
    // ...构造方法...
}

public class PingPongCoach extends Coach implements SpeakEnglish {
    @Override
    public void Teaching() { System.out.println("教打乒乓球"); }
    @Override
    public void speakEnglish() { System.out.println("说英语"); }
    // ...构造方法...
}

public class BasketballCoach extends Coach {
    @Override
    public void Teaching() { System.out.println("教打篮球"); }
    // ...构造方法...
}

public class BasketballPlayer extends Player {
    @Override
    public void Learning() { System.out.println("学习打篮球"); }
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

### JDK8

- **默认方法**  
  - 格式：`public default 返回值类型 方法名(参数列表) { }`
  - 注意：不是抽象方法，不强制重写；重写时去掉`default`。
  - 多接口冲突时，必须重写。
- **静态方法**  
  - 格式：`public static 返回值类型 方法名(参数列表) { }`
  - 只能通过接口名调用，不能被重写。

```java
public interface Any {
    public static void show() { System.out.println("show"); }
}
public class Any1 implements Any {
    public static void show() { System.out.println("Im showing"); }
}
```
> 注意：实现类的静态方法不是重写。

### JDK9

- **私有方法**  
  - 只能在接口内部使用，辅助默认方法/静态方法复用代码。

```java
public interface InterA {
    private void show3() { System.out.println("only"); }
    public default void show1() {
        System.out.println("...");
        show3();
    }
    public default void show2() {
        System.out.println("...");
        show3();
    }
}
```

---

## 适配器设计模式

- 当一个接口中抽象方法过多，但只需用部分时，可用适配器设计模式。
- 步骤：
  1. 编写中间类`XXXAdapter`，实现接口
  2. 对接口中抽象方法空实现
  3. 真正实现类继承中间类并重写需要用的方法
  4. 适配器类用`abstract`修饰，避免被直接实例化

---

## 总结

1. 接口代表规则，是行为的抽象。想让哪个类拥有某行为，让其实现对应接口即可。
2. 当方法参数是接口时，可以传递接口所有实现类对象，这称为接口多态。
        System.out.println("说英语");
    }

    public PingPongPlayer() {
    }

    public PingPongPlayer(String name, int age) {
        super(name, age);
    }
}

package Interface.P1;

public class PingPongCoach extends Coach implements SpeakEnglish{

    @Override
    public void Teaching() {
        System.out.println("教打乒乓球");
    }

    @Override
    public void speakEnglish() {
        System.out.println("说英语");
    }

    public PingPongCoach() {
    }

    public PingPongCoach(String name, int age) {
        super(name, age);
    }
}

package Interface.P1;

public class BasketballCoach extends Coach{

    @Override
    public void Teaching() {
        System.out.println("教打篮球");
    }

    public BasketballCoach() {
    }

    public BasketballCoach(String name, int age) {
        super(name, age);
    }
}

package Interface.P1;

public class BasketballPlayer extends Player {

    @Override
    public void Learning() {
        System.out.println("学习打篮球");
    }

    public BasketballPlayer() {
    }

    public BasketballPlayer(String name, int age) {
        super(name, age);
    }
}

还有一个说英语的接口
package Interface.P1;

public interface SpeakEnglish {
    public void speakEnglish();
}


接口拓展

JDK8新特性

1.可定义有方法体的默认方法
# 接口中默认方法的定义格式
格式：`public default 返回值类型 方法名(参数列表) { }`

# 接口中默认方法的注意事项
1. 默认方法不是抽象方法，所以不强制被重写。但是如果被重写，重写的时候去掉 `default` 关键字
2. `public` 可以省略，`default` 不能省略
3. 如果实现了多个接口，多个接口中存在相同名字的默认方法，子类就必须对该方法进行重写
在接口中定义->
`public(可省) default void method(){sout("1");}`
//不强制要求实现该接口的类重写method方法

2.允许在接口中定义静态方法，需要static修饰
# 接口中静态方法的定义格式：
- 格式：`public static` 返回值类型 方法名(参数列表){ }
- 范例：`public static void show(){ }`

# 接口中静态方法的注意事项：
- 无需重写,而且不能被重写
- 静态方法只能通过接口名调用，不能通过实现类名或者对象名调用
- `public`可以省略，`static`不能省略
public interface any{
public static void show(){sout("show");}
}
但是需要注意，比如
public class any1 implements any
{
    public static void show()
    {
        sout("Im showing");
    }
} 
//show方法并非重写，只是重名
//静态方法不会被添加到虚方法表;

JDK9新特性
可定义接口中的私有方法,只用于接口内使用
public interface InterA
{
    /*public default void show3()
    {
sout("only");
    }*/
    //上文用于java8
    private void show3()
    {
        sout("only");
    }//java9+
    public default void show1()
    {
        sout("...");
        show3();
        sout("...");
        sout("...");
    }
    public default void show2()
    {
        sout("...");
        show3();
        sout("...");
        sout("...");
    }
}

### 总结

1. 接口代表规则，是行为的抽象。想要让哪个类拥有一个行为，就让这个类实现对应的接口就可以了。
2. 当一个方法的参数是接口时，可以传递接口所有实现类的对象，这种方式称之为接口多态。


适配器设计模式
# 总结
1. 当一个接口中抽象方法过多，但是我只要使用其中一部分的时候，就可以适配器设计模式
2. 书写步骤：
   - 编写中间类`XXXAdapter`，实现对应的接口
   - 对接口中的抽象方法进行空实现
   - 让真正的实现类继承中间类，并重写需要用的方法
   - 为了避免其他类创建适配器类的对象，中间的适配器类用`abstract`进行修饰
