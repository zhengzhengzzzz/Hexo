---
title: jsp-请求转发的使用说明
date: 2022-03-30 19:59:14
tags: java
categories: JavaWeb
---

<!--more-->

 ![](https://img-blog.csdnimg.cn/b66d9ac94e9043c289b82396c921809a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

 

```java
package jspStudy;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

public class SearchStudentServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获取请求的参数
        //发sql语句查询学生的信
        //使用for循环生成查询到的数据做模拟
        List<Student> studentList = new ArrayList<Student>();
        for(int i = 0;i < 10;i++){
            int t = i + 1;
            studentList.add(new Student(t,"name"+t,18+t,"phone+t"));

        }
        //保存查询到的结果（学生信息）到request域中
        req.setAttribute("stuList",studentList);


        //请求转发到tes2.jsp
        req.getRequestDispatcher("/tes2.jsp").forward(req,resp);
    }
}
```

在web.xml中配置 

![](https://img-blog.csdnimg.cn/45cb11a15da248529f694ab93272954f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)