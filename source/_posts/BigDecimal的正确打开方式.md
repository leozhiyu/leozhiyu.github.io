---
title: BigDecimal的正确打开方式
copyright: true
date: 2017-11-22 16:41:27
tags:
- Java
categories:
- Java
---

在学习 Java 的过程中是否对一些小数的计算结果感到困惑？

0.01 + 0.05 的计算结果竟然是 0.060000000000000005 ？

这就是 Java 中的精度丢失问题，具体是为什么呢？这就涉及到计算机存储的一些知识，这就不展开讨论，详情请使用 Google。

那么这样的计算结果会带来什么问题呢？
<!-- more -->
假设你在天猫买一个 0.01 元和 0.05 元的东西，而此时你的余额刚好只有 0.06 元，如果天猫的后台系统用普通的方式进行小数运算，它要求你付款 0.060000000000000005 元，那岂不是买不起了。

所以在商业计算方面，涉及到小数精度的运算的地方，这个问题都是需要解决的。

我们先来看看几个奇怪的运算结果：

    System.out.println(0.011 + 0.05);//0.061
    System.out.println(0.01 + 0.05);//0.060000000000000005
    System.out.println(1.0 - 0.42);//0.5800000000000001
    System.out.println(4.015 * 100);//401.49999999999994
    System.out.println(123.3/100);//1.2329999999999999

对于这些情况我们都是无法理解的，那么我们用 `BigDecimal` 来解决这个问题

    BigDecimal b1 = new BigDecimal(0.01);
    BigDecimal b2 = new BigDecimal(0.02);
    System.out.println(b1.add(b2));
    //0.03000000000000000062450045135165055398829281330108642578125

结果竟然是更奇怪更长的一串数字，说好的 `BigDecimal` 大法呢？

实际上，`BigDecimal` 重载了很多个构造函数，而能解决问题的并不是这个构造函数，而是 `BigDecimal(String)`，我们需要使用它才能解决问题。

    BigDecimal b1 = new BigDecimal("0.01");
    BigDecimal b2 = new BigDecimal("0.02");
    System.out.println(b1.add(b2));//0.03

所以我们要解决精度问题，不仅要使用 `BigDecimal`，而且还一定要使用它的 `String` 构造方法。

那么在实际应用中都会涉及到从数据库中取值，而数据库中都是 `float` 或者 `double` 类型，此时我们最方便的做法便是手动写一个工具类 `BigDecimalUtil`，用来进行类型的转化，转成我们需要的 `String` 类型

    public class BigDecimalUtil {
    //创建私有构造器，防止外界实例化该类
    private BigDecimalUtil(){
    }

    //加法
    public static BigDecimal add(double v1,double v2){
        BigDecimal b1 = new BigDecimal(Double.toString(v1));
        BigDecimal b2 = new BigDecimal(Double.toString(v2));
        return b1.add(b2);
    }

    //减法
    public static BigDecimal sub(double v1,double v2){
        BigDecimal b1 = new BigDecimal(Double.toString(v1));
        BigDecimal b2 = new BigDecimal(Double.toString(v2));
        return b1.subtract(b2);
    }

    //乘法
    public static BigDecimal mul(double v1,double v2){
        BigDecimal b1 = new BigDecimal(Double.toString(v1));
        BigDecimal b2 = new BigDecimal(Double.toString(v2));
        return b1.multiply(b2);
    }

    //除法
    public static BigDecimal div(double v1,double v2){
        BigDecimal b1 = new BigDecimal(Double.toString(v1));
        BigDecimal b2 = new BigDecimal(Double.toString(v2));
        return b1.divide(b2);
    }
    }

看似一个小问题，但是不注意的话仍然会引发不可预算的灾难。所以很多细节都需要我们去把控，掌握一些基本的解决方法。

