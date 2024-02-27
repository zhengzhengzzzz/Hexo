---
title: JavaWeb的学习 -- 第一阶段 html
date: 2022-03-26 09:34:17
tags: java
categories: JavaWeb
---

<!--more-->

## 基础定义：（了解）

超文本标记语言（英语：HyperText Markup Language，简称：HTML）是一种用于创建网页的标准标记语言。描述页面，包含HTML标签及文本内容。

您可以使用 HTML 来建立自己的 WEB 站点，HTML 运行在浏览器上，由浏览器来解析。

第一个实例（上手多敲）

```XML
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>菜鸟教程(runoob.com)</title>
</head>
<body>
 
<h1>我的第一个标题</h1>
 
<p>我的第一个段落。</p>
 
</body>
</html>
```

## ![](https://img-blog.csdnimg.cn/214acf8394ba4a85be991b611a962fe8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)HTML标签

HTML 标记标签通常被称为 HTML 标签 \(HTML tag\)。

- HTML 标签是由_尖括号_包围的关键词，比如 \<html>
- HTML 标签通常是_成对出现_的，比如 \<b> 和 \</b>
- 标签对中的第一个标签是_开始标签_，第二个标签是_结束标签_
- 开始和结束标签也被称为_开放标签_和_闭合标签_

## _HTML元素_

\<p>这是一个段落。\</p>  从头到尾  整个是一个元素

