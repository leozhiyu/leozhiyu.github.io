---
title: jQuery 知识总结
copyright: true
date: 2017-10-15 09:37:44
tags: 
- jQuery
categories:
- javascrpit
- jQuery
---
个人建议：学习 jQuery 前先掌握基本的 JavaScrpit 语法，特别是对函数要掌握，jQuery 基本上是使用函数。

<!--more-->

# jQuery 简介 #
- jQuery 是一个轻量级 JavaScript 库

- jQuery 库位于一个 JavaScript 文件中，其中包含了所有的 jQuery 函数，需要通过 `<script>` 标签引入 jQuery 库才能使用



# jQuery 库的三种引入来源 #
- 本地引入
共有两个版本的 jQuery 可供下载 [http://jQuery.com](http://jQuery.com)：一份是精简过的，另一份是未压缩的（供调试或阅读）

- 从 Google 加载 CDN jQuery 核心文件（ 版本可更换 ）
>     src = http://ajax.googleapis.com/ajax/libs/jquery/1.4.0/jquery.min.js

- 从 Microsoft 加载 CDN jQuery 核心文件（ 版本可更换 ）
>     src = http://ajax.microsoft.com/ajax/jquery/jquery-1.4.min.js

# jQuery语法 #
**基础语法：`$(selector).action()` **
- 美元符号（$）定义 jQuery

- 选择符（selector）“ 查询 ” 和 “ 查找 ” HTML 元素

- jQuery 的 `action()` 执行对元素的操作

**文档就绪函数**
- 文档就绪函数，用于在页面加载成功后执行的指定代码  

- 如果在文档没有完全加载之前就运行函数，操作可能失败

- 通常该函数用于替换 `window.onload` 事件，文档就绪函数的执行效率更高  
```javascript
$(document).ready(function(){
    code block
});```
可以简写为:
```javascript
$(function(){
    code block
});
```
**jQuery 标识符**
jQuery 使用 $ 符号作为 jQuery 的简写

- 使用 jQuery 全名 
```javascript
jQuery(document).ready(function(){
  jQuery("button").click(function(){
    code block
  });
});
```
- 使用 jQuery 简写
```javascript
$(function(){
    $("button").click(function(){
        code block
    });
});
```
- 自定义 jQuery 别名
```javascript
var jq = $.noConflict();
jq(function(){
    jq("button").click(function(){
        code block
    });
});
```
注：因为 javascrpit 某些框架中也使用 $ 作为简写( 就像 jQuery )，`noConflict()` 方法是为了解决 javascrpit 框架之间符号冲突而定义的方法，它会释放 $ 标识符的控制，这样其他脚本也可以使用这个符号

# jQuery 选择器 #
** jQuery 元素选择器和属性选择器允许您通过标签名、属性名或内容对 HTML 元素进行选择 **

#### jQuery 元素选择器 ####
jQuery 使用 CSS 选择器来选取 HTML 元素
- `$(this)` 当前 HTML 元素
- `$("p")`  选取 `<p>` 元素
- `$("p.intro")` 选取所有 `class="intro"` 的 `<p>` 元素
- `$("p#demo")` 选取所有 `id="demo"` 的 `<p>` 元素
- `$("div#intro .head")` 选取 `id="intro"` 的 `<div>` 元素中的所有 `class="head"` 的元素

#### jQuery 属性选择器 ####
jQuery 使用 XPath 表达式来选择带有给定属性的元素

- `$("[href]")` 选取所有带有` href` 属性的元素
- `$("[href='#']")` 选取所有带有 `href` 值等于` "#"` 的元素
- `$("[href!='#']")` 选取所有带有 `href` 值不等于 `"#"` 的元素
- `$("[href$='.jpg']")` 选取所有 `href` 值以 `".jpg"` 结尾的元素

#### jQuery CSS 选择器 ####
jQuery CSS 选择器可用于改变 HTML 元素的 CSS 属性
- 把所有 p 元素的背景颜色更改为红色：`$("p").css("background-color","red")`

# jQuery 事件 #

- jQuery 事件处理方法是 jQuery 中的核心函数

- 事件处理程序指的是当 HTML 中发生某些事件时所调用的方法。术语由事件“触发”（或“激发”）经常会被使用

- 例子：按钮的点击事件被触发时会调用一个函数
`$("button").click(function() {..some code... } )`

**常用事件函数**

- `$(document).ready(function)` 将函数绑定到文档的就绪事件（当文档完成加载时）
- `$(selector).click(function)`	触发或将函数绑定到被选元素的点击事件
- `$(selector).change(function)`	触发、或将函数绑定到指定元素的 change 事件
- `$(selector).dblclick(function)`	触发或将函数绑定到被选元素的双击事件
- `$(selector).focus(function)`	触发或将函数绑定到被选元素的获得焦点事件
- `$(selector).mouseover(function)`	触发或将函数绑定到被选元素的鼠标悬停事件

# jQuery 效果 #
效果通常绑定在某在事件上，例如通过点击按钮产生隐藏效果

** 常见的用于效果的函数 **

1.隐藏、显示、切换
    - 隐藏 `$(selector).hide(speed,callback)` ： `$("p").hide();`

    - 显示`$(selector).show(speed,callback)` ： `$("p").show(1000);`

    - 切换隐藏/显示`$(selector).toggle(speed,callback)` ： `$("p").toggle();`

speed 和 callback 都是可选参数

speed 参数规定显示/隐藏的速度，可选值为："slow"、"fast" 或毫秒值

callback 参数是显示/隐藏完成后所执行的函数名称

2.淡入、淡出
    - 淡入 `$(selector).fadeIn(speed,callback)` ： `$("#div1").fadeIn("slow");`

    - 淡出 `$(selector).fadeOut(speed,callback)` ： `$("#div3").fadeOut(3000);`
 
    - 切换淡入/淡出 `$(selector).fadeToggle(speed,callback)` ： ` $("#div1").fadeToggle();`
 
    - 渐变为特定透明度 `$(selector).fadeTo(speed,opacity,callback)` ：`$("#div2").fadeTo("slow",0.4);` 
speed 和 callback 都是可选参数，opacity 为必需参数

speed 参数规定淡入/淡出的速度，可选值为："slow"、"fast" 或毫秒值

callback 参数是显示/隐藏完成后所执行的函数名称

opacity 参数将淡入淡出效果设置为给定的不透明度（值介于 0 与 1 之间)

