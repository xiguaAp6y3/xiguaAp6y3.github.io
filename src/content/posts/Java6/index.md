Java 接口 笔记

面向对象进阶 4
 
接口 
当需要给多个类同时定义规则，就要有接口。
接口就是一种规则，是对行为的抽象
插入 image.png

例1：
插入 image-1.png
实现：
Animal class
package Interface;

public abstract class Animal {
    private String name;
    private int age;

    public Animal() {
    }

    public Animal(String name, int age) {
        this.name = name;
        this.age = age;
    }

    /**
     * 获取
     * @return name
     */
    public String getName() {
        return name;
    }

    /**
     * 设置
     * @param name
     */
    public void setName(String name) {
        this.name = name;
    }

    /**
     * 获取
     * @return age
     */
    public int getAge() {
        return age;
    }

    /**
     * 设置
     * @param age
     */
    public void setAge(int age) {
        this.age = age;
    }

    public String toString() {
        return "Animal{name = " + name + ", age = " + age + "}";
    }
    public abstract void eat();
}

Frog class
package Interface;

public class Frog extends Animal implements Swim{
    @Override
    public void eat()
    {
        System.out.println("吃虫子");
    }

    public Frog() {
    }

    public Frog(String name, int age) {
        super(name, age);
    }

    @Override
    public void swim() {
        System.out.println("我会蛙泳");
    }

}

Dog class
package Interface;

public class Dog extends Animal implements Swim{

    @Override
    public void eat() {
        System.out.println("吃骨头");
    }

    public Dog() {
    }

    public Dog(String name, int age) {
        super(name, age);
    }

    @Override
    public void swim() {
        System.out.println("我会狗刨");
    }
}

Rabbit class 
package Interface;

public class Rabbit extends Animal {
    @Override
    public void eat() {
        System.out.println("吃胡萝卜");
    }

    public Rabbit() {
    }

    public Rabbit(String name, int age) {
        super(name, age);
    }
}

Swim interface

package Interface;

public interface Swim {
public abstract void swim();
}

Test class
package Interface;

public class Test {
    public static void main(String[] args)
    {
        Frog a = new Frog("96",14);
        System.out.println(a);
        a.eat();
        a.swim();
        Dog b = new Dog("旺财",14);
        System.out.println(b);
        b.eat();
        b.swim();
        Rabbit c = new Rabbit("土",14);
        System.out.println(c);
        c.eat();
    }

}
# 接口中成员的特点

- **成员变量**
  - 只能是常量
  - 默认修饰符：`public static final`

- **构造方法**
  - 没有

- **成员方法**
  - 只能是抽象方法
  - 默认修饰符：`public abstract`

- **JDK7以前**：接口中只能定义抽象方法。


# 接口和类之间的关系

- **类和类的关系**  
  继承关系，只能单继承，不能多继承，但是可以多层继承  

- **类和接口的关系**  
  实现关系，可以单实现，也可以多实现，还可以在继承一个类的同时实现多个接口  

- **接口和接口的关系**  
  继承关系，可以单继承，也可以多继承

类 实现 子接口 意味着要重写 该接口的所有方法。

例
### 编写带有接口和抽象类的标准Javabean类

我们现在有乒乓球运动员和篮球运动员，乒乓球教练和篮球教练。

为了出国交流，跟乒乓球相关的人员都需要学习英语。

请用所有知识分析，在这个案例中，哪些是具体类，哪些是抽象类，哪些是接口？

- 乒乓球运动员：姓名，年龄，学打乒乓球，说英语
- 篮球运动员：姓名，年龄，学打篮球
- 乒乓球教练：姓名，年龄，教打乒乓球，说英语
- 篮球教练：姓名，年龄，教打篮球

Person类 作为主类
package Interface.P1;

public abstract class Person {
    private String name;
    private int age;

    public Person() {
    }

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    /**
     * 获取
     * @return name
     */
    public String getName() {
        return name;
    }

    /**
     * 设置
     * @param name
     */
    public void setName(String name) {
        this.name = name;
    }

    /**
     * 获取
     * @return age
     */
    public int getAge() {
        return age;
    }

    /**
     * 设置
     * @param age
     */
    public void setAge(int age) {
        this.age = age;
    }

    public String toString() {
        return "Person{name = " + name + ", age = " + age + "}";
    }
}

Player Coach作为Person子类
package Interface.P1;
public abstract class Player extends Person {
    public abstract void Learning();

    public Player() {
    }

    public Player(String name, int age) {
        super(name, age);
    }
}

package Interface.P1;

public abstract class Coach extends Person{
    public abstract void Teaching();

    public Coach() {
    }

    public Coach(String name, int age) {
        super(name, age);
    }
}

分写四个类
package Interface.P1;


public class PingPongPlayer extends Player implements SpeakEnglish{

    @Override
    public void Learning() {
        System.out.println("学习打乒乓球");
    }

    @Override
    public void speakEnglish() {
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
