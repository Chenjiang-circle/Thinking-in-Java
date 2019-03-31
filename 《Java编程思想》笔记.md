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