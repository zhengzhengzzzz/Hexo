---
title: 关于JDK的下载安装与环境变量的配置
date: 2021-10-22 14:31:20
tags: oracle 数据库 java
categories: JDK的安装和环境变量的配置
---

<!--more-->

1.去官网下载，我下载的是jdk se 10.0.2（网址：[Java Archive Downloads \- Java SE 10 \(oracle.com\)](https://www.oracle.com/java/technologies/java-archive-javase10-downloads.html "Java Archive Downloads \- Java SE 10 (oracle.com)")根据电脑系统选择下列进行下载 

![](https://img-blog.csdnimg.cn/5a613fcd91e64fa28f28a963dbbe6c99.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

 2.下载完成后 进行安装 电脑给定的是C盘的program files这个路径 不用修改直接安装即可

然后找到文件夹java![](https://img-blog.csdnimg.cn/0ee02fd0f7274dc291d853bdd0f89710.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

 打开之后看到有两个文件夹一个是JDK 一个是JRE即可

![](https://img-blog.csdnimg.cn/ebcde466a26f4e799da69e2be3b30f72.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

 3.开始配置环境变量  打开我的电脑 找到属性 打开

![](https://img-blog.csdnimg.cn/049504e264484455beb9f73c4ebd45b9.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

 找到高级系统设置点开

![](https://img-blog.csdnimg.cn/6840a51c39854e6f9ca54dbba6a469dc.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

 然后会出现一个窗口哦

![](https://img-blog.csdnimg.cn/1366e2b7af9444cfa00ac2fed1e73ee4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbTBfMjI3,size_19,color_FFFFFF,t_70,g_se,x_16)

 点开环境变量 进行环境变量配置

![](https://img-blog.csdnimg.cn/ad5ab07929ca4353bc0b48482b5ae05b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

这是我配置好的 你们应该是没有Java\_Home和path 和class path 这需要你自己新建

4.点击新建 然后如图

![](https://img-blog.csdnimg.cn/bfc78f3c96604d18b717a6c8fdf85cde.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

变量值是jdk的存储路径  然后确定

5.新建Path和上面一样 变量名是Path 变量值如图

![](https://img-blog.csdnimg.cn/c3d4be0c7d1144ca8439ce1166e5328e.png)然后确定设置完成

上方的用户变量中也可以设置Path和下方的一样

然后设置classpath

![](https://img-blog.csdnimg.cn/3f073e282ce74b68be9760cde15deece.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

 新建 然后填写 变量名Classpath 变量值   .;C:\\Program Files\\Java\\jre-10.0.2\\lib

如果下载路径一样可以直接复制

然后一直确定 关闭窗口即可

6.测试安装 windows+r 

![](https://img-blog.csdnimg.cn/4763df63ce5e4ce49496bee881ceb654.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbTBfMjI3,size_16,color_FFFFFF,t_70,g_se,x_16)

 确定出现命令提示行

![](https://img-blog.csdnimg.cn/22ee847e071741069628d8346b3dfc61.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

 分别输入Java Javac   Java \-version

![](https://img-blog.csdnimg.cn/244536e222594347ad99ee5238b7bbbf.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

 ![](https://img-blog.csdnimg.cn/8584a7b934d04c72aac3d9b6ff1081f3.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

 ![](https://img-blog.csdnimg.cn/9af90092abfc48b29f94287c1ca43a71.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

 测试完成 成功了

7.JDK的使用 编写一个Java应用程序 文件名APP2\_1.java （我是在E盘新建的）

新建记事本

![](https://img-blog.csdnimg.cn/40d2e6dd64af4255ab16952a8f250976.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

名字改为这个 然后编辑 

//filename:App2\_1.java  
public class App2\_1  
\{<!-- -->  
    public static void main\(String\[\] args\)  
    \{<!-- -->  
      System.out.println\("Hello Java\!"\);  
    \}  
\}

加入此程序 打开另存为

![](https://img-blog.csdnimg.cn/14d7869716614e05ba182116395e2122.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

如图 文件名要一样 保存类型所有文件 编码ANSI 保存 然后你会有这个CLASS文件生成

![](https://img-blog.csdnimg.cn/659792920d8e4f148d10a45122583d1f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbTBfMjI3,size_19,color_FFFFFF,t_70,g_se,x_16) 

 打开命令提示行

![](https://img-blog.csdnimg.cn/e41880ba95404361a66491df0b9297cd.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

 这个需要输入你刚刚创的文件的盘 比如我的是E盘 输入E：点击enter

![](https://img-blog.csdnimg.cn/91e3a2d6dbd04e75bd35b48e2f362414.png)

然后 cd 文件名 进入子目录 

比如我的是在E盘的Java文件夹里

![](https://img-blog.csdnimg.cn/b9e6234414654c92bb98ee781bf2379d.png)

8.输入 javac App2\_1.java

![](https://img-blog.csdnimg.cn/c2febd2b21a840938a020ba485e1f437.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbTBfMjI3,size_19,color_FFFFFF,t_70,g_se,x_16)

 再输入 java App2\_1

![](https://img-blog.csdnimg.cn/bb98f80c26d54ae982c67f7514d6de89.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbTBfMjI3,size_12,color_FFFFFF,t_70,g_se,x_16)

出现这个我们就成功了