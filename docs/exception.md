## 异常


代码发生错误时都会抛出异常。

示例:

`ArrayIndexOutOfBounds` 当索引在数组中不存在时抛出数组越界异常 (e.g: 尝试获得 arr[5],
但是 arr 数组的大小为 4).

ArithmeticError 算术异常会在不合法的数字操作时抛出 (e.g: 42/0, 不能除0)

Java可以抛出很多异常 (比上面的更多).

但是，你当你不确定是否会发生异常时该怎么处理呢？

这就是 `try/catch` 语句的目的！如下是`try/catch`的语法：

```java            
      try {
        //Code here
      } catch (ExceptionHere name) {
            //可以根据你的经验处理该异常 .
            //如果发生 "ExceptionHere" 异常，就会在这处理.
      }
 ```   
    
在try块后面的代码将尝试运行，如果运行的过程中出现错误，将会在 catch 语句中被捕获。

提示：您可以主动抛出异常 或者直接进行异常捕捉。


当一个错误发生时，程序就会抛出异常. 

异常由 `try/catch` 语句处理. 可能抛出异常的代码都必须包含在try语句块中. 如果由于某种原因，它不适合或不能使用 `try/catch` 语句，则必须在方法声明的时候使用 `throws` 关键字抛出

```java
	public void writeFile() throws IOException 
```
也可以在代码中抛出一个新的异常：:

```java
	throw new IllegalArgumentException("Number not above 0");
	/* 将打印 
		Exception in thread "Main": java.lang.IllegalArgumentException: Number not above 0
	*/
```
