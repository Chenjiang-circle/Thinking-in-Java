# static关键字的用途

#### 1. static方法

static方法一般称作静态方法。由于静态方法不依赖于任何对象就可以进行访问，因此对于静态变量来说，是没有this的。

**在静态方法中不能访问类的非静态成员变量和非静态成员方法**，因为非静态成员方法/变量都是必须依赖具体的对象才能够被调用。但是在非静态成员方法中是可以访问静态成员方法/变量的。

#### 2. static变量

static变量也称作静态变量，静态变量和非静态变量的区别是：**静态变量被所有的对象所共享，在内存中只有一个副本，它当且仅当在类初次加载时会被初始化。而非静态变量时对象所拥有的，在创建对象的时候被初始化，存在多个副本，各个对象拥有的副本互不影响。**

#### 3. static代码块

static关键字还有一个比较关键的作用就是：用来形成静态代码块以优化程序性能。static块可以置于类中的任何地方，类中可以有多个static块。在类初次被加载的时候，会按照static块的顺序来执行每个static块，并且只执行一次（因为只执行一次，所以优化了程序的性能）。例子如下：
```java
calss Person{
    private Date birthDate;

    public Person(Date birthDate) {
        this.birthDate = brithDate;
    }

    boolean isBornBoomer(){
        Date startDate  = Date.valueOf("1946");
        Date endDate  = Date.valueOf("1964");
        return birthDate.compareTo(startDate) >= 0 && birthDate.compareTo(endDate) < 0;
    }
}
```
isBornBoomer用来判断此人是不是1946-1964年出生的，但是每一次调用isBornBommer，都会生成startDate和birthDate两个对象，造成空间的浪费。如果改成这样效率就会更高：
```java
class Person{
    private Date birthDate;
    private static Date startDate, endDate;

    static{
        startDate = Date.valueOf("1946");
        endDate = Date.valueOf("1964");
    }

    public Person(Date birthDate) {
        this.birthDate = birthDate;
    }

    boolean isBornBoomer() {
        return birthDate.compareTo(startDate) >= 0 && birthDate.compareTo(endDate) < 0;
    }
}
```

# static关键字的误区

#### 1. static关键字并不改变类成员的访问权限。

#### 2. 能通过this访问静态成员变量么？

在非静态方法中能够通过this访问静态成员变量，例子如下：
```java
public class Main{
    static int value = 33;

    public static void main(String[] args) throws Exception{
        new Main().printValue();
    }

    private void printValue(){
        int value = 3;
        System.out.println(this.value);
    }
}
/*Output:
33
*/
```

static变量时被对象所享有的，this代表当前对象，理所当然地可以访问value值。

**静态成员变量虽然独立于对象，但是不代表不可以通过对象去访问，所有的静态方法和静态变量都可以通过对象访问（只要访问权限足够）。**

# 有关static的题目

#### 1. 下列代码的输出结果为：
```java
public class Test extends Base{
    
    static{
        System.out.println("test static");
    }
    
    public Test(){
        System.out.println("test constructor");
    }

    public static void main(String[] args){
        new Test();
    }
}

class Base{
    static{
        System.out.println("base static");
    }

    public Base(){
        System.out.println("base constructor");
    }
}
/*Output:
base static
test static
base constructor
test constructor
*/
```

上面代码的执行过程如下：

先要寻找到`main`方法，因为`main`方法时程序的入口，但是在执行`main`方法之前，必须先加载`Test`类，而在加载`Test`类的时候发现`Test`类继承自`Base`类，因此会转去加载`Base`类，在加载`Base`类的时候，发现有`static`块，便执行了`static`块。在`Base`类加载完成之后，便继续加载`Test`类，然后发现`Test`类也有`static`块，就由执行了`static`块。在加载完所需要的类之后，便开始执行`main`方法。在`main`方法中执行`new Test()`的时候会**先调用父类的构造器，然后再调用自身的构造器**。

#### 2. 下列代码的输出结果为：
```java
public class Test {
    Person person = new Person("Test");
    static{
        System.out.println("test static");
    }
     
    public Test() {
        System.out.println("test constructor");
    }
     
    public static void main(String[] args) {
        new MyClass();
    }
}
 
class Person{
    static{
        System.out.println("person static");
    }
    public Person(String str) {
        System.out.println("person "+str);
    }
}
 
 
class MyClass extends Test {
    Person person = new Person("MyClass");
    static{
        System.out.println("myclass static");
    }
     
    public MyClass() {
        System.out.println("myclass constructor");
    }
}
/*Output:
test static
myclass static
person static
person Test
test constructor
person MyClass
myclass constructor
*/
```

上面代码的执行过程如下：

首先加载`Test`类，因此先执行`Test`类中`static`块。接着执行`mian`方法中的`new MyClass()`，但是`MyClass`类没有加载，所以先去加载`MyClass`类，于是`MyClass`类中的`static`块就被执行。加载完成之后，就通过构造器来生成对象。而**在生成对象之前，必须先初始化父类的成员变量**，因此会执行`Test`类中的`Person person = new Person()`，而`Person`类还没有被加载过，因此先加载`Person`类，于是就执行了`Person`类中的`static`块，然后执行`Person`的构造器，接着执行父类的构造器，完成父类初始化，接着就该完成自身的初始化（即`MyClass`类的初始化），因此先执行`MyClass`中的`Person person  = new Person()`，最后执行`MyClass`的构造器。