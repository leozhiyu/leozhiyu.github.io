---
title: String 的两种创建方式的区别以及 intern() 方法
copyright: true
date: 2017-11-25 10:44:44
tags:
- Java
categories:
- Java
---

`Java` 两种创建字符串的方式，在内存中的存取机制不同。
```java
String a = "abc";
String b = new String("abc");
```
<!-- more -->

**前提**

双等号==的含义：
- 基本数据类型之间应用双等号，比较的是他们的数值。
- 引用类型之间应用双等号，比较的是他们在内存中的存放地址。

重写 equals() 方法是比较数值，String 已经重写了该方法，所以直接使用。

**双引号型**

Java 为 String 类型提供了常量池机制，当使用双引号定义对象时，Java 环境首先去字符串常量池池寻找相同内容的字符串，如果存在就直接拿出来应用，如果不存在则创建一个新的字符串放在常量池中。
```java
String a = "abc";
String b = "abc";
System.out.println(a == b); // true
System.out.println(a.equals(b)); // true
```
变量 a 和 b 使用的是缓冲区中的同一个存储对象，该对象存放在方法区的常量池中。

**new型**

通过 `new`关键字创建 String 对象，每次调用都会创建一个新的对象。
```java
String c = new String("abc");
String d = new String("abc");
System.out.println(c == d); //false
System.out.println(c.equals(d)); //true
```
new 出来的对象，每次调用时，都会产生一个新的对象，因此对象的引用是不同的，只是值相等而已。

**字符串连接的情况**
```java
String a = "ab";
String b = "abc";
String c = "ab" + "c";
String d = a + "c";
System.out.println(b == c); // true
System.out.println(b == d); // false
System.out.println(b.equals(d)); // true
```
b 与 c 创建的字符串是完全等价的
d 等号右边有一个非常量，得到的结果相当于是 new 创建的一个对象

**intern() 方法**

intern()方法使用的特性是运行时常量池，具备动态性，常量不一定只有编译时产生，运行时也可能将新的常量放入池中

intern 方法会返回一个字符串对应的常量值，在执行 intern 方法时，JVM 会检查常量池中是否存在和该字符串相同的常量值，如果有，则返回该常量值，若没有，则创建该常量值，并返回。即，intern 返回的是值常量池中的 String，不是堆上的String，相当于用双引号创建 String

```java
String a = "ab";
String b = "abc";
String d = (a + "c").intern();
System.out.println(b == d); // true
System.out.println(b.equals(d)); // true
```
```java
String a = new String("abc");
String b = new String("abc");
System.out.println(a == b); // false
System.out.println(a.intern() == b.intern()); // true
```

**常量池的好处**

常量池是为了避免频繁的创建和销毁对象而影响系统性能，实现了对象的共享。
例如字符串常量池， 在编译阶段就把所有的字符串文字放到一个常量池中。
- 节省内存空间：常量池中所有相同的字符串常量被合并，只占用一个空间。
- 节省运行空间：比较字符串时，== 比 equals 快。对于两个引用变量，只用 == 判断引用是否相等，也就可以判断实际值是否相等。





