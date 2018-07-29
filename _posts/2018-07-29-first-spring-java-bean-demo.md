---
layout: post
title: "First Spring Java bean demo"
author: "Eddy Hu"
categories: journal
tags: [spring,java]
image: spring.png
---

#### applicationContext.xml

    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://www.springframework.org/schema/beans" xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.2.xsd ">
    
    	<bean  name="user" class="bean.User" ></bean>
    </beans>

#### User.java

    package bean;

    public class User {
	
	private String firstName;
	private String lastName;
	private Integer age;
	public String getFirstName() {
		return firstName;
	}
	public void setFirstName(String firstName) {
		this.firstName = firstName;
	}
	public String getLastName() {
		return lastName;
	}
	public void setLastName(String lastName) {
		this.lastName = lastName;
	}
	public Integer getAge() {
		return age;
	}
	public void setAge(Integer age) {
		this.age = age;
	}
    	
    }




#### Demo.java

```
package test.demo;

import org.junit.jupiter.api.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import bean.User;

public class Demo {
	
	@Test
	public void demo() {
		//create ApplicationContext object
		ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
		//get User object from Spring
		User user = (User) ac.getBean("user");
		//print user object
		System.out.println(user);
	}
}

```
