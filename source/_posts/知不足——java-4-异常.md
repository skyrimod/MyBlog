---
title: 知不足——java(4)异常
date: 2020-06-23 10:13:29
tags: java
---

介绍异常

<!-- more -->

# 异常

异常用于处理代码中的出现的问题。 **所有的异常类都派生于Throwable类的子类Exception类**。在标准库中提供了RuntimeException异常和IOException异常。异常类分为检查型异常和非检查型异常，这里的检查指的是编译器检查。

**检查型异常：**也叫做编译时异常，包含除RuntimExcetion异常及其子类的所有异常。编译器会检查这类异常，当程序中出现这类异常时，需要使用 throws 抛出，或者使用 try-catch 捕获并处理，如果不在代码中处理这类异常，将不能通过编译。

**非检查型异常：**RuntimeException异常，也叫做运行时异常，编译器不会检查这类异常，所以程序可以通过编译，但在运行时会报错，例如：ArrayIndexOutBoundException数组下标越界异常、NullPointerException空指针异常等。<font color = red>这些异常应该在程序中避免出现！</font>

## 相关语法

### try-catch

使用 try 捕获异常，使用 catch 处理异常。

```java
class ExceptionTest{
	try{						// try语句块内的程序会抛出检查型异常
		. . . 					// 打开资源
	}catch(ExceptionType e1){	// 捕获异常后，运行catch语句块内的程序
		. . .
	}catch(ExceptionType e2){	// 可以使用多个catch处理多个不同的异常
        . . .
    }finally{					// 当执行完try后执行finally语句块
        . . .					// 关闭资源
    }
}
```

finally语句块还是很有必要的，当try语句块发生异常时，会跳过try语句块中发生异常后面的程序，如果被跳过的程序中有打开的资源，就需要在finally语句块中将资源关闭，例如打开的文件等。

### try-with-Resources

在try-catch中使用finally来关闭try中打开的资源，现在可以用另外一种语法来代替try中打开资源和finally中关闭资源的操作。

```java
try (Resources res = ...){		// 打开资源
    . . .
}catch(){}
```

在try块结束的时候，会自动调用 res.close()，不需要再手动关闭。

### throws

当发生异常时，如果你希望交给调用这段程序的人去处理这个异常，那么就在方法名的后面抛出这个异常。

```java
void ThrowsExceptionTest() throws ExceptionType {
}
```

调用ThrowsExceptionTest()方法的人就要使用try-catch来捕获并处理这个异常。

```java
try{
    ...
	ThrowsExceptionTest();
    ...
}catch(ExceptionType e){
    ...
}
```

### throw

throws用在方法名后，而throw则用在语句中。使用throw时，通常涉及异常的封装再抛出，用在catch语句块中。

```java
try {
    . . .
}catch(ExceptionType exception){
    var e = new MyException("exception");	// 新建一个异常实例，即封装后的异常
    e.initCause(exception);					// 为对象设置原因，原因为捕获的异常
    throw e;								// 抛出封装后的异常
}
```

通过异常的封装再抛出，可以在子系统中抛出高层异常，而不会丢失原始异常的细节。

### 创建异常类

所有的异常都是派生于Exception类，因此，创建异常类只需要创建一个继承Exception类的子类就可以了。

```java
class MyException extends Exception{
    MyException(){					// 无参构造器
        
    }
    MyException(String grip){		// 包含异常信息的有参构造器
        super(grip);
    }
}
```

然后，就可以在我们的程序中抛出新创建的异常类。

```java
void throwsTest() throws MyException{
    . . .
}
```

