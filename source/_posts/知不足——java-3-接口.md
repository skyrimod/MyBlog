---
title: 知不足——java(3))接口
date: 2020-06-22 11:37:45
tags: java
---

包含内容：抽象类、接口、Lambda表达式

ps：内容待完善...


<!-- more -->

&emsp;&emsp;在介绍接口之前，先来看下抽象类。接口和抽象类有很多相似点，它更像是在抽象类的基础上，补充了更多的功能。

# 抽象类

在继承部分提到过，把类的共同点抽象出来作为父类。如果我们只需要这个父类作为派生其他类的基类，而不去实例化一个类，那么就可以把这个类使用<font color = red>修饰词 abstract </font>修饰，作为抽象类来使用。定义抽象类的语法如下所示。

```java
[修饰符] abstract class 类名{
    . . .
}
```

## 字段

抽象类可以包含字段。而且使用方法和普通类的使用方法完全一样。

## 构造器

抽象类的一大特点就是不能被实例化！因此，抽象类没有构造器。但是，我们可以使用抽象类引用去指向一个继承并且实现抽象方法的子类。这点和继承中的父类引用指向子类对象是一样的。

```java
Person student = new Student();		// Person是抽象类，Student是继承了Person的子类
```

## 方法

在抽象类中，方法可以抽象，也可以不抽象。

**抽象方法**：使用 abstract 修饰的方法是抽象方法。

```java
[修饰符] abstract 返回值 方法名();
```

注意：这里的方法<font color = red>没有方法体！</font>这也是抽象类和普通类的最大区别。

抽象方法只是告诉子类需要做什么，而不是该怎么做。如果子类没有重写抽象类的抽象方法，那么子类也将自动变成抽象类，所以需要实例化时，必须重写所有的抽象方法。

**非抽象方法**：非抽象方法使用方法和普通类中的方法一样。

如果一个类含有一个或多个抽象方法，那这个类一定是抽象类；而抽象类中可以不含抽象方法。

# 接口

java不支持多继承，所以引入了接口的概念。下面先给出接口的基本语法。

使用 interface 建立接口。

```java
[修饰符] interface 接口名{}
```

使用 implements 使用接口。一个类可以实现多个接口。

```java
class 类名 implements 接口1，接口2，...{}
```

**什么是接口？**

&emsp;&emsp;如书上所说：<font color = red>接口是用来描述类应该做什么，而不指定它们具体应该如何做。</font>这是接口和抽象类最相似的地方。接口像是<font color = red>一种行为规范，是一个协议</font>，比如：U盘实现了USB接口，只要是实现USB接口的设备，我们都可以把U盘插上去传输数据；再比如：手机数据线实现了Type-C接口，只要手机支持Type-C接口，那么无论是哪家厂商的手机数据线，只要它实现了Type-C接口，那么就可以和手机连接。

接口不是类，但与抽象类十分相似。下面将从多方面给出接口和抽象类的区别。

## 字段

抽象类中的字段可以是普通字段，而<font color = red>接口中的字段只能是常量。</font>且是全局常量。

```java
public static final 常量名 = 值;
```

在声明时可以省略 static final 修饰符，编译器会自动为字段加上这两个修饰符，切记，声明常量的时候必须给常量赋值。

## 构造器

接口和抽象类一样，不能实例化对象，所以没有构造器，但可以使用接口类型引用指向实现该接口的类。

```java
Person student = new Student(); 	// Person是接口，Student是实现了Person接口的类
```

## 方法

接口中的方法在编译时都会默认加上 abstract ，这样<font color = red>接口中的所有方法都是抽象方法。</font>所以实现接口的类中，必须实现该接口的所有方法。

**默认方法：**因为是抽象方法，所以不含方法体。但是可以使用 default 修饰符使方法成为默认方法，这样就可以添加函数体。

```java
default 返回值 方法名(){
    ...
}
```

**用处：**如果我们给一个接口中新增了一个方法，那么之前已经实现这个接口的类中由于没有实现这个新增的方法所以出现错误，这时，我们可以把这个新增的方法设置为 default 方法，并提供函数体，这样就不会出现错误。

下面给出一个标准库中的一个接口Comparable实例。

```java
public interface Comparable<T> {	// Comparable接口
    public int compareTo(T o);
} 
```

```java
class Employee implements Comparable{
	public int compareTo(T o){		// 实现 compareTo 方法
		return T.compare(参数1， 参数2);
	}
}
```

# Lambda表达式

Lambda表达式是一个可以传递的代码块。

```java
参数 -> 表达式	// lambda表达式格式
```

如何使用Lambda表达式？例：

```java
sort(T[] a, Comparator<? super T> c){};	//参数1是T数组,参数2是实现Comparator接口的实例
int compare(T, T){}; //compare()是接口Comparator的一个方法。
```

```java
Arrays.sort(planeets, (first, second) -> {	// 使用Lambda表达式
    first.length() - second.length();
});
```

```java
// 下面这个表达式就是对compare()方法的实现
(first, second) -> first.length() - second.length();
```

这里对Lambda表达式的使用就是下面将要介绍的函数式接口。

## 函数式接口

&emsp;&emsp;在一些接口中只有一个方法，每次使用这个方法时必须创建一个实现这个接口的类，然后重写并调用这个方法。为了简化代码，可以使用Lambda表达式来代替创建类等操作。笼统地说，就是需要传递一个接口变量（函数）时，直接使用Lambda表达式将方法体传递过去，省去中间步骤。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       

&emsp;&emsp;可见<font color = red>Lambda表达式本身就是对接口的实现</font>，