---
title: Shiro的学习
date: 2022-10-24 11:06:28
tags: 1024程序员节
categories: java
---

<!--more-->

# Shiro的学习

## 权限管理

什么是权限管理\?

为了实现用户访问系统资源的控制,属于一个系统的安全范畴

## 范围大小

粗粒度 表示类级别, 仅考虑对象的类别,当然,在权限管理这里,是指对资源类型的权限管理

细粒度 表示实例,对象实例或者类中的子集部分,在权限管理这里,是指对具体资源的权限管理

## 什么是Shiro

官方解释:

Apache Shiro™ is a powerful and easy-to-use Java security framework that performs authentication, authorization, cryptography, and session management. With Shiro’s easy-to-understand API, you can quickly and easily secure any application – from the smallest mobile applications to the largest web and enterprise applications.

翻译为:

Apache Shiro™是一个功能强大且易于使用的Java安全框架，可执行身份验证、授权、加密和会话管理。借助Shiro易于理解的API，您可以快速，轻松地保护任何应用程序 \- 从最小的移动应用程序到最大的Web和企业应用程序。

实际上，它实现了管理应用程序安全性的所有方面，同时尽可能地远离危险。它建立在 完善的界面驱动设计 和 面向对象原则 之上，可在您能想象到的任何地方实现自定义行为。但是对于所有内容都具有合理的默认值，它就像应用程序安全性一样“放手”。至少这是我们努力的目标。

## 原理图

