---
title: jQuery的学习
date: 2022-03-26 10:36:42
tags: java
categories: JavaWeb
---

<!--more-->

jQuery就是js和查询，是辅助js开发的js类库

核心思想:写的少，做得多，实现了很多的浏览器兼容问题
操作文档对象，选择DOM元素，制作动画效果，事件处理，使用Ajax以及其他功能

```XML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>jQuery</title>
    <script type="text/javascript" src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-1.9.0.min.js" ></script>
    <script type="text/javascript">

        $(function () {
            var $btnObj = $("#btnId");
            $btnObj.click(function () {
                alert("jQuery的点击事件");
            });
        });

        $(function (){
            $(" <div>"+"   <div><div-span1/span>"+
            "     <span>div-span2</span>"+
            "</div>").appendTo("boddy");

            var btnObj = document.getElementById("btn01");

            alert($(btnObj));


        })


    </script>
</head>
<body>
<button id="1001">按钮1</button>
<button id="btnId">SayHello</button>
</body>
</html>

<!--JQuery
  jQuery就是js和查询，是辅助js开发的js类库

  核心思想:写的少，做得多，实现了很多的浏览器兼容问题
  操作文档对象，选择DOM元素，制作动画效果，事件处理，使用Ajax以及其他功能


  常见的问题：
  1.需要引入jQuery类库吗？          是 必须的
  2.jQuery中$是什么                它是函数   核心函数
  3.怎么为按钮添加点击响应函数的？
     使用jQuery查询到标签对象
     使用标签对象.click(function(){});

1.传入的参数为函数时
在文档加载完成后执行这个函数

2.传入的参数为HTML字符串时
根据这个字符串创建元素节点对象

3.传入参数为选择器字符串时
根据字符串查找元素节点对象
$("#id属性")  id选择器，根据id查询标签对象
$("标签名")    标签名选择器，根据指定的标签名查询标签对象
$(".class属性") 类型选择器，根据class属性查询标签对象
4.传入参数为DOM对象时  会把dom对象转化为jquery对象
将DOM对象包装为jQuery对象返回



什么是jQuery对象，什么是Dom对象
    DOM对象:
    通过getElementById（）查询出来的是DOM对象
    通过getElementsByName（）查询出来的是DOM对象
    通过getElementsByTagName（）查询出来的标签对象时DOM对象
    通过createElement（）方法创建的对象，是DOM对象


    jQuery对象:
    通过jQuery提供的API创建的对象，是jQuery对象
    通过jQuery包装的对象，也是jQuery对象
    通过jQuery提供的API查询到的对象，是jQuery对象



-->
```

##  用jQuery进行查找元素

```XML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
  <script type="text/javascript" src="https://apps.bdimg.com/libs/jquery/2.1.4/jquery.min.js"></script>
  <script type="text/javascript">

      $("#myDiv");<!--查找ID为”myDiv“的元素-->

      $("div");<!--查找一个div-->

      $(".myClass");

      <!--组合选择器，找到匹配任意一个类的元素
      $("div,span,p.myClass");
      -->

      $(function () {
          $("#btn1").click(function () {
              $("#one").css("background-color","pink");
          })
      })

  </script>
</head>
<body>

<div id="notMe"><p>id="notMe"</p></div>
<div id="myDiv">id="myDiv"</div>


<input type="button" value="选择id为one的元素"  id="btn1"/>

<div class="one" id="one">
    id为one,class为one的div
    <div class = "mini">class为mini</div>
</div>
</body>
</html>

<!--
1.dom转为jQuery
先有dom
$(dom对象)就可以转换成为jQuery对象

2.jQuery对象转为dom对象
现有jQuery对象
jQuery对象[下标]取出对应的DOM对象




--.
```

## jQuery选择器

```XML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
  <script type="text/javascript" src="https://apps.bdimg.com/libs/jquery/2.1.4/jquery.min.js"></script>
  <script type="text/javascript">

    $(document).ready(function () {
      $("#a1").click(function () {
        $("p").hide();
      });
    });

   /* $(document).ready(function () {
      $("#a2").click(function () {
        $("#test").hide();
      });
    });
*/

  </script>
</head>
<body>
<p>我是第一个p</p>
<p>我是第二个p</p>
<p>我是第三个p</p>
<p>我是第四个p</p>
<p>我是第五个p</p>

<p id="test">这是另外一个段落</p>
<div><p>我是带div的p</p></div>





<button id="a1">隐藏1</button>
<button id="a2">隐藏2</button>
</body>
</html>


<!--
jQuery 选择器基于元素的 id、类、类型、属性、属性值等"查找"（或选择）HTML 元素。 它基于已经存在的 CSS 选择器，除此之外，它还有一些自定义的选择器。

jQuery 中所有选择器都以美元符号开头：$()。


id选择器
$(function () {
          $("#btn1").click(function () {
              $("#one").css("background-color","pink");
          })

class选择器
$("#btn2").click(function(){
           $(".mini").css("background-color","pink");
          })
选择元素名是div的元素
$("#btn3".click(function(){
           $("div").css("background-color","pink");

           })
})











-->
```

## class 选择器  
jQuery 类选择器可以通过指定的 class 查找元素

```XML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>.class 选择器</title>
  <!--.class 选择器
jQuery 类选择器可以通过指定的 class 查找元素。-->
  <script type="text/javascript" src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
  <script type="text/javascript">
  $(document).ready(function () {
    $("button").click(function (){
      $(".test").hide();
    })
  })


  </script>
</head>
<body>
  <h2 class="test">这是一个标题</h2>
  <p class="test">这是一个段落</p>
  <p>这是另一个段落</p>
  <button>点我</button>
</body>
</html>

<!--
:contains(text)  匹配包含给定文本的元素
例:
查找所有包含“john”的div元素
<div>John Re2sig  </div>
<div>George Martin</div>
<div>J.Ohn

$("div:contains('John')");

:empty
匹配所有不包含子元素或者文本的空元素

<table>
  <tr><td>Value 1</td><td></td></tr>
  <tr><td>Value 2</td><td></td></tr>
</table>
$("td:empty")



-->
```

## 使用jQuery显示helo

