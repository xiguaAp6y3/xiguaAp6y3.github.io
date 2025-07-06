---
title: Java 静态 继承 学习笔记
published: 2025-07-06
description: "Java面向对象编程进阶知识，包括静态、继承等核心概念"
tags: ["Java", "面向对象", "静态", "继承"]
category: Java
draft: false
---

# Java面向对象进阶

## Chapter 1 静态

### 1.1 静态变量

```java
public static int a; // 类中静态变量，类似C++
```

静态变量用于同类的所有对象，存储在堆内存中。

#### 被 `static` 修饰的成员变量，叫做静态变量

**特点**：
- 被该类所有对象共享
- 不属于对象，属于类
- 随着类的加载而加载，优先于对象存在

**调用方式**：
- 类名调用（推荐）
- 对象名调用

### 1.2 静态方法

被`static`修饰的成员方法，叫做静态方法。

#### 特点：
- 多用在测试类和工具类中
- Javabean类中很少会用

#### 调用方式：
- 类名调用（推荐）
- 对象名调用

> **注意**：Javabean类是描述一类事物的类，还有测试类和工具类。工具类不描述任何事物，工具类就放方法。

### 1.3 静态方法示例

以下演示工具类 `StudentUtil` 返回最大年龄值：

**主函数**：
```java
package ToObjectAdvanced;
import java.util.ArrayList;

public class StudentTest {
    public static void main(String[] args) {
        Student s1 = new Student();
        s1.setAge(18);
        s1.setName("zhangsan");
        s1.setSex("nan");
        s1.tn = "Sun";
        
        Student s2 = new Student();
        s2.setAge(24);
        s2.setName("lisi");
        s2.setSex("nan");
        System.out.println(s2.tn);
        
        ArrayList<Student> STD = new ArrayList<>();
        STD.add(s1);
        STD.add(s2);
        System.out.println(StudentUtil.Studentmaxage(STD));
    }
}
```

**工具类**：
```java
package ToObjectAdvanced;
import java.util.ArrayList;

public class StudentUtil {
    public static int Studentmaxage(ArrayList<Student> list) {
        int tmax = -1;
        for (Student student : list) {
            tmax = Math.max(tmax, student.getAge());
        }
        return tmax;
    }
}
```

### 1.4 静态方法的访问规则

- 静态方法只能访问静态变量，没有this关键字
- 非静态方法可以访问所有成员
- 对于类内方法，下面两个效果相同：
  ```java
  public void show1(Student this); 
  public void show1();
  ```
- 静态数据在类加载后即在堆内存加载，默认为null

---

## Chapter 2 继承

### 2.1 继承基础

#### 关键字 `extends`

```java
public class Student extends Person {}
```

#### 继承的特点：
1. Java只能单继承，不能多继承，但是可以多层继承
2. Java中所有的类都直接或者间接继承于 `Object` 类
3. 子类只能访问父类中非私有的成员

### 2.2 继承练习1 - 动物继承体系

**需求**：设计四种动物的继承体系
- 布偶猫：吃饭、喝水、抓老鼠
- 中国狸花猫：吃饭、喝水、抓老鼠
- 哈士奇：吃饭、喝水、看家、拆家
- 泰迪：吃饭、喝水、看家、蹭一蹭

**设计思路**：
```
动物 → 猫，狗
猫 → 布偶，狸花
狗 → 哈士奇，泰迪
```

### 2.3 继承的重要概念

- 父类的构造方法不能被子类继承
- 一个类会默认加载Object作为父类
- 子类可以继承所有成员变量，但是父类的private变量不能直接访问，可以调用父类的public方法
- 内类可以直接访问外类的private变量
- 每次继承会把虚方法添加入虚方法表

#### 虚方法定义：
虚方法是公开权限/非静态/不被final修饰的方法，如果不是虚方法就要去找，找到了被private修饰就拒绝访问，没找到就一直顺路往上找。

### 2.4 继承中的访问规则

继承中遵循就近原则，如果子类方法/变量存在就优先子类，但是还有super关键字，用super可以调用基类的变量/方法，但是最多一次。

```java
class Fu {
    String name = "Fu";
    String hobby = "喝茶";
}

class Zi extends Fu {
    String name = "Zi";
    String game = "吃鸡";

    public void show() {
        // 如何打印 Zi
        // System.out.println(name); // Zi
        // System.out.println(this.name); // Zi
        
        // 如何打印 Fu
        // System.out.println(super.name); // Fu
        
        // 如何打印喝茶
        // System.out.println(hobby); // 喝茶
        // System.out.println(this.hobby); // 喝茶
        // System.out.println(super.hobby); // 喝茶
        
        // 如何打印吃鸡
        System.out.println(game);
        System.out.println(this.game);
    }
}
```