3.滑动
    - 向下滑动 `$(selector).slideDown(speed,callback)` ： `$("#panel").slideDown();`

    - 向上滑动 `$(selector).slideUp(speed,callback)` : `$("#panel").slideUp();`
 
    - 切换向上滑动/向下滑动 `slideToggle()` : `$("#panel").slideToggle();`

speed 和 callback 都是可选参数

speed 参数规定向上滑动/向下滑动的速度，可选值为："slow"、"fast" 或毫秒值

callback 参数是向上滑动/向下滑动完成后所执行的函数名称

4.动画
    - 自定义动画 `$(selector).animate({params},speed,callback)`

params 是必需参数， speed 和 callback 是可选参数

params 参数定义形成动画的 CSS 属性

speed 参数规定效果的时长，可选值为："slow"、"fast" 或毫秒值

callback 参数是动画完成后所执行的函数名称


注： 默认地，所有 HTML 元素都有一个静态位置，且无法移动，如需对位置进行操作，要记得首先把元素的 CSS position 属性设置为 relative、fixed 或 absolute！

    - 停止动画 ·`$(selector).stop(stopAll,goToEnd);`

stopAll 和 goToEnd 都是可选参数

stopAll 参数规定是否应该清除动画队列。默认是 false，即仅停止活动的动画，允许任何排入队列的动画向后执行

goToEnd 参数规定是否立即完成当前动画。默认是 false。

默认地，stop() 会清除在被选元素上指定的当前动画

** 实例 **

使用绝对值
```javascript
$("div").animate({
    left:'250px',
    opacity:'0.5',
    height:'150px',
    width:'150px'
  });```
使用相对值
```javascript
$("button").click(function(){
  $("div").animate({
    left:'250px',
    height:'+=150px',
    width:'+=150px'
  });
});
```

使用队列功能 ( 逐一进行 animate 调用 )
```javascript
$("button").click(function(){
  var div=$("div");
  div.animate({height:'300px',opacity:'0.4'},"slow");
  div.animate({width:'300px',opacity:'0.8'},"slow");
  div.animate({height:'100px',opacity:'0.4'},"slow");
  div.animate({width:'100px',opacity:'0.8'},"slow");
});```

