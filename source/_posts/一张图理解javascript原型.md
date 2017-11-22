---
title: 用结果理解 javascript 原型
copyright: true
date: 2017-09-30 11:49:48
tags:
- javascript 原型
categories:
- javascript
---
##### 每个函数都有一个 prototype 属性， 对象都有 __proto__ 属性，所有实例的原型引用的是函数的 prototype 属性，还得注意函数也是对象。

<!--more-->

- 对于所有的对象，都有\_\_proto\_\_属性，这个属性对应该对象的原型
- 对于函数对象，除了\_\_proto\_\_属性之外，还有prototype属性，当一个函数被用作构造函数来创建实例时，该函数的prototype属性值将被作为原型赋值给所有对象实例（也就是设置实例的\_\_proto\_\_属性）

----------

![代码1](https://i.imgur.com/AXiNJPB.png)

----------

![代码运行结果1](https://i.imgur.com/rxKS5Wr.png)

----------
![代码2](https://i.imgur.com/hamoDrc.png)

----------

![代码运行结果2](https://i.imgur.com/lAvstEk.png)

----------
![代码3](https://i.imgur.com/QGgHLKk.png)

----------
![代码运行结果3](https://i.imgur.com/HR0Ek5X.png)