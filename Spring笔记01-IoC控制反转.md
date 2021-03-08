# Spring

spring,springmvc,springboot,springcloud

- Spring简介

  Spring出现于2002年左右，解决企业级开发的难度，减轻对项目模块之间的管理，类和类之间的管理，帮助开发人员创建对象，管理对象之间的关系

  Spring核心技术：ioc（控制反转），aop（面向切面编程）。能实现模块之间、类之间的解耦合

  依赖：classA中使用了classB的属性或者方法，叫做classA依赖classB

  官网：https://spring.io/



## IoC 控制反转

Inversion of Control

- 简述

  控制反转，是一个理论，概念，思想

  把对象的创建，赋值，管理工作都交给代码之外的容器实现，也就是对象的创建是由其他外部资源完成

  - 控制：创建对象，对象的属性赋值，对象之间的关系管理

  - 反转：把原来开发人员管理对象，创建对象的权限转移给代码之外的容器实现，由容器代替开发人员管理对象，创建对象

    （正转：由开发人员在代码中，使用new构造方法创建对象，开发人员主动管理对象）

    （容器：服务器软件或者框架）

  使用IoC目的在于减少对代码的改动也能实现不同的功能，实现解耦合

  

- IoC的技术实现

  DI(Dependency Injection)依赖注入是IoC的技术实现，

  至于对象如何在容器中创建，赋值，查找都由容器内部实现

  Spring使用了DI实现了IoC的功能，Spring底层创建对象，使用的是反射机制



- Spring的第一次练习

  Spring的IoC由spring创建对象

  - 实现步骤

    1. 创建maven项目

    2. 加入maven依赖

       spring依赖

       junit依赖

    3. 创建类（接口和其他使用类）

       和没有使用框架一样，就是普通的类

    4. 创建spring需要使用的配置文件

       声明类的信息，这些类由spring创建和管理

    5. 测试spring创建的对象

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd">
        <!--
        告诉spring创建对象
        声明bean，就是告诉spring要创建某个类的对象
        id：对象的自定义名称，唯一值，spring通过这个名称找到对象
        class：类的全限定名称（不能是接口，因为spring是反射机制创建对象，必须使用类）
    
        spring就完成 OneService oneService = new OneServiceImpl();
        spring是把创建好的对象放入到map中，spring框架有一个map存放对象
          springMap.put(id的值,对象);
          例如：springMap.put(oneService,new OneServiceImpl());
    
        一个bean标签声明一个对象
        -->
        <bean id="oneService" class="org.example.service.impl.OneServiceImpl"/>
    
        <!--  spring创建一个非自定义类的、存在的某个类的对象  -->
        <bean id="mydate" class="java.util.Date" />
    
    </beans>
    ```

    - Spring的配置文件
      1. beans：根标签，spring把java对象称为bean
      2. spring-beans.xsd：约束文件，与mybatis之中的dtd是一样的

    ```java
    package org.example;
    
    import org.example.service.OneService;
    import org.example.service.impl.OneServiceImpl;
    import org.junit.Test;
    import org.springframework.context.ApplicationContext;
    import org.springframework.context.support.ClassPathXmlApplicationContext;
    
    import java.util.Date;
    
    public class myTest {
        @Test
        //正转
        public void test01() {
            OneService service = new OneServiceImpl();
            service.doSome();
        }
    
        @Test
        //反转
        public void test02() {
            //使用spring容器创建的对象
            //1.指定spring配置文件的名称
            String config = "beans.xml";
            //2.创建表示spring容器的对象，ApplicationContest
            //ApplicationContest就是表示spring容器，通过容器获取对象
            //ClassPathXmlApplicationContext表示从类路径中加载spring的配置文件
            ApplicationContext ac = new ClassPathXmlApplicationContext(config);
    
            //从容器中获取某个对象，你要调用的方法
            //getBean("配置文件中的bean的id值")
            OneService service = (OneService) ac.getBean("oneService");
    
            //使用spring创建好的对象
            service.doSome();
    
            //spring默认创建对象的事件
            //在创建spring的容器时，会创建配置文件中的所有对象
        }
    
        @Test
        public void test03() {
            String config = "beans.xml";
            ApplicationContext ac = new ClassPathXmlApplicationContext(config);
            //使用spring提供的方法，获取容器中定义的对象的数量
            int nums = ac.getBeanDefinitionCount();
            System.out.println("容器中定义的对象数量：" + nums);
            //容器中每个定义的对象名称
            String names[] = ac.getBeanDefinitionNames();
            for (String name : names) {
                System.out.println("对象名称：" + name);
            }
    
        }
    
        @Test
        //获取一个非自定义的类对象
        public void test04() {
            String config = "beans.xml";
            ApplicationContext ac = new ClassPathXmlApplicationContext(config);
            //使用getBean();
            Date md = (Date) ac.getBean("mydate");
            System.out.println("Date:"+md);
        }
    }
    ```

    

    