5.方法连接
允许我们在相同的元素上运行多条 jQuery 命令，一条接着另一条，这样的话，浏览器就不必多次查找相同的元素。如需链接一个动作，您只需简单地把该动作追加到之前的动作上

例如： 把 `css()`, `slideUp()` 和  `slideDown()` 链接在一起。"p1" 元素首先会变为红色，然后向上滑动，然后向下滑动
`$("#p1").css("color","red").slideUp(2000).slideDown(2000);`

# jQuery 操作 HTML 元素和属性 #
** 对内容操作 **

1.获取内容 
    - `$(selector).text();` 设置或返回所选元素的文本内容
    
    - `$(selector).html();` 设置或返回所选元素的内容（包括 HTML 标记）
    
    - `$(selector).val();` 设置或返回表单字段的值
2.设置内容
    - `$(selector).text(string);` 设置所选元素的文本内容
    
    - `$(selector).val(string);` 设置所选元素的内容（包括 HTML 标记）
    
    - `$(selector).html(string);` 设置表单字段的值

3.回调函数

i：被选元素列表中当前元素的下标

origText：原始（旧的）值

res：以函数新值返回您希望使用的字符串
    - `$(selector).text(function(i,origText){return res;});` 设置或返回所选元素的文本内容
    
    - `$(selector).val(function(i,origText){return res;});` 设置或返回所选元素的内容（包括 HTML 标记）
    
    - `$(selector).html(function(i,origText){return res;});` 设置或返回表单字段的值
 

** 对属性操作 **

1.获取属性
    - `$(selector).attr("attribute");` 获取指定元素的所选属性

2.设置属性
    - `$(selector).attr("attribute","value");` 设置所选属性的值
    
    - `$(selector).attr({"attribute1":"value1", "attribute2":"value2"});` 同时设置多个属性的值

3.attr() 的回调函数

i ： 被选元素列表中当前元素的下标

origValue : 原始（旧的）值

res ： 以函数新值返回您希望使用的字符串

    - `$(selector).attr("attribute",function(i,origValue){return res});`


** 对元素/内容操作 **

与前面的 对内容操作 不同的是：上面的三个方法会将原来的值覆盖，而这里的方法是在原值基础上进行修改

1.添加

参数可以是多个，如果多个含有 html 的内容，则相当于增加了多个 html 元素
    - `$(selector).append("text");` 在被选元素的结尾插入内容
    
    - `$(selector).prepend("text");` 在被选元素的开头插入内容
    
    - `$(selector).prepend("text");` 在被选元素之后插入内容

    - `$(selector).before("text");` 在被选元素之前插入内容
 
2.删除

    - `$(selector).remove();` 删除被选元素（及其子元素）

    - `$(selector).empty();` 从被选元素中删除子元素

    - `$(selector).remove(selector);` 删除指定选择器的元素

** 对 CSS 元素操作 **

前三种方法是针对已经写好的样式
    - `$(selector).addClass("className1 className2");` 向被选元素添加一个或多个类
    
    - `$(selector).removeClass("className1 className2");` 从被选元素删除一个或多个类
    
    - `$(selector).toggleClass("className");` 对被选元素进行添加/删除类的切换操作

    - `$(selector).css();`返回样式属性

    - `$(selector).css("attribute","value");`设置单个样式属性
    
    - `$(selector).css({"propertyname":"value","propertyname":"value",...});`设置多个样式属性

** 对尺寸操作 **

注意参数，中间四种没有没有参数，不能进行设置

    - `$(selector).width("text");` 设置或返回元素的宽度（不包括内边距、边框或外边距）
    
    - `$(selector).height("text");` 设置或返回元素的高度（不包括内边距、边框或外边距）
    
    - `$(selector).innerWidth();`返回元素的宽度（包括内边距）

    - `$(selector).innerHeight(");`返回元素的高度（包括内边距）

    - `$(selector).outerWidth();`返回元素的宽度（包括内边距和边框）

    - `$(selector).outerHeight();` 返回元素的高度（包括内边距和边框）

    - `$(selector).outerWidth(true);`返回元素的宽度（包括内边距、边框和外边距）

    - `$(selector).outerHeight(true);` 返回元素的高度（包括内边距、边框和外边距）


