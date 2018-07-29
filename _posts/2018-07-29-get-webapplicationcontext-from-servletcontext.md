---
layout: post
title: "Get WebApplicationContext from ServletContext"
author: "Eddy Hu"
categories: journal
tags: [spring,java]
image: spring-runtime.png
---

#### applicationContext.xml

   

     <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://www.springframework.org/schema/beans" xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.2.xsd ">
    
    	<!-- Dao -->
    	<bean name="customerDao" class="cn.itheima.dao.impl.CustomerDaoImpl" ></bean>
    	<bean name="userDao" class="cn.itheima.dao.impl.UserDaoImpl" ></bean>
    	<!--Service -->
    	<bean name="customerService" class="service.impl.CustomerServiceImpl" >
    		<property name="customerDao" ref="customerDao" ></property>
    	</bean>
    	<bean name="userService" class="service.impl.UserServiceImpl" >
    		<property name="userdao" ref="userDao" ></property>
    	</bean>
    </beans>



#### web.xml

        <?xml version="1.0" encoding="UTF-8"?>
    <web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" id="WebApp_ID" version="2.5">
      <display-name>demo</display-name>
      <!-- listener-->
      <listener>
      	<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
      </listener>
      <!-- load spring -->
      <context-param>
      	<param-name>contextConfigLocation</param-name>
      	<param-value>classpath:applicationContext.xml</param-value>
      </context-param>
    
      <!-- struts2 filter -->
      <filter>
      	<filter-name>struts2</filter-name>
      	<filter-class>org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter</filter-class>
      </filter>
      <filter-mapping>
      	<filter-name>struts2</filter-name>
      	<url-pattern>/*</url-pattern>
      </filter-mapping>
    </web-app>

#### CustomerAction.java

    import java.util.List;
    import javax.servlet.ServletContext;
    import org.apache.commons.lang3.StringUtils;
    import org.apache.struts2.ServletActionContext;
    import org.hibernate.criterion.DetachedCriteria;
    import org.hibernate.criterion.Restrictions;
    import org.springframework.context.ApplicationContext;
    import org.springframework.context.support.ClassPathXmlApplicationContext;
    import org.springframework.web.context.WebApplicationContext;
    import org.springframework.web.context.support.WebApplicationContextUtils;
    import com.opensymphony.xwork2.ActionContext;
    import com.opensymphony.xwork2.ActionSupport;
    import com.opensymphony.xwork2.ModelDriven;
    import domain.Customer;
    import service.CustomerService;
    
    public class CustomerAction extends ActionSupport implements ModelDriven<Customer> {
    	private Customer customer = new Customer();
    	
    	public String list() throws Exception {
    	    //1 get servletContext
    	    ServletContext sc = ServletActionContext.getServletContext();
    	    //2.get WebApplicationContext from ServletContext
    	    WebApplicationContext ac = WebApplicationContextUtils.getWebApplicationContext(sc);
    	    //3.get CustomerService
    	    CustomerService cs = (CustomerService) ac.getBean("customerService");
    		//-----------------------------------------------------------------
    		//1 get parameter
    		String cust_name = ServletActionContext.getRequest().getParameter("cust_name");
    		//2 create DetachedCriteria 
    		DetachedCriteria dc =DetachedCriteria.forClass(Customer.class);
    		//3 validate
    		if(StringUtils.isNotBlank(cust_name)){
    			dc.add(Restrictions.like("cust_name", "%"+cust_name+"%"));
    		}
    		//4 get all
    		List<Customer> list = cs.getAll(dc);
    		//5 redirectory to list.jsp			        
    		ServletActionContext.getRequest().setAttribute("list", list);
    		// put it to ActionContext
    		ActionContext.getContext().put("list", list);
    		
    		return "list";
    	}
    
    	
    	//Add customer
    	public String add() throws Exception {
    	    //1 get servletContext
    	    ServletContext sc = ServletActionContext.getServletContext();
    	    //2.get WebApplicationContext from ServletContext
    	    WebApplicationContext ac = WebApplicationContextUtils.getWebApplicationContext(sc);
    	    //3.get CustomerService
    	    CustomerService cs = (CustomerService) ac.getBean("customerService");
    	//----------------------------------------------------------------
    		//1 save customer
    		cs.save(customer);
    		//2 redirectory to action
    		return "toList";
    	}

    	@Override
    	public Customer getModel() {
    		return customer;
    	}	
    }




