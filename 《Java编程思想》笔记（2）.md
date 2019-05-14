# Java编程思想笔记

- [第九章 接口](#第九章-接口)

## 第九章 接口

### 9.1 抽象类和抽象方法

Java提供一个叫做**抽象方法**的机制，这种方法是不完整的；仅有声明而没有方法体。具体语法：`abstract void f();`。

包含抽象方法的类叫做**抽象类**，++如果一个类包含一个或多个抽象方法，该类必须限定为抽象++。否则，编译器会报错。

> 如果从一个抽象类继承，并想创建该类的对象，那么就必须为基类中的所有抽象方法提供方法定义。如果不这样做，那么导出类仍然是抽象类，并且编译器会强制我们用`abstract`关键字来限定这个类。

当然我们也会创建一个没有任何抽象方法的抽象类。如果一个类，让它包含任何一个抽象类都没有意义，而且我们也想要组织产生一个该类的对象，那么创建一个没有任何抽象方法的抽象类就显得很有用了。

### 9.2 接口

`interface`关键字产生一个**完全抽象的类**，它根本就没有提供任何具体的实现。它允许创建者确定方法名，参数列表和返回类型，但没有任何方法体。接口只提供了形式，而未提供任何具体实现。

想要创建一个接口，需要用interface关键字来替代class关键字。就像类一样，可以在interface关键字前面添加public关键字。++如果不添加，则它只具有包访问权限++，这样它就只能在同一个包内可用。要让一个类遵循某个特定的接口，需要使用`implements`关键字，它表示：“interface只是它的外貌，但是现在我要**声明**它使如何工作的”。

### 9.3 完全解耦

```java
import java.util.*;
import static net.mindview.util.Print.*;

class Processor {
    public String name() {
        return getClass().getSimpleName();
    }
    Object process(Object input) {
        return input;
    }
}

class Upcase extends Processor {
    String process(Object input) {
        return ((String)input).toUppercase();
    }
}

class Downcase extends Processor {
    String process(Object input) {
        return ((String)input).toLowercase();
    }
}

class Splitter extends Processor {
    String process(Object input) {
        return Arrays.toString(((String)input).split(" "));
    }
}

public class Apply {
    public static void process(Processor p, Object s) {
        print("Using Processor " + p.name());
        print(p.process(s));
    }
    
    public static String s = "Abc abc";
    public static void main(String[] args) {
        process(new Upcase(), s);
        process(new Downcase(), s);
        process(new Splitter(), s);
    }
}/* Output:
Using Processor Upcase
ABC ABC
Using Processor Downcase
abc abc
Using Processor Splitter
[Abc, abc]
*///:~
```
`Apply.process()`方法可以接受任何类型的`Processor`，并将其应用到一个`Object`对象上，然后打印结果。像本例一样，**创建一个能够根据所传递的参数对象的不同而具有不同行为的方法，被称为策略设计模式。**这类方法包含所要执行的算法中固定不变的部分，而“策略”包含变化的部分。

~~~
待补充
~~~

### 9.4 多重继承

在C++中，组合多个类的接口的行为被称作**多重继承**。它可能会使你背负很沉重的包袱，因为每一个类都有具体的实现。在Java中，你可以执行相同的行为，但是只有一个类可以有具体实现（**通过extends继承来的类，可以有具体的实现**）
```java
interface CanFight {
    void fight();
}

interface CanSwim {
    void swim();
}

interface Canfly {
    void fly();
}

class ActionCharacter {
    public void fight();
}

class hero extends ActionCharacter implements CanFight, CanSwim, CanFly {
    public void swim();
    public void fly();
}

public class Adventure {
    public static void t(CanFight x) {
        x.fight();
    }
    public static void u(CanSwin x) {
        x.swim();
    }
    public static void v(CanFly x) {
        x.fly();
    }
    public static void w(ActionCharacter x) {
        x.fight();
    }
    
    public static void main(String[] args) {
        Hero h = new Hero();
        t(h);
        u(h);
        v(h);
        w(h);//Hero可以向上转型为每一个接口
    }
}
```
`extends`必须放在`implements`的前面,否则编译器会报错。

注意，`CanFight`接口与`ActionCharacter`类中的`fight()`方法的特征签名一样，而且，在`Hero`中并没有提供`fight()`的定义。即使`Hero`没有显示地提供`fight()`的定义，其定义也因`ActionCharacter`而随之而来，这样就使得创建`Hero`对象成为可能。

使用接口的核心原因：**为了能够向上转型为多个基类型**。使用接口的第二个原因（与使用抽象基类相同）：**防止客户端程序员创建该类的对象。**