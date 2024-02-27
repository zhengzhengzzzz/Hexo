---
title: 用switch语句
date: 2021-10-30 08:45:42
tags: java 开发语言 后端
categories: 练习
---

<!--more-->

```java
/*

将学生成绩按不同的分数段分为优，良，中，及格和不及格五个等级，从键盘上输入一个数0~100，输出相应的等级。要用swithc语句
*/


import java.util.Scanner;
public class switchTest {
    public static void main(String[] args){
	System.out.println("请输入一个0~100的成绩:");
   Scanner reader =new Scanner(System.in);
   int grade=reader.nextInt();

   switch(grade/10){
case 1 :
case 2 :
case 3 :
case 4 :
case 5 :
      System.out.println("不及格");
		   break;
case 6 :
      System.out.println("及格");
		   break;
case 7 :
      System.out.println("中");
		   break;
case 8 :
      System.out.println("良");
		   break;
case 9 :
      System.out.println("优，继续保持");
       }
   }
}
```