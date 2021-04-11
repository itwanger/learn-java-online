## 编译和运行


如果你要创建一个简单的程序并在屏幕上打印，那么你需要编译你的代码并运行它。

你不用管你用的是Linux, Mac 或者 Windows. 你需要在命令行中执行如下的命令来编译和运行Java程序。

* java
* javac

你首先需要下载安装JDK(Java Development Kit).

```java
    public class MyFirstClass {
        public static void main(String[] args) {
            System.out.println("Hello, World!");
        }
    }
```

我们创建了一个 MyFirstClass.java 文件，内容如上所示, 然后编译它:

```
    javac MyFirstClass.java
```
这会生成一个叫 MyFirstClass.class 的文件，它保存了编译后的代码.

然后执行它, 使用它的类名做参数 (不是文件名!)

错误的写法
```
    java MyFirstClass.class
```
正确的写法!
```
    java MyFirstClass
```
### 运行时带有参数

main方法可以传入一个字符串数组, 可以通过命名行传入.

数组有个长度length的属性（可以打印出来 看看数组包含了多少个数据）.

例如
```java
    public class Arguments {
        public static void main(String[] args) {
            for (int i = 0; i < args.length; i++) {
                System.out.println(args[i]);
            }
        }
    }
```
然后编译 并 运行（含参数）:
```
    javac Arguments.java
    java Arguments arg0 arg1 arg2
```
