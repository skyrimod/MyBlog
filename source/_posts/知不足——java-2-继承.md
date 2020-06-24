---
title: 知不足——java(2)继承
date: 2020-06-19 18:15:51
tags: java
---

<center>路漫漫其修远兮，吾将上下而求索。</center>

<!-- more -->

# 继承

&emsp;&emsp;继承是个很棒的功能，它使代码可以复用，极大程度上降低了工作量。

&emsp;&emsp;java中的继承只能**单继承**，不能多继承。但可以多层次继承。

&emsp;&emsp;从某种程度上说，继承是对抽象的进一步使用。我们可以抽象出猫和鸟的共同点，例如：它们都会叫、都会吃东西等，这样就可以得到一个实现它们共同点的类animal，然后，我们可以创建cat类和bird类，并让这两个类继承animal类。因为我们在animal类中实现了叫和吃的方法，所以不用在cat类和bird类中再次实现这两个方法。

```java
//animal类
class Animal{
	public void called(){}	//叫方法
	public void eat(){}		//吃方法
}
```

```java
//cat类
class Cat extends Animal{	//继承animal
}
Cat cat = new Cat();	//实例化猫对象
cat.called();			//调用叫方法
cat.eat();				//调用吃方法
```

```java
//bird类
class Bird extends Animal{	//继承animal
}
Bird bird = new Bird();	//实例化鸟对象
bird.called();			//调用叫方法
bird.eat();				//调用吃方法
```

## 字段

&emsp;&emsp;子类会继承父类的所有字段。这样子类就有了父类的属性。

## 构造器

&emsp;&emsp;**构造器并不会被继承**，但是在子类的构造器中总是显示或隐式调用父类构造器。

&emsp;&emsp;在实例化子类对象时，由于子类继承父类的字段，所以需要对父类字段进行初始化操作，这时，如果子类构造器没有显式调用父类构造器，就会自动调用父类的无参构造器。

```java
class Manager extends Employee{		// Manager 继承 Employee
	Manager(name,id,bonus){
		super(name,id);				// 使用super调用父类构造器，此处为显式调用
		this.bonus = bonus;			// 给manager的字段赋值
	}
}
```

## Getter、Setter

&emsp;&emsp;子类同样会继承父类的Getter和Setter方法。但不是用来调用自身的字段，而是用来调用父类的字段。如果尝试用来调用自身字段则是错误的，因为子类无法访问父类的private字段。

```java
class Manger extends Employee{	// Employee类有 name字段，getName()方法
	getName();	// Manager可以调用getName()，但是会出错，因为Manager类没有name字段
    super.getName();	// 使用 super 调用父类的getName()
}
```

&emsp;&emsp;<font color = red>虽然Manager类继承了Employee类的字段，但是不能直接访问。</font>

## 方法

&emsp;&emsp;继承的用处很大程度体现在方法上，可以直接调用、重写等

**重写**：子类对父类方法重写，以实现自身的行为。<font color = red>方法名不变，不改参数和返回值，只改变核心。</font>

**重载**：<font color = red>方法名不变，参数和返回值可变，且必须改变参数。</font>

***PS:把方法当做黑盒来看，重写是希望通过同样的黑盒（方法名）使用同样的输入（参数）得到不同的输出（返回值）；重载是希望通过同样的黑盒（方法名）使用不同的输入（参数）得到相应的输出（返回值）。***

## 多态

&emsp;&emsp;什么是多态？书上说，在java中，对象变量是多态的，即Employee类型的变量可以引用Employee类的对象，也可以引用Employee类任何子类的对象。也就是说，<font color = red>同一个事物，有不同的形态。</font>那么，重写也是多态的（重载不是多态，因为改变了参数列表）。

```java
Employee e = new Manager();		// 父类引用引用子类对象，向上转型
e.getBonus();	// 错误！e是Employee类，而getBonus()是Manager的方法
				// 这里编译器并没有把e作为Manager类，如果想要引用子类方法需要强制类型转换
Manager m = (Manager) e;	//向下转型
```

*PS：父类引用引用子类对象是向上转型；将父类引用强转为子类引用是向下转型（这里的父类引用必须是引用子类对象的父类引用，即向上转型后的父类引用）。切记，不能将父类引用赋值给子类变量*。

## final

&emsp;&emsp;使用final修饰的类或方法不能被继承。final类的字段不是final，方法自动成为final方法。因为fianl修饰字段时，字段为常量。

# Object类

&emsp;&emsp;Object类是所有类的父类，它实现了 equals() , hashCode() , toString() , clone() , getClass() 方法，其中 wait() , notify() , notifyAll() 方法用于多线程。

## equals()

&emsp;&emsp;equals()方法用来检测两个对象是否相等。

```java
public boolean equals(Object obj){
    return (this == obj);
}
```

&emsp;&emsp;这里比较的是对象引用，类似C语言中，直接比较两个指针是否相同。可以得知，如果它们引用的同一块内存，那这两个对象一定相同。在实际开发中，还需要检测对象的属性来判断是否相等。下面给出一个较完善的equals() 方法改写：比较两个员工的姓名、薪水和雇佣日期。

```java
public Employee{
    @Override
    puublic boolean equals(Object otherObject){
        if (this == otherObject)	// 1.比较this和otherObject是否相等
            return true;
        if (otherObject == null)	// 2.检测otherObject是否为空
            return false;
        if (getClass() != otherObject.getClass)	// 3.比较this和otherObject的类是否一样
            return false;
        Employee other = (Employee) otherObject;	// 4.强制类型转换otherObject
        return name.equals(other.name)		// 5.比较具体字段
            && salary == other.salary
            && hireDay.equals(other.hireDay);
    }
}
```

&emsp;&emsp;这5个步骤建议一步也不要少，其中2和3可以合并为一步。

```java
if ((otherObject == null) || (getClass() != otherObject.getClass())) 
    return false;
```

## hashCode()

&emsp;&emsp;hashCode()方法用于生成散列码，不同对象的散列码不同，<font color = red>如果 x.equals(y) 返回 true，那么 x.hashCode() 和 y.hashCode() 的返回值必须相同。</font>

```java
@HotSpotIntrinsicCandidate
public native int hashCode();
```

&emsp;&emsp;注解@HotSpotIntrinsicCandidate意味着该方法有一套高效的实现（目前还没学到注解和 HotSpot 这两个知识点）。

&emsp;&emsp;可以重写 hashCode() , 自定义根据哪些数据来生成散列码。

```java
public int hashCode(){
    return Objects.hash(name, salary, hireDay);	// 使用name, salary, hireDay生成散列码
}
```

## toString()

&emsp;&emsp;toString()方法是用来打印对象的信息。

```java
public String toString() {
    return getClass().getName() + "@" + Integer.toHexString(hashCOde());
}
```

&emsp;&emsp;如果没有对 toString() 方法重写，则会打印出这个类的类名和十六进制的散列码。下面给出一个重写例程。

``` java
@Override
public String toString(){
    return getClass().getName()
        + "[name=" + name
        + ", salary=" + salary
        + ", hireDay=" + hireDay
        + "]";
}
```

&emsp;&emsp;这段程序会打印出 Employee[name=. . ., salary=. . ., hireDay=. . .] 。

------

*PS：继承虽然实现了代码复用问题，但却很大程度上限制了子类的个性化，子类想要实现自身的功能就需要重写父类的方法。后面关于接口的内容就很好的解决了这个问题。（但我个人总觉得接口和继承差不多，就是实现了一些特殊功能）*