![img](https://img-blog.csdnimg.cn/3ae44159f8f34c3390122d143d957e25.png?)

Authentication: 有时被称为“登录”，验证用户的行为和身份。 Authorization: 控制访问的过程，也就是控制用户能访问什么东西. Session Management: 即便是在非WEB或EJB应用程序中都能管理用户特定的会话 Cryptography: 在使用加密算法保证数据安全的同时保障可用性和简便性。

## 核心部分：

![](https://img-blog.csdnimg.cn/2ac735d648f84335bd48b74cb347a22d.png)

 

## Shiro架构设计

![img](https://img-blog.csdnimg.cn/031caf6856c844ce835f3c35885e427e.png#pic_center)

Subject 与应用程序交互的用户，可以看成user，subject记录了当前的操作用户，将用户的一些信息作为当前操作的主题

SecurityManager 管理subject,认证授权会话以及缓存管理，负责对全部的subject进行安全管理，也是Shiro的核心。通过SecurityManager可以完成对subject的认证与授权等操作，具体就是通过Authenticator进行认证，而通过Authorizer进行授权，通过SessionManager进行会话管理

Realms 充当Shiro和应用程序安全数据之间的“桥梁”或者"连接器"。也就是说，当需要与安全相关数据进行实际交互以执行身份验证（登录）和授权（访问控制）的时候，Shiro会认为应用程序配置的一个或多个Realms中查找其中的内容，SecurityManager进行安全验证需要通过Realm获取用户权限数据。如果不太好理解你可以认为是Shiro的datasource数据源，并且除了从数据源获取数据外还进行有关校验等

## Shiro认证

创建maven项目，pom.xml引入如下依赖：

\<\!-- https://mvnrepository.com/artifact/org.apache.shiro/shiro-core -->
\<dependency>
    \<groupId>org.apache.shiro\</groupId>
    \<artifactId>shiro-core\</artifactId>
    \<version>1.9.1\</version>
\</dependency>
​

基本的过程

1.首先准备一些用户身份、凭据（shiro.ini）

\[users\]
zhang=123
wang=123

## 测试代码

\@Test
public void test1\(\)\{
    //1、获取SecurityManager工厂，此处使用Ini配置文件初始化SecurityManager
    Factory\<SecurityManager> factory = new IniSecurityManagerFactory\("classpath:shiro/shiro.ini"\);
    //2、得到SecurityManager实例 并绑定给SecurityUtils
    org.apache.shiro.mgt.SecurityManager securityManager = factory.getInstance\(\);
    SecurityUtils.setSecurityManager\(securityManager\);
    //3、得到Subject及创建用户名/密码身份验证Token（即用户身份/凭证）
    Subject subject = SecurityUtils.getSubject\(\);
    UsernamePasswordToken token = new UsernamePasswordToken\("zhang", "123"\);
    try \{
        //4、登录，即身份验证
        subject.login\(token\);
    \} catch \(AuthenticationException e\) \{
        //5、身份验证失败
    \}
    System.out.println\(token\);
    //6、退出
    subject.logout\(\);
\}
​

- 首先通过 new IniSecurityManagerFactory 并指定一个 ini 配置文件来创建一个 SecurityManager 工厂；因为需要通过这个SecurityManager管理整个shiro会话

- 接着获取 SecurityManager 并绑定到 SecurityUtils，这是一个全局设置，设置一次即可；

- 通过 SecurityUtils 得到 Subject，其会自动绑定到当前线程；如果在 web 环境在请求结束时需要解除绑定；然后获取身份验证的 Token，如用户名 / 密码；

- 调用 subject.login 方法进行登录，其会自动委托给 SecurityManager.login 方法进行登录；

- 如果身份验证失败请捕获 AuthenticationException 或其子类，常见的如： DisabledAccountException（禁用的帐号）、LockedAccountException（锁定的帐号）、UnknownAccountException（错误的帐号）、ExcessiveAttemptsException（登录失败次数过多）、IncorrectCredentialsException （错误的凭证）、ExpiredCredentialsException（过期的凭证）等，具体请查看其继承关系；对于页面的错误消息展示，最好使用如 “用户名 / 密码错误” 而不是 “用户名错误”/“密码错误”，防止一些恶意用户非法扫描帐号库；

- 最后可以调用 subject.logout 退出，其会自动委托给 SecurityManager.logout 方法退出。

- 

![在这里插入图片描述](https://img-blog.csdnimg.cn/d51a344a00304f66a16cd068d67c4495.png#pic_center)

1.  用户名 / 密码硬编码在 `ini` 配置文件，以后需要改成如数据库存储，且密码需要加密存储；

2.  用户身份 `Token` 可能不仅仅是用户名 / 密码，也可能还有其他的，如登录时允许用户名 / 邮箱 / 手机号同时登录。

## 认证的实现

\@Test
public void test3\(\)\{
    // 创建安全管理器对象
    DefaultSecurityManager manager = new DefaultSecurityManager\(\);
    // 通过安全管理对象设置realm
    IniRealm iniRealm = new IniRealm\("classpath:shiro/shiro-realm.ini"\);
    manager.setRealm\(iniRealm\);
    SecurityUtils.setSecurityManager\(manager\);
    // 通过SecurityUtils获取一个Subject
    Subject subject = SecurityUtils.getSubject\(\);
​
    // 此时需要创建一个用户令牌
    UsernamePasswordToken upt = new UsernamePasswordToken\("user1", "password1"\);
​
    // 然后只需要将这个用户令牌丢到subject里面就行了
    subject.login\(upt\);  // 如果用户名和密码都ok就莫得问题
​
    // 搞完了用户退出登录,就让subject执行logout就行了
    subject.logout\(\);
    
\}
​

## 认证中的异常

CredentialsException : 登录凭证有问题
UnsupportedTokenException : Token不能被证实
AccountException : 账户有问题
IncorrectCredentialsException : 凭证错误\(也就是密码错误\)
ExpiredCredentialsException : 凭证过期\(也就是密码过期了\)
DisabledAccountException : 账户无效
LockedAccountException : 账户已被冻结
UnknownAccountException : 未知账户
ConcurrentAccessException : 账户已经存在
ExcessiveAttemptsException : 登录失败次数过多

## 自定义Realm实现认证

package config;
​
import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.AuthenticationInfo;
import org.apache.shiro.authc.AuthenticationToken;
import org.apache.shiro.authc.SimpleAuthenticationInfo;
import org.apache.shiro.authz.AuthorizationInfo;
import org.apache.shiro.realm.AuthorizingRealm;
import org.apache.shiro.subject.PrincipalCollection;
​
public class MyCustomRealm1 extends AuthorizingRealm \{
​
    // 获取身份验证信息\(认证\)
    \@Override
    protected AuthenticationInfo doGetAuthenticationInfo\(AuthenticationToken token\) throws AuthenticationException \{
        String principal = \(String\) token.getPrincipal\(\);
        System.out.println\(principal\);
        // 根据用户名验证身份信息
        if\("xjk".equals\(principal\)\)\{
            SimpleAuthenticationInfo simpleAuthenticationInfo = new SimpleAuthenticationInfo\(principal,"password",this.getName\(\)\);
            return simpleAuthenticationInfo;
        \}
        return null;
    \}
    
    // 获取授权信息
    \@Override
    protected AuthorizationInfo doGetAuthorizationInfo\(PrincipalCollection principals\) \{
        return null;
    \}
\}
​

\@Test
public void test4\(\)\{
​
    DefaultSecurityManager manager = new DefaultSecurityManager\(\);
    // 通过安全管理对象设置自定义realm
    manager.setRealm\(new MyCustomRealm1\(\)\);
    // 设置安全工具类
    SecurityUtils.setSecurityManager\(manager\);
    // 通过SecurityUtils获取一个Subject
    Subject subject = SecurityUtils.getSubject\(\);
​
    // 此时需要创建一个用户令牌
    UsernamePasswordToken upt = new UsernamePasswordToken\("xjk", "password"\);
    try \{
        // 然后只需要将这个用户令牌丢到subject里面就行了
        subject.login\(upt\);  // 如果用户名和密码都ok就莫得问题
        System.out.println\("是否认证通过\?==>"+subject.isAuthenticated\(\)\);
    \} catch \(UnknownAccountException e\) \{
        e.printStackTrace\(\);
        System.out.println\("用户名错误"\);
    \} catch \(IncorrectCredentialsException e\) \{
        e.printStackTrace\(\);
        System.out.println\("密码错误"\);
    \}
​
    // 搞完了用户退出登录,就让subject执行logout就行了
    subject.logout\(\);
\}
​

## shiro加密例子

\@Test
public void test1\(\)\{
    // Sha256加密
    Sha256Hash sha256Hash = new Sha256Hash\("Shiro的学习\~"\);
    System.out.println\(sha256Hash.toHex\(\)\);
​
    // Sha256加密+盐
    Sha256Hash ss = new Sha256Hash\("Shiro的学习\~","盐"\);
    System.out.println\(ss.toHex\(\)\);
​
    // 在第二个的基础上,再加一个整数参与加密
    Sha256Hash sha256hashAndSH = new Sha256Hash\("Shiro的学习\~","盐","散列".hashCode\(\)\);
    System.out.println\(sha256hashAndSH.toHex\(\)\);
​
    // 纯Md5Hash加密
    Md5Hash md5Hash = new Md5Hash\("Shiro的学习\~"\);
    System.out.println\(md5Hash.toHex\(\)\);
​
    // 纯Md5Hash加密+盐
    Md5Hash md5HashAndSalt = new Md5Hash\("Shiro的学习\~","盐"\);
    System.out.println\(md5HashAndSalt.toHex\(\)\);
​
    // 在第二个的基础上,再加一个整数参与加密  MD5加密
    Md5Hash md5HSH = new Md5Hash\("Shiro的学习\~","盐","散列".hashCode\(\)\);
    System.out.println\(md5HSH.toHex\(\)\);
​
\}
​

## Shiro中自定义加密Realm

public class MyCustomMD5Realm extends AuthorizingRealm \{
    \@Override
    protected AuthenticationInfo doGetAuthenticationInfo\(AuthenticationToken token\) throws AuthenticationException \{
		
        String principal = \(String\) token.getPrincipal\(\);
        System.out.println\(principal\);
        String correctPassword="root";
        String correctUsername="xjk";
        String salt = "1qaz2wsx";
        int hashIterations=1024;
        if\(correctUsername.equals\(principal\)\)\{
            return new SimpleAuthenticationInfo\(principal,
                   	// 正确的密码按照指定的要求进行加密生成对应的MD5字符串
                    new Md5Hash\(correctPassword,salt,hashIterations\).toHex\(\),
                    // 通过对应的盐来判断是否一致
                    ByteSource.Util.bytes\(salt\),
                    this.getName\(\)\);
        \}
        return null;
    \}

    \@Override
    protected AuthorizationInfo doGetAuthorizationInfo\(PrincipalCollection principals\) \{
        return null;
    \}
\}

测试代码

\@Test
public void test2\(\)\{
    // 创建SecurityManager
    DefaultSecurityManager defaultSecurityManager = new DefaultSecurityManager\(\);
    // 设置自定义realm
    MyCustomMD5Realm realm = new MyCustomMD5Realm\(\);
    // 为realm设置匹配器
    HashedCredentialsMatcher credentialsMatcher = new HashedCredentialsMatcher\(\);
    // 选择加密算法
    credentialsMatcher.setHashAlgorithmName\("md5"\);
    // 设置hash次数
    credentialsMatcher.setHashIterations\(1024\);
    realm.setCredentialsMatcher\(credentialsMatcher\);
    defaultSecurityManager.setRealm\(realm\);
    // 设置安全工具类
    SecurityUtils.setSecurityManager\(defaultSecurityManager\);
    // 通过安全工具类获取subject
    Subject subject = SecurityUtils.getSubject\(\);
    // 创建token
    UsernamePasswordToken token = new UsernamePasswordToken\("xjk", "root"\);
    try \{
        // 登录认证
        subject.login\(token\);
        System.out.println\("认证成功"\);
    \} catch \(UnknownAccountException e\) \{
        e.printStackTrace\(\);
        System.out.println\("用户名错误"\);
    \} catch \(IncorrectCredentialsException e\) \{
        e.printStackTrace\(\);
        System.out.println\("密码错误"\);
    \}
    subject.logout\(\);
\}

## Shiro授权

授权就是:用户需要访问系统资源,这个资源有一定的权限,只有满足权限的用户才能访问这个资源,用户对哪些资源都有哪些操作权限

### 通过写 if/else 授权代码块完成

Subject subject = SecurityUtils.getSubject\(\);
if\(subject.hasRole\(“admin”\)\) \{
    //有权限
\} else \{
    //无权限
\}

### 通过角色检查

//get the current Subject
Subject currentUser = SecurityUtils.getSubject\(\);

if \(currentUser.hasRole\("administrator"\)\) \{
    //show a special button
\} else \{
    //don’t show the button
\}

### 通过许可检查

Subject currentUser = SecurityUtils.getSubject\(\);

Permission printPermission = new PrinterPermission\("laserjet3000n","print"\); // 这是自定义的Permission

If \(currentUser.isPermitted\(printPermission\)\) \{
    //do one thing \(show the print button
\} else \{
    //don’t show the button\?
\}

### 通过基于String的许可检查

Subject currentUser = SecurityUtils.getSubject\(\);
String perm = "printer:print:laserjet4400n";
if\(currentUser.isPermitted\(perm\)\)\{
    //show the print button
\} else \{
    //don’t show the button
\}

在shiro:haspermission标签中，如果用户有我们要检查的权限，我们将放置要执行的代码。

如果我们想在用户缺乏权限的情况下采取某些行为，那么我们还需要添加shiro:lackspermission 标签，再次检查users:manage

如果用户缺乏权限，我们所需要执行的任何代码都得放置在shiro:lackspermission 标签中。

\<\%\@ taglib prefix="shiro" uri=http://shiro.apache.org/tags \%>
\<html>
\<body>
    \<shiro:hasPermission name="users:manage">
        \<a href="manageUsers.jsp">
            Click here to manage users
        \</a>
    \</shiro:hasPermission>
    \<shiro:lacksPermission name="users:manage">
        No user management for you\!
    \</shiro:lacksPermission>
\</body>
\</html>

## 授权的实现

在之前的自定义Realm类中修改doGetAuthorizationInfo方法

\@Override
    protected AuthorizationInfo doGetAuthorizationInfo\(PrincipalCollection principals\) \{
        // 从系统返回的身份信息集合中获取主身份信息
        String primaryPrincipal = \(String\) principals.getPrimaryPrincipal\(\);

        // 新创建一个SimpleAuthorizationInfo权限对象
        SimpleAuthorizationInfo simpleAuthorizationInfo = new SimpleAuthorizationInfo\(\);

        // 将"admin"角色和"user"角色信息赋值给权限对象
        simpleAuthorizationInfo.addRole\("admin"\);
        simpleAuthorizationInfo.addRole\("user"\);


        // 将"writer:write:XJK"和"car:drive"权限信息赋值个权限对象 基于String的许可检查  \*号通配
        simpleAuthorizationInfo.addStringPermission\("writer:write:\*"\);
        simpleAuthorizationInfo.addStringPermission\("root:\*:\*"\);
        simpleAuthorizationInfo.addStringPermission\("car:drive"\);

        return simpleAuthorizationInfo;
    \}

\@Test
public void test5\(\)\{
    DefaultSecurityManager manager = new DefaultSecurityManager\(\);
    // 通过安全管理对象设置自定义realm
    manager.setRealm\(new MyCustomRealm1\(\)\);
    // 设置安全工具类
    SecurityUtils.setSecurityManager\(manager\);
    // 通过SecurityUtils获取一个Subject
    Subject subject = SecurityUtils.getSubject\(\);

    // 此时需要创建一个用户令牌
    UsernamePasswordToken upt = new UsernamePasswordToken\("xjk", "password"\);
    try \{
        // 然后只需要将这个用户令牌丢到subject里面就行了
        subject.login\(upt\);  // 如果用户名和密码都ok就莫得问题
        System.out.println\("是否认证通过\?==>"+subject.isAuthenticated\(\)\);
    \} catch \(UnknownAccountException e\) \{
        e.printStackTrace\(\);
        System.out.println\("用户名错误"\);
    \} catch \(IncorrectCredentialsException e\) \{
        e.printStackTrace\(\);
        System.out.println\("密码错误"\);
    \}
	System.out.println\("=======授权========="\);
    //授权
    if\(subject.isAuthenticated\(\)\)\{
        //基于角色权限控制
        System.out.println\("当前subject是否有super角色=>"+subject.hasRole\("super"\)\);

        //同时具有多个角色权限
        System.out.println\("当前subject是否同时拥有如下\(\\"admin\\", \\"super\\"\)角色=>"
                +subject.hasAllRoles\(Arrays.asList\("admin", "super"\)\)\);

        //是否具有其中一个角色
        boolean\[\] hasRoles = subject.hasRoles\(Arrays.asList\("admin", "super", "user"\)\);
        System.out.println\("\\"admin\\"\\t\\"super\\"\\t\\"user\\""\);
        for \(boolean b : hasRoles\) \{
            System.out.print\(b+"\\t"\);
        \}

        System.out.println\(\);
        System.out.println\("=============================================="\);

        //基于权限字符串的访问控制  资源标识符:操作:资源类型
        System.out.println\("root资源,update操作,资源类型为file==>权限:"+subject.isPermitted\("root:update:file"\)\);

        //分别具有那些权限
        boolean\[\] permitted = subject.isPermitted\("user:\*:01", "order:\*:10"\);
        System.out.println\("\\"user:\*:01\\"\\t\\"order:\*:10\\""\);
        for \(boolean b : permitted\) \{
            System.out.print\(b+"\\t\\t"\);
        \}
        System.out.println\(\);
        //同时具有哪些权限
        boolean permittedAll = subject.isPermittedAll\("user:\*:01", "product:create:01"\);
        System.out.println\("是否同时拥有\\"user:\*:01\\", \\"product:create:01\\"权限=>"+permittedAll\);
    \}
    // 搞完了用户退出登录,就让subject执行logout就行了
    subject.logout\(\);
\}

## 授权中的相关方法

**SimpleAuthorizationInfo.java**

- void addRole\(String role\) 向与帐户关联的人分配角色

- void addRoles\(Collection\<String> roles\) 向与帐户关联的人分配多个角色

- void addStringPermission\(String permission\) 基于字符串,添加分配给帐户权限

- void addStringPermissions\(Collection\<String> permissions\) 基于字符串,添加分配给帐户多个权限

- void addObjectPermission\(Permission permission\)基于Permission对象,添加分配给帐户权限

- void addObjectPermissions\(Collection\<Permission> permissions\) 基于Permission对象,添加分配给帐户多个权限

  **Subject.java**

- boolean hasRole\(String roleIdentifier\); 是否具有指定角色

- boolean\[\] hasRoles\(List\<String> roleIdentifiers\);是否具有指定角色

- boolean hasAllRoles\(Collection\<String> roleIdentifiers\);是否具有指定的所有角色,同时满足

- boolean isPermitted\(String permission\);基于字符串,是否具有指定的权限

- boolean isPermitted\(Permission permission\); 基于Permission对象,是否具有指定的权限

- boolean\[\] isPermitted\(String... permissions\);基于字符串,是否具有指定的权限

- boolean\[\] isPermitted\(List\<Permission> permissions\);基于Permission对象,是否具有指定的权限

- boolean isPermittedAll\(String... permissions\);基于字符串,是否具有指定的所有权限,同时满足

- boolean isPermittedAll\(Collection\<Permission> permissions\);基于Permission对象,是否具有指定的所有权限,同时满足

## shiro缓存

缓存的意义：在没有缓存的时候，每次都会调用Realm中的doGetAuthorizationInfo方法进行授权操作，但是对于授权信息来讲，一般不会频繁更改，所以为了加快访问速度，防止系统频繁执行获取权限的操作做么就需要用到缓存

首先导入依赖

\<dependency>
  \<groupId>org.apache.shiro\</groupId>
  \<artifactId>shiro-ehcache\</artifactId>
  \<version>1.9.1\</version>
\</dependency>

开启缓存管理

realm.setCacheManager\(new EhCacheManager\(\)\);
realm.setCachingEnabled\(true\);//开启缓存
realm.setAuthenticationCachingEnabled\(true\);//开启认证缓存
realm.setAuthorizationCachingEnabled\(true\);// 开启授权缓存

## SpringBoot与Shiro的整合

### 搭建一个springboot - web项目

index.html

\<\!DOCTYPE html>
\<html lang="en">
\<head>
    \<meta charset="UTF-8">
    \<title>Title\</title>
\</head>
\<body>
Hello
\</body>
\</html>

login.html

\<\!DOCTYPE html>
\<html lang="en">
\<head>
    \<meta charset="UTF-8">
    \<title>Title\</title>
\</head>
\<body>
登录页
\</body>
\</html>

product.html

\<\!DOCTYPE html>
\<html lang="en">
\<head>
    \<meta charset="UTF-8">
    \<title>Title\</title>
\</head>
\<body>
资源页
\</body>
\</html>

controller

package com.xjk.shiro.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

/\*\*
 \* \@author: XJK
 \*/
\@Controller
public class MyController \{
    

    \@RequestMapping\("/login"\)
    public String getLogin\(\)\{
        return "login";
    \}
    
    \@RequestMapping\("/product"\)
    public String getResource\(\)\{
        return "product";
    \}

\}

### 搭建环境

\<\!-- https://mvnrepository.com/artifact/org.apache.shiro/shiro-spring-boot-starter -->
\<dependency>
    \<groupId>org.apache.shiro\</groupId>
    \<artifactId>shiro-spring-boot-starter\</artifactId>
    \<version>1.8.0\</version>
\</dependency>

pom.xml

\<\?xml version="1.0" encoding="UTF-8"\?>
\<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    \<modelVersion>4.0.0\</modelVersion>
    \<groupId>com.xjk\</groupId>
    \<artifactId>shiro\</artifactId>
    \<version>0.0.1-SNAPSHOT\</version>
    \<name>shiro\</name>
    \<description>Demo project for Spring Boot\</description>

    \<properties>
        \<java.version>1.8\</java.version>
        \<project.build.sourceEncoding>UTF-8\</project.build.sourceEncoding>
        \<project.reporting.outputEncoding>UTF-8\</project.reporting.outputEncoding>
        \<spring-boot.version>2.3.7.RELEASE\</spring-boot.version>
    \</properties>

    \<dependencies>
        \<dependency>
            \<groupId>org.springframework.boot\</groupId>
            \<artifactId>spring-boot-starter-thymeleaf\</artifactId>
        \</dependency>
        \<dependency>
            \<groupId>org.springframework.boot\</groupId>
            \<artifactId>spring-boot-starter-web\</artifactId>
        \</dependency>
        \<dependency>
            \<groupId>junit\</groupId>
            \<artifactId>junit\</artifactId>
            \<version>4.9\</version>
        \</dependency>
        \<dependency>
            \<groupId>commons-logging\</groupId>
            \<artifactId>commons-logging\</artifactId>
            \<version>1.1.3\</version>
        \</dependency>
        \<dependency>
            \<groupId>org.apache.shiro\</groupId>
            \<artifactId>shiro-spring-boot-starter\</artifactId>
            \<version>1.8.0\</version>
        \</dependency>
        \<dependency>
            \<groupId>org.projectlombok\</groupId>
            \<artifactId>lombok\</artifactId>
            \<version>1.18.18\</version>
        \</dependency>
        \<dependency>
            \<groupId>org.springframework.boot\</groupId>
            \<artifactId>spring-boot-devtools\</artifactId>
            \<scope>runtime\</scope>
            \<optional>true\</optional>
        \</dependency>
        \<dependency>
            \<groupId>org.springframework.boot\</groupId>
            \<artifactId>spring-boot-starter-test\</artifactId>
            \<scope>test\</scope>
            \<exclusions>
                \<exclusion>
                    \<groupId>org.junit.vintage\</groupId>
                    \<artifactId>junit-vintage-engine\</artifactId>
                \</exclusion>
            \</exclusions>
        \</dependency>
    \</dependencies>

    \<dependencyManagement>
        \<dependencies>
            \<dependency>
                \<groupId>org.springframework.boot\</groupId>
                \<artifactId>spring-boot-dependencies\</artifactId>
                \<version>\$\{spring-boot.version\}\</version>
                \<type>pom\</type>
                \<scope>import\</scope>
            \</dependency>
        \</dependencies>
    \</dependencyManagement>

    \<build>
        \<plugins>
            \<plugin>
                \<groupId>org.apache.maven.plugins\</groupId>
                \<artifactId>maven-compiler-plugin\</artifactId>
                \<version>3.8.1\</version>
                \<configuration>
                    \<source>1.8\</source>
                    \<target>1.8\</target>
                    \<encoding>UTF-8\</encoding>
                \</configuration>
            \</plugin>
            \<plugin>
                \<groupId>org.springframework.boot\</groupId>
                \<artifactId>spring-boot-maven-plugin\</artifactId>
                \<version>2.3.7.RELEASE\</version>
                \<configuration>
                    \<mainClass>com.xjk.shiro.ShiroApplication\</mainClass>
                \</configuration>
                \<executions>
                    \<execution>
                        \<id>repackage\</id>
                        \<goals>
                            \<goal>repackage\</goal>
                        \</goals>
                    \</execution>
                \</executions>
            \</plugin>
        \</plugins>
    \</build>

\</project>

### 编写自定义Realm

package com.xjk.shiro.realms;

import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.AuthenticationInfo;
import org.apache.shiro.authc.AuthenticationToken;
import org.apache.shiro.authz.AuthorizationInfo;
import org.apache.shiro.realm.AuthorizingRealm;
import org.apache.shiro.subject.PrincipalCollection;
import org.springframework.context.annotation.Bean;

/\*\*
 \* \@author: XJK
 \*/
public class MyRealm2 extends AuthorizingRealm \{
    \@Override
    protected AuthorizationInfo doGetAuthorizationInfo\(PrincipalCollection principals\) \{
        return null;
    \}

    \@Override
    protected AuthenticationInfo doGetAuthenticationInfo\(AuthenticationToken token\) throws AuthenticationException \{
        return null;
    \}
\}

### 编写配置类

package com.xjk.shiro.config;

import com.xjk.shiro.realms.MyRealm2;
import org.apache.shiro.realm.Realm;
import org.apache.shiro.spring.web.ShiroFilterFactoryBean;
import org.apache.shiro.web.mgt.DefaultWebSecurityManager;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import java.util.HashMap;
import java.util.Map;

/\*\*
 \* \@author: XJK
 \*/
\@Configuration
public class ShiroConfiguration \{

    //1.创建shiroFilter  //负责拦截所有请求  
    \@Bean
    public ShiroFilterFactoryBean getShiroFilterFactoryBean\(DefaultWebSecurityManager defaultWebSecurityManager\)\{
        ShiroFilterFactoryBean shiroFilterFactoryBean = new ShiroFilterFactoryBean\(\);

        //给filter设置安全管理器
        shiroFilterFactoryBean.setSecurityManager\(defaultWebSecurityManager\);

        //配置系统受限资源
        //配置系统公共资源
        Map\<String,String> map = new HashMap\<String,String>\(\);
        map.put\("/product","authc"\);//authc 请求这个资源需要认证和授权 这是Shiro的内置过滤器

        //默认认证界面路径
        shiroFilterFactoryBean.setLoginUrl\("/login"\);
        shiroFilterFactoryBean.setFilterChainDefinitionMap\(map\);

        return shiroFilterFactoryBean;
    \}

    //2.创建安全管理器
    \@Bean
    public DefaultWebSecurityManager getDefaultWebSecurityManager\(\@Qualifier\("MyRealm2"\) MyRealm2 myRealm2\)\{
        DefaultWebSecurityManager defaultWebSecurityManager = new DefaultWebSecurityManager\(\);
        //给安全管理器设置
        defaultWebSecurityManager.setRealm\(myRealm2\);

        return defaultWebSecurityManager;
    \}

    //3.创建自定义realm
    \@Bean\(name="MyRealm2"\)
    public Realm getRealm\(\)\{
        MyRealm2 customerRealm = new MyRealm2\(\);
        return customerRealm;
    \}

\}

### Shiro内置过滤器

常用的过滤器： **anon**: 无需认证（登录）可以访问 **authc**: 必须认证才可以访问 **user**: 如果使用rememberMe的功能可以直接访问 **perms**： 该资源必须得到资源权限才可以访问 **role**: 该资源必须得到角色权限才可以访问

### 自定义Realm

package com.xjk.shiro.realms;

import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.AuthenticationInfo;
import org.apache.shiro.authc.AuthenticationToken;
import org.apache.shiro.authc.SimpleAuthenticationInfo;
import org.apache.shiro.authz.AuthorizationInfo;
import org.apache.shiro.crypto.hash.Md5Hash;
import org.apache.shiro.realm.AuthorizingRealm;
import org.apache.shiro.subject.PrincipalCollection;
import org.apache.shiro.util.ByteSource;
import org.springframework.context.annotation.Bean;

/\*\*
 \* \@author: XJK
 \*/
public class MyRealm2 extends AuthorizingRealm \{
    \@Override
    protected AuthorizationInfo doGetAuthorizationInfo\(PrincipalCollection principals\) \{
        return null;
    \}

    \@Override
    protected AuthenticationInfo doGetAuthenticationInfo\(AuthenticationToken token\) throws AuthenticationException \{
        String principal = \(String\) token.getPrincipal\(\);
        String pwd = String.valueOf\(\(char\[\]\) token.getCredentials\(\)\);
        System.out.println\(principal\);
        String correctUsername="xjk";	// 模拟存在数据库中的账户
        String correctPassword="root";  // 模拟存在数据库中的密码
        String salt = "1qaz2wsx";
        int hashIterations=1024;
        if\(correctUsername.equals\(principal\)\)\{
            return new SimpleAuthenticationInfo\(principal,
                    new Md5Hash\(correctPassword,salt,hashIterations\).toHex\(\),
                    ByteSource.Util.bytes\(salt\),
                    this.getName\(\)\);
        \}
        return null;
    \}
\}

### 编写配置文件

package com.xjk.shiro.config;

import com.xjk.shiro.realms.MyRealm2;
import org.apache.shiro.realm.Realm;
import org.apache.shiro.spring.web.ShiroFilterFactoryBean;
import org.apache.shiro.web.mgt.DefaultWebSecurityManager;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import java.util.HashMap;
import java.util.Map;

/\*\*
 \* \@author: XJK
 \*/
\@Configuration
public class ShiroConfiguration \{

    //1.创建shiroFilter  //负责拦截所有请求
    \@Bean
    public ShiroFilterFactoryBean getShiroFilterFactoryBean\(DefaultWebSecurityManager defaultWebSecurityManager\)\{
        ShiroFilterFactoryBean shiroFilterFactoryBean = new ShiroFilterFactoryBean\(\);

        //给filter设置安全管理器
        shiroFilterFactoryBean.setSecurityManager\(defaultWebSecurityManager\);

        //配置系统受限资源
        //配置系统公共资源
        Map\<String,String> map = new HashMap\<String,String>\(\);
        map.put\("/product","authc"\);//authc 请求这个资源需要认证和授权

        // 默认认证界面路径
        shiroFilterFactoryBean.setLoginUrl\("/login"\);
        // 设置拦截链定义Map
        shiroFilterFactoryBean.setFilterChainDefinitionMap\(map\);
        return shiroFilterFactoryBean;
    \}

    //2.创建安全管理器
    \@Bean
    public DefaultWebSecurityManager getDefaultWebSecurityManager\(\@Qualifier\("MyRealm2"\) MyRealm2 myRealm2\)\{
        DefaultWebSecurityManager defaultWebSecurityManager = new DefaultWebSecurityManager\(\);
        //给安全管理器设置
        defaultWebSecurityManager.setRealm\(myRealm2\);

        return defaultWebSecurityManager;
    \}

    //3.创建自定义realm
    \@Bean\(name="MyRealm2"\)
    public MyRealm2 getRealm\(\)\{
        MyRealm2 customerRealm = new MyRealm2\(\);
        return customerRealm;
    \}

\}

### 编写controller

package com.xjk.shiro.controller;

import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.IncorrectCredentialsException;
import org.apache.shiro.authc.UnknownAccountException;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.crypto.hash.Md5Hash;
import org.apache.shiro.subject.Subject;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.servlet.ModelAndView;

/\*\*
 \* \@author: XJK
 \*/
\@Controller
public class MyController \{

    \@RequestMapping\("/product"\)
    public String getResource\(\)\{
        return "product";
    \}

    \@RequestMapping\("/login"\)
    public String login\(\)\{
        return "login";
    \}

    \@RequestMapping\("/userLogin"\)
    public String login\(\@RequestParam\("username"\) String username, \@RequestParam\("password"\) String password, Model model\)\{

        System.out.println\("username:"+username\);
        System.out.println\("password:"+password\);
        Subject subject = SecurityUtils.getSubject\(\);
        Md5Hash pwd = new Md5Hash\(password, "1qaz2wsx", 1024\);
        try \{
            subject.login\(new UsernamePasswordToken\(username, pwd.toHex\(\)\)\);
            return "product";
        \} catch \(UnknownAccountException e\) \{
            System.out.println\("用户名错误\!\!"\);
            e.printStackTrace\(\);
            model.addAttribute\("usernameError","用户名错误"\);
        \} catch \(IncorrectCredentialsException e\)\{
            System.out.println\("密码错误\!\!"\);
            e.printStackTrace\(\);
            model.addAttribute\("passwordError","密码错误"\);
        \}
        return "login";
    \}

\}

### 认证的完整实现

### pom.xml依赖

\<dependency>
    \<groupId>org.mybatis.spring.boot\</groupId>
    \<artifactId>mybatis-spring-boot-starter\</artifactId>
    \<version>2.2.2\</version>
\</dependency>
\<dependency>
    \<groupId>com.baomidou\</groupId>
    \<artifactId>mybatis-plus-boot-starter\</artifactId>
    \<version>3.5.1\</version>
\</dependency>
\<dependency>
    \<groupId>mysql\</groupId>
    \<artifactId>mysql-connector-java\</artifactId>
    \<version>8.0.22\</version>
\</dependency>
\<dependency>
    \<groupId>com.alibaba\</groupId>
    \<artifactId>druid-spring-boot-starter\</artifactId>
    \<version>1.2.6\</version>
\</dependency>

### 配置数据源，整合mybatis

#指定Mybatis的Mapper文件
mybatis.mapper-locations=classpath:mappers/\*xml
#指定Mybatis的实体目录
mybatis.type-aliases-package=com.xjk.mybatis.entity
# 数据库驱动：
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
# 数据源类型
spring.datasource.type=com.alibaba.druid.pool.DruidDataSource
# 数据库连接地址
spring.datasource.url=jdbc:mysql://localhost:3306/test\?serverTimezone=UTC
# 数据库用户名\&密码：
spring.datasource.username=root
spring.datasource.password=xjk15574262358

### 创建数据库表

![在这里插入图片描述](https://img-blog.csdnimg.cn/dce5ac17e94347adac62da9ab5735bbb.png#pic_center)

### 创建实体类

package com.xjk.shiro.entity;

import lombok.\*;
import lombok.experimental.Accessors;

/\*\*
 \* \@author: XJK
 \*/

\@NoArgsConstructor
\@AllArgsConstructor
\@Setter
\@Getter
\@ToString
\@Data
\@Accessors\(chain = true\)
public class User \{

    private Integer id;
    private String username;
    private String password;
    private String salt;

\}

### 编写mapper接口

package com.xjk.shiro.mappers;

import com.xjk.shiro.entity.User;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Param;

/\*\*
 \* \@author: XJK
 \*/
\@Mapper
public interface UserMapper \{

    void insertUser\(\@Param\("user"\) User user\);

    User findUserByUsername\(\@Param\("username"\) String username\);

\}

### 编写mybatis映射文件

\<\?xml version="1.0" encoding="UTF-8" \?>
\<\!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
\<mapper namespace="com.xjk.shiro.mappers.UserMapper">

    \<insert id="insertUser">
        insert into shiro\_user\(id,username,password,salt\) valus
            \(#\{user.id\},#\{user.username\},#\{user.password\},#\{user.salt\}\);
    \</insert>
    \<select id="findUserByUsername" resultType="com.xjk.shiro.entity.User">
        select \* from shiro\_user where username=#\{username\};
    \</select>
\</mapper>

### 编写随机Salt生成工具类方法

package com.xjk.shiro.utils;

import java.util.Random;

/\*\*
 \* \@author: XJK
 \*/
public class RandomSaltUtil \{
    public static String getSalt\(int n\) \{
        char\[\] chars = \("ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz" +
                "1234567890\!\@#\$\%\^\&\*\(\)\_+\~\`"\).toCharArray\(\);
        StringBuilder sb = new StringBuilder\(\);
        for\(int i = 0; i \< n; i++\)\{
            //Random\(\).nextInt\(\)返回值为\[0,n\)
            char aChar = chars\[new Random\(\).nextInt\(chars.length\)\];
            sb.append\(aChar\);
        \}
        return sb.toString\(\);
    \}
\}

### 编写service接口

package com.xjk.shiro.service;

import com.xjk.shiro.entity.User;

public interface UserService \{

    void registeredUser\(User user\);

    User findByUsername\(String username\);

\}

### 编写service实现类

package com.xjk.shiro.service.impl;

import com.xjk.shiro.entity.User;
import com.xjk.shiro.mappers.UserMapper;
import com.xjk.shiro.service.UserService;
import com.xjk.shiro.utils.RandomSaltUtil;
import org.apache.shiro.crypto.hash.Md5Hash;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

/\*\*
 \* \@author: XJK
 \*/
\@Service\("UserService"\)
public class UserServiceImpl implements UserService \{

    \@Autowired
    private UserMapper userMapper;

    \@Override
    public void registeredUser\(User user\) \{
        String salt = RandomSaltUtil.getSalt\(10\);
        user.setSalt\(salt\);
        // 通过MD5给密码加密后存数据库
        Md5Hash md5Hash = new Md5Hash\(user.getPassword\(\), salt, 2048\);
        user.setPassword\(md5Hash.toHex\(\)\);
        userMapper.insertUser\(user\);
    \}

    \@Override
    public User findByUsername\(String username\) \{

        return userMapper.findUserByUsername\(username\);
    \}
\}

### 编写controller

package com.xjk.shiro.controller;

import com.xjk.shiro.entity.User;
import com.xjk.shiro.service.UserService;
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.IncorrectCredentialsException;
import org.apache.shiro.authc.UnknownAccountException;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.crypto.hash.Md5Hash;
import org.apache.shiro.subject.Subject;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

/\*\*
 \* \@author: XJK
 \*/
\@Controller
public class UserController \{
    \@Autowired
    private UserService userService;

    \@RequestMapping\("/index"\)
    public String home\(\)\{
        return "index";
    \}

    \@RequestMapping\("/regist"\)
    public String register\(\)\{
        return "regist";
    \}

    \@RequestMapping\("/userRegist",method = RequestMethod.POST\)
    public String register\(User user\) \{
        try \{
            userService.registeredUser\(user\);
            return "login";
        \} catch \(Exception e\) \{
            e.printStackTrace\(\);
            return "regist";
        \}
    \}

    \@RequestMapping\("/login"\)
    public String login\(\)\{
        return "login";
    \}

    \@RequestMapping\("/logout"\)
    public String logout\(\) \{
        Subject subject = SecurityUtils.getSubject\(\);
        subject.logout\(\);
        return "login";
    \}
    \@RequestMapping\(value = "/userLogin",method = RequestMethod.POST\)
    public String login\(String username, String password\) \{
        //获取主题对象
        Subject subject = SecurityUtils.getSubject\(\);
        User user = userService.findByUsername\(username\);
        System.out.println\(user\);
        try \{
            subject.login\(new UsernamePasswordToken\(username,password\)\);
            System.out.println\("登录成功！！！"\);
            return "product";
        \} catch \(UnknownAccountException e\) \{
            System.out.println\("用户错误！！！"\);
        \} catch \(IncorrectCredentialsException e\) \{
            System.out.println\("密码错误！！！"\);
        \}
        return "login";
    \}
\}

### 编写四个页面

index.html

\<\!DOCTYPE html>
\<html lang="en">
\<head>
    \<meta charset="UTF-8">
    \<title>Title\</title>
\</head>
\<body>
\<a href="/login">前往登录\</a>
\<a href="/regist">前往注册\</a>
\</body>
\</html>

login.html

\<\!DOCTYPE html>
\<html xmlns:th="http://www.thymeleaf.org" lang="ch" >
\<head>
    \<meta charset="UTF-8">
    \<title>Title\</title>
\</head>
\<body>
登录页
\<form action="/userLogin" method="post">
    username: \<input name="username" type="text">
    password: \<input name="password" type="text">
    \<input type="submit" value="login">
\</form>

\</body>
\</html>

regist.html

\<\!DOCTYPE html>
\<html lang="en">
\<head>
    \<meta charset="UTF-8">
    \<title>Title\</title>
\</head>
\<body>
Hello, 欢迎注册\~
\<form action="/userRegist" method="post">
    username:\<input type="text" name="username" >
    password: \<input type="text" name="password">
    \<input type="submit" value="I want to register">
\</form>
\</body>
\</html>

product.html

\<\!DOCTYPE html>
\<html lang="en">
\<head>
    \<meta charset="UTF-8">
    \<title>Title\</title>
\</head>
\<body>
商品页

这是受限的,没登陆不准访问
\</body>
\</html>

### 编写自定义Component

ApplicationContextComponent

编写这个的原因是需要获取当前spring上下文中的那些Bean对象,尤其是你的Service,因为你不能再重新new,所以你需要让spring将对象放进你的自定义Context中来

package com.xjk.shiro.utils;

import org.springframework.beans.BeansException;
import org.springframework.context.ApplicationContext;
import org.springframework.context.ApplicationContextAware;
import org.springframework.stereotype.Component;

/\*\*
 \* \@author: XJK
 \*/
\@Component
public class MyAppContextComponent implements ApplicationContextAware \{
    private static ApplicationContext context;

    \@Override
    public void setApplicationContext\(ApplicationContext applicationContext\) throws BeansException \{
        context = applicationContext;
    \}

    public static ApplicationContext getAppContext\(\)\{
        return context;
    \}
\}

### 编写Realm

package com.xjk.shiro.realms;

import com.xjk.shiro.entity.User;
import com.xjk.shiro.service.UserService;
import com.xjk.shiro.utils.MyAppContextComponent;
import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.AuthenticationInfo;
import org.apache.shiro.authc.AuthenticationToken;
import org.apache.shiro.authc.SimpleAuthenticationInfo;
import org.apache.shiro.authz.AuthorizationInfo;
import org.apache.shiro.realm.AuthorizingRealm;
import org.apache.shiro.subject.PrincipalCollection;
import org.apache.shiro.util.ByteSource;
import org.springframework.util.ObjectUtils;

/\*\*
 \* \@author: XJK
 \*/
public class UserRealm extends AuthorizingRealm \{
    \@Override
    protected AuthorizationInfo doGetAuthorizationInfo\(PrincipalCollection principalCollection\) \{
        return null;
    \}

    \@Override
    protected AuthenticationInfo doGetAuthenticationInfo\(AuthenticationToken token\) throws AuthenticationException \{
        String p = \(String\) token.getPrincipal\(\);
        // 通过ApplicationContext获取UserService
        UserService userService = \(UserService\) MyAppContextComponent.getAppContext\(\).getBean\("UserService"\);
        User userByUsername = userService.findByUsername\(p\);
        if \(\!ObjectUtils.isEmpty\(userByUsername\)\) \{
            return new SimpleAuthenticationInfo\(userByUsername.getUsername\(\),
                    userByUsername.getPassword\(\),
                    ByteSource.Util.bytes\(userByUsername.getSalt\(\)\),
                    this.getName\(\)\);
        \}
        return null;
    \}
\}

### 编写ShiroConfiguration

package com.xjk.shiro.config;

import com.xjk.shiro.realms.MyRealm2;
import com.xjk.shiro.realms.UserRealm;
import org.apache.shiro.authc.credential.HashedCredentialsMatcher;
import org.apache.shiro.realm.Realm;
import org.apache.shiro.spring.web.ShiroFilterFactoryBean;
import org.apache.shiro.web.mgt.DefaultWebSecurityManager;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import java.util.HashMap;
import java.util.Map;

/\*\*
 \* \@author: XJK
 \*/
\@Configuration
public class ShiroConfiguration \{

    //ShiroFilter过滤所有请求
    \@Bean
    public ShiroFilterFactoryBean getShiroFilterFactoryBean\(DefaultWebSecurityManager securityManager\) \{
        ShiroFilterFactoryBean shiroFilterFactoryBean = new ShiroFilterFactoryBean\(\);
        //给ShiroFilter配置安全管理器
        shiroFilterFactoryBean.setSecurityManager\(securityManager\);
        Map\<String, String> map = new HashMap\<String, String>\(\);
        map.put\("/index","anon"\);
        map.put\("/login","anon"\);//公共资源
        map.put\("/userLogin","anon"\);//公共资源
        map.put\("/regist","anon"\);//公共资源
        map.put\("/userRegist","anon"\);//公共资源
        map.put\("/\*\*","authc"\);//受限资源
        // 设置认证界面路径
        shiroFilterFactoryBean.setLoginUrl\("/index"\);
        shiroFilterFactoryBean.setFilterChainDefinitionMap\(map\);

        return shiroFilterFactoryBean;
    \}
    //创建安全管理器
    \@Bean
    public DefaultWebSecurityManager getDefaultWebSecurityManager\(\@Qualifier\("UserRealm"\) UserRealm userRealm\) \{
        DefaultWebSecurityManager securityManager = new DefaultWebSecurityManager\(\);
        securityManager.setRealm\(userRealm\);
        return securityManager;
    \}
    //创建自定义Realm
    \@Bean\("UserRealm"\)
    public UserRealm getRealm\(\) \{
        UserRealm realm = new UserRealm\(\);
        HashedCredentialsMatcher credentialsMatcher = new HashedCredentialsMatcher\(\);
        //设置使用MD5加密算法
        credentialsMatcher.setHashAlgorithmName\("md5"\);
        //散列次数
        credentialsMatcher.setHashIterations\(2048\);
        realm.setCredentialsMatcher\(credentialsMatcher\);
        return realm;
    \}

\}

### 授权的实现

![在这里插入图片描述](https://img-blog.csdnimg.cn/59732308fff2453da62368af5a96a74e.png#pic_center)

### 修改Mapper接口,添加能够根据用户名查询所有角色方法

//查询一个用户名所有的角色
UserRoles findUserRolesByUsername\(\@Param\("username"\) String username\); // 添加到UserMapper.java

### 修改UserMapper映射文件,实现findUserRolesByUsername方法

\<\!--由于需要另外封装Role对象,所以应该用resultMap给User赋值-->
\<resultMap id="userRoleMap" type="com.xjk.shiro.entity.UserRoles">
    \<id column="suid" property="id"/>
    \<result column="suname" property="username"/>
    \<result column="supwd" property="password"/>
    \<result column="susalt" property="salt"/>
    \<\!--角色信息-->
    \<collection property="roleList" javaType="list" ofType="com.xjk.shiro.entity.Role">
        \<id column="srid" property="id"/>
        \<result column="srname" property="name"/>
    \</collection>
\</resultMap>

\<select id="findUserRolesByUsername" resultMap="userRoleMap">

    select shiro\_user.id suid,shiro\_user.username suname,shiro\_user.\`password\` supwd,
    shiro\_user.salt susalt,shiro\_role.id srid,shiro\_role.\`name\` srname
    FROM shiro\_user
    LEFT JOIN shiro\_user\_role
    on shiro\_user.id=shiro\_user\_role.id
    LEFT JOIN shiro\_role
    on shiro\_user\_role.id=shiro\_role.id
    WHERE shiro\_user.username=#\{username\};
\</select>

\<\!-- 添加到UserMapper.xml -->

### 修改UserService接口及其实现类

// 在UserService接口内添加
UserRoles findRoleByUsername\(String username\);




// 在实现类添加
\@Override
public UserRoles findRoleByUsername\(String username\) \{
    return userMapper.findUserRolesByUsername\(username\);
\}

### 修改自定义的Realm

\@Override
protected AuthorizationInfo doGetAuthorizationInfo\(PrincipalCollection principalCollection\) \{
    String primaryPrincipal = \(String\) principalCollection.getPrimaryPrincipal\(\);
    // 通过ApplicationContext获取UserService
    UserService userService = \(UserService\) MyAppContextComponent.getAppContext\(\).getBean\("UserService"\);
    // 通过service获取userRoles对象
    UserRoles userRoles = userService.findRoleByUsername\(primaryPrincipal\);
    // userRoles中的roleList不能为空,要有角色信息
    if\(\!userRoles.getRoleList\(\).isEmpty\(\)\)\{
        SimpleAuthorizationInfo simpleAuthorizationInfo = new SimpleAuthorizationInfo\(\);
        // 遍历list将每一个角色名称添加到授权info中
        for \(Role role : userRoles.getRoleList\(\)\) \{
            simpleAuthorizationInfo.addRole\(role.getName\(\)\);
        \}
        return simpleAuthorizationInfo;
    \}
    return null;
\}

### 实现权限授权

package com.xjk.shiro.mappers;

import com.xjk.shiro.entity.Permission;
import com.xjk.shiro.entity.Role;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Param;

import java.util.List;

/\*\*
 \* \@author: XJK
 \*/
\@Mapper
public interface PermissionMapper \{

    //根据角色id查询权限集合并添加到Role对象返回
    Role findPermissionsToRoleByRoleId\(\@Param\("role\_id"\) Integer id\);
    //根据角色id查询权限集合
    List\<Permission> findPermissionsByRoleId\(\@Param\("role\_id"\) Integer id\);

\}

\<\?xml version="1.0" encoding="UTF-8" \?>
\<\!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
\<mapper namespace="com.xjk.shiro.mappers.PermissionMapper">

    \<resultMap id="rolePermissionMap" type="com.xjk.shiro.entity.Role">
        \<id column="srid" property="id"/>
        \<result column="srname" property="name"/>
        \<collection property="permissionList" ofType="com.xjk.shiro.entity.Permission">
            \<id column="spid" property="id"/>
            \<result column="spv" property="pValue"/>
            \<result column="spname" property="name"/>
        \</collection>
    \</resultMap>

    \<select id="findPermissionsToRoleByRoleId" resultMap="rolePermissionMap">
        SELECT shiro\_role.id srid, shiro\_role.\`name\` srname,
               shiro\_permission.id spid, shiro\_permission.p\_value spv ,shiro\_permission.name spname
        FROM shiro\_role
                 LEFT JOIN shiro\_role\_permission on shiro\_role.id = shiro\_role\_permission.id
                 LEFT JOIN shiro\_permission on shiro\_role\_permission.id = shiro\_permission.id
        WHERE shiro\_role.\`name\` = "123";
    \</select>

    \<select id="findPermissionsByRoleId" resultType="com.xjk.shiro.entity.Permission">
        SELECT \*
        FROM shiro\_permission
        WHERE id = #\{role\_id\};
    \</select>
\</mapper>

package com.xjk.shiro.service;

import com.xjk.shiro.entity.Permission;
import com.xjk.shiro.entity.Role;

import java.util.List;

public interface PermissionService \{

    Role findRolePermissionByRoleID\(Integer roleId\);

    List\<Permission> findPermissionByRoleId\(Integer roleId\);

\}

package com.xjk.shiro.service.impl;

import com.xjk.shiro.entity.Permission;
import com.xjk.shiro.entity.Role;
import com.xjk.shiro.mappers.PermissionMapper;
import com.xjk.shiro.service.PermissionService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import java.util.List;

/\*\*
 \* \@author: XJK
 \*/
\@Service\("permissionService"\)
public class PermissionServiceImpl implements PermissionService \{

    \@Autowired
    private PermissionMapper permissionMapper;

    \@Override
    public Role findRolePermissionByRoleID\(Integer roleId\) \{
        return permissionMapper.findPermissionsToRoleByRoleId\(roleId\);
    \}

    \@Override
    public List\<Permission> findPermissionByRoleId\(Integer roleId\) \{
        return permissionMapper.findPermissionsByRoleId\(roleId\);
    \}
\}

\@Override
protected AuthorizationInfo doGetAuthorizationInfo\(PrincipalCollection principalCollection\) \{
    String primaryPrincipal = \(String\) principalCollection.getPrimaryPrincipal\(\);
    // 通过ApplicationContext获取UserService
    UserService userService = \(UserService\) MyAppContextComponent.getAppContext\(\).getBean\("UserService"\);
    PermissionService permissionService = \(PermissionService\) MyAppContextComponent.getAppContext\(\).getBean\("PermissionService"\);
    // 角色信息
    UserRoles userRoles = userService.findRoleByUsername\(primaryPrincipal\);
    if\(\!userRoles.getRoleList\(\).isEmpty\(\)\)\{
        SimpleAuthorizationInfo simpleAuthorizationInfo = new SimpleAuthorizationInfo\(\);
        for \(Role role : userRoles.getRoleList\(\)\) \{
            simpleAuthorizationInfo.addRole\(role.getName\(\)\);


            //权限信息
            List\<Permission> permissionByRoleId = permissionService.findPermissionByRoleId\(role.getId\(\)\);
            if\(\!\(permissionByRoleId.isEmpty\(\)\)\&\&permissionByRoleId.get\(0\)\!=null\)\{
                for \(Permission permission : permissionByRoleId\) \{
                    simpleAuthorizationInfo.addStringPermission\(permission.getPValue\(\)\);
                \}
            \}
        \}
        return simpleAuthorizationInfo;
    \}
    return null;
\}

\@RequestMapping\(value = "/userLogin",method = RequestMethod.POST\)
public String login\(String username, String password\) \{
    //获取主题对象
    Subject subject = SecurityUtils.getSubject\(\);
    try \{
        subject.login\(new UsernamePasswordToken\(username,password\)\);
        System.out.println\("登录成功！！！"\);
    \} catch \(UnknownAccountException e\) \{
        System.out.println\("用户错误！！！"\);
    \} catch \(IncorrectCredentialsException e\) \{
        System.out.println\("密码错误！！！"\);
    \}

    // 只有登录成功了才能授权
    if\(subject.isAuthenticated\(\)\)\{
        // 当前用户是否是admin角色用户
        if\(subject.hasRole\("admin"\)\)\{
            // 当前这个 admin 是否有"admin:manage:admin"权限
            if \(subject.isPermitted\("admin:manage:admin"\)\)\{
                return "manager";
            \}
        \}
        return "product";
    \}
    
    return "login";
\}

## thymeleaf权限控制

### 引入pom.xml依赖

\<dependency>
    \<groupId>com.github.theborakompanioni\</groupId>
    \<artifactId>thymeleaf-extras-shiro\</artifactId>
    \<version>2.0.0\</version>
\</dependency>

### 加入shiro的配置

\@Bean\(name = "ShiroDialect"\)
public ShiroDialect shiroDialect\(\)\{
  return new ShiroDialect\(\);
\}

### 页面中引入命名空间

\<html xmlns="http://www.w3.org/1999/xhtml" xmlns:th="http://www.thymeleaf.org"
      xmlns:shiro="http://www.pollix.at/thymeleaf/shiro">

### 常见权限控制标签使用

未认证 + Remember=false:

\<div shiro:guest="">\</div>

未认证通过 + Remember=true

\<div shiro:notAuthenticated="">\</div>

认证通过 + Remember=false

\<div shiro:authenticated="">\</div>

认证通过 + Remember=true

\<div shiro:user="">
	\<div shiro:principle="">
    \</div>
\</div>

直接显示shiro中的账户名\(principal\)

\<shiro:principal/>

是否拥有某个角色

\<div shiro:hasRole="admin">\</div>

没有这个角色

\<div shiro:lacksRole="admin">\</div>

角色所属其中之一

\<div shiro:hasAnyRoles="admin, user, vip, root">
   
\</div>

满足所有角色

\<div shiro:hasAllRoles="admin, user, vip, root">
    
\</div>

是否拥有权限

\<div shiro:hasPermission="admin:get:\*">
    
\</div>

是否拥有权限之一

\<div shiro:hasAnyPermissions="user:view:\*, user:delete">
    
\</div>

是否拥有所有权限

\<div shiro:hasAllPermissions="user:view:\*, user:delete">
    
\</div>