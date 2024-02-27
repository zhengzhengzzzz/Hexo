---
title: 论我idea怎么出现控制台中文乱码
date: 2022-04-28 16:42:09
tags: java
categories: JavaWeb
---

<!--more-->

# ![](https://img-blog.csdnimg.cn/1c296eaa08ea43ab878f08c2de8c27de.png)

害

 ![](https://img-blog.csdnimg.cn/bd2a7532d84947f7bac21d1750b03bf0.png)

 ![](https://img-blog.csdnimg.cn/511fa55d06a245e1a90b7ba877f0d9f4.png)

 ![](https://img-blog.csdnimg.cn/081ed58edfb34f389046659e74154691.png)

 

# 就很离谱    我乱码在网上找了好久 都试了还是没解决

# 没想到 是因为这个 

# 一、 SDK

SDK就是指可以为第三方开发者提供特定的软件包、软件[框架](https://so.csdn.net/so/search?q=%E6%A1%86%E6%9E%B6&spm=1001.2101.3001.7020 "框架")、硬件平台、操作系统等创建应用软件开发工具的集合，并且SDK还能简单的为某个程序设计语言提供应用程序接口API的一些文件。

**SDK工程师为辅助开发某类软件的相关文档、范例和工具的集合，使用SDK可以提高开发效率，更简单的接入某个功能。一个产品想实现某个功能，可以找到相关的SDK，工程师直接接入SDK，就不用再重新开发了**

通常SDK是由专业性质的公司提供专业服务的[集合](https://so.csdn.net/so/search?q=%E9%9B%86%E5%90%88&spm=1001.2101.3001.7020 "集合")，比如提供安卓开发工具、或者基于硬件开发的服务等。也有针对某项软件功能的SDK，如推送技术、图像识别技术、移动支付技术、语音识别分析技术等，在互联网开放的大趋势下，一些功能性的SDK已经被当作一个产品来运营。

开发者不需要再对产品的每个功能进行开发，选择合适稳定的SDK服务并花费很少的经历就可以在产品中集成某项功能。

# 二、SDK和[API](https://so.csdn.net/so/search?q=API&spm=1001.2101.3001.7020 "API")

**SDK和API的区别**SDK相当于开发集成工具环境，API就是数据接口。在SDK环境下调用API数据

实际上SDK包含了API的定义，API定义一种能力，一种接口的规范，而SDK可以包含这种能力、包含这种规范。但是SDK又不完完全全只包含API以及API的实现，它是一个软件工具包，它还有很多其他辅助性的功能。

SDK 包含了使用 API 的必需资料，所以人们也常把仅使用 API 来编写 Windows 应用程序的开发方式叫做“SDK编程”。

# 三、JDK

JDK：开发JAVA程序的开发包，JDK里面有JAVA的运行环境\(JRE\)，包括client和server端，需要配置环境变量