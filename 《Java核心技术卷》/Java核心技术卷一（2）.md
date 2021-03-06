# 第6章 借口、lambda表达式与内部类

## 接口
在借口中所有的方法都是public。不过在实现接口时，必须把方法声明为public。

接口不是类！！

尽管不能构造接口对象，却能声明接口的变量，接口变量必须引用实现了接口的类对象。接口允许被扩展（extends）。虽然在接口中不能包含实例域或静态方法，但却可以包含常量：
```java
public interface Powered wxtends Moveable
{
  double milesPeGallon();
  double SPEED_LIMIT = 95;
}
```
与接口中的方法都自动地被设置为public一样，借口中的域将被自动设为public static final

尽管每个类只能够拥有一个超类，但却可以实现多个接口。（使用逗号将实现的各个接口分隔开）

### 默认方法
可以为接口提供一个默认实现。必须用default修饰符标记这样一个方法。
```java
public interface Compareable<T>
{
  default int compareTo(T other)
  {
    return 0;
  }
}
```

### 解决默认方法冲突
如果先在一个接口中将一个方法定义为默认方法，然后又在超类或者另一个接口中定义了同样的方法，会发生什么？Java有响应的规则：
1. 超类优先。如果超类提供了一个具体方法，同名而且有相同参数类型的默认方法会被忽略。
2. 接口冲突。如果一个超类提供看一个默认方法，另一个接口提供了一个同名而且参数类型（不论是否是默认参数）相同的方法，必须覆盖这个方法来解决冲突。

## 接口示例
## lambda表达式
### 构造器引用

构造器引用与方法引用很类似，只不过方法名为new。例如，Person::new是Person构造器的一个引用。哪个构造器呢？这取决于上下文。假设你有一个字符串列表。可以把它转化为一个Person对象数组，为此需要在各个字符串上调用构造器，调用如下：
```java
ArrayList<String> names = ...;
Stream<Person> stream = names.stream().map(Person::new)；
List<Person> people = stream.collect(Collectors.toList());
```
map方法会为各个列表调用Person(String)构造器。如果有多个构造器，编译器会选择有一个String参数的构造器。

可以用数组类型建立构造器引用。例如，int[]::new是一个构造器引用，它有个参数：数组的长度。这等价于lambda表达式`x->new int[x]`。

### 变量作用域

在lambda表达式中，只能引用值不会改变的变量。例如，下面的做法是不合法的：

```java
public static void countDown(int start, int delay)
{
  ActionListener listener = event ->
  {
    start--; //Error:Can't mutate caputured variable
    SYstem.out.println(start);
  };
  new Timer(delay, listener).start();
}
```

之所以有这个限制是有原因的，如果在lambda表达式中改变变量，并发执行多个动作是就会不安全。另外，如果在lambda表达式中引用变量，而这个变量可能在外部改变，这也是不合法的。

这里有条规则：**lambda表达式中不会的变量必须是最终变量**。最终变量是指，这个变量在初始化之后就不会再为它赋新值。

lambda表达式的体与嵌套块有相同的作用域。这里同样适用名字冲突和遮蔽的有关规则。在lambda表达式中声明与一个局部变量同名的参数或局部变量是不合法的。

```java
Path first = Path.get("/usr/bin");
Comparator<String> comp =
  (first, second) -> first.length() - second.length();
  // Error: Variable first already defined
```

在一个lambda表达式中适用this关键字时，是指创建这个lambda表达式的方法的this参数。例如，
```java
public class Application()
{
  public void init()
  {
    ActionListener listener = event -> 
    {
      System.out.println(this.toString());
      ...
    }
    ...
  }
}
```

表达式this.toString()会调用Application对象的toString方法，而不是ActionListener实例的方法。在lambda表达式中，this的适用并没有任何特殊之处。

## 内部类

使用内部类的原因：
- 内部类方法可以访问该类定义所在的作用域中的数据，包括私有的数据。
- 内部类可以对同一个包中的其他类隐藏起来。
- 当想要定义一个回调函数且不想编写大量代码时，使用匿名内部类比较便捷

### 使用内部类访问对象状态

内部类既可以访问自身的数据域，与可以访问创建它的外围类对象的数据域。

只有内部类可以是私有类，而常规类只可以具有报可见性或共有可见性。

### 内部类的特殊语法规则

> 内部类中声明的所有静态域都必须是final。原因很简单，我们希望一个静态域只有一个实例，不过对于每个外部对象，会分别有一个单独的内部类实例。如果这个域不是final，它可能就不是唯一的。

### 内部类是否有用、必要和安全

内部类是一种编译器现象，与虚拟机无关。编译器将会把内部类翻译成用`$`分隔外部类名与内部类名的常规类文件，而虚拟机则对此一无所知。

### 局部内部类

局部类不能用public或private访问说明符进行说明。它的作用域被限制在声明这个局部类的块中。

```java
// 这是一个方法
public void start() {
  // 在调用这个方法的时候，才会知道这个内部类的存在
  class InnerClassName(Type param) {
    public staic void MethodName(Type param) {
      ...
    }
  }
}
```

### 由外部方法访问变量

