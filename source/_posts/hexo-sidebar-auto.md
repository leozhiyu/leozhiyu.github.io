---
title: 2017版 Hexo Next主题侧边栏 Sidebar 配置自动展开教程
copyright: true
date: 2017-09-08 11:29:00
tags:
    - hexo
categories:
    - 工具
    - hexo
---


## 背景
从搭建博客就想自动展开侧边栏， 结果网上找了很多方法都不生效， 今天找到一篇博客试着重新设置了下， 仍然无效， 但是找到了设置的思路， 于是自己找相关的文件进行设置， 在此分享。

<!--more-->

## 说明
由于主题版本不同， 以下方法只是对应当前版本， 但是思路是一样的， 只是文件的路径和文件名称可能不同

## Step 1 修改主题配置文件  display: always 

路径：/hexo/themes/next/_config.yml

```javascript
sidebar: 
  # Sidebar Position, available value: left | right
  position: left
  #position: right

  #display: post
  display: always 
  #display: hide
  #display: remove
```

## Step 2 修改 motion.js 文件

路径：/hexo/themes/next/source/js/src/motion.js
 
在文件末尾

```javascript
    sidebar: function (integrator) {
      //if (CONFIG.sidebar.display === 'always') {  //注释
        NexT.utils.displaySidebar();
      //} //注释
      integrator.next();
    }
```

## Step 3 修改post-details.js

路径： /hexo/themes/next/source/js/src/post-details.js

在文件末尾

```javascript
/*var $tocContent = $('.post-toc-content');
  var isSidebarCouldDisplay = CONFIG.sidebar.display === 'post' ||
      CONFIG.sidebar.display === 'always';
  var hasTOC = $tocContent.length > 0 && $tocContent.html().trim().length > 0;
  if (isSidebarCouldDisplay && hasTOC) {
    CONFIG.motion ?
      (NexT.motion.middleWares.sidebar = function () {
          NexT.utils.displaySidebar();
      }) : NexT.utils.displaySidebar();
  }*/
```
将以上代码段落全部注释， 只用最后一句

```javascript
NexT.utils.displaySidebar();
```


##### 这是我使用的主题版本的设置方法， 希望对需要的人有帮助