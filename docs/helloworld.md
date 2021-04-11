## hello world

Java 是一门面向对象的编程语言 (OOP). 

我们来看一个简单的 Hello world 程序, 用来在屏幕上打印出 "Hello, World!" .

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

第一行定义了一个叫 Main 的类.

```public class Main {```

在 Java 中,每一行实际运行的代码，都必须包含在类的内部。这行声明了一个名为 Main 的类，它是 public 的, 这意味着其他任何类都可以访问它。

请注意，当我们声明了一个 public 类，我们必须使用相同的名称（Main.java）的来命名这个文件，否则编译会出错。

下一行代码是:


```public static void main(String[] args) {```

这是 Java 程序的入口，以确保我们可以运行 Java 程序。 

- `public` 意味着任何人都可以访问它.
- `static` 意味着你可以不创建实例运行此 Main 方法.
- `void` 意味着此方法不返回任何值。
- `main` 方法的名称。
- `String[] args` 运行程序的参数。这是一个字符串数组。

`System.out.println("Hello, World!");`

- System 是一个预先定义的类，Java 为我们提供，它拥有一些有用的方法和变量。
- out 是 System 类中的静态变量，可以提供打印输出。
- println 是输出的一种方法，可用于打印一行信息。