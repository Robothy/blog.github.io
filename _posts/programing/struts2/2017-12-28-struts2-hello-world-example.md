---
layout: post
categories: programing java struts2
title: "创建 Struts2 Web 应用程序"
---

本文将描述在eclipse下基于 Struts2 一步步创建 maven project。

打开eclipse， File > New > Maven Project 。

![](https://gitee.com/robothy/img/raw/master/luofuxiang/764971-20171227175329785-892475365.png)

勾选 Use default Workspace location > Next

![](https://gitee.com/robothy/img/raw/master/luofuxiang/764971-20171228095726675-1351783120.png)

Catalog 选中 Internal, Archetype 选中 Artifact Id 为 maven-archetype-webapp 的项, Next 。

![](https://gitee.com/robothy/img/raw/master/luofuxiang/764971-20171228100735425-102618026.png)

 填入 Group Id 和 Artifact Id，其中 Artifact Id 为工程名，Finish。

![](https://gitee.com/robothy/img/raw/master/luofuxiang/764971-20171228101705894-1511850885.png)

创建完成之后工程的目录结构如下所示，其中有一个index.jsp文件报错，可将其删除或将内容修改正确，默认的 JRE 版本为 1.5，可以在工程的 Build Path 中修改。

![](https://gitee.com/robothy/img/raw/master/luofuxiang/764971-20171228102128378-849649523.png)

 maven 工程创建完成之后，在 pom.xml 中添加 Struts2 的依赖。

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>helloworld</groupId>
  <artifactId>struts2maven</artifactId>
  <packaging>war</packaging>
  <version>0.0.1-SNAPSHOT</version>
  <name>struts2maven Maven Webapp</name>
  <url>http://maven.apache.org</url>
  <dependencies>
    <dependency>
        <!-- Struts2 -->
        <groupId>org.apache.struts</groupId>
        <artifactId>struts2-core</artifactId>
        <version>2.3.16.3</version>
    </dependency>
  </dependencies>
  <build>
    <finalName>struts2maven</finalName>
  </build>
</project>
```
在 Deployed Resources/webapp/WEB-INF/web.xml 文件中添加 Struts2 的过滤器。

```xml
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
  <display-name>Archetype Created Web Application</display-name>
      <filter>
        <filter-name>struts2</filter-name>
        <!-- 不同版本 Struts2 的 filter-class 不同 -->
        <filter-class>org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter</filter-class>
    </filter>

    <filter-mapping>
        <filter-name>struts2</filter-name>
        <!-- 过滤所有的请求 -->
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>
```

在 Java Resources/src/main/resources 目录下创建 struts.xml，文件里面添加如下内容：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE struts PUBLIC
    "-//Apache Software Foundation//DTD Struts Configuration 2.5//EN"
    "http://struts.apache.org/dtds/struts-2.5.dtd">

<struts>
    <!-- 开发模式 -->
    <constant name="struts.devMode" value="true" />
    <package name="basicstruts2" extends="struts-default">
        <!-- 指定 Action 类为 IndexAction，方法为exe -->
        <action name="index" class="org.struts2.helloworld.IndexAction" method="exe">
            <!-- 当方法返回值为 succ 时，重定向到 index.jsp -->
            <result name="succ">/index.jsp</result>
        </action>
    </package>
</struts>
```

在包 `org.struts2.helloworld` 下面创建类 Action，添加如下代码：

package org.struts2.helloworld;

```java
public class IndexAction {
    
    private String hello = null;
    
    public String getHello(){
        return hello;
    }
    
    public void setHello(String hello){
        this.hello = hello;
    }
    
    public String exe(){
        setHello("Hello World");
        return "succ";
    }
}
```

在 src/main/webapp 下创建 index.jsp，添加如下代码：

```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!-- 引入 Struts2 标签库 -->
<%@ taglib prefix = "s" uri = "/struts-tags"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Struts2</title>
</head>
<body>
    <!-- 取出值栈中 hello 的值 -->
    <s:property value="hello"/>
</body>
</html>
```

右击工程名 > Run As > Run on Server，启动Tomcat服务，然后在浏览器中输入 `http://127.0.0.1:8080/struts2maven/index` ，页面上显示 Hello World。这说明Struts2成功拦截了对 index 的请求，给Action中的hello 赋值 "Hello World"，并把值传到了index.jsp。

![](https://gitee.com/robothy/img/raw/master/luofuxiang/764971-20171228130803347-1436012144.png)