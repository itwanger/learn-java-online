## 条件语句

Java 使用布尔值作为条件，布尔值有 `true` and `false` 。它们可以作为条件或者结果返回 ，如下

```java
    int a = 4;
    boolean b = a == 4;
    if (b) {
        System.out.println("It's true!");
    }
```

通常我们不会定义一个条件语句给 boolean 变量，而是用一个更简洁的语法
```java
    int a = 4;
    if (a == 4) {
        System.out.println("Ohhh! So a is 4!");
    }
```
###  布尔运算符

并没有很多语法，大多都是很简单的
```java
    int a = 4;
    int b = 5;
    boolean result;
    result = a < b; // true
    result = a > b; // false
    result = a <= 4 // a 小于或者等于 4 - true
    result = b >= 6 // b 大于或者等于 6 - false
    result = a == b // a 等于 b - false
    result = a != b // a 不等于 b - true
    result = a > b || a < b // 逻辑或 or - true
    result = 3 < a && a < 6 // 逻辑与 and - true
    result = !result // 逻辑非 not - false
```
### if - else 和 between

IF 语句非常简单.
```java
    if (a == b) {
        // a and b are equal, let's do something cool
    }
```
可以在后面加上else语句，当if非真时，执行else括号中的代码
```java
    if (a == b) {
        //  a 等于 b 的时候
    } else {
        //  a 不等于 b 的时候
    }
```
也可以省略大括号，但是后面只能有一行语句
```java
    if (a == b) System.out.println("欧耶!");
    else System.out.println("噢噢噢噢...");
```
或者这样写
```java
    if (a == b)
        System.out.println("左边画龙!");
    else
        System.out.println("右边彩虹!");
```
建议不要省略括号，因为当if后面的语句不止一行时，就不起作用了，而且不利于阅读

### 还有一种简洁的方式替代if

还有一种可以写在一行替代if的方式就是使用三元操作符 `? :`
```java
    int a = 4;
    int result = a == 4 ? 1 : 8;
    // 结果为 1
    // 结果一样
    int result;
    if (a == 4) {
        result = 1;
    } else {
        result = 8;
    }
```
建议使用 if语句

### == 和 equals

操作符 `==` 在基本数据类型与引用数据类型之间的比较处理方法有一点不同. 当你比较两个对象时，操作符 `==` 比较的是他们是否是同一个对象, 如果你要比较他们的内容是否一样, 就要使用 `equals` 方法. 
列如：
```java
    String a = new String("哇");
    String b = new String("哇");
    String sameA = a;
    
    boolean r1 = a == b;      // 返回false, 因为ab不是同一个对象
    boolean r2 = a.equals(b); // true, ab的内容是一样的
    boolean r3 = a == sameA;  // true, a 和 sameA 就是同一个对象
```

