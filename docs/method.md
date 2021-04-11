## 方法

在Java中，方法都是定义在类中的，  如下：
```java
    public class Main {
        public static void foo() {
            // Do something here
        }
    }
```
`foo` 是我们在Main类中定义的方法. 

- `static` 意思是这个方法属于Main 类， 而不是属于Main类的实例化对象. 也就是说我们可以在另外一个类里面像这样 `Main.foo()` 直接用。
- `void` 意思是这个方法没有返回值。 
- 现在这个方法没有任何参数, 但是在其他场景的Java方法中，可以有一个或多个参数。

### 带参的方法

在 Java 中，方法参数只有值传递.

值传递表示，当方法运行时，将会拷贝参数，然后在方法内使用。下面是我们定义的方法
```java
    public void bar(int num1, int num2) {
        ...
    }
```
这是在另外一个地方调用这个方法
```java
    int a = 3;
    int b = 5;
    bar(a, b);
```
当 `bar(a, b)` 运行时，你可以想象在运行 `bar` 的后两行加上一个这样的代码 :
```java
    int num1 = a;
    int num2 = b;
```
然后再执行方法的其余部分。

这意味着 `a` 的值拷贝到 `num1` ， `b` 的值拷贝到 `num2`. 在方法内改变 `num1` 和 `num2`不会影响 `a` 和 `b`.

如果参数是对象,规则是一样的,但是执行时有一点点区别. 如下:
```java
    public void bar2(Student s1, Student s2) {
        ...
    }
```
现在我们使用这个方法
```java
    Student joe = new Student("王二");
    Student jack = new Student("王三");
    bar2(joe, jack);
```
同样我们在运行`bar2(joe, jack)`后加入这两段代码：
```java
    Student s1 = joe;
    Student s2 = jack;
```
但是我们在对象赋值时, 和基本数据类型赋值时有一点区别. `s1` 和`joe` 是两个不同的引用指向同一个对象 . `s1 == joe` 是`true`，这意味着我们运行方法时改变 `s1` 同时会改变`joe` . 但是我们改变`s1`引用指向到其他值， 那么不会影响到索引 `joe`.
```java
    s1.setName("张三"); // joe 的 name 也会变为 张三 
    s1 = new Student("张二"); //  s1 是一个新的 student对象 , 已经和 joe指向的对象不同了
    // s1 == joe    这个时候s1 和 joe 已经不想等了 为false
```
### 非静态方法

非静态方法在Java中比静态方法用得多得多。非静态方法只能通过对象调用，而静态方法可以直接通过类调用（整个类都能用，不需要类实例化）

非静态方法可以访问和修改该对象的字段， 如下的 name 值。
```java
    public class Student {
        private String name;
        public String getName() {
            return name;
        }
        public void setName(String name) {
            this.name = name;
        }
    }
```
 调用方法需要先声明一个 `Student` 类的对象， 如下
```java
    Student s = new Student();
    s.setName("王五");
    String name = s.getName();

    Student.setName("王六"); // 不能执行 这是静态方法的方式
    Student.getName(); // 不能执行
```
### 总结

- 每个Java方法必须在类中
- 静态方法属于类，而非静态方法属于对象
- 所有方法的参数都是按值传递的，基本数据类型的内容被复制，而对象不会被复制，有些人会说“按引用传递”——是错误的。


