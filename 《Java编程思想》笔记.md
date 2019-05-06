# Java编程思想笔记

- [第一章 对象导论](#第一章-对象导论)
- [第二章 一切都是对象](#第二章-一切都是对象)
- [第三章 操作符](#第三章-操作符)
- [第四章 控制执行流程](#第四章-控制执行流程)
- [第五章 初始化与清理](#第五章-初始化与清理)
- [第六章 访问权限控制](#第六章-访问权限控制)
- [第七章 复用类](#第七章-复用类)

## 第一章 对象导论

面向对象程序设计方式：

1. 万物皆为对象。
2. 程序是对象的集合，他们通过发送消息来告知彼此所要做的。
3. 每个对象都有自己的由其他对象所构成的存储。换句话说，可以通过创建包含 现有对对象的包的方式来创建新类型的对象。
4. 每个对象都有其类型。即“每个对象都是某个类的实例”。
5. 某个特定类型的所有对象都可以接受同样的信息。


Booch对对象提出了一个更加简洁的描述：“对象具有状态、行为和标识。”也就是       说，每个对象都可以有内部数据（状态）和方法（行为）。

因为类描述了具有相同特征（数据元素）和行为（功能）的对象集合，所以一个类实际上就是一个数据类型，例如所有的浮点型数字都具有相同的特性和行为集合。二者的差异就在于，程序员通过定义类来适应问题，而不再被迫只能使用现有的用来表示机器中的存储单元的数据类型。可以根据需求，通过添加新的数据类型来扩展编程语言。编程系统欣然接受新的类，并且像对待内置类型一样地照管它们地进行类型检测。

## 第二章 一切都是对象

### 2.2.2 基本类型
Java要确定每种基本类型所占存储空间的大小。它们的大小并不像其他大多数语言那样随机器硬件架构的变化而变化。这种所占存储空间大小的不变性是Java程序比其他大多数语言编写的程序更***具可移植性的原因之一***。


基本类型 | 大小 | 最大值 | 最小值 | 包装器类型
---|---|---|---|---
boolean | - | - | - | Boolean
char | 16-bit | Unicode o | Unicode 2<sup>16</sup>-1 | Character
byte | 8 bits | -128 | +127 | Byte
short | 16 bits | -2<sup>15</sup> | +2<sup>15</sup>-1 | Short
int | 32 bits | -2<sup>31</sup> | +2<sup>31</sup>-1 | Integer
long | 64 bits | -2<sup>63</sup> | +2<sup>63</sup>-1 | Long
float | 32 bits | IEEE754 | IEEE754 | Float
double | 64 bits | IEEE754 | IEEE754 | Double
void | - | - | - | Void

Java SE5的自动包装功能将自动地将基本类型转化为包装器类型：
```java
Character ch = 'x';
```
并可以反向转换：
```java
char c = ch;
```
**高精度数字**

Java提供了两个用于高精度计算的类：BigInteger 和 BigDecimal。这两个类包含的方法，提供的操作与对基本数据类型所能执行的操作相似。也就是说，能作用于int 和 float 的操作，也同样能作用于BigInteger 或 BigDecimal。不过必须以方法调用方式取代运算符方式来实现。

在使用任何引用之前，必须为其指定一个对象；如果试图使用一个还是null的引用，在运行时将会报错。

若类的某个成员是基本数据类型，即使没有进行初始化，Java也会确保它获得一个默认值，如图：

基本类型 | 默认值
---|---
boolean | false
char | ‘\uoooo’(null)
byte | (byte)o
short | (short)o
int | o
long | oL
float | o.of
double | o.od

然而上述确保初始化的方法并不适用于“局部变量”（即并非某个类的字段）。因此在某个**方法**定义中有：

>int x;

那么变量x得到的值可能是任意值，而不会被自动初始化为零。

## 第三章 操作符

要生成数字，车徐首先要创建一个Random类的对象。如果创建过程中没有传递任何参数，那么Java就会将当前时间作为随机数生成器的种子，并由此在程序每一次执行时都产生不同的输出。

对于前缀递增和前缀递减（如\++a或a--），会先执行运算，再生成值。而对于后缀递增和后缀递减（如 a++ 或 a--），会先生成值，再执行运算。如下：
```java
//: operators/AutoInc.java
// Demonstrates the ++ and -- opterators.
import static net.mindview.util.Print.*;

public class AutoInc {
    public static void main(String[] args){
        int i = 1;
        print("i : " + i);
        print("++i : " + ++i);
        print("i++ : " + i++);
        print("i :" + i);
        print("--i : " + --i);
        print("i-- : " + i--);
        print("i : " + i);
    }
} /* Output:
i : 1
++i : 2
i++ : 2
1 : 3
--i : 2
i-- : 2
i : 1
*///:~
```
## 第四章 控制执行流程

### 4.7 臭名昭著的goto

1. 一般的continue会退回最内层循环的开头，并继续执行。
2. 带标签的continue会到达标签的位置，并重新进入紧接再那个标签后面的循环。
3. 一般的break会中断并跳出当前循环。
4. 带标签的break会终端并跳出标签所指的循环。
```java
import static net.mindview.util.Print.*;

public class LabledWhile{
    public static void main(String[] args){
        int i = 0;
        outer:
        while(true){
            print("Outer while loop");
            while(true){
                i++;
                print("i = " + i);
                if(i == 1) {
                    print("continue");
                    continue;
                }
                if(i == 3) {
                    print("continue outer");
                    continue outer;
                }
                if(i == 5) {
                    print("break");
                    break;
                }
                if(i == 7) {
                    print("break outer");
                    break outer;
                }
            }
        }
    }
} /* Output:
Outer while loop
i = 1
continue
i = 2
i = 3
continue outer
Outer while loop
i = 4
i = 5
break
Outer while loop
i = 6
i = 7
break outer
*///:~
```
>**注意**：在Java里需要使用标签的唯一理由就是因为有循环嵌套存在，而且向从多层嵌套中**break**或**continue**。

## 第五章 初始化与清理

### 5.3 默认构造器

默认构造器（又名“无参”构造器）时没有形式参数的————它的作用是创建一个“默认对象”。如果你写的类中没有构造器，则编译器会自动帮你创建一个默认的构造器。但是，如果定义了一个构造器，无论是否有参数，编译器就不会帮你自动创建默认构造器：
```java
class Bird {
    Bird(int i);
    Bird(double d);
}
public class NoSynthesis {
    //! Bird b = new Bird(); // No default
    Bird b2 = new Bird(1);
    Bird b3 = new Bird(1.0);
}
```

### 5.4 this关键字

**this关键字只能在方法内部使用，表示对“调用方法的那个对象”的引用。**

**this**关键字对于将当前对象传递给其他方法也很有用：
```java
class Person {
    public void eat(Apple apple) {
        Apple peeled = apple.getPeeled();
        System.out.println("Yummy");
    }
}

class Peeler {
    static Apple peel(Apple apple) {
        // ... remove peel
        return apple; // Peeled
    }
}

class Apple {
    Apple getPeeled() {
        return Peeler.peel(this);
    }
}

public class PassingThis {
    public static void main(String[] args) {
        new Person().eat(new Apple());
    }
} /* Output:
Yummy
*///:~
```

#### 5.4.1 在构造器中调用构造器

通常写this的时候，都是指“这个对象”或者“当前对象”，而且它本身表示对当前对象的引用。在构造器中，如果为this添加了参数列表，那么就有了不同的含义。这将产生对***符合此参数列表的某个构造器***的明确调用；这样，调用其他构造器就有了直接的途径：
```java
//: initialization/Flower.java
// Calling constructors with "this"

import static net.mindview.util.Print.*;
public class Flower {
    int petalCount = 0;
    String s = "initial value";
    Flower(int petals) {
        tetalCount  = perals;
        print("Constructor w/ int arg only, petalCount= " + petalCount);
    }
    Flower(String ss) {
        print("Constructor w/ String arg only, s = " + ss);
        s = ss;
    }
    Flower(String s, int petals) {
        this(petals);
        //!  this(s); // Can't call two!
        this.s = s;
        print("String & int args");
    }
    Flower() {
        this("hi", 47);
        print("default constructor (no args)")
    }
    
    void printPetalCount() {
        //! this(11); // Not inside non-constructor!
        print("petalCount = " + petalCount + " s = " + s);
    }
    
    public static void main(String[] args) {
        Flower x = new Flower();
        x.printPetalCount();
    }
} /* Output:
Constructor w/ int arg only, petalCount= 47
String & int args
default constructor (no args)
petalCount = 47 s = hi
*///:~
```
构造器**Flower(String s, int petals)** 表明：尽管可以用**this**调用一个构造器，但却不能调用两个。此外，必须将构造器调用置于最起始处，否则编译会出错。

**除构造器外，编译器禁止在其他任何方法中调用构造器。**

### 5.7 构造器的初始化

#### 5.7.1 初始化顺序

再类的内部，变量定义的先后顺序决定了初始化的顺序。即使变量定义散步于方法定义之间，它们仍旧会再任何方法（包括构造器）被调用之前得到初始化。

例如：
```java
//: initialization/OrderOfInitialization.java
// Demonstrates initialization order.
import static net.mindview.util.Print.*;

// When the constructor is called to create a
// Window object, you'll see a message:
class Window{
    Window(int marker){
        print("Window(" + marker + ")");
    }
}

class House {
    Window w1 = new Window(1); // Before constructor
    House() {
        // Show taht we're in the constructor;
        print("House()");
        w3 = new Window(33);
    }
    Window w2 = new Window(2); // After constructor;
    void f(){
        print("f()");
    }
    Window w3 = new Window(3); // At end
}

public static OrderOfInitialization {
    public static void main(String[] args) {
        House h = new House();
        h.f(); // Shows that construction is done
    }
} /* Output:
Window(1)
Window(2)
Window(3)
House()
Window(33)
f()
*///:~
```

### 5.8 数组的初始化

再Java中可以将一个数组赋值给另一个数组，其实真正做的只是赋值一个引用，就像下面演示的那样：
```java
//: initialization/ArraysOfPrimitives.java
import static net mindview.util.Print.*;

public class ArrayOfPrimitives {
    public static void main(String[] args) {
        int[] a1 = { 1, 2, 3, 4, 5 };
        int[] a2;
        a2 = a1;
        for(int i = 0; i < a2.length; i++) {
            a2[i] = a2[i] + 1;
        }
        for(int i = 0; i < a1.length; i++) {
            print("a1[" + i + "] = " + a1[i]);
        }
    }
} /* Output:
a1[0] = 2
a1[1] = 3
a1[2] = 4
a1[3] = 5
a1[4] = 6
*///:~
```
可以看到代码中给出了a1的初始值，a2却没有；再本例中，a2是在后面被赋给另一个数组的。由于a2和a1是相同数组的别名，因此通过a2所做的修改在a1中可以看到。

### 5.9 枚举类型

关于枚举类型的介绍，请参见[这里](https://blog.csdn.net/javazejian/article/details/71333103)

## 第六章 访问权限控制

### 6.1 包：库单元

当编写一个Java源代码文件时，此文件通常被称为==编译单元==（有时也被称为转译单元）。每个编译单元都必须有一个后缀`.java`，而在编译单元中则可以有一个public类，该类的名称必须与文件的名称相同（包括大小写，但是不包括文件的后缀名.java）。每个编译单元只能有一个public类，否则编译器就不会接受。如果在该编译器单元之中还有额外的类的话，那么在包之外的世界是无法看见这些类的，这是因为它们不是public类，而且它们主要用来为主public类提供支持。

#### 6.1.1 代码组织

当编译一个`.java`文件时，在.java文件中的每一个类都会有一个输出文件，而该输出文件的名称与.java文件中每一个类的名称相同，只是多了一个后缀名`.class`。

==java包的命名规则全部使用小写字母，包括中间字==

package个import关键字允许你做的，是将单一的全局名字空间分割开，使无论多少人使用Internet以及Java开始编写类，都不会出现名称冲突问题。

**冲突**

如果将两个含有相同名称的类库以“*”形式同时导入，将会出现什么情况呢？例如，假设某程序这样写：
```java
import net.mindview.simple.*;
import java.util.*;
```
由于`java.util.*`也会含有`Vector`类(net.mindview.simple类库中含有Vector类),这样就存在潜在的冲突。但是只要你不写那些冲突的程序代码，就不会有什么问题，否则就得做很多的类型检查工作来防止那些根本不会出现的冲突。

如果现在要创建一个Vector类，就会产生冲突：
```java
Vector v = new Vector();
```
这行到底取用的时哪个Vector类？编译器不知道，读者也不知。于是编译器提出错误信息，强制你明确指明。于是你就得这样写：
```java
java.util.Vector v = new java.util.Vector();
```

## 第七章 复用类

### 7.1 组合语法
```java
//: resing/SprinklerSystem.java
// Composition for code reuse.

class WaterSource {
    private String s;
    WaterSource() {
        System.out.println("WaterSource");
        s = "Constructed";
    }
    public String toString() {
        return s;
    }
}

public class SprinklerSystem {
    private String value1, value2, value3, value4;
    private WaterSource source = new WaterSource();
    private int i;
    private float f;
    public String toString() {
        return "value1 = " + value1 + " " + "value2 = " + value2 + " " + "value3 = " + value3 + " " + "value4 = " + value4 + "\n" + "i = " + i + " " + "f = " + f + " " + "source = " + source;
    }
}

public static void main(String[] args) {
    SprinklerSystem sprinklers = new SprinklerSystem();
    System.out.println("sprinklers");
} /* Output:
WaterSource()
value1 = null value2 = null value3 = null value4 = null
i = 0 f = 0.0 source = Constructed
```
每一个非基本类型的对象都有一个toString()方法，而且当编译器需要一个String而你确只有一个对象时，该方法便会被调用，所以在SprinklerSystem.toString()的表达式中：
```java
"source = " + source；
```
编译器将会得知你想要将一个String对象("source=")同WaterSource相加。

编译器并不时简单地为每一个引用都创建默认对象。如果想初始化引用，可以在代码中的下列位置进行：
1. 在定义对象的地方。
2. 在类的构造器中。
3. 就在正要使用这些对象之前，这种方式称为`惰性初始化`，在生成对象不值得或者不必要每次都成成对象的情况下，这种方式可以减少额外的负担。
4. 使用实例初始化。
```java
class Soap {
    private String s;
    Soap {
        print("Soap()");
        s = "Constructed";//在类构造器中实例化。
    }
    public String toString() {
        return s;
    }
}

public class Bath {
    private String s1 = "Happy", s2 = "Happy", s3, s4;
    private Soap castille;
    private int i;
    private float toy;
    public Bath() {
        print("Inside Bath()");
        s3 = "Joy";
        toy = 3.14f;
        castille = new Soap();
    }
    
    {
        i = 47;
    }
    
    public String toString() {
        if(s4 == null){
            s4 = "Joy";
        }
        return "s1 = " + s1 + "\n" + "s2 = " + s2 + "\n" + "s3 = " + s3 + "\n" + "s4 = " + s4 + "\n" + "i = " + i + "\n" + "toy = " + toy + "\n" + "castille = " + castille;
    }
    
    public static void main(String[] args) {
        Bath b = new Bath();
        print(b);
    }
} /* Output:
Inside Bath()
Soap()
s1 = Happy
s2 = Happy
s3 = Joy
s4 = Joy
i = 47
toy = 3.14
castille = Constructed
*/ //:~
```

### 7.2 继承语法

当创建一个类时，总是在继承，除非已经明确指出要从其他类中继承，否则就是在隐式地从Java地标准根类`Object`进行继承。

#### 7.2.1 初始化基类

对基类子对象的正确初始化是至关重要的，而且仅有一种方法来保证这一点：**在构造器中调用基类构造器来执行初始化**。Java会自动在导出类的构造器中插入对基类构造器的调用。
```java
import static net.mindview.util.Print.*;

class Art {
    Art() {
        print("Art constructor");
    }
}

class Drawing extends Art {
    Drawing() {
        print("Drawing constructor");
    }
}

public class Cartoon extends Drawing {
    public Cartoon() {
        print("Cartoon constructor");
    }
    public static void main(String[] args) {
        Cartoon x = new Cartoon();
    }
} /* Output:
Art constructor
Drawing constructor
Cartoon constructor
*///:~
```
**带参数的构造器**

如果没有默认的基类构造器，或者想要调用一个带参数的基类构造器，就必须用关键字`super`显示地编写调用基类构造器地语句，并且配以适当地参数列表：
```java
import static net.mindview.util.Print.*;

class Game {
    Game(int i) {
        print("Game constructor");
    }
}

class BoardGame extends Game {
    BoardGame(int i) {
        super(i);
        print("BoardGame constructor");
    }
}

public class Chess extends BoardGmae {
    Chess() {
        super(i);
        print("Chess constructor");
    }
    public static void main(String[] args) {
        Chess x = new Chess();
    }
} /* Output:
Game constructor
BoardGame constructor
Chess constructor
*///:~  
```

### 7.3 代理

先看例子：
```java
public class SpaceShipControls {
    void up(int velocity) {}
    void down(int velocity) {}
    void left(int velocity) {}
    void right(int velocity) {}
    void forward(int velocity) {}
    void back(int velocity) {}
    void turboBoost() {}
}
// 构造太空船地一种方式是使用继承
public class SpaceShip extends SpaceShipControls {
    private String name;
    public SpaceShip(String name) {
        this.name = name;
    }
    public String toString() {
        return name;
    }
    public static void main(String[] args) {
        SpaceShip protector = new SpaceShip("NSEA Protector");
        protector.forward(100);
    }
}
```
尚书代码中，`Space`并非真正地`SpaceShipControls`类型，即使你能告诉他让他`forward(100)`。更准确地讲，SpaceShip包含SpaceShipControls，与此同时，SpaceShipControls地所有方法在SpaceShip中都暴露了出来。***代理***就很好地解决了此难题：
```java
public class SpaceShipDelegation {
    private String name;
    private SpaceShipControls controls = new SpaceShipControls();
    public SpaceShipDelegation(String name) {
        this.name = name;
    }
    
    public void back(int velocity) {
        controls.back(velocity);
    }
    public void down(int velocity) {
        controls.down(velocity);
    }
    public void forward(int velocity) {
        controls.forward(velocity);
    }
    public void left(int velocity) {
        controls.left(velocity);
    }
    
    public static void main(String[] args) {
        SpaceShipDelegation protector = new SpaceShipDelegation("NSEA Protector");
        protector.forward(100);
    }
}
```
我们使用代理时可以拥有更多的控制力，因为我们可以选择只提供在成员对象的方法的某个子集。

### 7.6 protected关键字

在实际项目中，经常会想要将某个事物尽可能对这个世界隐藏起来，但仍然允许导出类的成员访问它们。关键字protected就是起这个作用。它指明：
> “就类的用户而言，这是private的，但对于任何继承此类的导出类或其他任何位于同一包内的类来说，它却是可以访问的。”

#### 7.7.2 再论组合和继承

到底是该用组合还是用继承，一个最清晰的判断办法就是问一问自己**是否需要从新类向基类进行向上转型**。如果必须向上转型，则继承是必要的；但是如果不需要，则应当好好考虑自己是否需要继承。

### 7.8 final关键字

#### 7.8.1 final数据

带有恒定初始值（即，编译期常量）的final static基本类型全用大写字母命名，并且字与字之间用下划线隔开。


**空白final**

Java允许生成“空白final”，所谓空白fianl是指被声明为final但未给定初值的域。无论什么情况，编译器都确保空白final在使用前必须被初始化。一个类中的final域可以做到根据对象而有所不同，却又保持其恒定不变的特性。
```java
class Poppet {
    private int i;
    Poppet(int i1) {
        i = i1;
    }
}

public class BlankFinal {
    private final int i = 0;
    private final int j;
    private final Poppet p;
    public BlankFinal() {
        j = 1;
        p = new Poppet(1);
    }
    public BlankFinal(int x) {
        j = x;
        p = new Poppet(x);
    }
    public static void main(String[] args) {
        new BlankFinal();
        new BlankFinal(47);
    }
}
```
必须在域的定义处或者每个构造器中用表达式对final进行赋值，这正是final域在使用前总是被初始化的原因所在。

**final参数**

Java允许在参数列表中以声明的方式将参数指明为final。这意味着你无法在方法中更改参数引用所指向的对对象。

#### 7.8.2 final方法

使用final方法可以防止任何继承类修改它的含义。这是处于设计的考虑：想要确保在继承中使用方法行为保持不变，并且不会被覆盖。

**final和private关键字**

类中所有的private方法都隐式地指定为final的。
如果你试图覆盖一个private方法，似乎是奏效的，而且编译器也不会给出错误信息
```java
import static net.mindview.util.Print.*;

class WithFinals {
    private final void f() {
        print("WithFinals.f()");
    }
    private void g() {
        print("WithFinals.g()");
    }
}

class OverridingPrivate extends WithFinals {
    private final void f() {
        print("OverridingPrivate.f()");
    }
    private void g() {
        print("OverrindPrivate.g()");
    }
}

class OverridingPrivate2 extends OverridingPrivate {
    private final void f() {
        print("OverridingPrivate2.f()");
    }
    private void g() {
        print("OverrindPrivate2.g()");
    }
}

public class FinalOverridingIllusion {
    public static void main(String[] args) {
        OverridingPrivate2 op2 = new OverridingPriavte2();
        op2.f();
        op2.g();
        // you can uocast:
        OverridingPrivate op = op2;
        // But you can't call the methods:
        //!op.f();
        //!op.g();
        //Same here:
        WithFinals wf = op2;
        //! wf.f();
        //! wf.g();
    }
} /* Output:
OverridingPrivate2.f();
OverridingPrivate2.g();
*///:~
```
“覆盖”只有在某方法是基类的接口的一部分时才会出现。如果某方法为private，它就不是基类接口的一部分。它仅是一些藏于类中的程序代码，只不过是具有相同的名称而已。如果在导出类中以相同的名称生成一个public、protected或包访问权限方法的话，该方法就不会产生在基类中出现的“仅具有相同名称”的情况。此时**你并没有覆盖该方法，仅是生成一个新的方法**。

#### 7.8.3 final类

当将某一个类的整体定义为final时，就表明了你不打算继承该类，而且也不允许其他人这么做。

## 第八章 多态

### 8.3构造器和多态

#### 8.3.1 构造器的调用顺序

下面的例子展示了组合、继承以及多态在构建顺序上的作用：
```java
import static net.mindview.util.Print.*;

class Meal {
    Meal() {
        print("Meal()");
    }
}

class Bread {
    Bread() {
        print("Bread()");
    }
}

class Cheese {
    Cheese() {
        print("Cheese()");
    }
}

class Lettuce {
    Lettuce() {
        print("Lettuce");
    }
}

class Lunch extends Meal {
    Lunch() {
        print("Lunch");
    }
}

class PortableLunch extends Lunch {
        portableLunch() {
            print("PortableLunch()");
        }
}

public class Sandwich extends PortableLunch {
    private Bread b = new Bread();
    private Cheese c = new Cheese();
    private Lettuce l = new  Lettuce();
    public Sandwich() {
        print("Sandwich");
    }
    public static void main(String[] args) {
        new Sandwich;
    }
}/* Output:
Meal()
Lunch()
PortableLunch()
Bread()
Cheese()
Lettuce()
Sandwich()
*///:~
```
上面复杂的对象调用构造器要遵照下面的顺序：
1. 调用基类构造器。这个步骤会不断地反复递归下去，首先是构造这种层次结构地根，然后是下一层导出类，等等，直到最底层地导出类。
2. 按照声明顺序调用成员地初始化方法。
3. 调用导出类构造器地主体

#### 8.3.3 构造器内部的多态方法的行为

如果在一个构造器的内部调用正在构造的对象的某个动态绑定方法，那么会发生什么呢？

在一般的方法内部，动态绑定的调用是在运行时才决定的。**如果要调用构造器内部的一个动态绑定方法，就要用到那个方法的被覆盖后的定义**。然而，这个调用的效果可能难以预料，因为被覆盖的方法在对象被完全构造之前就会被调用。这可能会造成一些难以发现的隐藏错误。
```java
import static net.mindview.util.Print.*;

class Glyph {
    void draw() {
        print("Glyph().draw()");
    }
    
    Glyph() {
        print("Glyph() before draw()");
        draw();
        print("Glyph() after draw()");
    }
}
class RoundGlyph extends Glyph {
    private int radius = 1;
    RoundGlyph(int r) {
        radius = r;
        print("RoundGlyph.RoundGlyph(), radius = " + radius);
    }
    void draw() {
        print("RoundGlyph.draw(), radius = " + radius);
    }
}

public class PolyConstructors {
    public static void main(String[] args) {
        new RoundGlyph(5);
    }
} /* Ouput:
Glyph() before draw()
RoundGlyph.draw(), radius = 0
Glyph() after draw()
RoundGlyph.RoundGlyph(), radius = 5
*///:~
```
**`Glyph.draw()`方法设计为将要被覆盖**，这种覆盖是在RoundGlyph中发生的。但是GLyph构造器会调用这个方法，结果导致了对RoundGlyph.draw()的调用。如果看到输出结果，我们会发现当Glyph的构造器调用draw()方法时，radius不是默认的值1，而是0。

初始化的实际过程是：
1. 在其他任何事物发生之前，将分配给对象的存储空间初始化成二进制零。
2. 如前述那样调用基类构造器。
3. 按照声明的顺序调用成员的初始化方法。
4. 调用导出类的构造器主体。

### 8.4 协变返回类型

所谓协变返回类型，表示在导出类中的被覆盖方法可以返回基类方法的返回类型的某种导出类型
```java
class Graip {
    public String toString() {
        return "Graip";
    }
}
class Wheat extends Graip {
    public String toString() {
        return "Wheat";
    }
}
class Mill {
    Grain process() {
        return new Graip;
    }
}
class WheatMill extends Mill {
    Wheat process() {
        return new Wheat;
    }
}

public class CovariantReturn {
    public static void main(String[] args) {
        Mill m = new Mill();
        Graip g = m.process();
        System.out.print(g);
        m = new WheatMill();
        g = m.process();
        System.out.print(g);
    }
} /* Output:
Graip
Wheat
*///:~
```
