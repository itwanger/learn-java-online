
## ArrayList

![](../images/collections/arraylist/01.png)

ArrayList 实现了 List 接口，是基于数组实现的。小伙伴们都知道，数组的大小是固定的，创建的时候指定了大小，就不能再调整了，如果数组满了，就不能再添加任何元素了。ArrayList 是数组很好的替代方案，它提供了比数组更丰富的预定义方法（增删改查），并且大小是可以根据元素的多少进行自动调整的，非常灵活。

准备在 ArrayList 的第四个位置（下标为 3）上添加一个元素 55。

![](../images/collections/arraylist/02.png)

此时 ArrayList 中第五个位置以后的元素将会向后移动。

![](../images/collections/arraylist/03.png)

准备把 23 从 ArrayList 中移除。

![](../images/collections/arraylist/04.png)

此时下标为 7、8、9 的元素往前挪。

![](../images/collections/arraylist/05.png)

### 01、如何创建一个 ArrayList 

```java
ArrayList<String> alist = new ArrayList<String>();
```

可以通过上面的语句来创建一个字符串类型的 ArrayList（通过尖括号来限定 ArrayList 中元素的类型，如果尝试添加其他类型的元素，将会产生编译错误），更简化的写法如下：

```java
List<String> alist = new ArrayList<>();
```

由于 ArrayList 实现了 List 接口，所以 alist 变量的类型可以是 List 类型；new 关键字声明后的尖括号中可以不再指定元素的类型，因为编译器可以通过前面尖括号中的类型进行智能推断。

如果非常确定 ArrayList 中元素的个数，在创建的时候还可以指定初始大小。

```java
List<String> alist = new ArrayList<>(20);
```

这样做的好处是，可以有效地避免在添加新的元素时进行不必要的扩容。但通常情况下，我们很难确定  ArrayList 中元素的个数，因此一般不指定初始大小。

### 02、向 ArrayList 中添加一个元素

可以通过 `add()` 方法向 ArrayList 中添加一个元素，如果不指定下标的话，就默认添加在末尾。

```java
alist.add("沉默王二");
```

感兴趣的小伙伴可以研究一下 `add()` 方法的源码，它在添加元素的时候会执行 `grow()` 方法进行扩容，这个是面试官特别喜欢考察的一个重点。

下面是 `add(E e)` 方法的源码：

```java
public boolean add(E e) {
    modCount++;
    add(e, elementData, size);
    return true;
}
```

调用了私有的 `add(E e, Object[] elementData, int s)` 方法：

```java
private void add(E e, Object[] elementData, int s) {
    if (s == elementData.length)
        elementData = grow();
    elementData[s] = e;
    size = s + 1;
}
```

然后调用了非常关键的 `grow(int minCapacity)` 方法：

```java
private Object[] grow(int minCapacity) {
    int oldCapacity = elementData.length;
    if (oldCapacity > 0 || elementData != DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
        int newCapacity = ArraysSupport.newLength(oldCapacity,
                minCapacity - oldCapacity, /* minimum growth */
                oldCapacity >> 1           /* preferred growth */);
        return elementData = Arrays.copyOf(elementData, newCapacity);
    } else {
        return elementData = new Object[Math.max(DEFAULT_CAPACITY, minCapacity)];
    }
}
```

如果创建 ArrayList 的时候没有指定初始大小，那么 ArrayList 的初始大小就是 DEFAULT_CAPACITY：

```java
private static final int DEFAULT_CAPACITY = 10;
```

可以容纳 10 个元素。

还可以通过 `add(int index, E element)` 方法把元素添加到指定的位置：

```java
alist.add(0, "沉默王三");
```

 `add(int index, E element)` 方法的源码如下：

```java
public void add(int index, E element) {
    rangeCheckForAdd(index);
    modCount++;
    final int s;
    Object[] elementData;
    if ((s = size) == (elementData = this.elementData).length)
        elementData = grow();
    System.arraycopy(elementData, index,
            elementData, index + 1,
            s - index);
    elementData[index] = element;
    size = s + 1;
}
```

该方法会调用到一个非常重要的本地方法 `System.arraycopy()`，它会对数组进行复制（要插入位置上的元素往后复制，参照文章一开头提到的两张图片）。

### 03、更新 ArrayList 中的元素

可以使用 `set()` 方法来更改 ArrayList 中的元素，需要提供下标和新元素。

```java
alist.set(0, "沉默王四");
```

原来 0 位置上的元素为“沉默王三”，现在将其更新为“沉默王四”。

来看一下 `set()` 方法的源码：

```java
public E set(int index, E element) {
    Objects.checkIndex(index, size);
    E oldValue = elementData(index);
    elementData[index] = element;
    return oldValue;
}
```

该方法会先对指定的下标进行检查，看是否越界，然后替换新值并返回旧值。

