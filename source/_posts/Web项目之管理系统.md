---
title: Web项目之管理系统
date: 2022-05-08 14:43:15
tags: 前端 tomcat java
categories: JavaWeb
---

<!--more-->

# **一、项目目的**

**1.**掌握Web前端工程开发的基本流程；

2.掌握Web前端工程开发的基本流程；

# **二、项目内容**

使用html，jsp，servlet，el表达式，tomcat等javaweb学习过程中所用到的知识。

然后做一个人员的管理系统，实现登录注册，增删改查的功能。

# **三、项目过程**

本项目涉及到使用数据库的使用，没学数据库的一定先学完数据库。

我们想一想做一个项目首先需要什么呢？

![](https://img-blog.csdnimg.cn/1b3da22aef854b51b61f18312bc08a9e.png)

 那肯定是先想好你要做什么项目。就比如我想做一个管理信息的项目。

那第二步呢？你首先要需要一个页面对吧，不管是静态的还是动态的，就像当初我们学js的时候，我们需要一个按钮，然后给它绑定单击事件，然后他才能实现一定的功能。

所以呢，我们需要先想好自己的页面，和页面之间的跳转，比如我先有个登录界面，然后嘞我要跳转到那个界面，一定要衔接的上。

![](https://img-blog.csdnimg.cn/04bd878927f14566bc37af467bff1348.png)

 第三步，我们想，我们点击登录它实现一定的功能，那我数据到哪里了呢那肯定是数据库了。我们要把数据存到数据库才能方便调用。

![](https://img-blog.csdnimg.cn/4f87d21e64e44f009a1be5b3b8f737ac.png)

 下面是创建几个表格，只要是需要存储数据的，就建表就对了。

用于算法信息的界面 

```

CREATE TABLE `suanfa` (
  `title` varchar(100) NOT NULL COMMENT '标题',
  `acurl` varchar(100) NOT NULL COMMENT '活动地址',
  `pingtai` varchar(100) DEFAULT NULL COMMENT '平台',
  `yaoqingma` varchar(100) DEFAULT NULL COMMENT '邀请码',
  `jieshushijian` varchar(100) DEFAULT NULL COMMENT '结束时间',
  `zhuangtai` varchar(100) DEFAULT NULL COMMENT '状态',
  `cankaodaan` varchar(100) DEFAULT NULL COMMENT '参考答案',
  `id` int(40) NOT NULL AUTO_INCREMENT,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=9 DEFAULT CHARSET=utf8
```

 用于人员的信息管理

```
CREATE TABLE `t_manage` (
  `id` int(20) NOT NULL AUTO_INCREMENT COMMENT '编号',
  `name` varchar(20) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NOT NULL COMMENT '姓名',
  `banji` varchar(20) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci DEFAULT NULL COMMENT '班级',
  `phonenumber` varchar(100) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci DEFAULT NULL COMMENT '电话',
  `email` varchar(20) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci DEFAULT NULL COMMENT '邮箱',
  `fuzeren` varchar(20) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci DEFAULT NULL COMMENT '负责人',
  `fangxiang` varchar(20) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci DEFAULT NULL COMMENT '方向',
  `bokeurl` varchar(400) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci DEFAULT NULL COMMENT '博客地址',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=23 DEFAULT CHARSET=utf8
```

 用于注册登录界面

```
CREATE TABLE `t_user` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `username` varchar(20) NOT NULL,
  `password` varchar(20) NOT NULL,
  `email` varchar(100) DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `username` (`username`)
) ENGINE=InnoDB AUTO_INCREMENT=45 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci
```

![](https://img-blog.csdnimg.cn/7ce0532500af442a901a95868e19ac96.png)

 我悄悄的打开了idea，就是这个东东

![](https://img-blog.csdnimg.cn/d9e1f273fd38420680763956a6217a15.png)

我就在那敲啊敲啊，然后就是乱码和报错，啊啊我真的是，一个bug好长时间都没解决

![](https://img-blog.csdnimg.cn/b43e98971db7402694358f19285ea5b9.png)![](https://img-blog.csdnimg.cn/5d1f8a85210247729e3e8f5235ee7cbd.png)

言归正传，改学习了，

建一个工程，在工程中建一个模块

![](https://img-blog.csdnimg.cn/2f6ba23f8f70426cb77b3aee707e05c3.png)

 具体的操作，csdn有，大家可以去搜搜

先给大家看看我的目录

不要在意是书还是人，俺也是小白，俺需要学习和借鉴，嘿嘿嘿

![](https://img-blog.csdnimg.cn/1683aee901384ad680753f801087aa61.png)

 ![](https://img-blog.csdnimg.cn/0180d2df8e5f4169a21f507eb7102ee4.png)

因为要用到数据库，所以咱们先连接数据库

我用的阿里云的durid连接池，大家用之前先导包

![](https://img-blog.csdnimg.cn/78d5d07f064442cb963125d47ccc51ef.png)

然后把它添加到模块的library当中

![](https://img-blog.csdnimg.cn/ea56895c4bd7473896e3e9b44172ec98.png)

![](https://img-blog.csdnimg.cn/2ca11cc9cdb04c76946baf9d03f76628.png)

暂时我们用这两个jar包

编写JdbcUtils

```java
package com.gukeyang.pro.utils;

import com.alibaba.druid.pool.DruidDataSource;
import com.alibaba.druid.pool.DruidDataSourceFactory;

import java.io.InputStream;
import java.sql.Connection;
import java.sql.SQLException;
import java.util.Properties;

public class JdbcUtils {

    private static DruidDataSource dataSource;

    static {
        try {
            Properties properties = new Properties();
            // 读取 jdbc.properties属性配置文件
            InputStream inputStream = JdbcUtils.class.getClassLoader().getResourceAsStream("jdbc.properties");
            // 从流中加载数据
            properties.load(inputStream);
            // 创建 数据库连接 池
            dataSource = (DruidDataSource) DruidDataSourceFactory.createDataSource(properties);

        } catch (Exception e) {
            e.printStackTrace();
        }

    }

    /**
     * 获取数据库连接池中的连接
     * @return 如果返回null,说明获取连接失败<br/>有值就是获取连接成功
     */
    public static Connection getConnection(){

        Connection conn = null;

        try {
            conn = dataSource.getConnection();
        } catch (Exception e) {
            e.printStackTrace();
        }

        return conn;
    }

    /**
     * 关闭连接，放回数据库连接池
     * @param conn
     */
    public static void close(Connection conn){
        if (conn != null) {
            try {
                conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }

}
```

 配置文件，我放在了资源包下

连接数据库的配置文件

容易错的地方cj那个地方还有book 这个是自己的数据库名

下面的就是自己访问数据库的账号密码

```java
driverClassName=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://localhost:3306/book?useUnicode=true&characterEncoding=utf-8&serverTimezone=GMT%2B8&useSSL=false
username=root
password=123456
#最小空闲连接数
initialSize=5
#最大连接数
maxActive=10
```

 编写测试类JdbcUtilsTest，看看自己的连接是否成功

```java
package com.gukeyang.pro.test;


import com.gukeyang.pro.utils.JdbcUtils;
import org.junit.Test;

import java.sql.Connection;

public class JdbcUtilsTest {

    @Test
    public void testJdbcUtils(){
        for (int i = 0; i < 100; i++){
            Connection connection = JdbcUtils.getConnection();
            System.out.println(connection);
            JdbcUtils.close(connection);
        }
    }

}
```

 编写User（对象）

生成get和set方法，构造器和toString方法

```java
package com.gukeyang.pro.pojo;

public class User {
    private Integer id;
    private String username;
    private String password;
    private String email;

    public User() {
    }

    public User(Integer id, String username, String password, String email) {
        this.id = id;
        this.username = username;
        this.password = password;
        this.email = email;
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }


    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", password='" + password + '\'' +
                ", email='" + email + '\'' +
                '}';
    }
}

```

 然后去写方法，先编写一个UserDao的接口，然后实现它的方法

```java
package com.gukeyang.pro.dao;


import com.gukeyang.pro.pojo.User;

public interface UserDao {

    /**
     * 根据用户名查询用户信息
     * @param username 用户名
     * @return 如果返回null,说明没有这个用户。反之亦然
     */
    public User queryUserByUsername(String username);

    /**
     * 根据 用户名和密码查询用户信息
     * @param username
     * @param password
     * @return 如果返回null,说明用户名或密码错误,反之亦然
     */
    public User queryUserByUsernameAndPassword(String username, String password);

    /**
     * 保存用户信息
     * @param user
     * @return 返回-1表示操作失败，其他是sql语句影响的行数
     */
    public int saveUser(User user);

}
```

 从我们的目的看出，我们需要增删改查

所以我们把这个写成一个BaseDao的类，然后我们的实现类实现功能只需要继承它就可以了。

![](https://img-blog.csdnimg.cn/779a950cd2fd45c5b6bfcc5314cd69bd.png)

 我们用的是dbutils工具类，里面是已经封装好的增删改查的功能

我们只需要创建queryrunner对象，连接数据库，进行调用功能就可以了

```java
package com.gukeyang.pro.dao.impl;


import com.gukeyang.pro.utils.JdbcUtils;
import org.apache.commons.dbutils.QueryRunner;
import org.apache.commons.dbutils.handlers.BeanHandler;
import org.apache.commons.dbutils.handlers.BeanListHandler;
import org.apache.commons.dbutils.handlers.ScalarHandler;

import java.sql.Connection;
import java.sql.SQLException;
import java.util.List;

public abstract class BaseDao {

    //使用DbUtils操作数据库
    private QueryRunner queryRunner = new QueryRunner();

    /**
     * update() 方法用来执行：Insert\Update\Delete语句
     *
     * @return 如果返回-1,说明执行失败<br/>返回其他表示影响的行数
     */
    public int update(String sql, Object... args) {
        Connection connection = JdbcUtils.getConnection();
        try {
            return queryRunner.update(connection, sql, args);
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            JdbcUtils.close(connection);
        }
        return -1;
    }

    /**
     * 查询返回一个javaBean的sql语句
     *
     * @param type 返回的对象类型
     * @param sql  执行的sql语句
     * @param args sql对应的参数值
     * @param <T>  返回的类型的泛型
     * @return
     */
    public <T> T queryForOne(Class<T> type, String sql, Object... args) {
        Connection con = JdbcUtils.getConnection();
        try {
            return queryRunner.query(con, sql, new BeanHandler<T>(type), args);
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            JdbcUtils.close(con);
        }
        return null;
    }

    /**
     * 查询返回多个javaBean的sql语句
     *
     * @param type 返回的对象类型
     * @param sql  执行的sql语句
     * @param args sql对应的参数值
     * @param <T>  返回的类型的泛型
     * @return
     */
    public <T> List<T> queryForList(Class<T> type, String sql, Object... args) {
        Connection con = JdbcUtils.getConnection();
        try {
            return queryRunner.query(con, sql, new BeanListHandler<T>(type), args);
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            JdbcUtils.close(con);
        }
        return null;
    }

    /**
     * 执行返回一行一列的sql语句
     * @param sql   执行的sql语句
     * @param args  sql对应的参数值
     * @return
     */
    public Object queryForSingleValue(String sql, Object... args){

        Connection conn = JdbcUtils.getConnection();

        try {
            return queryRunner.query(conn, sql, new ScalarHandler(), args);
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            JdbcUtils.close(conn);
        }
        return null;

    }

}
```

之后我们开始写实现类UserDaoImpl

```java
package com.gukeyang.pro.dao.impl;

import com.gukeyang.pro.dao.UserDao;
import com.gukeyang.pro.pojo.User;

public class UserDaoImpl extends BaseDao implements UserDao {
    @Override
    public User queryUserByUsername(String username) {
        String sql = "select `id`,`username`,`password`,`email`from t_user where username = ?";
        return queryForOne(User.class, sql, username);
    }

    @Override
    public User queryUserByUsernameAndPassword(String username, String password) {
        String sql = "select `id`,`username`,`password`,`email` from t_user where username = ? and password = ?";
        return queryForOne(User.class, sql, username,password);
    }

    @Override
    public int saveUser(User user) {
        String sql = "insert into t_user(`username`,`password`,`email`) values(?,?,?)";
        return update(sql, user.getUsername(),user.getPassword(),user.getEmail());
    }

}
```

大家注意写的时候sql语句不要写错了，

写好之后我们需要对Dao持久层进行测试UserDaoTest

```java
package com.gukeyang.pro.test;


import com.gukeyang.pro.dao.UserDao;
import com.gukeyang.pro.dao.impl.UserDaoImpl;
import com.gukeyang.pro.pojo.User;
import org.junit.Test;


public class UserDaoTest {

    UserDao userDao = new UserDaoImpl();

    @Test
    public void queryUserByUsername() {

        if (userDao.queryUserByUsername("admin") == null ){
            System.out.println("用户名可用！");
        } else {
            System.out.println("用户名已存在！");
        }
    }

    @Test
    public void queryUserByUsernameAndPassword() {
        if ( userDao.queryUserByUsernameAndPassword("admin","admin") == null) {
            System.out.println("用户名或密码错误，登录失败");
        } else {
            System.out.println("查询成功");
        }
    }

    @Test
    public void saveUser() {
        System.out.println( userDao.saveUser(new User(null,"wzg16802", "123456", "wzg168@qq.com")) );
    }
}
```

如果成功运行，大家就可以进行下一步

这里做一个补充，要不然大家会比较迷

为什么我们有了Dao还要写Service层

![](https://img-blog.csdnimg.cn/93b7f97c17664dbab524497608447050.png)

接下来我们就开始写User的service层

实现登录和注册

```java
package com.gukeyang.pro.service;


import com.gukeyang.pro.pojo.User;

public interface UserService {
    /**
     * 注册用户
     * @param user
     */
    public void registUser(User user);

    /**
     * 登录
     * @param user
     * @return 如果返回null，说明登录失败，返回有值，是登录成功
     */
    public User login(User user);

    /**
     * 检查 用户名是否可用
     * @param username
     * @return 返回true表示用户名已存在，返回false表示用户名可用
     */
    public boolean existsUsername(String username);
}
```

实现类

```java
package com.gukeyang.pro.service.impl;


import com.gukeyang.pro.dao.UserDao;
import com.gukeyang.pro.dao.impl.UserDaoImpl;
import com.gukeyang.pro.pojo.User;
import com.gukeyang.pro.service.UserService;

public class UserServiceImpl implements UserService {

    private UserDao userDao = new UserDaoImpl();

    @Override
    public void registUser(User user) {
        userDao.saveUser(user);
    }

    @Override
    public User login(User user) {
        /**
         * 咱们的登录，只是需要查询数据库即可！！！
         *
         * QQ登录 ！！！
         * 1、查询用户名和密码
         * 2、查询天气信息
         * 3、查询QQ邮箱几封未读邮件
         * 4、查询QQ空间留言数
         * 5、还要查询QQ游戏 有没有什么更新
         * 6、查询QQ的黄钻，什么什么钻，的皮肤信息。
         * 7、还要查询个个好友的登录IP和登录位置信息
         * 8、发QQ好友登录提示音
         */
        return userDao.queryUserByUsernameAndPassword(user.getUsername(), user.getPassword());
    }

    @Override
    public boolean existsUsername(String username) {

        if (userDao.queryUserByUsername(username) == null) {
           // 等于null,说明没查到，没查到表示可用
           return false;
        }

        return true;

    }
}
```

测试

```java
package com.gukeyang.pro.test;


import com.gukeyang.pro.pojo.User;
import com.gukeyang.pro.service.UserService;
import com.gukeyang.pro.service.impl.UserServiceImpl;
import org.junit.Test;

import static org.junit.Assert.*;

public class UserServiceTest {

    UserService userService = new UserServiceImpl();

    @Test
    public void registUser() {
        userService.registUser(new User(null, "bbj168", "666666", "bbj168@qq.com"));
        userService.registUser(new User(null, "abc168", "666666", "abc168@qq.com"));
        userService.registUser(new User(null, "wzg168", "123456", "111@qq.com"));
    }

    @Test
    public void login() {
        System.out.println( userService.login(new User(null, "wzg168", "123456", null)) );
    }
/*
    User{id=4, username='wzg168', password='123456', email='111@qq.com'}
*/
    @Test
    public void existsUsername() {
        if (userService.existsUsername("wzg16888")) {
            System.out.println("用户名已存在！");
        } else {
            System.out.println("用户名可用！");
        }
    }
}
```

然后就是tomcat，我们要写servlet程序，让页面跳转到servlet程序，执行调用相应的功能，然后返回结果。也就是我们在页面上看到的结果。

![](https://img-blog.csdnimg.cn/e561d29e936846c8a92ed7d450994e8a.png)

 首先应该有这个，然后我们写servlet程序

页面的请求，到ser

vlet程序中，我们为了防止乱码，先设置字符集，然后通过反射找到相应的我们写的方法

我们首先写一个BaseServlet

```java
package com.gukeyang.pro.web;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.lang.reflect.Method;

public abstract class BaseServlet extends HttpServlet {

    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        request.setCharacterEncoding("UTF-8");
        response.setContentType("text/html; charset=UTF-8");
        //获取隐藏表单项的action的值
        String action = request.getParameter("action");

        //利用反射获取相应的用户行为，避免使用大量的if else
        //我们只需要在下面编写相应的方法即可，修改密码，绑定邮箱，绑定手机号等业务
        try {
            //获取action鉴别对象，获取相应的业务
            Method method = this.getClass().getDeclaredMethod(action,HttpServletRequest.class,HttpServletResponse.class);
            //调用目标业务
            method.invoke(this,request,response );

        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    //因为图书管理板块的提交方式是Get请求，所以我们让doGet执行doPost相同的操作
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doPost(request,response);
    }
}
```

然后写我们的UserServlet 让其继承于BaseServlet

这个程序要求我们要熟练应用httpservlet的dopost和doget方法，灵活使用请求转发和请求重定向，还有我们的域对象，我们把数据存到域对象，然后在页面中调用域中的数据。

```java
package com.gukeyang.pro.web;



import com.gukeyang.pro.pojo.User;
import com.gukeyang.pro.service.UserService;
import com.gukeyang.pro.service.impl.UserServiceImpl;
import com.gukeyang.pro.utils.WebUtils;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.Map;

public class UserServlet extends BaseServlet {

    private UserService userService = new UserServiceImpl();

//    @Override
//    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
//        String action = req.getParameter("action");
//        //判断是哪个表单提交了
//        if ("regist".equals(action)){
//            regist(req,resp);
//        }else if("login".equals(action)){
//            login(req,resp);
//        }
//    }
//
//    @Override
//    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
//
//    }


    /**
     * 处理登录的功能
     *
     * @param req
     * @param resp
     * @throws ServletException
     * @throws IOException
     */
    protected void login(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        //  1、获取请求的参数
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        // 调用 userService.login()登录处理业务
        User loginUser = userService.login(new User(null, username, password, null));
        // 如果等于null,说明登录 失败!
        if (loginUser == null) {
            // 把错误信息，和回显的表单项信息，保存到Request域中
            req.setAttribute("msg", "用户或密码错误！");
            req.setAttribute("username", username);
            //   跳回登录页面
            req.getRequestDispatcher("pages/user/login.jsp").forward(req, resp);
        }else  if(username.equalsIgnoreCase("admin")){
            req.getRequestDispatcher("pages/user/login_success.jsp").forward(req, resp);
        }else{
            // 登录 成功
            //跳到成功页面login_success.jsp
            req.getRequestDispatcher("pages/user/login_success_user.jsp").forward(req, resp);
        }

    }

    /**
     * 处理注册的功能
     *
     * @param req
     * @param resp
     * @throws ServletException
     * @throws IOException
     */
    protected void regist(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        //  1、获取请求的参数
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        String email = req.getParameter("email");
        String code = req.getParameter("code");


        //反射
//        User user = new User();
//        WebUtils.copyParamToBean((Map) req,user);


         if ("6n6np".equalsIgnoreCase(code)||"123456".equalsIgnoreCase(code)) {
//        3、检查 用户名是否可用
            if (userService.existsUsername(username)) {
                System.out.println("用户名[" + username + "]已存在!");

                // 把回显信息，保存到Request域中
                req.setAttribute("msg", "用户名已存在！！");
                req.setAttribute("username", username);
                req.setAttribute("email", email);

//        跳回注册页面
                req.getRequestDispatcher("pages/user/regist.jsp").forward(req, resp);
            } else {
                //      可用
//                调用Sservice保存到数据库
                userService.registUser(new User(null, username, password, email));
//
//        跳到注册成功页面 regist_success.jsp
                req.getRequestDispatcher("pages/user/regist_success.jsp").forward(req, resp);
            }
        } else {
            // 把回显信息，保存到Request域中
            req.setAttribute("msg", "验证码错误！！");
            req.setAttribute("username", username);
            req.setAttribute("email", email);

            System.out.println("验证码[" + code + "]错误");
            req.getRequestDispatcher("pages/user/regist.jsp").forward(req, resp);
        }
    }


}
```

然后需要去配置一下，找到web.xml

```java
    <servlet>
        <servlet-name>userServlet</servlet-name>
        <servlet-class>com.gukeyang.pro.web.UserServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>userServlet</servlet-name>
        <url-pattern>/userServlet</url-pattern>
    </servlet-mapping>
```

![](https://img-blog.csdnimg.cn/26b70b7051384280b2b15aeb26e68fe6.png)

 在页面登录的表单中，设置隐藏域和action的值，然后跳转到相应的servlet程序，执行相应的功能。

剩下的两个People和Suanfa和这个思路一样

就是一些细节不一样

![](https://img-blog.csdnimg.cn/146dca10dc1445cc946de30edd3c1a46.png)

![](https://img-blog.csdnimg.cn/2e098f80210946b3a77d21c95aaacaa8.png)

 这就是刚说的把查询到的map对象保存到域中，然后调出，显示到页面

![](https://img-blog.csdnimg.cn/e2fbe19630d74b70896eaba46c925319.png)

 就像这样，把存到数据库的数据显示出来

我们接下来看看功能：增删改查，这是项目中间必不可少的功能

1.增加

我们需要一个表单，来收集信息，然后将它提交到servlet程序，通过add功能添加到数据库

2.删除

就是找到相应的数据的id删除整个javabean对象，

3.修改

我们先应该请求，把数据从数据库中回显到表单当中，然后再修改其中的值，从新提交表单完成对数据的修改

![](https://img-blog.csdnimg.cn/2d21ad99d1e24b1a90d8b7ee9cdec3ed.png)

4.查询

通过某一参数查询，姓名或者id等等，我们通过某个参数，从数据库中查找然后被返回到request域中去，我们页面再从域中把数据显示出来。

下面说一下我的用户和管理层

其实我想的是，我固定一个管理员账号，我根据是不是管理员账号，跳转不同的页面

![](https://img-blog.csdnimg.cn/89386854eb184d16956eeeeac04478e5.png)

下面是两个页面，当然用户页面要隐藏一些功能。

![](https://img-blog.csdnimg.cn/b19cd13570424c74b73006a6cc56e92d.png)

![](https://img-blog.csdnimg.cn/bfaf0a876f834913b16716a25cc43910.png)

# **四、项目结果**

![](https://img-blog.csdnimg.cn/8ff7a9db979c46ae9c0cab06a0b06682.png)

![](https://img-blog.csdnimg.cn/e3a354de3d6e496796ac0ef5a5a18d4a.png)

 ![](https://img-blog.csdnimg.cn/7a9e61fbb774455fa64645b1838ef22f.png)

目前是只写了人员管理和算法，因为剩下两个和前两个的基本思路都是一样的，所以并没有往下进行完善。

![](https://img-blog.csdnimg.cn/edb3dab81d4542cc82444fa255279a60.png)![](https://img-blog.csdnimg.cn/6f12e978aa5e42fe857338da51486218.png)     这就是项目的最终成果，基本功能都实现了，然后我还增加了上传文件和前后台，就是用户和管理员的界面和功能不一样，上面的展示的都是管理员界面，虽然我用的方法简单，但是基本实现了用户和管理员的分离。

# **五、实验心得**

1、掌握Web前端开发的基础知识；

2、掌握Web前端工程开发的基本流程；

3、掌握HTML、CSS、JavaScript；

4、掌握Tomcat的使用和常见的bug

# **六、项目完整代码**

[阿里云盘分享](https://www.aliyundrive.com/s/Prp16MtMm36 "阿里云盘分享")         提取码 ： zu73