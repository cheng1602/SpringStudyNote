# Spring

## Dependency Injection

依赖注入，表示创建对象，给属性赋值

- di的实现语法有两种
  1. 在spring的配置文件中，使用标签和属性完成，叫做基于XML的di实现
  2. 在spring中的注解，完成属性赋值，叫做基于注解的di实现
  
- di的语法分类

  1. set注入（设值注入）：spring调用类的set方法，在set方法中实现属性的赋值

     - 简单类型的set注入

     ```xml
     <?xml version="1.0" encoding="UTF-8"?>
     <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
         <!-- 声明student对象
         注入：就是赋值的意思
         简单类型：spring中规定java的基本数据类型和String都是简单类型
         di：给属性赋值
         1. set注入(设值注入)：spring调用类的set方法，在set方法中实现属性的赋值
         - 简单类型的set注入
           <bean id="xx" class="xxx">
              <property name="属性名称" value="此属性的值" />
              <property......
              一个property只能给一个属性赋值
           </bean>
         -->
         <bean id="myStudent" class="org.example.package01.Student">
             <property name="name" value="张三"/>
             <property name="age" value="20"/>
         </bean>
     </beans>
     ```

     - 引用类型的set注入

     ```xml
     <?xml version="1.0" encoding="UTF-8"?>
     <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
         <!-- 声明student对象
         注入：就是赋值的意思
         简单类型：spring中规定java的基本数据类型和String都是简单类型
         di：给属性赋值
         1. set注入(设值注入)：spring调用类的set方法，在set方法中实现属性的赋值
         - 简单类型的set注入
           <bean id="xx" class="xxx">
              <property name="属性名称" value="此属性的值" />
              <property......
              一个property只能给一个属性赋值
           </bean>
         - 引用类型的set注入
           <bean id="xx" class="xxx">
              <property name="属性名称" ref="bean的id（对象的名称）" />
           </bean>
         -->
         <bean id="myStudent" class="org.example.package02.Student">
             <property name="name" value="张三"/>
             <property name="age" value="20"/>
             <!-- 引用类型 -->
             <property name="school" ref="mySchool"/>
         </bean>
         <!-- 声明School对象 -->
         <bean id="mySchool" class="org.example.package02.School">
             <property name="name" value="大学"/>
             <property name="address" value="湖南"/>
         </bean>
     </beans>
     ```

  2. 构造注入：spring调用类的有参数构造方法，创建对象，在构造方法中完成赋值

     ```xml
     <?xml version="1.0" encoding="UTF-8"?>
     <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
         <!--
         2. 构造注入：spring调用类的有参数构造方法，创建对象，在构造方法中完成赋值
     
         构造注入使用<constructor-arg>标签
         <constructor-arg>标签：一个<constructor-arg>表示构造方法的一个参数
         <constructor-arg>标签属性：
           - name：表示构造方法的形参名
           - index：表示构造方法的参数的位置，参数从左往右位置是0，1，2的顺序
           - value：构造方法的形参类型是简单类型的，使用value
           - ref：构造方法的形参类型是引用类型的，使用ref
         -->
     
         <bean id="myStudent" class="org.example.package03.Student">
             <constructor-arg name="name" value="张三"/>
             <constructor-arg name="age" value="20"/>
             <constructor-arg name="school" ref="mySchool"/>
         </bean>
     
         <!--使用index属性-->
         <bean id="myStudent2" class="org.example.package03.Student">
             <constructor-arg index="0" value="张三"/>
             <constructor-arg index="1" value="20"/>
             <constructor-arg index="2" ref="mySchool"/>
         </bean>
     
         <!-- 声明School对象 -->
         <bean id="mySchool" class="org.example.package03.School">
             <property name="name" value="大学"/>
             <property name="address" value="湖南"/>
         </bean>
     </beans>
     ```

  - 引用类型的自动注入

    spring框架根据某些规则可以给引用类型赋值

    - 常用使用规则

      1. `byName`（按名称注入）

         Java类中引用类型的属性名和spring容器中（配置文件）`<bean>`的id名称一样，且数据类型是一致的，这样的容器中的bean，spring能给赋值给引用类型

         - 语法规则：

           `<bean id="xx" class="xxx" autowire="byname">...</bean>` 

         ```xml
         <?xml version="1.0" encoding="UTF-8"?>
         <beans xmlns="http://www.springframework.org/schema/beans"
                xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
         
             <bean id="myStudent" class="org.example.package02.Student" autowire="byName">
                 <property name="name" value="张三"/>
                 <property name="age" value="20"/>
                 <!-- 引用类型 -->
                 <!-- <property name="school" ref="school"/> -->
             </bean>
             <!-- 声明School对象 -->
             <bean id="school" class="org.example.package02.School">
                 <property name="name" value="大学"/>
                 <property name="address" value="湖南"/>
             </bean>
         </beans>
         ```

      2. `byType`（按类型注入）

         Java类中引用类型的属性名和spring容器中（配置文件）`<bean>`的class属性是同源的关系，这样的容器中的bean，spring能给赋值给引用类型

         - 同源：同一类

           1. Java类中引用类型的数据类型和bean的class的值是一样的
           2. Java类中引用类型的数据类型和bean的class的值是父子类关系的
           3. Java类中引用类型的数据类型和bean的class的值是接口和实现类关系的

         - 语法规则：

           `<bean id="xx" class="xxx" autowire="byType">...</bean>` 

         ```xml
         <?xml version="1.0" encoding="UTF-8"?>
         <beans xmlns="http://www.springframework.org/schema/beans"
                xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
         
             <bean id="myStudent" class="org.example.package02.Student" autowire="byType">
                 <property name="name" value="张三"/>
                 <property name="age" value="20"/>
                 <!-- 引用类型 -->
                 <!-- <property name="school" ref="school"/> -->
             </bean>
             <!-- 声明School对象 -->
             <bean id="school" class="org.example.package02.MiddleSchool">
                 <property name="name" value="大学"/>
                 <property name="address" value="湖南"/>
             </bean>
         </beans>
         ```