![](https://img-blog.csdnimg.cn/34c059f9c0144718b41796eae8b68318.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

 咱们后端的学点javaweb用idea进行操作就可以了，不用下前端那么好使的软件。

## 开始第一个hellword程序

![](https://img-blog.csdnimg.cn/e1a54f66f6fb4216b713bf4bc887ee91.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_17,color_FFFFFF,t_70,g_se,x_16)

![](https://img-blog.csdnimg.cn/08be7d88c27046bf98f738b9dc6fc619.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_15,color_FFFFFF,t_70,g_se,x_16)

 ![](https://img-blog.csdnimg.cn/a7b0f60752db4a2c82772c4d3fc5e4fe.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

![](https://img-blog.csdnimg.cn/22301fdcaa5749ea928a339071ec0c1b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

 点击finish完成创建

![](https://img-blog.csdnimg.cn/e9179704d01d4d76a0256484865222c6.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

 ![](https://img-blog.csdnimg.cn/58d222def3a84c55a54a7356f8a74516.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_19,color_FFFFFF,t_70,g_se,x_16)

```XML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>Hello world</h1>
</body>
</html>
```

 ![](https://img-blog.csdnimg.cn/bfc8844f5a854df4a55876cf71062d96.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

 任意选择一个，可以用idea自带的，也可以使用本机上的浏览器

![](https://img-blog.csdnimg.cn/5fbd4542a1ff46c280ece70ceec2169b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_19,color_FFFFFF,t_70,g_se,x_16)

 这样咱们第一个就成功了；

## 常用标签

```XML
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>常用标签</title>
</head>
<body>

     <!--标题标签 -->
     <h1>早安</h1>
     <h2>早安</h2>
     <h3>早安</h3>
     <h4>早安</h4>
     <h5>早安</h5>
     <h6>早安</h6>
     
     <!-- 段落标签 -->
     <p>我是段落标签<p/>
     
     
     <!-- 换行标签 -->
          我是段落标签
     <hr>
          我是段落标签
     
     <!-- 无序标签 -->
     <ul>
     	<li>香蕉</li>
     	<li>橘子</li>
     	<li>辣条</li>
     	<li>榴莲</li>
     
     </ul>
     <!--ul定义一个无序列表 -->
     
     <!-- 有序列表ol li -->
     <ol start = "5">
        <li>香蕉</li>
     	<li>橘子</li>
     	<li>辣条</li>
     	<li>榴莲</li>
     </ol>

</body>
</html>
```

## 超链接

```XML
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<!-- 超链接: 链接到另一个资源 
	target 规定在何处打开链接文档
	_blank 表示在新窗口打开
	_self  表示在当前页面打开
	-->
	<a href=" https://www.bilibili.com/video/BV1r4411S7Eh?p=5&spm_id_from=pageDriver" target="_blank">我是超链接</a>
	<a href="https://www.bilibili.com/video/BV1r4411S7Eh?p=5&spm_id_from=pageDriver" target="targetFrame">我是超链接</a>
	<iframe src = "" name="targetFrame"></iframe>
	<iframe src = "https://www.bilibili.com/video/BV1r4411S7Eh?p=5&spm_id_from=pageDriver" name="targetFrame"></iframe>

</body>
</html>
```

## 内联框架

```XML
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<!-- 内联框架 iframe
	src= ""   代表资源的地址
	 -->
	 <iframe src="../img/课程表.png"></iframe>

</body>
</html>
```

## 图片标签

```XML
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<!-- 图片标签 img 表示引用图片
	alt=“ ”  如果图片找不到显示的提示内容
	src=“ ” 图片的源（路径）
	
路径有两种写法：
相对路径：不以/开始的路径都是相对路径，引入的资源相对于当前的资源的位置
绝对路径：以/开始的路径是绝对路径
	
	-->
	<img alt="课程表" src="../img/课程表.png"/>
	<img alt="课程表" src="/Html_Demo/img/课程表.png"/>

</body>
</html>
```

## 表格

```XML
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<!--在这里创建表格table 
	定义表格 
	tr 定义表格的一行
	td 定义表格的一列
	th 定义表头
	colspan  跨列
	rowspan  跨行
	-->
	<table border="1">
		<tr>
			<th>姓名</th> <th>性别</th> <th>爱好</th>
		</tr>
		<tr>
			<td colspan="2">张三</td> <td>男</td> <td>乒乓球</td>
		</tr>
		<tr>
			<td>李四</td> <td>女</td> <td>羽毛球</td>
		</tr>
	
	
	
	</table>
	
</body>
</html>
```

## 表单

```XML
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<!-- 表单：把数据提交给服务器
	数据一般都是用户填写的 
	action:指定要提交到的资源位置
	input 定义一个文本框（type="text）
	input 定义一个提交按钮(type="submit")
	input 定义一个密码框（type="password")
	input 定义一个单选按钮(type="radio")
	input 定义一个多选框(type="checkbox")
	
	-->
	<form action="target.html">
		用户名:<input type="text" name="username" value=""/>
		密码:<input type="password"name="pwd"value="">
		<br/>性别: 男:<input type="radio"name="gender"value="male"/> 女:<input type="radio"name="gender"value="female"/>
		<br/>爱好: 香蕉<input type="checkbox"name="aihao"value="xj"/>
			    辣条<input type="checkbox"name="aihao"value="lt"/>
			    茶叶蛋<input type="checkbox"name="aihao"value="cyd"/>
		<br/>
		最喜欢的老师:
		<select name="teacher">
			<option value="tong">童老师</option>
			<option value="liu">刘老师</option>
			<option value="lei">雷老师</option>
		</select>
		
		
		<input type="submit" value="登录">
	</form>
</body>
</html>
```

给大家几个小练习

```XML
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Hello world</title>
<script type="text/javascript">
//js代码
	window.onload = function(){
	var btn =document.getElementById("btnid");
	btn.onclick = function(){
		alert("hello world");
	}
	
}


</script>


</head>
<body>
	
	<button id="btnid">sayHello</button>
</body>
</html>
```

```XML
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<h3>欢迎登录</h3>
<form action="success.html" method="post">
	<table>
	 <tr>
	    <td>用户名:</td>
	    <td><input type="text" name="username" /></td>
	 </tr>
	 <tr>
	    <td>密码:</td>
	    <td><input type="password" name="pwd"/></td>
	 </tr>
	 <tr>
	   <td  colspan="2">
	   <input type ="submit" value="登录"/>
	 </tr>
	</table>
<!-- 	用户名： <br/><input type="text" name="username"/><br/>
	密码：<input type="password" name="pwd"/> <br/> -->
	



</form>
</body>
</html>
```

```XML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title> </title>
</head>
<body>
<h1>我的第一个标题</h1>
<h1>This is a heading</h1>
<h2>This is a heading</h2>
<h3>This is a heading</h3>


<p>我的第一个段落</p>
<p>This is a  paragrath.</p>
<p>This is a  paragrath.</p>




<br />

<h6>超链接</h6>
<a href="http://www.w3school.com.cn">This is a link</a>



<h6>html图像是通过<img>标签进行定义的</h6>
<img src = "谷科阳.png" width = "1100" height="946">


<br />


<!-- 图片的名称和尺寸是以属性的形式提供的 -->

<button  id="111"> 按钮 </button>
</body>
</html>


```

## HTML属性

```XML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body bgcolor="#faebd7"  onclick="alert('郭哥好帅')">
<!--元素的内容是开始标签与结束标签之间的内容
 某些html元素具有空元素
 空元素在开始标签中进行关闭
 大多数的html元素可拥有属性
 -->

<!--HTML属性
HTML标签可以拥有属性。属性提供了有关的HTML元素的更多的信息
属性总是以名称/值对的形式出现 name:"Value"
属性总是在HTML元素的开始标签中规定的
-->
<a href="http://www.w3school.com.cn">This is a link</a><br/>
<hr/>
<h1>这是一个不太明显的标题</h1>
<h1 align = "center">这是一个明显的标题</h1>    <!-- 居中排列标题 -->
<h1 align = "left">标题</h1>
<h1 align = "right">这是一个明显的标题</h1>

<p style="background: azure">This is a paragraph</p>
<hr />
<p style="height: complex">This is a paragraph</p>
<hr />
<!--  <body>定义HTML文档的主体
属性和属性值对大小写不敏感，不过万维网联盟html4推荐标准小写的属性/属性值

<hr/> 创建水平线 ，用于分隔内容
<br/> 换行



style 属性的作用：

提供了一种改变所有 HTML 元素的样式的通用方法。

标签属性：
基本属性
事件属性
-->
<p>This is a paragraph</p>
<hr />
<p>This is a paragraph</p>
</body>
</html>
```

## 表格表单

```XML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>我是一只小蠢猪</title>
</head>
<body bgcolor="aqua">
<button onclick="alert('猪')">按钮</button>


<font color="#4169e1" face="宋体" size="5">我是字体标签</font>

今天是个&nbsp&nbsp&nbsp&nbsp&nbsp好日子

<a href="day01.html" target="_blank">day01</a>
<a href="day01.html" target="_parent">day01</a>
<a href="day01.html" target="_self">day01</a>
<a href="day01.html" target="_top">day01</a>



<iframe src = "day01.1.html" width="500" height="400">biaoti</iframe>
<!--无序列表-->
<ul>
    <li>周一</li>
    <li>周二</li>
    <li>周三</li>
    <li>周四</li>
</ul>

<!--有序列表-->
<ol>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
</ol>

<table border="3" align="center" cellspacing="2">
    <th>
       <td>表头</td>
    </th>
    <tr>
        <td>111</td>
        <td>111</td>
        <td>111</td>

    </tr>
    <tr>
        <td>222</td>
        <td>222</td>
        <td>222</td>

    </tr>
    <tr>
        <td colspan="2">333</td>
        <td rowspan="2">333</td>
        <td>333</td>

    </tr>
</table>



<img  src= "谷科阳.png" width="500" height="500"/>
</body>
</html>





<!--
单标签   <br/>
双标签   <p></p>
align 属性是对齐属性

相对路径：从工程名开始算
绝对路径：从盘符开始算

colspan  跨列
rowspan  跨行

iframe框架标签


-->
```

```XML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>表单标签</title>
</head>
<body bgcolor="#ffe4c4">
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<form action="http://localhost:3306" method="post">
<h1 align="center">用户注册</h1>
<table align="center">
  <tr>
    <td>用户名称:</td>
    <td><input type="text" value="默认值"/></td>
  </tr>

  <tr>
    <td>用户密码:</td>
    <td><input type="password"value="00000000"></td>
  </tr>

  <tr>
    <td>确认密码:</td>
    <td><input type="password"value="00000000"></td>
  </tr>

  <tr>
    <td>性别:</td>
    <td><input type="radio" name="sex"/>男<input type="radio"name="sex">女</td>
  </tr>

  <tr>
    <td>兴趣爱好:</td>
    <td><input type="checkbox"checked="checked"/>java<input type="checkbox"checked="checked"/>javascript</td>
  </tr>


  <tr>
    <td>国籍:</td>
    <td>
      <select>
      <option>请选择国籍</option>
      <option>中国</option>
      <option>英国</option>
      <option>美国</option>
      <option>韩国</option>
      <option>日本</option>
     </select>
    </td>
  </tr>


  <tr>
    <td>自我评价:</td>
    <td><textarea rows="3" cols="20" >我是默认值</textarea></td>
  </tr>

  <tr>
    <td><input type="reset" value="重置"></td>
    <td><input type="submit" value="提交"></td>

  </tr>
</table>
</form>
:


<!--<form>
  用户名称:<input type="text" value="默认值"/><br/>
  用户密码:<input type="password"value="00000000"><br/>
  确认密码:<input type="password"value="00000000"><br/>
  性别:<input type="radio" name="sex"/>男<input type="radio"name="sex">女
  兴趣爱好:<input type="checkbox"checked="checked"/>java<input type="checkbox"checked="checked"/>javascript
  国籍:<select>
  <option>请选择国籍</option>
  <option>中国</option>
  <option>英国</option>
  <option>美国</option>
  <option>韩国</option>
  <option>日本</option>
</select>
  自我评价:<textarea rows="10" cols="20">我是默认值</textarea>
  <input type="reset" value="重置">
  <input type="submit" value="提交">
</form>-->

</body>
</html>


<!--
  表单标签
  表单就是html页面中，用来收集用户所有信息的所有元素集合，然后把这些信息发送给服务器

div  默认独占一行
span 它的长度时封装数据的长度
p段落标签 默认会在段落上方或下方各空出来一行


-->
```

## div span p三个标签

```XML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>其他标签</title>

</head>
<body>
    <!--
    需求  div span  p 标签的演示
    div  独占一行
    span  长度是封装数据的长度
    p     默认在段落的上方和下方有一行
    -->
<div>div标签1</div>
<div>div标签2</div>
<span>span标签1</span>
<span>span标签2</span>

<p>p段落标签1</p>
<p>p段落标签2</p>

</body>
</html>
```

## css引入

1.什么是css\?

css指层叠样式表，主要用来设置网页中元素的样式，边框，颜色，位置等。

第一种

\<style  type = text/css">

选择器\{<!-- -->

    样式名  ：样式值；

    。。。。。。。。

    。。。。。。。。

\}  

第二种 外部引入

\<link rel = "stylesheet"   type="text/css"  herf="cssd/basic.css"/>

css选择器

1.标签选择器

标签名\{<!-- -->

    样式

\}

2.绝大多数标签都有通用的class  id   style 

id选择器

#id值\{<!-- -->

    样式

\}

类选择器

. class值\{<!-- -->

    样式

\}

   

组选择器   选中的元素全部应用样式

选择器1，选择器2\{<!-- -->

    样式值

\}

css常用样式

颜色  color：red

宽度  width：19px

高度  height

背景颜色background-color

margin-left auto

margin-right auto

text-align：center    文本居中

去掉超链接的下划线   text-decoration：none；

表格细线

table\{<!-- -->

border：1px solid  black；设置边框

border-collapse：collapse 边框合并

\}

th，td\{<!-- -->

border：1px solid black；

\}

```XML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>css的开始</title>

    <!-- style标签专门用来定义css样式代码 -->


    <style type="text/css">

        #id1001{
            color: #3700ff;
            font-size: larger;

        }


    </style>

</head>

<body>
<div id="id1001">div标签1</div>
<div id="1002">div标签2</div>
<div id="1003">div标签3</div>
<div>div标签4</div>
<div>div标签5</div>

<span>标签a</span>
<span>标签b</span>
<span>标签c</span>
<span>标签d</span>
<span>标签e</span>

</body>
</html>



<!-- css技术
css 是 层叠样式表单 。 是用于增强控制网页样式并允许将样式信息与网页内容分离的一种标记性语言

把css样式写成一个单独的css文件，再通过link标签引入即可复用

标签名选择器的格式是:
    标签名{
        属性:值;
    }
id选择器的格式是:
    #id 属性值{
        属性:值;
    }
class类型选择器:
    .class名字{
        属性 : 值;
    }

组合选择器的格式是:
    选择器1，选择器2，选择器3...选择器n{
        属性 :  值;
    }


-->
```

```XML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

</body>
</html>
<!--
css的常用样式
color
border
width
height
background-color
font-size
margin-left: auto
margin-right:auto
text-align:center
text-decoration:none;

-->
```

## js的引入

1.javascript是什么？

javascript是一种直译式的脚本语言，是一种动态类型，弱类型，基于原型的语言，内置支持类型。它的解释器被称为javascript引擎，为浏览器的一部分，广泛的用于客户端脚本语言，最早在html网页上使用，用来增加动态功能。

```XML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>

    <script>
        alert("hello world");
    </script>

    <script type="text/javascript">

        var a = "12";
        var b = 12;

        alert(a == b);
        alert(a === b);


        var arr = []; //定义一个数组
        arr[0] = 12;
        arr[2] = "abc";
        alert(arr.length);

        //数组的遍历
        for(var i = 0;i < arr.length;i++){
            alert(arr[i]);
        }


        function fun(){
            alert("无参函数被调用了");
        }
        function fun1(a,b){
            alert("有参函数fun2()被调用了a=>"+a+",b=>"+b)
        }
        function  sum(num1,num2){
            var result = num1 + num2;

            return result;
        }
        alert(sum(100,50));

        var fun3 = function (a,b){
            alert("有参函数a="+a+",b="+b);
        }
        var fun4 = function (num1,num2){
            return num1 + num2;
        }

        var fun5 = function(){
            alert("无参函数");
        }


        alert (fun3(100,200));

    </script>


</head>
<body>

</body>
</html>

<!--JavaScript介绍
javascript语言诞生主要是完成页面的数据验证
js弱类型   类型可变
java是强类型 就是定义变量的时候。类型已确定。而且不可变

交互性：信息的动态交互
安全性：不允许直接访问本地硬盘
跨平台性：只要是可以解释js的浏览器都可以执行和平台无关

第一种：定义在head body 用<script></script>
第二种：src属性 导入外部文件



JavaScript的变量类型

数值类型   number
字符串类型 string
对象类型   object
布尔类型   boolean
函数类型   function

undefined  未定义
null 空值
NAN 非数字，非数值


关系运算
等于     ==   等于是简单的字面值的比较
全等于   ===  除了做字面值的比较之外，还会比较两个变量的数据类型


逻辑运算
且运算  &&
或运算 ||
取反运算 ！


数组
var arr = [];
alert(arr.length);


函数
函数定义的两种方式
第一种 可以使用function

var 函数名 = function(形参列表){函数体}


-->
```