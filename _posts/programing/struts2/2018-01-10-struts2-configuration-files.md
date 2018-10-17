---
layout: post
categories: programing java struts2
title: "Struts2 中的配置文件"
---
# Struts2 中的配置文件

创建一个基于 Struts2 的 Web 应用，涉及到多个配置文件，本文将描述各个配置文件的作用。

## 在 Web.xml 中配置 Struts2 的过滤器

web.xml 文件提供有关包含 Web 应用程序的 Web 组件的配置和部署信息，可以在其中配置 Servlet, Filter, Listener等， Struts2 需要在其中配置一个过滤器(Filter)才能正常使用。

过滤器的配置方式是在 web.xml 文件中的`<web-app>`节点下增加如下内容：

```xml
<filter>
      <filter-name>struts2</filter-name>
      <filter-class>
         org.apache.struts2.dispatcher.FilterDispatcher
      </filter-class>
   </filter>

   <filter-mapping>
      <filter-name>struts2</filter-name>
      <url-pattern>/*</url-pattern>
   </filter-mapping>
```

其中 Struts2 不同的发型版本`filter-class`的值不同，即过滤器类的名称不同。

## struts-config.xml 配置文件

struts-config.xml 配置文件是