- 多个配置文件

  - 优势

    1. 每个文件的大小笔一个文件要小很多，效率高
    2. 避免多人竞争带来的冲突

  - 多文件的分配方式

    1. 按功能模块，一个模块一个配置文件
    2. 按类的功能，数据库相关的配置一个配置文件，做事务的功能一个配置文件，做service的功能一个配置文件等

  - 包含关系的配置文件

    主配置文件一般不定义对象，但包含其他的配置文件

    - 语法

      `<import respirce="其他配置文件路径" />`

    - 关键字：`"classpath:"`

      表示类路径（class文件所在的目录），在spring的配置文件中要指定其他文件的位置，需要使用`classpath`告诉spring到哪儿去加载读取文件

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
        <!--spring-total 主配置文件-->
        <import resource="classpath:package02/spring-school.xml"/>
        <import resource="classpath:package02/spring-student.xml"/>
    </beans>
    ```

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    
        <bean id="myStudent" class="org.example.package02.Student" autowire="byType">
            <property name="name" value="张三"/>
            <property name="age" value="20"/>
        </bean>
    </beans>
    ```

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
        <!-- 声明School对象 -->
        <bean id="school" class="org.example.package02.MiddleSchool">
            <property name="name" value="大学"/>
            <property name="address" value="湖南"/>
        </bean>
    </beans>
    ```

    - 通配符

      `（*：表示任意字符）`

    ```xml
        <!--spring-total 主配置文件-->
        <import resource="classpath:package02/spring*.xml"/>
    ```

    



- （补充）junit：单元测试，一个工具类库，做测试方法使用

  单元：指定的是方法，一个类中又很多方法，一个方法称为单元

  - 使用步骤

    1. 加入junit依赖

       ```xml
       <dependency>
             <groupId>junit</groupId>
             <artifactId>junit</artifactId>
             <version>4.11</version>
             <scope>test</scope>
           </dependency>
       ```

    2. 创建测试作用的类（测试类）

       创建在`src/test/java`目录中

    3. 创建测试方法

       1. public 方法
       2. 没有返回值 voide
       3. 方法名称自定义，建议使用test+你要测试的方法
       4. 方法没有参数
       5. 方法上面加入`@Test`，这样的方法是可以直接执行的，不用使用main方法