### 2.5 方法的重写

类似C++的override，当父类的方法不能满足子类现在的需求时，需要进行方法重写。

#### 书写格式
在继承体系中，子类出现了和父类中一模一样的方法声明，我们就称子类这个方法是重写的方法。

#### @Override重写注解
1. @Override是放在重写后的方法上，校验子类重写时语法是否正确
2. 加上注解后如果有红色波浪线，表示语法错误
3. 建议重写方法都加@Override注解，代码安全，优雅！

### 2.6 继承练习2 - 狗类继承体系

**需求**：设计三种狗的继承结构
- 哈士奇：吃饭（吃狗粮）、喝水、看家、拆家
- 沙皮狗：吃饭（吃狗粮、吃骨头）、喝水、看家
- 中华田园犬：吃饭（吃剩饭）、喝水、看家

**实现**：

**父类 Dog**：
```java
package ToObjectAdvanced.Extends.P1;

public class Dog {
    public void eat() {
        System.out.println("吃狗粮");
    }
    
    public void drink() {
        System.out.println("喝水");
    }
    
    public void stay() {
        System.out.println("看家");
    }
}
```

**子类 Hashiqi**：
```java
package ToObjectAdvanced.Extends.P1;

public class Hashiqi extends Dog {
    public void destory() {
        System.out.println("拆家");
    }
}
```

**子类 Shapigou**：
```java
package ToObjectAdvanced.Extends.P1;

public class Shapigou extends Dog {
    @Override
    public void eat() {
        System.out.println("吃狗粮，吃骨头");
    }
}
```

**子类 Playmachine**：
```java
package ToObjectAdvanced.Extends.P1;

public class Playmachine extends Dog {
    @Override
    public void eat() {
        System.out.println("吃剩饭");
    }
}
```

### 2.7 继承中的构造方法 this & super

#### 重要规则：
- 子类不能继承父类的构造方法，但是可以通过 `super` 调用
- 子类构造方法的第一行，有一个默认的 `super()`
- 默认先访问父类中无参的构造方法，再执行自己
- 如果想要调用父类有参构造，必须手动书写

#### 示例：
```java
// 基类 person 有 name,age; person(String name,int age);
// 子类 student 另外还有 id; 那么就可以这么写
student(String id, String name, int age) {
    super(name, age);
    this.id = id;
}

// 还有如果想默认构造一个student对象; 
student() {
    this("001", null, 0);
} // 调用3个参数的构造函数。
  // 而且这里用了this，代表已经调用其他构造函数，该构造函数不会再用super()了;
```

### 2.8 继承练习3 - 带有继承结构的标准Javabean类

**需求**：
1. 经理
   - 成员变量：工号，姓名，工资，管理奖金
   - 成员方法：工作（管理其他人），吃饭（吃米饭）
2. 厨师
   - 成员变量：工号，姓名，工资
   - 成员方法：工作（炒菜），吃饭（吃米饭）

**实现**：

**父类 Worker**：
```java
package ToObjectAdvanced.Extends.P2;

public class worker {
    private String id, name;
    private int salary;
    
    public worker() {
        this(null, null, 0);
    }
    
    public worker(String id, String name, int salary) {
        this.id = id;
        this.name = name;
        this.salary = salary;
    }
    
    public void work() {
        System.out.println("只是工作");
    }
    
    public void eat() {
        System.out.println("吃米饭");
    }
}
```

**子类 Manager**：
```java
package ToObjectAdvanced.Extends.P2;

public class manager extends worker {
    private int manager_bonus;
    
    public manager() {
        this(null, null, 0, 0);
    }
    
    public manager(String id, String name, int salary, int mb) {
        super(id, name, salary);
        this.manager_bonus = mb;
    }
    
    @Override
    public void work() {
        System.out.println("我要管理其他人");
    }
}
```

**子类 Cook**：
```java
package ToObjectAdvanced.Extends.P2;

public class cook extends worker {
    public cook() {
        this(null, null, 0);
    }
    
    public cook(String id, String name, int salary) {
        super(id, name, salary);
    }
    
    @Override
    public void work() {
        System.out.println("炒菜");
    }
}
```

---

*最后更新：2024年7月6日*
