---
title: javascript 细节 - 基础
copyright: true
date: 2017-09-11 11:25:04
tags:
- javascript 细节
categories:
- javascript
---



### 语法部分
1. type 属性： 默认的 type 就是 javascript， 所以不必显式指定 type 为 javascript

<!--more-->

```javascript
<script>
...
</script>
```

2. javascript 不强制在每个语句结尾加 "；" ， javascript 会自动加分号， 但是在某些情况下会改变程序的语义， 所以最好主动加 "；"

3. 两个相等运算符比较
    - '==' 相等（  值相等 ）， 它会自动转换数据类型再比较， 很多时候会得到非常诡异的结果
    
    - '===' 严格相等（ 数据类型和值都相等 ） , 它不会自动转换数据类型， 如果数据类型不一致， 返回false， 如果一致， 再比较

4. NaN 与所有其他值都不相等， 包括它自己：
```javascript
NaN === NaN; // false
```
    唯一能判断 `NaN` 的方法是通过 `isNaN()` 函数：
```javascript
isNaN(NaN); // true
```

5. 浮点数比较

    浮点数在运算过程中会产生误差， 因为计算机无法精确表示无限循环小数。 要比较两个浮点数是否相等， 只能计算它们之差的绝对值， 看是否小于某个阈值：
```javascript
Math.abs(1 / 3 - (1 - 2 / 3)) < 0.0000001; // true
```

6. null 和 undefined

    大多数情况下， 我们都应该用 `null`， `undefined ` 仅仅在判断函数参数是否传递的情况下有用

7. 出于代码的可读性考虑， 创建数组建议用 `[]`， 而不使用 `new Array()`

8. 如果一个变量没有通过 `var` 申明就被使用， 那么该变量就自动被申明为全局变量。 使用 `var` 申明的变量则不是全局变量， 它的范围被限制在该变量被申明的函数体内

9. 启用 strict 模式（ 强制通过 `var` 声明变量）： 在 javascript 代码第一行写上
```javascript
'use strict';
```

