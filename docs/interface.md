## 接口

对于面向对象编程来说，抽象是一个极具魅力的特征。如果一个程序员的抽象思维很差，那他在编程中就会遇到很多困难，无法把业务变成具体的代码。在 Java 中，可以通过两种形式来达到抽象的目的，一种是抽象类，另外一种就是接口。

如果你现在就想知道抽象类与接口之间的区别，我可以提前给你说一个：

- 一个类只能继承一个抽象类，但却可以实现多个接口。

当然了，在没有搞清楚接口到底是什么，它可以做什么之前，这个区别理解起来会有点难度。

### 01、接口是什么

接口是通过 interface 关键字定义的，它可以包含一些常量和方法，来看下面这个示例。

```java
public interface Electronic {
    // 常量
    String LED = "LED";

    // 抽象方法
    int getElectricityUse();

    // 静态方法
    static boolean isEnergyEfficient(String electtronicType) {
        return electtronicType.equals(LED);
    }

    // 默认方法
    default void printDescription() {
        System.out.println("电子");
    }
}
```

1）接口中定义的变量会在编译的时候自动加上 `public static final` 修饰符，也就是说 LED 变量其实是一个常量。

Java 官方文档上有这样的声明：

>Every field declaration in the body of an interface is implicitly public, static, and final.

换句话说，接口可以用来作为常量类使用，还能省略掉 `public static final`，看似不错的一种选择，对吧？

不过，这种选择并不可取。因为接口的本意是对方法进行抽象，而常量接口会对子类中的变量造成命名空间上的“污染”。

2）没有使用 `private`、`default` 或者 `static` 关键字修饰的方法是隐式抽象的，在编译的时候会自动加上 `public abstract` 修饰符。也就是说 `getElectricityUse()` 其实是一个抽象方法，没有方法体——这是定义接口的本意。

3）从 Java 8 开始，接口中允许有静态方法，比如说 `isEnergyEfficient()` 方法。

静态方法无法由（实现了该接口的）类的对象调用，它只能通过接口的名字来调用，比如说 `Electronic.isEnergyEfficient("LED")`。

接口中定义静态方法的目的是为了提供一种简单的机制，使我们不必创建对象就能调用方法，从而提高接口的竞争力。

4）接口中允许定义 `default` 方法也是从 Java 8 开始的，比如说 `printDescription()`，它始终由一个代码块组成，为实现该接口而不覆盖该方法的类提供默认实现，也就是说，无法直接使用一个“;”号来结束默认方法——编译器会报错的。

