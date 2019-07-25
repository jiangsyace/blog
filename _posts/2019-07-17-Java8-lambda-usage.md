---
title: Java8-Lambda表达式
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

| 布尔表达式 | (List<String> list) -> list.isEmpty()  |
| -- | -- |
| 创建对象 | () -> new Apple(10)  |
| 消费一个对象 | (Apple a) -> {      System.out.println(a.getWeight()); }  |
| 从一个对象中选择/抽取 | (String s) -> s.length()  |
| 组合两个值 | (int a, int b) -> a * b  |
| 比较两个对象 | (Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight()) |


### **什么情况下可使用lambda表达式？**

你可以在函数式接口上使用Lambda表达式。  

#### 函数式接口

一言以蔽之，函数式接口就是只定义一个抽象方法的接口。比如Comparator和Runnable。 

```
java.util.Comparator
public interface Comparator<T> {     int compare(T o1, T o2);  } 
 
java.lang.Runnable
public interface Runnable{      void run(); } 
```
> 接口现在还可以拥有默认方法（即在类没有对方法进行实现时，其主体为方法提供默认实现的方法）。哪怕有很多默认方法，只要接口只定义了一个抽象方法，它就仍然是一个函数式接口。

使用示例
```
productList.sort((Product p1, Product p2) -> p1.getId().compareTo(p2.getId()));
new Thread( () -> System.out.println("Hello world") ).start();
```

#### 函数描述符

函数式接口的抽象方法的签名基本上就是Lambda表达式的签名。我们将这种抽象方法叫作 函数描述符。例如，Runnable接口可以看作一个什么也不接受什么也不返回（void）的函数的 签名，因为它只有一个叫作run的抽象方法，这个方法什么也不接受，什么也不返回（void）。① 我们在本章中使用了一个特殊表示法来描述Lambda和函数式接口的签名。() -> void代表 了参数列表为空，且返回void的函数。这正是Runnable接口所代表的。 举另一个例子，(Apple, Apple) -> int代表接受两个Apple作为参数且返回int的函数。我们会在3.4节和本章后面的 表3-2中提供关于函数描述符的更多信息。 


### 使用函数式接口

Java 8的库设计师帮你在java.util.function包中引入了几个新的函数式接口。我们接下 来会介绍Predicate、Consumer和Function

#### Predicate


#### Consumer


#### Function


### 类型检查、类型推断以及限制


#### 类型推断 

Java编译器会从上下文（目标类型）推断出用什么函数式接 口来配合Lambda表达式，这意味着它也可以推断出适合Lambda的签名，因为函数描述符可以通 过目标类型来得到。这样做的好处在于，编译器可以了解Lambda表达式的参数类型，这样就可 以在Lambda语法中省去标注参数类型。换句话说，Java编译器会像下面这样推断Lambda的参数 类型：① 
```
List<Apple> greenApples =     filter(inventory, a -> "green".equals(a.getColor())); 
```
参数a没有显示类型


#### 使用局部变量 

Lambda表达式可以引用类成员和局部变量（会将这些变量隐式得转换成final的），例如下列两个代码块的效果完全相同：

```
String separator = ",";
Arrays.asList( "a", "b", "d" ).forEach( 
    ( String e ) -> System.out.print( e + separator ) );
```
和
```
final String separator = ",";
Arrays.asList( "a", "b", "d" ).forEach( 
    ( String e ) -> System.out.print( e + separator ) );
```
	
被隐式转换为final后，不能再更改值，如下面的代码块编译不通过

```
String separator = ",";
Arrays.asList( "a", "b", "d" ).forEach( 
    ( String e ) -> System.out.print( e + separator ) );
separator = ".";
```

### 方法引用

| Lambda | 等效的方法引用 |
| -- | -- |
| (Apple a) -> a.getWeight() | Apple::getWeight  |
| () -> Thread.currentThread().dumpStack() | Thread.currentThread()::dumpStack  |
| (str, i) -> str.substring(i) | String::substring  |
| (String s) -> System.out.println(s) | System.out::println |

你可以把方法引用看作针对仅仅涉及单一方法的Lambda的语法糖，因为你表达同样的事情 时要写的代码更少了。 
 
如何构建方法引用 方法引用主要有三类。 
(1) 指向静态方法的方法引用（例如Integer的parseInt方法，写作Integer::parseInt）。
(2) 指向任意类型实例方法的方法引用（例如String 的 length 方法，写作 String::length）。 
(3) 指向现有对象的实例方法的方法引用（假设你有一个局部变量expensiveTransaction 用于存放Transaction类型的对象，它支持实例方法getValue，那么你就可以写expensive- Transaction::getValue）。 

