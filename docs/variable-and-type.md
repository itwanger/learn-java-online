## 变量和类型

尽管 Java 是面向对象的，但并非所有类型都是对象。

下面列出了 Java 中的基本数据类型:

```java
byte (number, 1 byte)
short (number, 2 bytes)
int (number, 4 bytes)
long (number, 8 bytes)
float (float number, 4 bytes)
double (float number, 8 bytes)
char (a character, 2 bytes)
boolean (true or false, 1 byte)
```

Java 是一种强类型语言，这意味着我们在使用变量前需要先定义它们的类型。

### 数字

声明一个数字的语法:

```java
int myNumber;
myNumber = 5;
```

或者组合在一起:

```java
int myNumber = 5;
```

定义一个双精度浮点数的语法:

```java
double d = 4.5;
d = 3.0;
```

如果要转变为单精度浮点数:

```java
float f = (float) 4.5;
```

也可以这样写:

```java
float f = 4.5f (f is a shorter way of casting float)
```

### 字符和字符串

在 Java 中，字符用单引号括起来：

```java
char c = 'g';
```

String 不是一个基本数据类型，它是一个真正的类，但是 Java 对这个类有些特殊处理。

下面介绍一些使用字符串的列子：

```java
// 使用构造器创建一个字符串
String s1 = new String("谁让狗狗跑出去了?");
// 使用双引号 "" 创建一个字符串.
String s2 = "到底是谁!";
// java 定义了 + 运算符来把两个字符串串联起来
String s3 = s1 + s2;
```

Java 中没有方法符重载，除了 + 号操作符可以用来连接字符串。

可以用字符串和基本数据类型做连接操作:

```java
int num = 5;
String s = "I have " + num + " cookies"; //不要把原型变量用""包起来.
```

### 布尔值

在 Java 中，任何比较操作都会返回一个布尔值，对应的值为 true 或者 false.

```java
boolean b = false;
b = true;

boolean toBe = false;
b = toBe || !toBe;
if (b) {
System.out.println(toBe);
}

int children = 0;
b = children; // 不支持，会报错
if (children) { // 不支持，会报错
// 不支持，会报错
}
```
