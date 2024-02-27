---
title: js的继续学习
date: 2022-03-26 10:13:39
tags: java
categories: JavaWeb
---

<!--more-->

##  js的注意点和事件的学习

```XML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
  <script>
    function fun(){
      // alert(arguments[0]);
      // alert(arguments[1]);
      // alert(arguments[2]);



      for(var i = 0;i < arguments.length;i++){
        alert(arguments[i]);
      }

      alert("无参函数fun()")
    }

    fun(1,"ad",true);




    function sum(sum1,sum2){
      var result = 0;
      for(var i = 0;i < arguments.length;i++){
        result += arguments[i];
      }
      return result;
    }
    alert(sum(1,2,3,4,5,6,7,8,9,10));

    var obj = new Object();
    obj.name = "华仔";
    obj.age = 18;
    obj.fun = function(){
        alert("姓名:"+this.name+",年龄:"+this.age);
    }
    //对象的访问
    //变量名.属性/函数名();

    alert(obj.age);
  </script>


</head>
<body>

</body>
</html>

<!--
js不允许重载，会直接覆盖上次的函数，覆盖上次的定义

js中的自定义对象（扩展内容）
var 变量名 = new Object();
变量名.属性名 = 值;
变量名.函数名 = function(){}

{}花括号形式的自定义对象
对象的定义：
    var 变量名 = {                         //空对象
        属性名 : 值；                      //定义一个属性
        属性名 : 值；                      //定义一个属性
        函数名 : function（）{}            //定义一个函数
    }


事件  事件就是电脑输入设备与页面进行交互的响应。我们称之为事件。
常用的事件：
onload 加载完成事件         页面加载完成之后，常用于做页面js代码初始化操作
onclick 单击事件           常用于按钮的点击响应操作
onblur 失去焦点事件         常用于输入框失去焦点后验证其输入内容是否合法
onchange 内容发生改变事件    常用于下拉列表和输入框内容发生改变操作
onsubmit 表单提交事件      常用于表单提交前，验证所有表单项是否合法


事件的注册分为静态注册和动态注册两种
静态注册事件


动态注册事件
-->
```

## 静态注册和动态注册

```XML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
  <script type="text/javascript">
    //静态注册
    function onLoadFun(){
      alert('静态注册onload事件，所有代码');
    }

   //动态注册
    window.onload = function (){
      alert("动态注册的onload事件");
    }
  </script>

</head>
<!--静态注册onload事件
    onload事件是浏览器解析完页面之后就会自动的执行
<body onload="onLoadFun()">
-->
<body>

</body>
</html>
```

## 静态和动态注册的onclick事件

```XML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
  <script type="text/javascript">
    function onclickFun(){
     alert("静态注册onclick事件")
    }


    window.onload = function (){
        //获取标签对象;
       var btn = document.getElementById("1001");
        //通过对象.事件名 = function(){}
        btn.onclick = function (){
            alert("动态的事件");
        }
    }

  </script>
</head>
<body>
<!-- 静态注册onclick事件-->
<button onclick="onclickFun()">按钮1</button>
<button id="1001">按钮2</button>
</body>
</html>
```

## 静态注册失去焦点事件      动态注册onblur事件

```XML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>

<script type="text/javascript">
  //静态注册失去焦点事件
  function onblurFun(){
    //console 控制台对象，是由JavaScript语言提供，专门用来向浏览器的控制台打印输出，用于测试使用
    console.log("静态注册失去焦点事件");
  }


  //动态注册onblur事件
  window.onload = function (){
    //获取标签对象

    var btn1 = document.getElementById("10012");
    //通过标签对象.事件名 = function(){}
    btn1.onclick = function (){
      console.log("动态的");
    }
  }

  function onchangeFun(){
      alert("女神改变了");
  }


  window.onload = function () {
      var id1 = document.getElementById("123");
      id1.onchange = function () {
          alert("男神已经改变了");
      }
  }

</script>

</head>
<body>
  用户名:<input type="text" id="10012"><br/>
  密码:<input type="password" id="10013"><br/>

  请你选择你心中的女神:
<select onchange="onchangeFun()">
    <option>--女神--</option>
    <option>芳芳</option>
    <option>佳佳</option>
    <option>娘娘</option>
</select>

请你选择你心中的男神:
<select id="123">
    <option>--男神--</option>
    <option>郭哥</option>
    <option>华仔</option>
    <option>杰伦</option>
</select>
</body>
</html>
```

## 练习

```XML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
  <script type="text/javascript">
    function onclickFun(){
      var usernameObj = document.getElementById("username");

      var usernameText = usernameObj.value;

      var patt = /^\w(5,12)$/;  //正则表达式


      if(patt.test(usernameText)){
        alert("用户名合法！");
      }else{
        alert("用户名不合法!");
      }

    }
  </script>


</head>
<body>

 用户名：<input type="text" id="username" value="wzg">
 <button onclick="onclickFun()">校验</button>
</body>
</html>
```

这个正则表达式大家可以上网查一查，这里就不展开了；

```XML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
  <script type="text/javascript">

      function onclickFun(){
          var usernameObj = document.getElementById("username");

          var usernameText = usernameObj.value;

          var patt = /^\w(5,12)$/;

          var usernameSpanObj = document.getElementById("usernameSpan");

          // alert(usernameSpanObj.innerHTML);



          usernameSpanObj.innerHTML = "郭哥真可爱";


          if(patt.test(usernameText)){
              usernameSpanObj.innerHTML = "用户名合法！";
          }else{
              usernameSpanObj.innerHTML = "用户名不合法";
          }

      }
  </script>

</head>
<body>
用户名：<input type="text" id="username" value="wzg">
<span id="usernameSpan" style="color:red;"></span>
<button onclick="onclickFun()">校验</button>
</body>
</html>
```

## 如何实现 全选反选和全不选

![](https://img-blog.csdnimg.cn/dde3255f197142c593fd65127161fbbc.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_16,color_FFFFFF,t_70,g_se,x_16)

```XML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>

  <!--getElementByName-->
  <script type="text/javascript">
    function checkAll(){
      var hobbies = document.getElementsByName("hobby");
      for(var i = 0;i < hobbies.length;i++){
        hobbies[i].checked = true;
      }
    }
    
    function checkNO() {
      var hobbies = document.getElementsByName("hobby");
      for(var i = 0;i < hobbies.length;i++){
        hobbies[i].checked = false;
      }
    }
    
    function checkReverse() {
      var hobbies = document.getElementsByName("hobby");
      for(var i = 0;i < hobbies.length;i++){
        if(hobbies[i].checked){
          hobbies[i].checked = false;
        }else{
          hobbies[i].checked = true;
        }
      }
    }
  </script>
  
</head>
<body>
  兴趣爱好：
  <input type="checkbox" name="hobby"  value="java">java
  <input type="checkbox" name="hobby" value="js">JavaScript
  <br/>
  <button onclick="checkAll()">全选</button>
  <button onclick="checkNO()">全不选</button>
  <button onclick="checkReverse()">反选</button>
</body>
</html>
```