# jQuery 遍历 DOM #

** 祖先 **

向上遍历 DOM 树

    - `$(selector).parent();` 返回被选元素的直接父元素，该方法只会向上一级对 DOM 树进行遍历
    
    - `$(selector).parents();` 返回被选元素的所有祖先元素，它一路向上直到文档的根元素 (<html>)
    
    - `$(selector).parents(selector);` 返回经过过滤的所有祖先元素，它一路向上直到文档的根元素 (<html>)
    
    - `$(selector).parentsUntil() ;`返回介于两个给定元素之间的所有祖先元素

** 祖先 **

向下遍历 DOM 树，以查找元素的后代

    - `$(selector).children();` 返回被选元素的所有直接子元素，该方法只会向下一级对 DOM 树进行遍历

     - `$(selector).children(selector);` 返回被选元素的经过过滤的子元素，该方法只会向下一级对 DOM 树进行遍历
    
    - `$(selector).find("selector");` 返回被选元素的后代元素，一路向下直到最后一个后代(此方法必须有参数，如果是全部则为 "*" )

** 同胞 **

 DOM 树中遍历元素的同胞元素

    - `$(selector).siblings(selector);` 返回被选元素的所有同胞元素（selector可选）

     - `$(selector).next();` 返回被选元素的下一个同胞元素
    
    - `$(selector).nextAll();` 返回被选元素的所有跟随的同胞元素

    - `$(selector).nextUntil(selecotr);` 返回介于两个给定参数之间的所有跟随的同胞元素

    - prev(), prevAll() 以及 prevUntil() 方法的工作方式与上面的方法类似，只不过方向相反而已：它们返回的是前面的同胞元素（在 DOM 树中沿着同胞元素向后遍历，而不是向前）

** 过滤 **

允许您基于其在一组元素中的位置来选择一个特定的元素

     - `$(selector).first();` 返回被选元素的首个元素
    
    - `$(selector).last();` 返回被选元素的最后一个元素

    - `$(selector).eq();` 返回被选元素中带有指定索引号的元素（索引号从 0 开始）

    - `$(selector).filter(selector);` 不匹配这个标准的元素会被从集合中删除，匹配的元素会被返回

    - `$(selector).not(selector);` not() 方法与 filter() 相反，返回不匹配标准的所有元素

# jQuery Ajax #

AJAX = 异步 JavaScript 和 XML（Asynchronous JavaScript and XML）

简短地说，在不重载整个网页的情况下，AJAX 通过后台加载数据，并在网页上进行显示

** load 方法**
    - `$(selector).load(URL,data,callback);` 
    方法从服务器加载数据，并把返回的数据放入被选元素中

必需的 URL 参数规定您希望加载的 URL。
可选的 data 参数规定与请求一同发送的查询字符串键/值对集合。
可选的 callback 参数是 load() 方法完成后所执行的函数名称

callback 回调函数

`$(selector).load(URL,data,function(responseTxt,statusTxt,xhr){});`

responseTxt - 包含调用成功时的结果内容
statusTXT - 包含调用的状态
xhr - 包含 XMLHttpRequest 对象

responseTxt - 包含调用成功时的结果内容
statusTXT - 包含调用的状态
xhr - 包含 XMLHttpRequest 对象

** get/post 方法**

必需的 URL 参数规定您希望请求的 URL
可选的 callback 参数是请求成功后所执行的函数名

    - `$.get(URL,callback);`
    通过 HTTP GET 请求从服务器上请求数据

GET - 从指定的资源请求数据
GET 基本上用于从服务器获得（取回）数据。注释：GET 方法可能返回缓存数据

    - `$.post(URL,data,callback);`
    通过 HTTP POST 请求从服务器上请求数据

data 是要提交给服务器的数据，如果数据有多个，使用 json 格式
POST - 向指定的资源提交要处理的数据
POST 也可用于从服务器获取数据。不过，POST 方法不会缓存数据，并且常用于连同请求一起发送数据

回调函数

data : 存有被请求页面的内容
status : 存有请求的状态( success/fail )
``` javascript
function(data,status){
    alert("Data: " + data + "\nStatus: " + status);
  });```

注意： ajax 不能访问本地文件，需要解决跨域访问的问题
