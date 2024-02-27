---
title: 美好的一天从学到新知识开始（for ，整除）
date: 2021-10-30 20:54:33
tags: java
categories: 练习
---

<!--more-->

```java
/*
 编写程序从1循环到150，并在每行打印一个值；
 另外在每个3的倍数上打印“foo”
 在每个5的倍数行上打印“biz”；
 在每个7的倍数行上打印输出“baz”；*/
class ForTest1 {
	public static void main(String[] args) {
		for(int i=1;i<=150;i++){
			System.out.print(i+" ");
			if(i%3==0){
				System.out.print("foo");
			}
			if(i%5==0){
				System.out.print("biz");
			}
			if(i%7==0){
				System.out.print("baz");
			}
			
			
			System.out.println();
		}
	}
}
```

![](https://img-blog.csdnimg.cn/18e08fbf1e39425b9283dd4693d10c92.png)（整除）

 ![](https://img-blog.csdnimg.cn/b960f372804245a8936f9dd41402eb12.png)

 学会用for语句

 

成功运行

![](https://img-blog.csdnimg.cn/c797f0cc940943d49b5db9fcfdad53d1.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbTBfMjI3,size_15,color_FFFFFF,t_70,g_se,x_16)