#### 构造函数引用

对于一个现有构造函数，你可以利用它的名称和关键字new来创建它的一个引用： ClassName::new。它的功能与指向静态方法的引用类似。例如，假设有一个构造函数没有参数。 它适合Supplier的签名() -> Apple。你可以这样做： 

Supplier<Apple> c1 = Apple::new; Apple a1 = c1.get(); 
 
这就等价于： 
 
Supplier<Apple> c1 = () -> new Apple(); Apple a1 = c1.get(); 
 
如果你的构造函数的签名是Apple(Integer weight)，那么它就适合Function接口的签 名，于是你可以这样写： 
 
Function<Integer, Apple> c2 = Apple::new;  Apple a2 = c2.apply(110);  
 
这就等价于： 
 
Function<Integer, Apple> c2 = (weight) -> new Apple(weight);  Apple a2 = c2.apply(110); 

### 复合Lambda表达式

Java 8的好几个函数式接口都有为方便而设计的方法。具体而言，许多函数式接口，比如用 于传递Lambda表达式的Comparator、Function和Predicate都提供了允许你进行复合的方法。 


#### 比较器复合

我们前面看到，你可以使用静态方法Comparator.comparing，根据提取用于比较的键值 的Function来返回一个Comparator，如下所示： 
Comparator<Apple> c = Comparator.comparing(Apple::getWeight); 
1. 逆序 如果你想要对苹果按重量递减排序怎么办？用不着去建立另一个Comparator的实例。接口 有一个默认方法reversed可以使给定的比较器逆序。因此仍然用开始的那个比较器，只要修改 一下前一个例子就可以对苹果按重量递减排序： 
```
inventory.sort(comparing(Apple::getWeight).reversed()); 
```
2. 比较器链 上面说得都很好，但如果发现有两个苹果一样重怎么办？哪个苹果应该排在前面呢？你可能 需要再提供一个Comparator来进一步定义这个比较。比如，在按重量比较两个苹果之后，你可 能想要按原产国排序。thenComparing方法就是做这个用的。它接受一个函数作为参数（就像 comparing方法一样），如果两个对象用第一个Comparator比较之后是一样的，就提供第二个 Comparator。你又可以优雅地解决这个问题了： 
```
inventory.sort(comparing(Apple::getWeight).reversed().thenComparing(Apple::getCountry)); 
```

#### 谓词复合

谓词接口包括三个方法：negate、and和or，让你可以重用已有的Predicate来创建更复 杂的谓词。比如，你可以使用negate方法来返回一个Predicate的非，比如苹果不是红的： 
Predicate<Apple> notRedApple = redApple.negate(); 
你可能想要把两个Lambda用and方法组合起来，比如一个苹果既是红色又比较重： 
Predicate<Apple> redAndHeavyApple =     redApple.and(a -> a.getWeight() > 150);  你可以进一步组合谓词，表达要么是重（150克以上）的红苹果，要么是绿苹果： 
Predicate<Apple> redAndHeavyAppleOrGreen =     redApple.and(a -> a.getWeight() > 150)             .or(a -> "green".equals(a.getColor())); 这一点为什么很好呢？从简单Lambda表达式出发，你可以构建更复杂的表达式，但读起来 仍然和问题的陈述差不多！请注意，and和or方法是按照在表达式链中的位置，从左向右确定优 先级的。因此，a.or(b).and(c)可以看作(a || b) && c。 


#### 函数复合

后，你还可以把Function接口所代表的Lambda表达式复合起来。Function接口为此配 了andThen和compose两个默认方法，它们都会返回Function的一个实例。 andThen方法会返回一个函数，它先对输入应用一个给定函数，再对输出应用另一个函数。 比如，假设有一个函数f给数字加1 (x -> x + 1)，另一个函数g给数字乘2，你可以将它们组 合成一个函数h，先给数字加1，再给结果乘2： 
Function<Integer, Integer> f = x -> x + 1; Function<Integer, Integer> g = x -> x * 2; Function<Integer, Integer> h = f.andThen(g); int result = h.apply(1);  



### 日常使用

```
//list 取属性生成数组
String[] productIds = productList.stream().map(Product::getId).toArray(String[]::new);
//list 取属性生成新list
List<String> productIdList = productList.stream().map(Product::getId).collect(Collectors.toList());
//list转map
Map<String, List<Product>> productMap = productList.stream().collect(Collectors.groupingBy(Product::getId));
//取出list中重复数据
ArrayList<Product> distinctedList = productList.stream()
                .collect(
                        Collectors.collectingAndThen(
                                Collectors.toCollection(() -> new TreeSet<>(Comparator.comparing(Product::getId))),
                                ArrayList::new));
```

