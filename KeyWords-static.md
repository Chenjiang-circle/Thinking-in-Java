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