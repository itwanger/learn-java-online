## 对象

在Java中，一切都包含在类和对象中. Java对象持有状态，状态是该对象用来保存数据的变量，我们称他们为字段或成员变量。

让我们从一个例子开始：
```java
    class Point {
        int x;
        int y;
    }
```
这个类定义一个点 （拥有x和y属性）

如果要实例化一个类, 需要使用关键字 `new`.
```java
    Point p = new Point();
```
在这个列子中, 我们使用了默认构造方法 (这个构造方法没有参数) 创建这个类. 所有的类不需要显式定义不干任何事情的默认构造方法.

我们可以定义一个自己的构造方法:
```java
    class Point {
        int x;
        int y;
        Point(int x, int y) {
            this.x = x;
            this.y = y;
        }
```
这意味着我们不能再用默认的构造方法 `new Point()` 了. 我们现在只能够使用这个构造方法 `new Point(4, 1)`.

我们也可以定义多个构造方法,  `Point` 对象可以用多种方式来创建.下面，我们重新定义了默认构造方法.
```java
    class Point {
        int x;
        int y;
        Point() {
            this(0, 0);
        }
        Point(int x, int y) {
            this.x = x;
            this.y = y;
        }
```
注意关键字 `this` 的使用. 我们可以在构造方法中调用其他的构造方法  (避免代码重复)，但只能写在构造方法的第一行.

在运行这个类时，我们也可以使用 `this` 关键字作为当前对象来使用 .

定义了对象`p`后， `p` 可以用来访问它的属性 `x` 和 `y`	.
```java
    p.x = 3;
    p.y = 6;
```
### 方法

我们可以在类 `Point` 中定义方法。
```java
    class Point {
        ... // 打印
        void printPoint() {
            System.out.println("(" + x + "," + y + ")");
        }
        Point center(Point other) {
            // 返回中心点
			// 因为我们使用的是整形, 这会得到一个约值
            return new Point((x + other.x) / 2, (y + other.y) / 2);
        }
```
### public 和 private


当我们使用关键字 `private` 定义在变量和方法前时, 是指只用类本身才能访问方法和变量, 当使用 `public` 时，指所有的类都可以访问到它。 我们常常看到构造方法定义为`public`，变量定义为 private。

