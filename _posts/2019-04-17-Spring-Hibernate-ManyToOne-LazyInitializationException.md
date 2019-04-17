---
layout: post
title: "Spring Hibernate ManyToOne LazyInitializationException"
author: "Eddy Hu"
categories: journal
tags: [Spring, Hibernate]
image: hibernate.png
---

**Exception: LazyInitializationException: could not initialize proxy - no Session**

**Root Cause**

    org.hibernate.LazyInitializationException: could not initialize proxy - no Session
    	org.hibernate.proxy.AbstractLazyInitializer.initialize(AbstractLazyInitializer.java:165)
    	org.hibernate.proxy.AbstractLazyInitializer.getImplementation(AbstractLazyInitializer.java:286)
    	org.hibernate.proxy.pojo.javassist.JavassistLazyInitializer.invoke(JavassistLazyInitializer.java:185)
    	com.****.****web.domain.Course_$$_jvsta05_0.getCourseName(Course_$$_jvsta05_0.java)
    	sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
    	sun.reflect.NativeMethodAccessorImpl.invoke(Unknown Source)
    	sun.reflect.DelegatingMethodAccessorImpl.invoke(Unknown Source)
    	java.lang.reflect.Method.invoke(Unknown Source)
    	org.codehaus.jackson.map.ser.BeanPropertyWriter.get(BeanPropertyWriter.java:483)
    	org.codehaus.jackson.map.ser.BeanPropertyWriter.serializeAsField(BeanPropertyWriter.java:418)
    	org.codehaus.jackson.map.ser.std.BeanSerializerBase.serializeFields(BeanSerializerBase.java:150)
    	org.codehaus.jackson.map.ser.BeanSerializer.serialize(BeanSerializer.java:112)
    	org.codehaus.jackson.map.ser.BeanPropertyWriter.serializeAsField(BeanPropertyWriter.java:446)
    	org.codehaus.jackson.map.ser.std.BeanSerializerBase.serializeFields(BeanSerializerBase.java:150)
    	org.codehaus.jackson.map.ser.BeanSerializer.serialize(BeanSerializer.java:112)
    	org.codehaus.jackson.map.ser.std.StdContainerSerializers$IndexedListSerializer.serializeContents(StdContainerSerializers.java:122)
    	org.codehaus.jackson.map.ser.std.StdContainerSerializers$IndexedListSerializer.serializeContents(StdContainerSerializers.java:71)
    	org.codehaus.jackson.map.ser.std.AsArraySerializerBase.serialize(AsArraySerializerBase.java:86)
    	org.codehaus.jackson.map.ser.StdSerializerProvider._serializeValue(StdSerializerProvider.java:610)
    	org.codehaus.jackson.map.ser.StdSerializerProvider.serializeValue(StdSerializerProvider.java:256)
    	org.codehaus.jackson.map.ObjectMapper._configAndWriteValue(ObjectMapper.java:2575)
    	org.codehaus.jackson.map.ObjectMapper.writeValueAsString(ObjectMapper.java:2097)
    	com.****.****web.controller.****WebRestController.getClassesByCourseId(****WebRestController.java:328)
    	sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
    	sun.reflect.NativeMethodAccessorImpl.invoke(Unknown Source)
    	sun.reflect.DelegatingMethodAccessorImpl.invoke(Unknown Source)
    	java.lang.reflect.Method.invoke(Unknown Source)
    	org.springframework.web.method.support.InvocableHandlerMethod.doInvoke(InvocableHandlerMethod.java:221)
    	org.springframework.web.method.support.InvocableHandlerMethod.invokeForRequest(InvocableHandlerMethod.java:137)
    	org.springframework.web.servlet.mvc.method.annotation.ServletInvocableHandlerMethod.invokeAndHandle(ServletInvocableHandlerMethod.java:110)
    	org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.invokeHandleMethod(RequestMappingHandlerAdapter.java:777)
    	org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.handleInternal(RequestMappingHandlerAdapter.java:706)
    	org.springframework.web.servlet.mvc.method.AbstractHandlerMethodAdapter.handle(AbstractHandlerMethodAdapter.java:85)
    	org.springframework.web.servlet.DispatcherServlet.doDispatch(DispatcherServlet.java:943)
    	org.springframework.web.servlet.DispatcherServlet.doService(DispatcherServlet.java:877)
    	org.springframework.web.servlet.FrameworkServlet.processRequest(FrameworkServlet.java:966)
    	org.springframework.web.servlet.FrameworkServlet.doGet(FrameworkServlet.java:857)
    	javax.servlet.http.HttpServlet.service(HttpServlet.java:634)
    	org.springframework.web.servlet.FrameworkServlet.service(FrameworkServlet.java:842)
    	javax.servlet.http.HttpServlet.service(HttpServlet.java:741)
    	org.apache.tomcat.websocket.server.WsFilter.doFilter(WsFilter.java:53)


**Solutions:**

 1. Use Join statement fetch from different tables
 2. Change FetchType.LAZY to FetchType.EAGER
 3. Config web.xml
 `<filter>  
  <filter-name>openSessionInView</filter-name>  
  <filter-class>org.springframework.orm.hibernate3.support.OpenSessionInViewFilter</filter-class>  
</filter>  
<filter-mapping>  
  <filter-name>openSessionInView</filter-name>  
  <url-pattern>/*</url-pattern>  
</filter-mapping> `
 4. In Service Layer call Hibernate.initialize()
