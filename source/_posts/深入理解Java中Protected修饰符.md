---
title: 深入理解 Java 中 protected 修饰符
copyright: true
date: 2017-11-30 14:00:27
tags:
- Java
categories:
- Java
---

看似简单的东西可以引出很多问题， 学习过程中很多概念我们都只是「好像了解」、「貌似是这样」、「应该没问题」， 其实缺乏的是仔细思考， 对自己少问了几个「为什么」。

在 Java 中， 访问权限修饰符属于最最基础的知识， protected 修饰符只是其中一个， 如果你要问为什么不拿 public、default、private 来深究呢？ 那么看完这篇文章你会知道为何 protected 更值得深入思考。

<!-- more -->

在 《Thinking in Java》 中，protected 的名称是「继承访问权限」，这也就是我们记忆中的 protected：protected 必须要有继承关系才能够访问。 所以你以为你懂了， 可是你真的理解了这句话吗？

先思考几个问题：
1. 同一个包中， 子类对象能访问父类的 protected 方法吗？

2. 不同包下， 在子类中创建该子类对象能访问父类的 protected 方法吗？

3. 不同包下， 在子类中创建父类对象能访问父类的 protected 方法吗？

4. 不同包下， 在子类中创建另一个子类的对象能访问公共父类的 protected 方法吗？

5. 父类 protected 方法加上 static 修饰符又会如何呢？

《Thinking in Java》中有一句话：「protected 也提供包访问权限， 也就是说，相同包内的其他类可以访问 protected元素」， 其实就是 protected 修饰符包含了 default 默认修饰符的权限， 所以第 1 个问题你已经知道答案了， 在同一个包中， 普通类或者子类都可以访问基类的 protected 方法。

## 父类为非静态 protected 修饰类 ##

```java
package com.protectedaccess.parentpackage;

public class Parent {

    protected String protect = "protect field";
    
    protected void getMessage(){
        System.out.println("i am parent");
    }
}
```

**不同包下，在子类中通过父类引用不可以访问其 protected 方法**


无论是创建 Parent 对象还是通过多态创建 Son1 对象， 只要 Parent 引用， 则不可访问， 编译器会提示错误。

```java
package com.protectedaccess.parentpackage.sonpackage1;

import com.protectedaccess.parentpackage.Parent;

public class Son1 extends Parent{
    public static void main(String[] args) {
        Parent parent1 = new Parent();
        // parent1.getMessage();   错误

        Parent parent2 = new Son1();
        // parent2.getMessage();  错误
    }
}
```

**不同包下，在子类中通过该子类引用可以访问其 protected 方法**

子类中实际上把父类的方法继承下来了， 可以通过该子类对象访问， 也可以在子类方法中直接访问，  还可以通过 super 关键字调用父类中的该方法。

```java
package com.protectedaccess.parentpackage.sonpackage1;

import com.protectedaccess.parentpackage.Parent;

public class Son1 extends Parent{
    public static void main(String[] args) {
        Son1 son1 = new Son1();
        son1.getMessage(); // 输出：i am parent,
    }
    private void message(){
        getMessage();  // 如果子类重写了该方法， 则输出重写方法中的内容
        super.getMessage(); // 输出父类该方法中的内容
    }
}
```

**不同包下，在子类中不能通过另一个子类引用访问共同基类的 protected 方法**

```java
package com.protectedaccess.parentpackage.sonpackage2;

import com.protectedaccess.parentpackage.Parent;

public class Son2 extends Parent {

}
```

注意是 Son2 是另一个子类， 在 Son1 中创建 Son2 的对象是无法访问父类的 protected 方法的

```java
package com.protectedaccess.parentpackage.sonpackage1;

import com.protectedaccess.parentpackage.Parent;
import com.protectedaccess.parentpackage.sonpackage2.Son2;

public class Son1 extends Parent{
    public static void main(String[] args) {
        Son2 son2 = new Son2();
        // son2.getMessage(); 错误
    }
}
```

## 父类为静态 protected 修饰类 ##

对于protected的静态变量， 在子类中可以直接访问， 在不同包的非子类中则不可访问

```java
package com.protectedaccess.parentpackage;

public class Parent {

    protected String protect = "protect field";
    
    protected static void getMessage(){
        System.out.println("i am parent");
    }
}
```

静态方法直接通过类名访问

**无论是否同一个包，在子类中均可直接访问**

```java
package com.protectedaccess.parentpackage.sonpackage1;

import com.protectedaccess.parentpackage.Parent;

public class Son3 extends Parent{
    public static void main(String[] args) {
        Parent.getMessage(); // 输出： i am parent
    }
}
```

**在不同包下，非子类不可访问**

```
package com.protectedaccess.parentpackage.sonpackage1;

import com.protectedaccess.parentpackage.Parent;

public class Son4{
    public static void main(String[] args) {
       // Parent.getMessage(); 错误
    }
}
```

看到这里你应该知道有多少种情况了， 针对不同的情况都可能出现意外的结果， 所以还是得多实践， 仅仅在书上看一遍 protected 修饰符的作用是无法真正发现它的微妙。

欢迎关注我的公众号：IT从业者说
![IT从业者说](http://upload-images.jianshu.io/upload_images/6753966-2a33084ce1643224.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)