10. 多行字符串用反引号表示  \` ... \` 
```javascript
`这是一个
多行
字符串`;
```

11. 模版字符串
```javascript
alert(`你好, ${name}, 你今年 ${age} 岁了!`);
```

12. 要获取字符串某个指定位置的字符， 使用类似 `Array` 的下标操作， 索引号从 0 开始。 字符串是不可变的， 如果对字符串的某个索引赋值， 不会有任何错误， 但是， 也没有任何效果

13. 直接给 `Array` 的 `length` 赋一个新的值会导致 `Array` 大小的变化：
```javascript
var arr = [1, 2, 3];
arr.length; // 3
arr.length = 6;
arr; // arr变为[1, 2, 3, undefined, undefined, undefined]
```
14. 如果通过索引赋值时， 索引超过了范围， 同样会引起 `Array` 大小的变化， 但是不会有任何错误， 在编写代码时， 不建议直接修改 `Array` 的大小， 访问索引时要确保索引不会越界
```javascript
var arr = [1, 2, 3];
arr[5] = 'x';
arr; // arr变为[1, 2, 3, undefined, undefined, 'x']
```

15. 数字 30 和字符串 '30' 是不同的元素

16. `slice()` 的起止参数包括开始索引， 不包括结束索引。

17. 如果不给 `slice()` 传递任何参数， 它就会从头到尾截取所有元素。 利用这一点， 我们可以很容易地复制一个 `Array`

18. 空数组继续 `pop` 不会报错，而是返回 `undefined`

19. `concat()` 方法并没有修改当前 `Array`， 而是返回了一个新的 `Array`，`concat()` 方法可以接收任意个元素和 `Array`， 并且自动把 `Array` 拆开， 然后全部添加到新的 `Array` 里
```javascript
var arr = ['A', 'B', 'C'];
arr.concat(1, 2, [3, 4]); // ['A', 'B', 'C', 1, 2, 3, 4]
```

20. `javascript` 对象属性名必须是一个有效的变量名。 如果属性名包含特殊字符， 就必须用 `''` 括起来
```javascript
var xiaohong = {
    name: '小红',
    'middle-school': 'No.1 Middle School'
};
```

    `xiaohong` 的属性名 `middle-school` 不是一个有效的变量， 就需要用 `''` 括起来。 访问这个属性也无法使用 `.` 操作符， 必须用 `['xxx']` 来访问：
```javascript
xiaohong['middle-school']; // 'No.1 Middle School'
xiaohong['name']; // '小红'
xiaohong.name; // '小红'
```

21. 如果我们要检测 `xiaoming` 是否拥有某一属性， 可以用in操作符,  不过要小心， 如果 `in` 判断一个属性存在， 这个属性不一定是`xiaoming` 的， 它可能是 `xiaoming` 继承得到的
```javascript
'name' in xiaoming; // true
```

22. 要判断一个属性是否是 `xiaoming` 自身拥有的，而不是继承得到的，可以用 `hasOwnProperty()` 方法

23. JavaScript把 `null`、`undefined`、`0`、`NaN` 和空字符串  `''` 视为 `false`，其他值一概视为 `true`

24. 由于 `Array` 也是对象， 而它的每个元素的索引被视为对象的属性， 因此， `for ... in` 循环可以直接循环出 `Array` 的索引

25. `for ... in` 循环由于历史遗留问题， 它遍历的实际上是对象的属性名称， `for ... of` 循环则完全修复了这些问题， 它只循环集合本身的元素
```javascript
var a = ['A', 'B', 'C'];
a.name = 'Hello';
for (var x in a) {
    alert(x); // '0', '1', '2', 'name'
}
```
    ```javascript
    var a = ['A', 'B', 'C'];
    a.name = 'Hello';
    for (var x of a) {
        alert(x); // 'A', 'B', 'C'
    }
    ```
### 函数

1. 函数如果没有 `return` 语句， 函数执行完毕后也会返回结果， 只是结果为 `undefined`

2. 由于 `JavaScript` 允许传入任意个参数而不影响调用， 因此传入的参数比定义的参数多也没有问题， 虽然函数内部并不需要这些参数， 传入的参数比定义的少也没有问题

3. 关键字 `arguments` 类似 `Array` 但它不是一个 `Array`

4. 不在任何函数内定义的变量就具有全局作用域。 实际上， `JavaScript` 默认有一个全局对象 `window`。 以变量方式 `var foo = function () {}` 定义的函数实际上也是一个全局变量。

5. 用 `var that = this;` ， 你就可以放心地在方法内部定义其他函数，而不是把所有语句都堆到一个方法中。 对于普通函数调用， 通常把 `this` 绑定为 `null`。

6. `apply()` 与 `call()`的唯一区别
 
    - `apply()` 把参数打包成 `Array` 再传入；

    - `call()` 把参数按顺序传入。

7. 所有实例的原型引用的是函数的 prototype 属性

8. 箭头函数
    - 如果要返回一个对象，就要注意，如果是单表达式，这么写的话会报错
```javascript
// SyntaxError:
x => { foo: x }
```
    - 因为和函数体的 `{ ... }` 有语法冲突，所以要改为：
```javascript
// ok:
x => ({ foo: x })
```

9. 箭头函数内部的 `this` 是词法作用域（ 写代码或者定义时确定的 作用域，动态作用域是在运行时确定 ），由上下文确定。箭头函数完全修复了 `this` 的指向，`this` 总是指向词法作用域

### 标准对象
1. 如果我们在使用 `Number`、`Boolean` 和 `String` 时， 没有写 `new` ，`Number()`、`Boolean()` 和 `String()` 被当做普通函数，把任何类型的数据转换为 `number`、`boolean` 和 `string` 类型（注意不是其包装类型）
2. 不要使用 `new Number()`、`new Boolean()`、`new String()` 创建包装对象；

3. 用 `parseInt()` 或 `parseFloat()` 来转换任意类型到 `number`；

4. 用 `String()` 来转换任意类型到 `string` ，或者直接调用某个对象的 `toString()` 方法；

5. 通常不必把任意类型转换为 `boolean` 再判断，因为可以直接写 `if (myVar) {...}` ；

6. `typeof` 操作符可以判断出 `number` 、`boolean`、`string`、`function` 和 `undefined` ；

7. 判断 `Array` 要使用 `Array.isArray(arr)`；

8. 判断 `null` 请使用 `myVar === null`；

9. 判断某个全局变量是否存在用 `typeof window.myVar === 'undefined'`；

10. 函数内部判断某个变量是否存在用 `typeof myVar === 'undefined'`。

11. 任何对象都有 `toString()` 方法吗？ `null` 和 `undefined` 就没有！

12. `number` 对象调用 `toString()` 报 `SyntaxError`
```javascript
123.toString(); // SyntaxError
```
    解决办法：
    ```javascript
    123..toString(); // '123', 注意是两个点！
    (123).toString(); // '123'
    ```

注：以上内容整理自[廖雪峰的官方网站](https://www.liaoxuefeng.com/)