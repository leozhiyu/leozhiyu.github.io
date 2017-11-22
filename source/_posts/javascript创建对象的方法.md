---
title: javascript 创建对象的方法
copyright: true
date: 2017-09-30 10:39:19
tags:
- javascript 对象
categories:
- javascript
---
Javascript 对象和原型是一个重点难点，今天就总结一下 javascript 创建对象的几种方式

<!--more-->

----------


####  "继承"（原型指向）
```javascript
var Student = {
  name : "Robot",
  height : 1.2,
  run:function(){
  console.log(this.name + " is running");
  }
};

var xiaoming = {
  name : "小明"
};

xiaoming.__proto__ = Student;
console.log(xiaoming.name);  //小明
xiaoming.run(); // 小明 is running
```
1. 通过指定 `xiaoming` 的原型， 看上去 `xiaoming` 仿佛是从 `Student` 继承下来的

2. JavaScript 的原型链和 Java 的 Class 区别就在，它没有 “Class” 的概念，所有对象都是实例，所谓继承关系不过是把一个对象的原型指向另一个对象而已

3. 在编写 JavaScript 代码时，不要直接用 `obj.__proto__` 去改变一个对象的原型，并且，低版本的 IE 也无法使用 `__proto__`

----------


#### 使用 `Object.create()` 创建对象
```javascript
var Student = {
  name : "Robot",
  height : 1.2,
  run:function(){
  console.log(this.name + "is running");
  }
};

var xiaoming = Object.create(Student);
xiaoming.name = "小明";
console.log(xiaoming.name);//小明
console.log(Student.name);//Robot
xiaoming.__proto__ === Student; // true
```
此种方法不同与构造函数，刚创建的对象只有原型中的默认值

----------


#### 基于 `Object.create()` 创建可初始化的对象

```javascript
var Student = {
    name: 'Robot',
    height: 1.2,
    run: function () {
        console.log(this.name + ' is running...');
    }
};

function createStudent(name){
  var stu = Object.create(Student);
  stu.name = name;
  return stu;
}

var xiaoming = createStudent("小明");
xiaoming.run();
console.log(xiaoming.name);
```

----------
#### 构造函数
```javascript
function Student(name){
  this.name = name;
  this.hello = function(){
  alert('hello,' + this.name + '!');
  }
}

var xiaoming = new Student('小明');
xiaoming.hello();
console.log(xiaoming.name);
```
1. 通过 `new` 关键字创建对象并且可以初始化值
2. 如果不写 `new`，这就是一个普通函数，它返回 `undefined`。但是，如果写了 `new`，它就变成了一个构造函数，它绑定的 `this` 指向新创建的对象，并默认返回 `this`，也就是说，不需要在最后写 `return this`;
3. 原型链： `xiaoming ----> Student.prototype ----> Object.prototype ----> null`

----------
#### 构造函数，并且创建共享函数，节省内存
```javascript
function Student(name){
  this.name = name;
}

Student.prototype.hello = function(){
  console.log('hello' + this.name + '!');
}

var xiaoming = new Student('小明');
xiaoming.hello();
console.log(xiaoming.name);
```
1. `Student.prototype` 是所有 `Student` 实例的共同原型，所以将方法定义在它上面
2. 如果写在构造函数里面，每创建一个实例，每个函数也会开辟内存

#### 创建对象忘记写关键字 new 如何应对
1. 在 `strict ` 模式下，`this.name = name`将报错，因为 `this` 绑定为 `undefined`，在非 `strict` 模式下，`this.name = name` 不报错，因为 `this` 绑定为 `window`，于是无意间创建了全局变量`name`，并且返回 `undefined`，这个结果更糟糕。
2. 封装 `new` 操作
```javascript
function Student(obj){
  this.name = obj.name || '匿名';
  this.grade = obj.grade || 1;
}

Student.prototype.hello = function(){
  console.log('hello' + this.name + '!');
}

function createStudent(obj){
  return new Student(obj || {});
}

var xiaoming = createStudent({name:'小明'});
console.log(xiaoming.name);//小明
console.log(xiaoming.grade);//1
```
1. 不需要用 `new` 创建对象
2.  与 基于 `Object.create()` 创建可初始化的对象 的方式类似，但优点在于通过 JSON 作为参数，参数非常灵活，可以不传参数使用默认值
3. 参数没有顺序，传一个 JSON 对象即可
----------

以上就是总结的几种创建对象的方法，注意对比它们之间的差别

资料参考来源：[廖雪峰的官方网站](http://www.liaoxuefeng.com)






