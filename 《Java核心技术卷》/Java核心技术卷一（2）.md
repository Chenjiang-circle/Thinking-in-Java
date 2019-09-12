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

