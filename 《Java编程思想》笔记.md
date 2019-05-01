# Java编程思想笔记

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

## 第5章 初始化与清理

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

