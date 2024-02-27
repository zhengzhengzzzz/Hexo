---
title: jsp（全称JavaServer Pages）的学习
date: 2022-03-29 20:19:38
tags: java
categories: JavaWeb
---

<!--more-->

## 咱就是说咱也是刚刚才开始学习：

这就存个档，记录一下咱们听课的笔记 顺便打个卡。

**/\*什么是jsp？
全称 java server pages。Java的服务器页面
jsp主要的作用是代替Servlet程序回传html页面的数据
因为serlet程序的回传html页面数据是一件非常繁琐的事情。开发和维护成本高;\*/**

jsp的page指令

1.language属性          表示jsp翻译后是什么语言文件。暂时只支持java

2.contentType属性     表示jsp返回的数据类型是什么

3.pageEncoding属性  表示jsp页面本身的字符集

4.import属性               跟java源代码中一样，用于导包，导类

\=================以下两个属性是给out输出流使用=============

5.autoFlush属性          设置当out输出流缓冲区满了之后，是否自动刷新冲级区，默认值是true；

6.buffer属性                设置out缓冲区的大小，默认值是8KB

\=================以上两个属性是给out输出流使用=============

7.errrorPage属性        设置当jsp页面运行错误时，自动跳转去的错误页面

8.isErrorPage属性       设置当前jsp页面是否是错误信息页面。默认是false。如果是true可以获取异常信息

9.session属性  设置访问当前jsp页面，是否会创建HttpSession对象，默认是true

10.extends属性   设置jsp翻译出来的java类默认继承谁

## 声明脚本

声明脚本的格式是：

\<\%\! 声明java代码 \%>

作用：可以给jsp翻译出来的java类定义属性和方法甚至是代码块，内部类

![](https://img-blog.csdnimg.cn/fb3fca099594476984b962b9c04931f1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

 

## 表达式脚本

表达式脚本的格式：\<\% = 表达式\%>

作用：jsp页面上输出数据

![](https://img-blog.csdnimg.cn/b441c629c4bf4a65a681e144e80317f4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

 

## 代码脚本：

\<\% java语句 \%>

![](https://img-blog.csdnimg.cn/9633d98289ed48eea90392b6634871bd.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

 

## jsp的三种注释

![](https://img-blog.csdnimg.cn/7a5342e97dea4415adcb045f3c8de590.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

 

## jsp的九大内置对象

![](https://img-blog.csdnimg.cn/87c43f0f46a44c94b1a8d64330163ceb.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

![](https://img-blog.csdnimg.cn/435b40cd57434de5a36239d76f179dc5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

##   
 静态包含和动态包含 

![](https://img-blog.csdnimg.cn/488e140460af43e788dbd10b6ccab366.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

##  jsp 标签-转发

![](https://img-blog.csdnimg.cn/611ea1877ff64740a07e19e7f5dbb625.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

 

参考视频：尚硅谷的javaweb视频