在Java SE8之前，必须把从局部类访问的局部变量声明为final。有时，final限制显得并不太方便。例如，假设想更新在一个封闭作用域内的计数器。这栗想要统计一下在排列过程中调用compareTo方法的次数。
```java
int counter = 0;
Date[] dates = new Date[100];
for (int i = 0; i < dates.length; i++) {
  dates[i] = new Date()
  {
    public int compareTo(Date other) {
      counter++; // Error!!!!!!!
      return super.compareTo(other);
    }
  };
}
Arrays.sort(dates);
System.out.println(counter + " comparisons.");
```

由于清楚地知道counter需要更新，所以不能将counter声明为final。由于Integer对象是不可变的，所以也不能用Integer代替它。补救的方法是使用一个长度为1的数组：
```java
int[] counter = new int[1];
for (int i = 0; i < dates.length; i++) {
  dates[i] = new Date()
  {
    public int compareTo(Date other) {
      counter[0]++;
      return super.compareTo(other);
    }
  };
}
```

### 匿名内部类

加入只创建这个类的一个对象，就不必命名了。这种类被称为**匿名内部类**。
```Java
public void start(int interval, boolean beep) {
  // ActionListener是Java中的一个接口，需要实现它的方法：actionPerformed()
  ActionListener listener = new ActionListener() {
    public void actionPerformed(ActionEvent event) {
      System.out.println("At the tone, the time is " + new Date());
      if (beep) {
        Toolkit.getDefaultTookit().beep();
      }
    }
  };
  Timer t = new Timer(interval, listener);
  t.start();
} 
```

这种语法确实有些难以理解。它的含义是：创建一个实现ActionListener接口的类的新对象，需要实现的方法actionPerformed定义在括号{}中。

通常的语法格式为：
```java
new SuperType(construction parameters)
{
  inner class methods and data
}
```

其中，SuperType可以是ActionListener这样的接口，于是内部类就是要实现这个接口。SuperType也可以是一个类，于是内部类就要扩展它。

由于构造器的名字必须与类名相同，而匿名类没有类名，所以，**匿名类不能有构造器**。取而代之的是，将构造器参数传递给超类构造器。尤其是在内部类实现接口的时候，不能有任何构造器参数。不仅如此，还要像下面这样提供一组括号：
```java
new InterfaceType()
{
 methods and data
}
```

下面对比一下“构造一个类的新对象”与“构造一个扩展了这个类的匿名内部类的对象”之间的差别：
```java
Person queen = new Person("Mary"); // a person object
Person count = new Person("Dracula") {...}; // an object of an inner class extending Preson
```

如果构造参数的小括号后面跟一个大括号，正在定义的就是匿名内部类。

多年来，Java程序员习惯的做法是用匿名内部类实现事件监听器和其他回调。如今最好还是使用lambda表达式。

<hr>

**“双括号初始化”技巧**：假设你想构造一个数组列表，并将它传递到一个方法：
```java
ArrayListL<String> friends = new ArrayList<>();
friends.add("Harry");
friends.add("Tony");
invite(friends);
```

如果不再需要这个数组列表，最好让它作为一个匿名列表。方法如下：
```java
invite(new ArrayList<String>() {{ add("Harry"); add("Tony");}});
```

注意这里的双括号。外层括号建立了ArrayList的一个匿名子类。内层括号是一个对象构造块。

<hr>

建立一个与超类大体类似（但不完全一样）的匿名子类通常会很方便。不过，对于equals方法要特别小心。曾建议equals方法最好使用以下测试：<br>
`if（getClass() != other.getClass()） return false;`<br>
但是对匿名子类做这个测试是失败的。

生成日志或调试消息时，通常希望包含当前类的类名，如：<br>
`System.err.println("Somethis awful happened in " + getClass());`<br>
不过。这对静态方法不奏效。毕竟，调用getClass时调用的是this.getClass()，而静态方法没有this。所以应该使用一下表达式：<br>
`new Object(){}.getClss().getEnclosingClass() // gets class of static method`<br>
在这里，new Object(){}会建立Object的一个匿名子类的一个匿名对象，getEnclosingClass则得到其外围类，也就是包含这个静态方法的类。

### 静态内部类

只有内部类可以声明为static。静态内部类的对象除了没有对生成它的外围类对象的引用特权外，与其他所有内部类完全一样。在下面的例子中，必须使用静态内部类，这是由于内部类对象是在静态方法中构造的：
```java
// 这是一个静态方法，该方法需要返回一个调用该方法的类的内部类（Pair）对象
public static Pair minmax(double[] d) {
  ...
  return new Pair(min, max);
}
```

如果没有将内部类Pair声明为static，那么编译器将会给出错误报告：没有可用的隐式ClassName类型对象初始化内部类对象。

与常规内部类不同，静态内部类可以有静态域和方法。

声明在接口中的内部类自动成为static和public类。

## 代理

# 第十章 部署应用程序和applet

## JAR 文件

### 资源

在applet和应用程序中使用的类通常需要使用一些相关的数据文件，例如：
- 图像和声音文件。
- 带有消息的字符串和按钮标签的文本文件。
- 二进制数据文件，例如。描述地图布局的文件。

在 Java 中，这些关联的文件被称为资源。

