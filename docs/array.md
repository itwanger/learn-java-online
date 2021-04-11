## 数据

### 01、什么是数组

数组是一个对象，它包含了一组固定数量的元素，并且这些元素的类型是相同的。数组会按照索引的方式将元素放在指定的位置上，意味着我们可以通过索引来访问到这些元素。在 Java 中，索引从 0 开始。

我们可以将数组理解为一个个整齐排列的单元格，每个单元格里面存放着一个元素。

数组元素的类型可以是基本数据类型（比如说 int、double），也可以是引用数据类型（比如说 String），包括自定义类型的对象。

了解了数组的定义后，让我们来深入地研究一下数组的用法。

在 Java 中，数组的声明方式有两种。

先来看第一种：

```java
int[] anArray;
```

再来看第二种：

```java
int anOtherArray[];
```

不同之处就在于中括号的位置，是紧跟类型，还是放在变量名的后面。前者比后者的使用频率更高一些。

接下来就该看看怎么初始化数组了，同样有多种方式可以初始化数组，比如说最常见的是：

```java
int[] anArray = new int[10];
```

使用了 new 关键字，对吧？这就意味着数组的确是一个对象。然后，在方括号中指定了数组的长度，这是必须的。

这时候，数组中的每个元素都会被初始化为默认值，int 类型的就为 0，Object 类型的就为 null。

另外，还可以使用大括号的方式，直接初始化数组中的元素：

```java
int anOtherArray[] = new int[] {1, 2, 3, 4, 5};
```

这时候，数组的元素分别是 1、2、3、4、5，索引依次是 0、1、2、3、4。

### 02、访问数组

前面提到过，可以通过索引来访问数组的元素，就像下面这样：

```java
anArray[0] = 10;
System.out.println(anArray[0]);
```

通过数组的变量名，加上中括号，加上元素的索引，就可以访问到数组，通过“=”操作符进行赋值。

如果索引的值超出了数组的界限，就会抛出 `ArrayIndexOutOfBoundException`，关于这方面的知识，我之前特意写过一篇文章，如果你感兴趣的话，可以跳转过去看看。

[为什么会发生ArrayIndexOutOfBoundsException](https://mp.weixin.qq.com/s/TRyVTQqMGmqs4lmHtsgRuw)

我觉得原因挺有意思的。

既然数组的索引是从 0 开始，那就是到数组的 `length - 1` 结束，不要使用超出这个范围内的索引访问数组，就不会抛出数组越界的异常了。 

### 03、遍历数组

当数组的元素非常多的时候，逐个访问数组就太辛苦了，所以需要通过遍历的方式。

第一种，使用 for 循环：

```java
int anOtherArray[] = new int[] {1, 2, 3, 4, 5};
for (int i = 0; i < anOtherArray.length; i++) {
    System.out.println(anOtherArray[i]);
}
```

通过 length 属性获取到数组的长度，然后索引从 0 开始遍历，就得到了数组的所有元素。

第二种，使用 for-each 循环：

```java
for (int element : anOtherArray) {
    System.out.println(element);
}
```

如果不需要关心索引的话（意味着不需要修改数组的某个元素），使用 for-each 遍历更简洁一些。当然，也可以使用 while 和 do-while 循环。

### 04、可变参数

可变参数用于将任意数量的参数传递给方法：

```java
void varargsMethod(String... varargs) {}
```

`varargsMethod()` 方法可以传递任意数量的字符串参数，可以是 0 个或者 N 个，本质上，可变参数就是通过数组实现的，为了证明这一点，我们可以通过 jad 反编译一下字节码：

```java
public class VarargsDemo
{

    public VarargsDemo()
    {
    }

    transient void varargsMethod(String as[])
    {
    }
}
```

所以我们其实可以直接将数组作为参数传递给可变参数的方法：

```java
VarargsDemo demo = new VarargsDemo();
String[] anArray = new String[] {"沉默王二", "一枚有趣的程序员"};
demo.varargsMethod(anArray);
```

也可以直接传递多个字符串，通过逗号隔开的方式：

```java
demo.varargsMethod("沉默王二", "一枚有趣的程序员");
```

### 05、把数组转成 List

List 封装了很多常用的方法，方便我们对集合进行一些操作，而如果直接操作数组的话，多有不便，因此有时候我们需要把数组转成 List。

最原始的方式，就是通过遍历数组的方式，一个个将数组添加到 List 中。

```java
int[] anArray = new int[] {1, 2, 3, 4, 5};

List<Integer> aList = new ArrayList<>();
for (int element : anArray) {
    aList.add(element);
}
```

更优雅的方式是通过 Arrays 类的 `asList()` 方法：

```java
List<Integer> aList = Arrays.asList(anArray);
```

但需要注意的是，该方法返回的 ArrayList 并不是 `java.util.ArrayList`，它其实是  Arrays 类的一个内部类：

```java
private static class ArrayList<E> extends AbstractList<E>
        implements RandomAccess, java.io.Serializable{}
```

如果需要添加元素或者删除元素的话，最好把它转成 `java.util.ArrayList`。

```java
new ArrayList<>(Arrays.asList(anArray));
```

### 06、把数组转成 Stream

Java 8 新增了 Stream 流的概念，这就意味着我们也可以将数组转成 Stream 进行操作，而不是 List。

```java
String[] anArray = new String[] {"沉默王二", "一枚有趣的程序员", "好好珍重他"};
Stream<String> aStream = Arrays.stream(anArray);
```

也可以直接对数组的元素进行剪辑，通过指定索引的方式：

```java
Stream<String> anotherStream = Arrays.stream(anArray, 1, 3);
```

结果包含"一枚有趣的程序员"和"好好珍重他"，1 这个索引位置包括，3 这个索引位置不包括。

### 07、数组排序

Arrays 类提供了一个 `sort()` 方法，可以对数组进行排序。

- 基本数据类型按照升序排列
- 实现了 Comparable 接口的对象按照 `compareTo()` 的排序

来看第一个例子：

```java
int[] anArray = new int[] {5, 2, 1, 4, 8};
Arrays.sort(anArray);
```

排序后的结果如下所示：

```java
[1, 2, 4, 5, 8]
```

来看第二个例子：

```java
String[] yetAnotherArray = new String[] {"A", "E", "Z", "B", "C"};
Arrays.sort(yetAnotherArray, 1, 3,
                Comparator.comparing(String::toString).reversed());
```

只对 1-3 位置上的元素进行反序，所以结果如下所示：

```
[A, Z, E, B, C]
```

### 08、数组搜索

有时候，我们需要从数组中查找某个具体的元素，最直接的方式就是通过遍历的方式：

```java
int[] anArray = new int[] {5, 2, 1, 4, 8};
for (int i = 0; i < anArray.length; i++) {
    if (anArray[i] == 4) {
        System.out.println("找到了 " + i);
        break;
    }
}
```

上例中从数组中查询元素 4，找到后通过 break 关键字退出循环。

如果数组提前进行了排序，就可以使用二分查找法，这样效率就会更高一些。`Arrays.binarySearch()` 方法可供我们使用，它需要传递一个数组，和要查找的元素。

```java
int[] anArray = new int[] {1, 2, 3, 4, 5};
int index = Arrays.binarySearch(anArray, 4);
```