```XML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
  <script type="text/javascript" src="jQuery/jquery-1.7.2.js"></script>
  <script type="text/javascript">

    //使用$()代替window.onload
    $(function () {
      //使用选择器获取按钮对象，随后绑定单击响应函数
      $("#btnId").click(function () {
        //弹出hello
        alert("hello");

      });

    });

  </script>
</head>
<body>
  <button id="btnId">SayHello</button>
</body>
</html>
```

## jQuery的点击事件

```XML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Insert title Here</title>
  <script type="text/javascript" src="jQuery/jquery-1.7.2.js"></script>
  <script type="text/javascript">
/*   window.onload = function () {
     var btnObj = document.getElementById("btnId");
     btnObj.onclick = function () {
       alert("js的点击事件");
     }
   }*/


    $(function () {
      var $btnObj = $("#btnId");
      $btnObj.click(function () {
       alert("jQuery的点击事件");
      });
    });
  </script>





</head>
<body>
  <button id="btnId">SayHello</button>
</body>
</html>
```

## 核心函数的4个作用

//传入参数为\[函数\]时：在文档加载完成后执行这个函数  
//传入参数为\[HTML字符串\]时：根据这个字符串创建元素节点对象  
//传入参数为\[选择器字符串\]时：根据这个字符串查找元素节点对象  
//传入参数为\[DOM对象\]时：将DOM对象包装为jQuery对象返回

```XML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
  <script type="text/javascript" src="jQuery/jquery-1.7.2.js"></script>
  <script type="text/javascript">
/*    $(function () {
      alert($);
    });*/
  //核心函数的4个作用
$(function () {
  alert("页面加载完成之后，自动调用");
  $("    <div>"+
  "     <span>div-span1</span>"+
  "     </div>").appendTo("body");
  alert($("button".length))
  var btnObj = document.getElementById("btn01");
  alert(btnObj);
  alert($(btnObj));
  alert($("<h1></h1>"));
  alert($("button"));
})


//传入参数为[函数]时：在文档加载完成后执行这个函数
//传入参数为[HTML字符串]时：根据这个字符串创建元素节点对象
//传入参数为[选择器字符串]时：根据这个字符串查找元素节点对象
//传入参数为[DOM对象]时：将DOM对象包装为jQuery对象返回

  </script>

</head>
<body>
<button id="btn01">按钮1</button>
<button>按钮2</button>
<button>按钮3</button>
</body>
</html>
```

## 获取输入内容 

### 不传参数，是获取，传递参数是设置

```XML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
  <script src="jQuery/jquery-1.7.2.js"></script>
  <script type="text/javascript">

    $(function () {
      // 不传参数，是获取，传递参数是设置
      $("button").click(function () {
        alert($("#username").val());//获取
        $("#username".val("超级程序员"));//设置
      });
    });



  </script>
</head>
<body>
    <div>我是div标签<span>我是div中的span</span></div>
    <input type="text" name="username" id="username"/>
<button>操作输入框</button>
</body>
</html>
```

## 用jQuery给标签赋值

```XML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
  <script type="text/javascript" src="jQuery/jquery-1.7.2.js"></script>
  <script type="text/javascript">
    $(function () {
      // $(":radio").val(["checkbox3","checkbox2"]);
      // $(":checkbox").val(["checkbox3","checkbox2"]);
      // $("#multiple").val(["mul2","mul3","mul4"]);
      // $("#single").val(["sin2"]);

      $("#multiple, #single:radio,:checkbox").val(["radio2","checkbox1","checkbox3","mul1","mul4","sin3"]);
    });


  </script>

</head>
<body>
  单选:
  <input name="radio" type="radio" value="radio1"/>radio1
  <input name="radio" type="radio" value="radio2"/>radio2
  <br/>
  多选:
  <input name="checkbox" type="checkbox" value="checkbox1"/>checkbox1
  <input name="checkbox" type="checkbox" value="checkbox2"/>checkbox2
  <input name="checkbox" type="checkbox" value="checkbox3"/>checkbox3
  <br/>
  下拉多选:
  <select id="multiple" multiple="multiple" size="4">
      <option value="mul1">mul1</option>
      <option value="mul2">mul2</option>
      <option value="mul3">mul3</option>
      <option value="mul4">mul4</option>
  </select>
  <br/>
  下拉单选:
  <select id="single">
    <option value="sin1">sin1</option>
    <option value="sin2">sin2</option>
    <option value="sin3">sin3</option>
  </select>

</body>
</html>
```

## jQuery选择器

```XML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style type="text/css">
        div, span, p {
            width: 140px;
            height: 140px;
            margin: 5px;
            background: #aaa;
            border: #000 1px solid;
            float: left;
            font-size: 17px;
            font-family: Verdana;
        }

        div.mini {
            width: 55px;
            height: 55px;
            background-color: #aaa;
            font-size: 12px;
        }

        div.hide {
            display: none;
        }
    </style>
  <script type="text/javascript" src="jQuery/jquery-1.7.2.js"></script>
  <script type="text/javascript">

    //1.选择 id 为 one 的元素 "background-color","#bbffaa"
    $(function () {
        $("#btn1").click(function () {
            $("#one").css("background-color","#381111");
        });


    //2.选择 class 为 mini 的所有元素
        $("#btn2").click(function () {
            $(".mini").css("background-color","#b7aaff");
        });


    //3.选择 元素名是 div 的所有元素
        $("#btn3").click(function () {
           $("div").css("background-color","#bbffaa")
        });


    //4.选择所有的元素
        $("#btn4").click(function () {
           $("*").css("background-color","#ccffaa"); 
        });


    //5.选择所有的 span 元素和id为two的元素
        $("#btn5").click(function () {
            $("span,#two").css("background-color","#bbaadd")
            
        })
      });
  </script>
</head>
<body>
<!-- 	<div>
		<h1>基本选择器</h1>
	    </div>	 -->
    <input type="button" value="选择 id为 one的元素" id = "btn1"/>
    <input type="button" value="选择class为mini的所有元素" id="btn2"/>
    <input type="button" value="选择元素名是div的所有元素" id="btn3"/>
    <input type="button" value="选择所有的元素" id="btn4"/>
    <input type="button" value="选择所有的span元素和id为two的元素" id="btn5"/>
    <br/>
<div class="one" id="one">
        id为one,class 为 one的div
    <div class="mini">class 为 mini</div>
</div>

<div class="one" id="two" title="test">
    id为two,class为one,title为test的div
    <div class="mini" title="other">class为mini,title为other</div>
    <div class="mini" title="test">class为mini,title为test</div>
</div>

<div class="one">
    <div class="mini">class为mini</div>
    <div class="mini">class为mini</div>
    <div class="mini">class为mini</div>
    <div class="mini"></div>
</div>

<div class="one">
    <div class="mini">class为mini</div>
    <div class="mini">class为mini</div>
    <div class="mini">class为mini</div>
    <div class="mini" title="tesst">class为mini,title为tesst</div>
</div>

<div style="display:none;" class="none">style的display为"none"的div</div>
<div class="hide">class为"hide"的div</div>
<div>
    包含input的type为"hidden"的div<input type="hidden" size="8">
</div>
<span class="one" id="span">^^span元素^^</span>


</body>
</html>
```