### 04、删除 ArrayList 中的元素

`remove(int index)` 方法用于删除指定下标位置上的元素，`remove(Object o)` 方法用于删除指定值的元素。

```java
alist.remove(1);
alist.remove("沉默王四");
```

先来看 `remove(int index)` 方法的源码：

```java
public E remove(int index) {
    Objects.checkIndex(index, size);
    final Object[] es = elementData;

    @SuppressWarnings("unchecked") E oldValue = (E) es[index];
    fastRemove(es, index);

    return oldValue;
}
```

该方法返回要删除的元素，真正的删除操作在 `fastRemove(es, index)` 方法中。

再来看 `remove(Object o)` 方法的源码：

```java
public boolean remove(Object o) {
    final Object[] es = elementData;
    final int size = this.size;
    int i = 0;
    found: {
        if (o == null) {
            for (; i < size; i++)
                if (es[i] == null)
                    break found;
        } else {
            for (; i < size; i++)
                if (o.equals(es[i]))
                    break found;
        }
        return false;
    }
    fastRemove(es, i);
    return true;
}
```

该方法通过 break label 的方式找到要删除元素（null 的时候使用 == 操作符判断，非 null 的时候使用 `equals()` 方法，意味着如果有相同元素时，删除第一个）的下标，然后调用 `fastRemove()` 方法。

既然都调用了 `fastRemove()` 方法，那就继续来跟踪一下源码：

```java
private void fastRemove(Object[] es, int i) {
    modCount++;
    final int newSize;
    if ((newSize = size - 1) > i)
        System.arraycopy(es, i + 1, es, i, newSize - i);
    es[size = newSize] = null;
}
```

当删除的是末尾的元素时，不需要复制数组，直接把末尾的元素赋值为 null 即可；否则的话，就需要调用 `System.arraycopy()` 对数组进行复制。参照文章一开头提到的第三张、第四张图片。

### 05、查找 ArrayList 中的元素

如果要正序查找一个元素，可以使用 `indexOf()` 方法；如果要倒序查找一个元素，可以使用 `lastIndexOf()` 方法。

```java
alist.indexOf("沉默王二");
alist.lastIndexOf("沉默王二");
```

来看一下 `indexOf()` 方法的源码：

```java
public int indexOf(Object o) {
    return indexOfRange(o, 0, size);
}

int indexOfRange(Object o, int start, int end) {
    Object[] es = elementData;
    if (o == null) {
        for (int i = start; i < end; i++) {
            if (es[i] == null) {
                return i;
            }
        }
    } else {
        for (int i = start; i < end; i++) {
            if (o.equals(es[i])) {
                return i;
            }
        }
    }
    return -1;
}
```

如果元素为 null 的时候使用“==”操作符，否则使用 `equals()` 方法——该方法不是 null 安全的。

`lastIndexOf()` 方法和 `indexOf()` 方法类似，不过遍历的时候从最后开始。

`contains()` 方法可以判断 ArrayList 中是否包含某个元素，其内部调用了 `indexOf()` 方法：

```java
public boolean contains(Object o) {
    return indexOf(o) >= 0;
}
```

如果 ArrayList 中的元素是经过排序的，就可以使用二分查找法，效率更快。

`Collections` 类的 `sort()` 方法可以对 ArrayList 进行排序，该方法会按照字母顺序对 String 类型的列表进行排序。如果是自定义类型的列表，还可以指定 Comparator 进行排序。

```java
List<String> copy = new ArrayList<>(alist);
copy.add("a");
copy.add("c");
copy.add("b");
copy.add("d");

Collections.sort(copy);
System.out.println(copy);
```

输出结果如下所示：

```
[a, b, c, d]
```

排序后就可以使用二分查找法了：

```java
int index = Collections.binarySearch(copy, "b");
```

### 06、最后

关于 ArrayList，就先介绍这么多吧，通过源码的角度，我想小伙伴们一定对 ArrayList 有了更深刻的印象。

简单总结一下 ArrayList 的时间复杂度，方便后面学习 LinkedList 时作为一个对比。

1）通过下标（也就是 `get(int index)`）访问一个元素的时间复杂度为 O(1)，因为是直达的，无论数据增大多少倍，耗时都不变。

2）添加一个元素（也就是 `add()`）的时间复杂度为 O(1)，因为直接添加到末尾。

3）删除一个元素的时间复杂度为 O(n)，因为要遍历列表，数据量增大几倍，耗时也增大几倍。

4）查找一个未排序的列表时间复杂度为 O(n)，因为要遍历列表；查找排序过的列表时间复杂度为 O(log n)，因为可以使用二分查找法，当数据增大 n 倍时，耗时增大 logn 倍（这里的 log 是以 2 为底的，每找一次排除一半的可能）。

