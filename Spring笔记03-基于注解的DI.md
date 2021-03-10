# Spring

## Dependency Injection

- 基于注解的DI

  通过注解完成Java对象的创建、属性赋值

  - 使用注解的步骤

    1. 加入maven的依赖spring-context，在加入spring-context的同时，间接加入spring-aop的依赖，使用注解必须使用spring-aop依赖
    2. 在类中加入spring的注解（多个不同功能的注解)
    3. 在spring的配置文件中，加入一个组件扫描器的标签，说明注解在你的项目中的位置

  - 常用注解

    1. 创建对象的注解

       - @Component

         创建对象，等同于<bean>的功能

         - 属性：value 就是对象的名称，也就是bean的id值，value的值是唯一的，创建对象在整个spring容器中只有一个
         - 位置：在类的上面

         ```java
         package org.example.p1;
       
         import org.springframework.stereotype.Component;
       
         @Component(value = "myStudent")
         //等同于<bean id="myStudent" class="org.example.p1.Student" />
         public class Student {
             private String name;
             private Integer age;
       
             public void setName(String name) {
                 this.name = name;
             }
       
             public void setAge(Integer age) {
                 this.age = age;
             }
       
             @Override
             public String toString() {
                 return "Student{" +
                         "name='" + name + '\'' +
                         ", age=" + age +
                         '}';
             }
         }
         ```

         - 组件扫描器（component-scan）

           组件就是Java对象

           - base-package

             指定注解在你的项目中的包名

           - component-scan工作方式
           - spring会扫描遍历base-package指定的包，把包和子包中的所有类，找到类中的注解，按照注解的功能创建对象，给属性赋值

         - 注解的几种方式

           ```java
           //@Component(value = "myStudent")
           //等同于<bean id="myStudent" class="org.example.p1.Student" />
           //省略value
           //@Component("myStudent")
           //不指定对象名称，由spring提供默认名称，默认名称为类名的小写
           @Component
           public class Student {}
           ```

         - 指定多个包的方式

           1. 使用多次组件扫描器，指定不同的包

              ```xml
                  <context:component-scan base-package="org.example.p1"/>
                  <context:component-scan base-package="org.example.p2"/>
              ```

           2. 使用分隔符 , 或 ; 分隔

              ```xml
                  <context:component-scan base-package="org.example.p1;org.example.p2"/>
              ```

           3. 指定父包

              ```xml
                  <context:component-scan base-package="org.example"/>
              ```

       - spring中和@Componet功能一致，创建对象的注解还有

         1. @Respotory（用在持久层类的上面）
            放在dao的实现类上面，表示创建dao对象，dao对象时能访问数据库的

         2. @Service（用在业务层类上面）

            放在service的实现类上面，创建service对象，service对象是做业务处理，可以有事务等的功能

         3. @Controller（用在控制器的上面）

            放在控制器（处理器）类的上面，创建控制器对象的，控制器对象，能够接受用户提交的参数，显示请求的处理结果

         *以上三个注解的使用语法和@Componet一致，都能创建对象，但是三者还有额外的功能*

         *@Respotory、@Service、@Controllert是给项目的对象分层的*

    2. 属性赋值的注解

       - @Value

         简单类型的属性赋值

         - 属性

           value是String类型的，表示简单类型的属性值

         - 位置

           1. 在属性定义的上面，无需set方法，推荐使用

              ```java
                  @Value(value = "张三")
                  private String name;
                  @Value(value = "20")
                  private Integer age;
              ```

              ```java
                  //简略写法
                  @Value("张三")
                  private String name;
                  @Value("20")
                  private Integer age;
              ```

           2. 在set方法的上面

              ```java
                  @Value("张三")
                  public void setName(String name) {
                      this.name = name;
                  }
            
                  @Value("20")
                  public void setAge(Integer age) {
                      this.age = age;
                  }
              ```

       - @Autowired

         引用类型的属性赋值

         - spring框架提供的注解，实现引用类型的赋值，使用的是自动注入的原理，支持byName、byType，默认使用的是byType自动注入

         - 位置：同@Value笔记

         - 使用byName方式

           1. 在属性的上面加入`@Autowired`

           2. 在属性的上面加入`@Qualifier(value = "bean的id")`

              表示使用指定名称的bean完成赋值

           ```java
               @Autowired
               @Qualifier("school")
               private School school;
           ```

         - require属性

           是一个Boolean类型，默认为true

           require-true：表示引用类型赋值失败，程序报错，并终止执行

           require-false：引用类型如果赋值失败，程序正常执行，引用类型是null

       - @Resource

         引用类型的属性赋值

         - 来自JDK中的注解，spring框架提供了对这个注解的功能支持，可以使用它给引用类型赋值，使用的也是自动注入原理，支持byName、byType，默认使用的是byName自动注入
         - 位置：同@Value笔记
         - 默认为byName，先使用byName自动注入，如果byName自动赋值失败，再使用byType，若要只使用byName，需要增加一个属性name，name的值是bean的id


