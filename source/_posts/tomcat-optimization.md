---
title: tomcat web.xml配置
categories:
  - 后端
tags:
  - Tomcat
author: geepair
date: 2021-01-29 18:44:00
---

 记录tomcat web.xml配置文件的配置，以tomcat8.5大版本为例

<!-- more -->

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!-- webapp最外层 -->
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                             http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">
    <!-- 默认的servlet会和application的web.xml合并,用于处理static file
		SSM框架中可以使用DespatcherServlet实现类似的功能，与此同时DefaultServlet被覆盖 -->
	<servlet>
    	<servlet-name>dafault</servlet-name>
        <servlet-class>org.apache.catalina.servlets.DefaultServlet</servlet-class>
        <init-param>
        	<param-name>debug</param-name>
            <param-value>0</param-value>
        </init-param>
        <init-param>
        	<param-name>listings</param-name>
            <param-value>false</param-value>
            <load-on-startup>1</load-on-startup>
        </init-param>
    </servlet>
    <!-- .jsp的java dynamic service自动处理，例如index.jsp -->
    <servlet>
    	<servlet-name>jsp</servlet-name>
        <servlet-class>org.apache.jasper.servlet.JspServlet</servlet-class>
        <init-param>
        	<param-name>fork</param-name>
            <param-value>false</param-value>
        </init-param>
        <init-param>
        	<param-name>xpoweredBy</param-name>
            <param-value>false</param-value>
            <load-on-startup>3</load-on-startup>
        </init-param>
    </servlet>
    
	... 其他Servlet
    
    <!-- mapping for DefaultServlet -->
    <servlet-mapping>
    	<servlet-name>default</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
    <!-- mapping for JspServlet -->
    <servlet-mapping>
    	<servlet-name>jsp</servlet-name>
        <url-pattern>*.jsp</url-pattern>
        <url-pattern>*.jspx</url-pattern>
    </servlet-mapping>
    
    ... 其他Filter
    
    <!-- session 配置 session超时时间 30s-->
    <session-config>
    	<session-timeout>30</session-timeout>
    </session-config>
    
    ... mime 能处理的文件类型
    
    <!-- 欢迎页 -->
    <welcome-file-list>
   		<welcome-file>index.html</welcome-file>
        <welcome-file>index.htm</welcome-file>
        <welcome-file>index.jsp</welcome-file>
    </welcome-file-list>
    
    
</web-app>
```