![](https://upload-images.jianshu.io/upload_images/1179389-68bde3d324ab5d44.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

允许在接口中定义默认方法的理由是很充分的，因为一个接口可能有多个实现类，这些类就必须实现接口中定义的抽象类，否则编译器就会报错。假如我们需要在所有的实现类中追加某个具体的方法，在没有 `default` 方法的帮助下，我们就必须挨个对实现类进行修改。

来看一下 Electronic 接口反编译后的字节码吧，你会发现，接口中定义的所有变量或者方法，都会自动添加上 `public` 关键字——假如你想知道编译器在背后都默默做了哪些辅助，记住反编译字节码就对了。
```java
public interface Electronic
{

    public abstract int getElectricityUse();

    public static boolean isEnergyEfficient(String electtronicType)
    {
        return electtronicType.equals("LED");
    }

    public void printDescription()
    {
        System.out.println("\u7535\u5B50");
    }

    public static final String LED = "LED";
}
```

有些读者可能会问，“二哥，为什么我反编译后的字节码和你的不一样，你用了什么反编译工具？”其实没有什么秘密，微信搜「沉默王二」回复关键字「**JAD**」就可以免费获取了，超级好用。

### 02、定义接口的注意事项

由之前的例子我们就可以得出下面这些结论：

- 接口中允许定义变量
- 接口中允许定义抽象方法
- 接口中允许定义静态方法（Java 8 之后）
- 接口中允许定义默认方法（Java 8 之后）

除此之外，我们还应该知道：

1）接口不允许直接实例化。

![](https://upload-images.jianshu.io/upload_images/1179389-4a3a762eae494531.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

需要定义一个类去实现接口，然后再实例化。

```java
public class Computer implements Electronic {

    public static void main(String[] args) {
        new Computer();
    }

    @Override
    public int getElectricityUse() {
        return 0;
    }
}
```

2）接口可以是空的，既不定义变量，也不定义方法。

```java
public interface Serializable {
}
```

Serializable 是最典型的一个空的接口。

3）不要在定义接口的时候使用 final 关键字，否则会报编译错误，因为接口就是为了让子类实现的，而 final 阻止了这种行为。

![](https://upload-images.jianshu.io/upload_images/1179389-25bbbe251b2d4e77.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4）接口的抽象方法不能是 private、protected 或者 final。

![](https://upload-images.jianshu.io/upload_images/1179389-c385bee5b112fde3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](https://upload-images.jianshu.io/upload_images/1179389-71a24aff16c3f3c4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](https://upload-images.jianshu.io/upload_images/1179389-3d2f21dcc78e39f8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

5）接口的变量是隐式 `public static final`，所以其值无法改变。

### 03、接口可以做什么

1）使某些实现类具有我们想要的功能，比如说，实现了 Cloneable 接口的类具有拷贝的功能，实现了 Comparable 或者 Comparator 的类具有比较功能。

Cloneable 和 Serializable 一样，都属于标记型接口，它们内部都是空的。实现了 Cloneable 接口的类可以使用 `Object.clone()` 方法，否则会抛出 CloneNotSupportedException。

```java
public class CloneableTest implements Cloneable {
    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }

    public static void main(String[] args) throws CloneNotSupportedException {
        CloneableTest c1 = new CloneableTest();
        CloneableTest c2 = (CloneableTest) c1.clone();
    }
}
```

运行后没有报错。现在把 `implements Cloneable` 去掉。

```java
public class CloneableTest {
    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }

    public static void main(String[] args) throws CloneNotSupportedException {
        CloneableTest c1 = new CloneableTest();
        CloneableTest c2 = (CloneableTest) c1.clone();

    }
}
```

运行后抛出 CloneNotSupportedException：

```
Exception in thread "main" java.lang.CloneNotSupportedException: com.cmower.baeldung.interface1.CloneableTest
	at java.base/java.lang.Object.clone(Native Method)
	at com.cmower.baeldung.interface1.CloneableTest.clone(CloneableTest.java:6)
	at com.cmower.baeldung.interface1.CloneableTest.main(CloneableTest.java:11)
```




2）Java 原则上只支持单一继承，但通过接口可以实现多重继承的目的。

可能有些读者会问，“二哥，为什么 Java 只支持单一继承？”简单来解释一下。

如果有两个类共同继承（extends）一个有特定方法的父类，那么该方法会被两个子类重写。然后，如果你决定同时继承这两个子类，那么在你调用该重写方法时，编译器不能识别你要调用哪个子类的方法。这也正是著名的菱形问题，见下图。

![](https://upload-images.jianshu.io/upload_images/1179389-da9057082e43aad1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

ClassC 同时继承了 ClassA 和 ClassB，ClassC 的对象在调用 ClassA 和 ClassB 中重载的方法时，就不知道该调用 ClassA 的方法，还是 ClassB 的方法。

接口没有这方面的困扰。来定义两个接口，Fly 会飞，Run 会跑。

```java
public interface Fly {
    void fly();
}
public interface Run {
    void run();
}
```

然后让一个类同时实现这两个接口。

```java
public class Pig implements Fly,Run{
    @Override
    public void fly() {
        System.out.println("会飞的猪");
    }

    @Override
    public void run() {
        System.out.println("会跑的猪");
    }
}
```

这就在某种形式上达到了多重继承的目的：现实世界里，猪的确只会跑，但在雷军的眼里，站在风口的猪就会飞，这就需要赋予这只猪更多的能力，通过抽象类是无法实现的，只能通过接口。

3）实现多态。

什么是多态呢？通俗的理解，就是同一个事件发生在不同的对象上会产生不同的结果，鼠标左键点击窗口上的 X 号可以关闭窗口，点击超链接却可以打开新的网页。

多态可以通过继承（`extends`）的关系实现，也可以通过接口的形式实现。来看这样一个例子。

Shape 是表示一个形状。

```java
public interface Shape {
    String name();
}
```

圆是一个形状。

```java
public class Circle implements Shape {
    @Override
    public String name() {
        return "圆";
    }
}
```

正方形也是一个形状。

```java
public class Square implements Shape {
    @Override
    public String name() {
        return "正方形";
    }
}
```

然后来看测试类。

```java
List<Shape> shapes = new ArrayList<>();
Shape circleShape = new Circle();
Shape squareShape = new Square();

shapes.add(circleShape);
shapes.add(squareShape);

for (Shape shape : shapes) {
    System.out.println(shape.name());
}
```

多态的存在 3 个前提：

1、要有继承关系，Circle 和 Square 都实现了 Shape 接口
2、子类要重写父类的方法，Circle 和 Square 都重写了 `name()` 方法
3、父类引用指向子类对象，circleShape 和 squareShape 的类型都为 Shape，但前者指向的是 Circle 对象，后者指向的是 Square 对象。

然后，我们来看一下测试结果：

```
圆
正方形
```

也就意味着，尽管在 for 循环中，shape 的类型都为 Shape，但在调用 `name()` 方法的时候，它知道 Circle 对象应该调用 Circle 类的 `name()` 方法，Square 对象应该调用 Square 类的 `name()` 方法。

### 04、接口与抽象类的区别

好了，关于接口的一切，你应该都搞清楚了。现在回到读者春夏秋冬的那条留言，“兄弟，说说抽象类和接口之间的区别？”

1）语法层面上

- 接口中不能有 public 和 protected 修饰的方法，抽象类中可以有。
- 接口中的变量只能是隐式的常量，抽象类中可以有任意类型的变量。
- 一个类只能继承一个抽象类，但却可以实现多个接口。

2）设计层面上

抽象类是对类的一种抽象，继承抽象类的类和抽象类本身是一种 `is-a` 的关系。

接口是对类的某种行为的一种抽象，接口和类之间并没有很强的关联关系，所有的类都可以实现 `Serializable` 接口，从而具有序列化的功能。

