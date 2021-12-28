---
title: tomcat server.xml配置调优
categories:
  - 后端
tags:
  - Tomcat
author: geepair
date: 2021-01-29 19:05:00
---

 记录Tomcat server.xml配置文件的各属性用于调优

<!-- more -->

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!-- Server是最外层标签，包含了所有的子标签 -->
<Server port="8005" shutdown="SHUTDOWN">
    <!-- 注册所有的监听器 -->
    <Listener className="org.apache.catalina.startup.VersionLoggerListener" />
    <Listener calssName="org.apache.catalina.core.AprLifecycleListener" SSLEngine="on" />
    <Listener className="org.apache.catalina.mbeans.GlobalResourcesLifecycleListener" />
    <Listener className="org.apache.catalina.core.ThreadLocalLeakPreventionListener" />
    
    <!-- 全局的 JNDI 资源, tomcat管理用户信息 -->
    <GlobalNamingResources>
    <Resource name="UserDatabase" auth="Container"
              type="org.apache.catalina.UserDatabase"
              description="User database that can be updated and saved"
              factory="org.apache.catalina.users.MemoryUserDatabaseFactory"
              pathname="conf/tomcat-users.xml" />
    </GlobalNamingResources>
    <!-- 一个Service包含了一个或者多个Container容器 -->
    <Service name="Catalina">
    	<!-- connector使用的线程池Executor定义，参考Java线程池各个参数的意义 -->
        <Executor name="tomcatThreadPool" namePrefix="catalina-exec-"
                  maxThreads="500" 
                  minSpareThreads="80" 
                  maxQueueSize="100" 
                  maxIdleTime="60000"
                  prestartminSpareThreads="true" />
        <!-- Connector配置 http 8080 -->
        <Connector port="8080" // 运行的端口
                   executor="tomcatThreadPool" // 之前定义的线程池
                   protocol="org.apache.coyote.http11.Http11NioProtocol" //Bio Nio Aio
                   connectionTimeout="20000"
                   redirectPort="8443"
                   maxConnections="10000"
                   enableLookups="false"
                   acceptCount="100"
                   maxPostSize="10485760"
                   maxHttpHeaderSize="4096"
                   compression="on"
                   disableUploadTimeout="true"
                   compressionMinSize="1024"
                   acceptorThreadCount="2" // cpu数量+1
                   processorCache="20000"
                   tcpNoDelay="true"
                   connectionLinger="5"
                   URIEncoding="utf-8"
                   server="Tengine"
                   compressableMimeType="text/html,text/plain,application/x-javascript,text/css,application/xml,text/javascript,application/x-httpd-php,image/jpeg,image/gif,image/png" />
        
        <!-- 使用SSL证书 http+SSL 8443 -->
        
        <!-- 使用AJP -->
        ...
        
        <!-- jvmRouter 负载均衡 -->
        <Engine name="Catalina" defaultHost="localhost">
            ...
        	<Cluster className="org.apache.catalina.ha.tcp.SimpleTcpCluster"/> 
            <Realm className="org.apache.catalina.realm.LockOutRealm">
                <!-- guess -->
				<Realm className="org.apache.catalina.realm.UserDatabaseRealm"
                       resourceName="UserDatabase"/>
            </Realm>
            <!-- 主机配置 -->
            <Host name="localhost" appBase="webapps"
                  unpackWARS="true" autoDeploy="true">
                <!-- 日志 -->
                <Valve className="org.apache.catalina.valves.AccessLogValve"
                       directory="logs"
                       prefix="localhost_access_log"
                       suffix=".txt"
                       pattern="%h %l %u %t &quot;%r&quot; %s %b" />
            </Host>
        </Engine>
    </Service>
</Server>
```