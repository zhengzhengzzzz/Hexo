---
title: js高级你真的学会了吗
date: 2022-04-02 20:42:53
tags: javascript 前端
categories: bo‘k
---

<!--more-->

### 关于js高级

最近这一段时间一直在学习js高级，感觉这才是js的本质，而且这也是面试官都爱问的一块。  
在js高级中，有两块公认难以理解的“神兽”，一个是闭包，另一个是原型；所以这篇博客我重要来与大家讨论一下这两个难点。  
首先是原型，先给大家看一张图片：![](https://img-blog.csdnimg.cn/ca808a835d0e4e1ea4f24509645a5bf1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5LiN6YCDNzY=,size_19,color_FFFFFF,t_70,g_se,x_16#pic_center)  
大家看了这张图片之后是不是觉得原型，原型链之间的关系特别乱；不过不用担心，首先我们得理解函数和对象的关系。  
首先大家需要记住一句话：对象都是通过函数创建的！然后每个函数都有一个默认的prototype属性，每个对象都有一个\_\_proto\_\_可成为隐式原型。我这样说这张图就好理解一点了。首先是一个构造函数Foo，既然是函数就有prototype属性，指向一个Foo.prototype对象，而Foo函数new出来的f1,f2对象，都有\_\_proto\_\_属性，而且这个属性也指向Foo.prototype这个对象，也让我们知道凡是一个构造函数创建的对象都有一个共同的原型，这样可以达到某种共享，比如共享方法，减少了代码的重复。关于原型我先说到这里吧，我们下回分析闭包。