```XML

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
<head>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
  <title>Untitled Document</title>
  <style type="text/css">
    div, span, p {
      width: 140px;
      height: 140px;
      margin: 5px;
      background: #aaa;
      border: #000 1px solid;
      float: left;
      font-size: 17px;
      font-family: Verdana;
    }

    div.mini {
      width: 55px;
      height: 55px;
      background-color: #aaa;
      font-size: 12px;
    }

    div.hide {
      display: none;
    }
  </style>
  <script type="text/javascript" src="jQuery/jquery-1.7.2.js"></script>
  <script type="text/javascript">
    $(document).ready(function(){
      function anmateIt(){
        $("#mover").slideToggle("slow", anmateIt);
      }
      anmateIt();
    });

    $(document).ready(function(){
      //1.选择第一个 div 元素
      $("#btn1").click(function(){
        $("div:first").css("background", "#bbffaa");
      });

      //2.选择最后一个 div 元素
      $("#btn2").click(function(){
        $("div:last").css("background", "#bbffaa");
      });

      //3.选择class不为 one 的所有 div 元素
      $("#btn3").click(function(){
        $("div:not(.one)").css("background", "#bbffaa");
      });

      //4.选择索引值为偶数的 div 元素
      $("#btn4").click(function(){
        $("div:even").css("background", "#bbffaa");
      });

      //5.选择索引值为奇数的 div 元素
      $("#btn5").click(function(){
        $("div:odd").css("background", "#bbffaa");
      });

      //6.选择索引值为大于 3 的 div 元素
      $("#btn6").click(function(){
        $("div:gt(3)").css("background", "#bbffaa");
      });

      //7.选择索引值为等于 3 的 div 元素
      $("#btn7").click(function(){
        $("divL:eq(3)").css("background", "#bbffaa");
      });

      //8.选择索引值为小于 3 的 div 元素
      $("#btn8").click(function(){
        $("div:lt(3)").css("background", "#bbffaa");
      });

      //9.选择所有的标题元素
      $("#btn9").click(function(){
        $(":header").css("background", "#bbffaa");
      });

      //10.选择当前正在执行动画的所有元素
      $("#btn10").click(function(){
        $(":animated").css("background", "#bbffaa");
      });
    });
  </script>
</head>
<body>
<!-- 	<div>
	:first
	:last
	:not(selector)
	:even
	:odd
	:eq(index)
	:gt(index)
	:lt(index)
	:header
	:animated
	</div> -->
<input type="button" value="选择第一个 div 元素" id="btn1" />
<input type="button" value="选择最后一个 div 元素" id="btn2" />
<input type="button" value="选择class不为 one 的所有 div 元素" id="btn3" />
<input type="button" value="选择索引值为偶数的 div 元素" id="btn4" />
<input type="button" value="选择索引值为奇数的 div 元素" id="btn5" />
<input type="button" value="选择索引值为大于 3 的 div 元素" id="btn6" />
<input type="button" value="选择索引值为等于 3 的 div 元素" id="btn7" />
<input type="button" value="选择索引值为小于 3 的 div 元素" id="btn8" />
<input type="button" value="选择所有的标题元素" id="btn9" />
<input type="button" value="选择当前正在执行动画的所有元素" id="btn10" />
<input type="button" value="选择没有执行动画的最后一个div" id="btn11" />

<h3>基本选择器.</h3>
<br><br>
<div class="one" id="one">
  id 为 one,class 为 one 的div
  <div class="mini">class为mini</div>
</div>
<div class="one" id="two" title="test">
  id为two,class为one,title为test的div
  <div class="mini" title="other">class为mini,title为other</div>
  <div class="mini" title="test">class为mini,title为test</div>
</div>
<div class="one">
  <div class="mini">class为mini</div>
  <div class="mini">class为mini</div>
  <div class="mini">class为mini</div>
  <div class="mini"></div>
</div>
<div class="one">
  <div class="mini">class为mini</div>
  <div class="mini">class为mini</div>
  <div class="mini">class为mini</div>
  <div class="mini" title="tesst">class为mini,title为tesst</div>
</div>
<div style="display:none;" class="none">style的display为"none"的div</div>
<div class="hide">class为"hide"的div</div>
<div>
  包含input的type为"hidden"的div<input type="hidden" size="8">
</div>
<div id="mover">正在执行动画的div元素.</div>
</body>
</html>

```

