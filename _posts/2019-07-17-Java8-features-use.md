---
title: Java8新特性使用
date: 2019-7-17
categories:
- tech
tags:
- java
---

Java8新增了非常多的新特性，其中部分特性在开发过程中很有用。Java 8 (又称为 jdk 1.8) 是 Java 语言开发的一个主要版本。 Oracle 公司于 2014 年 3 月 18 日发布 Java 8 ，它支持函数式编程，新的 JavaScript 引擎，新的日期 API，新的Stream API 等。



<!-- more -->

### 什么是Lambda表达式？

lambda 是为了解决在行为参数化模式下产生的大量重复的代码。可以把lambda表达式理解为简洁地表示可传递的匿名函数的一种方式：它没有名称，但它有参数列表、函数主体、返回类型，可能还有一个可以抛出的异常列表
> 行为参数化，就是一个方法接受多个不同的行为作为参数，并在内部使用它们，完成不同行为的能力。
> 行为参数化可让代码更好地适应不断变化的要求，减轻未来的工作量。


Lambda 的基本语法是 
`(parameters) -> expression`
或者
`(parameters) -> { statements; }`

Lambda使用实例：

布尔表达式 (List<String> list) -> list.isEmpty() 
创建对象 () -> new Apple(10) 
消费一个对象 (Apple a) -> {      System.out.println(a.getWeight()); } 
从一个对象中选择/抽取 (String s) -> s.length() 
组合两个值 (int a, int b) -> a * b 
比较两个对象 (Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight())


**什么情况下可使用lambda表达式？**

你可以在函数式接口上使用Lambda表达式。

一言以蔽之，函数式接口就是只定义一个抽象方法的接口。你已经知道了Java API中的一些 其他函数式接口，比如Comparator和Runnable。 


java.util.Comparator
public interface Comparator<T> {     int compare(T o1, T o2);  } 
 
java.lang.Runnable
public interface Runnable{      void run(); } 



> 接口现在还可以拥有默认方法（即在类没有对方法进行实现时， 其主体为方法提供默认实现的方法）。哪怕有很多默认方法，只要接口只定义了一个抽象 方法，它就仍然是一个函数式接口。
 


### 使用局部变量 



Lambda表达式可以引用类成员和局部变量（会将这些变量隐式得转换成final的），例如下列两个代码块的效果完全相同：


String separator = ",";
Arrays.asList( "a", "b", "d" ).forEach( 
    ( String e ) -> System.out.print( e + separator ) );

和

final String separator = ",";
Arrays.asList( "a", "b", "d" ).forEach( 
    ( String e ) -> System.out.print( e + separator ) );
	
	
	
但是被隐式转换为final后，不能再更改值，如下面的代码块编译不通过


String separator = ",";
Arrays.asList( "a", "b", "d" ).forEach( 
    ( String e ) -> System.out.print( e + separator ) );
	
separator = "."



### 方法引用

Lambda 等效的方法引用 
(Apple a) -> a.getWeight() Apple::getWeight 
() -> Thread.currentThread().dumpStack() Thread.currentThread()::dumpStack 
(str, i) -> str.substring(i) String::substring 
(String s) -> System.out.println(s) System.out::println

### 构造函数引用

对于一个现有构造函数，你可以利用它的名称和关键字new来创建它的一个引用： ClassName::new。它的功能与指向静态方法的引用类似。例如，假设有一个构造函数没有参数。 它适合Supplier的签名() -> Apple。你可以这样做： 


Supplier<Apple> c1 = Apple::new; Apple a1 = c1.get(); 
 
这就等价于： 
 
Supplier<Apple> c1 = () -> new Apple(); Apple a1 = c1.get(); 
 
如果你的构造函数的签名是Apple(Integer weight)，那么它就适合Function接口的签 名，于是你可以这样写： 
 
Function<Integer, Apple> c2 = Apple::new;  Apple a2 = c2.apply(110);  
 
这就等价于： 
 
Function<Integer, Apple> c2 = (weight) -> new Apple(weight);  Apple a2 = c2.apply(110); 


### 复合Lambda表达式


**比较器复合**


**谓词复合**


**函数复合**