![](https://img-blog.csdnimg.cn/ee7b1b00bf474fd6b710eb569e123899.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

```XML

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
<head>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
  <title>Untitled Document</title>
  <style type="text/css">
    div, span, p {
      width: 140px;
      height: 140px;
      margin: 5px;
      background: #aaa;
      border: #000 1px solid;
      float: left;
      font-size: 17px;
      font-family: Verdana;
    }

    div.mini {
      width: 55px;
      height: 55px;
      background-color: #aaa;
      font-size: 12px;
    }

    div.hide {
      display: none;
    }
  </style>
  <script type="text/javascript" src="jQuery/jquery-1.7.2.js"></script>
  <script type="text/javascript">
    $(document).ready(function(){
      function anmateIt(){
        $("#mover").slideToggle("slow", anmateIt);
      }

      anmateIt();
    });

    /**
     :contains(text)
     :empty
     :has(selector)
     :parent
     */
    $(document).ready(function(){
      //1.选择 含有文本 'di' 的 div 元素
      $("#btn1").click(function(){
        $("div:contains('di')").css("background", "#bbffaa");
      });

      //2.选择不包含子元素(或者文本元素) 的 div 空元素
      $("#btn2").click(function(){
        $("div:empty").css("background", "#bbffaa");
      });

      //3.选择含有 class 为 mini 元素的 div 元素
      $("#btn3").click(function(){
        $("div:has(.mini)").css("background", "#bbffaa");
      });

      //4.选择含有子元素(或者文本元素)的div元素
      $("#btn4").click(function(){
        $("div:parent").css("background", "#bbffaa");
      });
    });
  </script>
</head>
<body>
<input type="button" value="选择 含有文本 'di' 的 div 元素" id="btn1" />
<input type="button" value="选择不包含子元素(或者文本元素) 的 div 空元素" id="btn2" />
<input type="button" value="选择含有 class 为 mini 元素的 div 元素" id="btn3" />
<input type="button" value="选择含有子元素(或者文本元素)的div元素" id="btn4" />

<br><br>
<div class="one" id="one">
  id 为 one,class 为 one 的div
  <div class="mini">class为mini</div>
</div>
<div class="one" id="two" title="test">
  id为two,class为one,title为test的div
  <div class="mini" title="other">class为mini,title为other</div>
  <div class="mini" title="test">class为mini,title为test</div>
</div>
<div class="one">
  <div class="mini">class为mini</div>
  <div class="mini">class为mini</div>
  <div class="mini">class为mini</div>
  <div class="mini"></div>
</div>
<div class="one">
  <div class="mini">class为mini</div>
  <div class="mini">class为mini</div>
  <div class="mini">class为mini</div>
  <div class="mini" title="tesst">class为mini,title为tesst</div>
</div>
<div style="display:none;" class="none">style的display为"none"的div</div>
<div class="hide">class为"hide"的div</div>
<div>
  包含input的type为"hidden"的div<input type="hidden" size="8">
</div>
<div id="mover">正在执行动画的div元素.</div>
</body>
</html>

```

```XML

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
<head>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
  <title>Untitled Document</title>
  <style type="text/css">
    div, span, p {
      width: 140px;
      height: 140px;
      margin: 5px;
      background: #aaa;
      border: #000 1px solid;
      float: left;
      font-size: 17px;
      font-family: Verdana;
    }

    div.mini {
      width: 55px;
      height: 55px;
      background-color: #aaa;
      font-size: 12px;
    }

    div.hide {
      display: none;
    }
  </style>
  <script type="text/javascript" src="jQuery/jquery-1.7.2.js"></script>
  <script type="text/javascript">
    $(document).ready(function(){
      function anmateIt(){
        $("#mover").slideToggle("slow", anmateIt);

    }

      anmateIt();
    });
    /**
     :hidden
     :visible
     */
    $(document).ready(function(){
      //1.选取所有可见的  div 元素
      $("#btn1").click(function(){
        $("div:visible").css("background", "#bbffaa");
      });

      //2.选择所有不可见的 div 元素
      //不可见：display属性设置为none，或visible设置为hidden
      $("#btn2").click(function(){
        $("div:hidden").show("slow").css("background", "#bbffaa");
      });

      //3.选择所有不可见的 input 元素
      $("#btn3").click(function(){
        alert($("input:hidden").attr("value"));
      });
    });
  </script>
</head>
<body>
<input type="button" value="选取所有可见的  div 元素" id="btn1">
<input type="button" value="选择所有不可见的 div 元素" id="btn2" />
<input type="button" value="选择所有不可见的 input 元素" id="btn3" />

<br>
<div class="one" id="one">
  id 为 one,class 为 one 的div
  <div class="mini">class为mini</div>
</div>
<div class="one" id="two" title="test">
  id为two,class为one,title为test的div
  <div class="mini" title="other">class为mini,title为other</div>
  <div class="mini" title="test">class为mini,title为test</div>
</div>
<div class="one">
  <div class="mini">class为mini</div>
  <div class="mini">class为mini</div>
  <div class="mini">class为mini</div>
  <div class="mini"></div>
</div>
<div class="one">
  <div class="mini">class为mini</div>
  <div class="mini">class为mini</div>
  <div class="mini">class为mini</div>
  <div class="mini" title="tesst">class为mini,title为tesst</div>
</div>
<div style="display:none;" class="none">style的display为"none"的div</div>
<div class="hide">class为"hide"的div</div>
<div>
  包含input的type为"hidden"的div<input type="hidden" value="123456789" size="8">
</div>
<div id="mover">正在执行动画的div元素.</div>
</body>
</html>

```

```XML

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
<head>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
  <title>Untitled Document</title>
  <style type="text/css">
    div,span,p {
      width: 140px;
      height: 140px;
      margin: 5px;
      background: #aaa;
      border: #000 1px solid;
      float: left;
      font-size: 17px;
      font-family: Verdana;
    }

    div.mini {
      width: 55px;
      height: 55px;
      background-color: #aaa;
      font-size: 12px;
    }

    div.hide {
      display: none;
    }
  </style>
  <script type="text/javascript" src="jQuery/jquery-1.7.2.js"></script>
  <script type="text/javascript">
    /**
     [attribute]
     [attribute=value]
     [attribute!=value]
     [attribute^=value]
     [attribute$=value]
     [attribute*=value]
     [attrSel1][attrSel2][attrSelN]


     */
    $(function() {
      //1.选取含有 属性title 的div元素
      $("#btn1").click(function() {
        $("div[title]").css("background", "#bbffaa");
      });
      //2.选取 属性title值等于'test'的div元素
      $("#btn2").click(function() {
        $("div[title='test']").css("background", "#bbffaa");
      });
      //3.选取 属性title值不等于'test'的div元素(*没有属性title的也将被选中)
      $("#btn3").click(function() {
        $("div[title!='test']").css("background", "#bbffaa");
      });
      //4.选取 属性title值 以'te'开始 的div元素
      $("#btn4").click(function() {
        $("div[title^='te']").css("background", "#bbffaa");
      });
      //5.选取 属性title值 以'est'结束 的div元素
      $("#btn5").click(function() {
        $("div[title$='est']").css("background", "#bbffaa");
      });
      //6.选取 属性title值 含有'es'的div元素
      $("#btn6").click(function() {
        $("div[title*='es']").css("background", "#bbffaa");
      });

      //7.首先选取有属性id的div元素，然后在结果中 选取属性title值 含有'es'的 div 元素
      $("#btn7").click(function() {
        $("div[id][title*='es']").css("background", "#bbffaa");
      });
      //8.选取 含有 title 属性值, 且title 属性值不等于 test 的 div 元素
      $("#btn8").click(function() {
        $("div[title][title!='test']").css("background", "#bbffaa");
      });
    });
  </script>
</head>
<body>
<input type="button" value="选取含有 属性title 的div元素." id="btn1" />
<input type="button" value="选取 属性title值等于'test'的div元素." id="btn2" />
<input type="button"
       value="选取 属性title值不等于'test'的div元素(没有属性title的也将被选中)." id="btn3" />
<input type="button" value="选取 属性title值 以'te'开始 的div元素." id="btn4" />
<input type="button" value="选取 属性title值 以'est'结束 的div元素." id="btn5" />
<input type="button" value="选取 属性title值 含有'es'的div元素." id="btn6" />
<input type="button"
       value="组合属性选择器,首先选取有属性id的div元素，然后在结果中 选取属性title值 含有'es'的 div 元素."
       id="btn7" />
<input type="button"
       value="选取 含有 title 属性值, 且title 属性值不等于 test 的 div 元素." id="btn8" />

<br>
<br>
<div class="one" id="one">
  id 为 one,class 为 one 的div
  <div class="mini">class为mini</div>
</div>
<div class="one" id="two" title="test">
  id为two,class为one,title为test的div
  <div class="mini" title="other">class为mini,title为other</div>
  <div class="mini" title="test">class为mini,title为test</div>
</div>
<div class="one">
  <div class="mini">class为mini</div>
  <div class="mini">class为mini</div>
  <div class="mini">class为mini</div>
  <div class="mini"></div>
</div>
<div class="one">
  <div class="mini">class为mini</div>
  <div class="mini">class为mini</div>
  <div class="mini">class为mini</div>
  <div class="mini" title="tesst">class为mini,title为tesst</div>
</div>
<div style="display: none;" class="none">style的display为"none"的div</div>
<div class="hide">class为"hide"的div</div>
<div>
  包含input的type为"hidden"的div<input type="hidden" value="123456789"
                                  size="8">
</div>
<div id="mover">正在执行动画的div元素.</div>
</body>
</html>

```

## 表单对象属性过滤选择器

```XML

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
<head>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
  <title>Untitled Document</title>
  <script type="text/javascript" src="jQuery/jquery-1.7.2.js"></script>
  <script type="text/javascript">
    $(function(){


      /**
       :input
       :text
       :password
       :radio
       :checkbox
       :submit
       :image
       :reset
       :button
       :file
       :hidden

       表单对象的属性
       :enabled
       :disabled
       :checked
       :selected
       */


      //1.对表单内 可用input 赋值操作
      $("#btn1").click(function(){
        $(":input").val("New Value");
      });
      //2.对表单内 不可用input 赋值操作
      $("#btn2").click(function(){
        $(":text:disabled").val("New Value Too");
      });
      //3.获取多选框选中的个数  使用size()方法获取选取到的元素集合的元素个数
      $("#btn3").click(function(){
        alert($(":checkbox[name='newsletter']:checked").size())
      });
      //4.获取多选框，每个选中的value值
      $("#btn4").click(function(){
        $(":checkbox[name='newsletter']:checked").each(function(){
          alert(this.value);
        });
      });

      //5.获取下拉框选中的内容
      $("#btn5").click(function(){
        //因为关键是看子元素有没有被选中，所有加空格
        $("select :selected").each(function(){
          alert(this.value);
        });
      });

    })
  </script>
</head>
<body>
<h3>表单对象属性过滤选择器</h3>
<button id="btn1">对表单内 可用input 赋值操作.</button>
<button id="btn2">对表单内 不可用input 赋值操作.</button><br /><br />
<button id="btn3">获取多选框选中的个数.</button>
<button id="btn4">获取多选框选中的内容.</button><br /><br />
<button id="btn5">获取下拉框选中的内容.</button><br /><br />

<form id="form1" action="#">
  可用元素: <input name="add" value="可用文本框1"/><br>
  不可用元素: <input name="email" disabled="disabled" value="不可用文本框"/><br>
  可用元素: <input name="che" value="可用文本框2"/><br>
  不可用元素: <input name="name" disabled="disabled" value="不可用文本框"/><br>
  <br>

  多选框: <br>
  <input type="checkbox" name="newsletter" checked="checked" value="test1" />test1
  <input type="checkbox" name="newsletter" value="test2" />test2
  <input type="checkbox" name="newsletter" value="test3" />test3
  <input type="checkbox" name="newsletter" checked="checked" value="test4" />test4
  <input type="checkbox" name="newsletter" value="test5" />test5

  <br><br>
  下拉列表1: <br>
  <select name="test" multiple="multiple" style="height: 100px" id="sele1">
    <option>浙江</option>
    <option selected="selected">辽宁</option>
    <option>北京</option>
    <option selected="selected">天津</option>
    <option>广州</option>
    <option>湖北</option>
  </select>

  <br><br>
  下拉列表2: <br>
  <select name="test2">
    <option>浙江</option>
    <option>辽宁</option>
    <option selected="selected">北京</option>
    <option>天津</option>
    <option>广州</option>
    <option>湖北</option>
  </select>
</form>
</body>
</html>

```

## 综合小练习 

```XML

<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
  <title>Insert title here</title>
  <script type="text/javascript" src="jQuery/jquery-1.7.2.js"></script>
  <script type="text/javascript">

    $(function(){
      var $items = $(":checkbox[name=items]");
      var items = $("[name='items']");
      //全选按钮
      $("#checkedAllBtn").click(function(){
        items.attr("checked",true);
        $("#checkedAllBox").attr("checked",true);
      });
      //全不选按钮
      $("#checkedNoBtn").click(function(){
        items.attr("checked",false);
        $("#checkedAllBox").attr("checked",false);
      });

      //反选按钮
      $("#checkedRevBtn").click(function(){
        items.each(function(){
          this.checked = !this.checked;
        });
        var flag = $("[name='items']:checked").length==4;
        $("#checkedAllBox").attr("checked",flag);
      });

      //提交按钮
      $("#sendBtn").click(function(){
        $(":checkbox[name='items']:checked").each(function(){
          alert(this.value);
        });
      });

      //全选/全不选复选框
      $("#checkedAllBox").click(function(){
        items.attr("checked",this.checked);
      });

      //全选/全不选复选框与items状态同步
      $("[name='items']").click(function(){
        var flag = $("[name='items']:checked").length==4;
        $("#checkedAllBox").attr("checked",flag);
      });
    });

  </script>
</head>
<body>

<form method="post" action="">

  你爱好的运动是？<input type="checkbox" id="checkedAllBox" />全选/全不选

  <br />
  <input type="checkbox" name="items" value="足球" />足球
  <input type="checkbox" name="items" value="篮球" />篮球
  <input type="checkbox" name="items" value="羽毛球" />羽毛球
  <input type="checkbox" name="items" value="乒乓球" />乒乓球
  <br />
  <input type="button" id="checkedAllBtn" value="全　选" />
  <input type="button" id="checkedNoBtn" value="全不选" />
  <input type="button" id="checkedRevBtn" value="反　选" />
  <input type="button" id="sendBtn" value="提　交" />
</form>

</body>
</html>
```

```XML

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
<head>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
  <title>DOM增删改</title>
  <link rel="stylesheet" type="text/css" href="style/css.css" />
  <script type="text/javascript" src="jQuery/jquery-1.7.2.js"></script>
  <script type="text/javascript">
    /**
     文档处理
     内部插入

     appendTo(content) 	   a.appendTo(b);  把a加到b里面				  添加到最后面
     prependTo(content)	   a.prependTo(b); 把a添加到b里面    			  添加到最前面

     外部插入
     insertAfter(content) 	a.insertAfter(b);  把a插入到b的后面
     insertBefore(content) 	a.insertBefore(b); 把a插入到b的前面


     替换
     replaceWith(content|fn) a.replaceWith(b)  把a用b替换
     replaceAll(selector) 	a.replaceAll(b)	  用a替换所有的b

     删除
     empty() 				a.empty()   把a掏空，把a里面的所有元素都删除
     remove([expr]) 			a.remove(b)  所有的a，是b的话就会删除	a.remove()删除a



     */
    $(function(){


      $("#btn01").click(function(){
        //创建一个"广州"节点,添加到#city下[appendTo()]
        //子节点.appendTo(父节点)



      });


      $("#btn02").click(function(){
        //创建一个"广州"节点,添加到#city下[prependTo()]


      });



      $("#btn03").click(function(){
        //将"广州"节点插入到#bj前面[insertBefore()]
        //前边.insertBefore(后边的)


      });


      $("#btn04").click(function(){
        //将"广州"节点插入到#bj后面[insertAfter()]
        //后边.insertAfter(前边的)


      });

      $("#btn05").click(function(){
        //使用"广州"节点替换#bj节点[replaceWith()]
        //被替换的.replaceWith()


      });

      $("#btn06").click(function(){
        //使用"广州"节点替换#bj节点[replaceAll()]
        //新的节点.replaceAll(旧的节点)


      });

      $("#btn07").click(function(){
        //删除#rl节点[remove()]
        //$("ul").remove("#rl");
        //$("#rl").remove();


      });

      $("#btn08").click(function(){
        //掏空#city节点[empty()]


      });

      $("#btn09").click(function(){
        //读取#city内的HTML代码

      });

      $("#btn10").click(function(){
        //设置#bj内的HTML代码


      });



    });


  </script>

</head>
<body>
<div id="total">
  <div class="inner">
    <p>
      你喜欢哪个城市?
    </p>

    <ul id="city">
      <li id="bj">北京</li>
      <li>上海</li>
      <li>东京</li>
      <li>首尔</li>
    </ul>

    <br>
    <br>

    <p>
      你喜欢哪款单机游戏?
    </p>

    <ul id="game">
      <li id="rl">红警</li>
      <li>实况</li>
      <li>极品飞车</li>
      <li>魔兽</li>
    </ul>

    <br />
    <br />

    <p>
      你手机的操作系统是?
    </p>

    <ul id="phone"><li>IOS</li><li id="android">Android</li><li>Windows Phone</li></ul>
  </div>

  <div class="inner">
    gender:
    <input type="radio" name="gender" value="male"/>
    Male
    <input type="radio" name="gender" value="female"/>
    Female
    <br>
    <br>
    name:
    <input type="text" name="name" id="username" value="abcde"/>
  </div>
</div>
<div id="btnList">
  <div><button id="btn01">创建一个"广州"节点,添加到#city下[appendTo()]</button></div>
  <div><button id="btn02">创建一个"广州"节点,添加到#city下[prependTo()]</button></div>
  <div><button id="btn03">将"广州"节点插入到#bj前面[insertBefore()]</button></div>
  <div><button id="btn04">将"广州"节点插入到#bj后面[insertAfter()]</button></div>
  <div><button id="btn05">使用"广州"节点替换#bj节点[replaceWith()]</button></div>
  <div><button id="btn06">使用"广州"节点替换#bj节点[replaceAll()]</button></div>
  <div><button id="btn07">删除#rl节点[remove()]</button></div>
  <div><button id="btn08">掏空#city节点[empty()]</button></div>
  <div><button id="btn09">读取#city内的HTML代码</button></div>
  <div><button id="btn10">设置#bj内的HTML代码</button></div>

</div>
</body>
</html>
>
```

![](https://img-blog.csdnimg.cn/c8c5a685fa5549289b26e15dd26a14a5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_16,color_FFFFFF,t_70,g_se,x_16)

如何实现上图；

```XML

<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
  <title>Insert title here</title>
  <style type="text/css">
    select {
      width: 100px;
      height: 140px;
    }

    div {
      width: 130px;
      float: left;
      text-align: center;
    }
  </style>
  <script type="text/javascript" src="jQuery/jquery-1.7.2.js"></script>
  <script type="text/javascript">
    // 页面加载完成
    $(function () {
      // 第一个按钮 【选中添加到右边】
      $("button:eq(0)").click(function () {
        $("select:eq(0) option:selected").appendTo($("select:eq(1)"));
      });

      // 第二个按钮 【全部添加到右边】
      $("button:eq(1)").click(function () {
        $("select:eq(0) option").appendTo($("select:eq(1)"));
      });

      // 第三个按钮 【选中删除到左边】
      $("button:eq(2)").click(function () {
        $("select:eq(1) option:selected").appendTo($("select:eq(0)"));
      });

      // 第四个按钮 【全部删除到左边】
      $("button:eq(3)").click(function () {
        $("select:eq(1) option").appendTo($("select:eq(0)"));
      });
    });
  </script>
</head>
<body>

<div id="left">
  <select multiple="multiple" name="sel01">
    <option value="opt01">选项1</option>
    <option value="opt02">选项2</option>
    <option value="opt03">选项3</option>
    <option value="opt04">选项4</option>
    <option value="opt05">选项5</option>
    <option value="opt06">选项6</option>
    <option value="opt07">选项7</option>
    <option value="opt08">选项8</option>
  </select>

  <button>选中添加到右边</button>
  <button>全部添加到右边</button>
</div>
<div id="rigth">
  <select multiple="multiple" name="sel02">
  </select>
  <button>选中删除到左边</button>
  <button>全部删除到左边</button>
</div>

</body>
</html>
```

 ![](https://img-blog.csdnimg.cn/ea1d3f39fa7e4de881a08bf5c1c9a5e0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_18,color_FFFFFF,t_70,g_se,x_16)

如何实现上图；

```XML

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
<head>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
  <title>Untitled Document</title>
  <link rel="stylesheet" type="text/css" href="styleB/css.css" />
  <script type="text/javascript" src="jQuery/jquery-1.7.2.js"></script>
  <script type="text/javascript">

    $(function () {

      // 创建一个用于复用的删除的function函数
      var deleteFun = function(){

        // alert("删除 操作 的function : " + this);

        // 在事件响应的function函数中，有一个this对象。这个this对象是当前正在响应事件的dom对象。
        var $trObj = $(this).parent().parent();

        var name = $trObj.find("td:first").text();

        /**
         * confirm 是JavaScript语言提供的一个确认提示框函数。你给它传什么，它就提示什么<br/>
         * 当用户点击了确定，就返回true。当用户点击了取消，就返回false
         */
        if( confirm("你确定要删除[" + name +  "]吗？") ){
          $trObj.remove();
        }

        // return false; 可以阻止 元素的默认行为。
        return false;
      };




      // 给【Submit】按钮绑定单击事件
      $("#addEmpButton").click(function () {
        // 获取输入框，姓名，邮箱，工资的内容
        var name = $("#empName").val();
        var email = $("#email").val();
        var salary = $("#salary").val();


        // 创建一个行标签对象，添加到显示数据的表格中
        var $trObj = $("<tr>" +
                "<td>" + name +  "</td>" +
                "<td>" + email + "</td>" +
                "<td>" + salary + "</td>" +
                "<td><a href=\"deleteEmp?id=002\">Delete</a></td>" +
                "</tr>");


        // 添加到显示数据的表格中
        $trObj.appendTo( $("#employeeTable") );

        // 给添加的行的a标签绑上事件
        $trObj.find("a").click( deleteFun );


      });


      // 给删除的a标签绑定单击事件
      $("a").click( deleteFun );


    });


  </script>
</head>
<body>

<table id="employeeTable">
  <tr>
    <th>Name</th>
    <th>Email</th>
    <th>Salary</th>
    <th>&nbsp;</th>
  </tr>
  <tr>
    <td>Tom</td>
    <td>tom@tom.com</td>
    <td>5000</td>
    <td><a href="deleteEmp?id=001">Delete</a></td>
  </tr>
  <tr>
    <td>Jerry</td>
    <td>jerry@sohu.com</td>
    <td>8000</td>
    <td><a href="deleteEmp?id=002">Delete</a></td>
  </tr>
  <tr>
    <td>Bob</td>
    <td>bob@tom.com</td>
    <td>10000</td>
    <td><a href="deleteEmp?id=003">Delete</a></td>
  </tr>
</table>

<div id="formDiv">

  <h4>添加新员工</h4>

  <table>
    <tr>
      <td class="word">name: </td>
      <td class="inp">
        <input type="text" name="empName" id="empName" />
      </td>
    </tr>
    <tr>
      <td class="word">email: </td>
      <td class="inp">
        <input type="text" name="email" id="email" />
      </td>
    </tr>
    <tr>
      <td class="word">salary: </td>
      <td class="inp">
        <input type="text" name="salary" id="salary" />
      </td>
    </tr>
    <tr>
      <td colspan="2" align="center">
        <button id="addEmpButton" value="abc">
          Submit
        </button>
      </td>
    </tr>
  </table>

</div>

</body>
</html>
```

##  JavaScript的复习小结

/\*
JavaScript是直译式语言，动态类型语言，弱类型语言，基于原型的语言
JavaScript开发网页行为
\<script type="text/javascript">
    //编写js代码
    window.onload = function\(\)\{
    //找到元素
    var btnEle = document.getElementById\("btn"\);
    //绑定单击事件
    btnEle.onclick = function\(\)\{
    //为单击事件赋值一个响应方法
    alert\("helloworld"\);

        \}

    \}
\</script>

\<button id = "btn">点击我\</button>

JS的基本语法

1.变量
    var 变量名;
    变量名 = 值;\(接收任何类型，也可以动态的改变值\)
2.函数
    function 方法名 \(参数\) \{方法体\}
    var 变量名 = function\(\)\{方法体\}
3.对象
    var obj = new Object\(\);
    var obj = \{\};

    对象.属性名 = 值 ;

DOM 文档对象模型
    把整个Html文档都封装成了一个对象，包括里面每一个元素
节点;
    元素节点    代表一整个元素
    属性节点    元素的属性节点
    文本节点    不同位置的文本会被封装成文本节点

节点的属性：
nodeName   标签名   属性名     #test
nodeType     1        2         3
nodeValue   null    属性值     文本内容


dom查询
document.getElementById\(\)   通过id获取元素
document.getElementByTagName\(\) 通过标签名获取元素
document.getElementsByName\(\)  通过标签的name属性值获取元素

dom的增删改
document.createElement\("标签名"\)
父节点.appendChild\(子节点\);
ele.appendChild\(childNode\)
删除子节点要找到父节点，使用removeChild\(\)方法
替换节点：
parentEle = XXXX;
newEle = XXXX;
oldEle = xxxx;
parentEle.replaceChild\(newEle,OldEle\);


    \$是 jQuery 核心函数，能完成jQuery的许多功能
    dom对象---》jquery对象   \$\(dom对象\)
    jquery对象--》dom对象  jquery对象\[0\]


\*/

```XML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
  <style type="text/css">
    div{
      width:100px;
      height:260px;
    }
    div.whiteborder{
      border: 2px white solid;
    }
    div.redDiv{
      background-color: red;

    }
    div.blueBorder{
      border: 5px blue solid;
    }
  </style>
  <script type="text/javascript" src="jQuery/jquery-1.7.2.js"></script>
  <script type="text/javascript">
    //jQuery - css
    $(function (){
      var $divEle = $('div:first');

      $('#btn01').click(function () {
        $divEle.addClass('redDiv blueBorder');
      });

      $('#btn02').click(function () {
        $divEle.removeClass('redDiv blueBorder');
      });

      $('#btn03').click(function () {
        $divEle.toggleClass('redDiv');
      });
      $('#btn04').click(function () {
        var pos = $divEle.offset();
        // console.log(pos);
        $divEle.offset({
          top:100,
          left:50
        });
      });

    });
  </script>
</head>
<body>

<table align="center">
  <tr>
    <td>
      <div class="border">
      </div>
    </td>

    <td>
      <div class="btn">
        <input type="button" value="addClass()" id="btn01"/>
        <input type="button" value="removeClass()" id="btn02"/>
        <input type="button" value="toggleClass()" id="btn03"/>
        <input type="button" value="offset()" id="btn04"/>
      </div>
    </td>
  </tr>
</table>
</body>
</html>
```

```XML
<!--

<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
  <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
  <title>品牌展示练习</title>
  <style type="text/css">
    * {
      margin: 0;
      padding: 0;
    }

    body {
      font-size: 12px;
      text-align: center;
    }

    a {
      color: #04D;
      text-decoration: none;
    }

    a:hover {
      color: #F50;
      text-decoration: underline;
    }

    .SubCategoryBox {
      width: 600px;
      margin: 0 auto;
      text-align: center;
      margin-top: 40px;
    }

    .SubCategoryBox ul {
      list-style: none;
    }

    .SubCategoryBox ul li {
      display: block;
      float: left;
      width: 200px;
      line-height: 20px;
    }

    .showmore , .showless{
      clear: both;
      text-align: center;
      padding-top: 10px;
    }

    .showmore a , .showless a{
      display: block;
      width: 120px;
      margin: 0 auto;
      line-height: 24px;
      border: 1px solid #AAA;
    }

    .showmore a span {
      padding-left: 15px;
      background: url(img/down.gif) no-repeat 0 0;
    }

    .showless a span {
      padding-left: 15px;
      background: url(img/up.gif) no-repeat 0 0;
    }

    .promoted a {
      color: #F50;
    }
  </style>
  <script type="text/javascript" src="script/jquery-1.7.2.js"></script>
  <script type="text/javascript">
    $(function() {
      // 基本初始状态
      $("li:gt(5):not(:last)").hide();

      // 给功能的按钮绑定单击事件
      $("div div a").click(function () {
        // 让某些品牌，显示，或隐藏
        $("li:gt(5):not(:last)").toggle();
        // 判断 品牌，当前是否可见
        if( $("li:gt(5):not(:last)").is(":hidden") ){
          // 品牌隐藏的状态 ：1 显示全部品牌    == 角标向下 showmore
          $("div div a span").text("显示全部品牌");

          $("div div").removeClass();
          $("div div").addClass("showmore");

          // 去掉高亮
          $("li:contains('索尼')").removeClass("promoted");

        } else {
          // 品牌可见的状态：2 显示精简品牌	 == 角标向上 showless
          $("div div a span").text("显示精简品牌");

          $("div div").removeClass();
          $("div div").addClass("showless");

          // 加高亮
          $("li:contains('索尼')").addClass("promoted");
        }

        return false;
      });
    });
  </script>
</head>
<body>
<div class="SubCategoryBox">
  <ul>
    <li><a href="#">佳能</a><i>(30440) </i></li>
    <li><a href="#">索尼</a><i>(27220) </i></li>
    <li><a href="#">三星</a><i>(20808) </i></li>
    <li><a href="#">尼康</a><i>(17821) </i></li>
    <li><a href="#">松下</a><i>(12289) </i></li>
    <li><a href="#">卡西欧</a><i>(8242) </i></li>
    <li><a href="#">富士</a><i>(14894) </i></li>
    <li><a href="#">柯达</a><i>(9520) </i></li>
    <li><a href="#">宾得</a><i>(2195) </i></li>
    <li><a href="#">理光</a><i>(4114) </i></li>
    <li><a href="#">奥林巴斯</a><i>(12205) </i></li>
    <li><a href="#">明基</a><i>(1466) </i></li>
    <li><a href="#">爱国者</a><i>(3091) </i></li>
    <li><a href="#">其它品牌相机</a><i>(7275) </i></li>
  </ul>
  <div class="showmore">
    <a href="more.html"><span>显示全部品牌</span></a>
  </div>
</div>
</body>
</html>

-->
```

##  往后还要学习的内容

![](https://img-blog.csdnimg.cn/6afc3d99a12b43ee819d727f58bafab5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)