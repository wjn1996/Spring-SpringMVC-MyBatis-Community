# Spring-SpringMVC-MyBatis-Community
《开源软件设计与开发》课程作业——开放前沿SSM框架
姓名：王嘉宁，学号：51195100016
## SSM community
## SSM框架简介
&emsp;&emsp;SSM全称为spring，springmvc，mybatis框架，是基于j2ee的新型WEB开源开发框架，其基于MVC模式架构，充分提现高内聚，低耦合的设计原则，已有诸多应用使用基于SSM框架开发。mvc框架是在ssh框架基础上演变而来，解决hibernate门槛高，成本高，效率低等缺点。

&emsp;&emsp;Spring是一个开源框架，Spring是于2003年兴起的一个轻量级的Java开发框架，由Rod Johnson在其著作《Expert One-On-One J2EE Development and Design》中阐述的部分理念和原型衍生而来。它是为了解决企业应用开发的复杂性而创建的。Spring使用基本的 JavaBean 来完成以前只可能由 EJB 完成的事情。然而，Spring 的用途不仅限于服务器端的开发。从简单性、可测试性和松耦合的角度而言，任何 Java 应用都可以从Spring中受益。简单来说， Spring是一个轻量级的控制反转（IoC）和面向切面（AOP）的容器框架。

&emsp;&emsp;Spring MV属于SpringFrameWork的后续产品，已经融合在Spring Web Flow面。 Spring MVC分离了控制器、模型对象、分派器以及处理程序对象的角色，这种分离让它们更容易进行定制。

&emsp;&emsp;MyBatis本是apache的一个开源项目iBatis，2010年这个项目由apache software foundation迁移到了google code，并且改名为MyBatis。MyBatis是一个基于Java的持久层框架。iBATIS提供的持久层框架包括SQL Maps和Data Access Objects（DAO）MyBatis  消除了几乎所有的 JDBC 代码和参数的手工设置以及结果集的检索。MyBatis使用简单的XML或注解用于配置和原始映射，将接口和Java的POJOs（Plain Old Java Objects，普通的  Java对象）映射成数据库中的记录。
## 我与SSM项目
&emsp;&emsp;SSM开源开发框架提出三年里，以其轻量级快速部署的特点，成为web应用主流前沿技术。本人也因此尝试使用SSM开发两款应用

&emsp;&emsp;（1）基于SSM的图像标注管理平台。这个系统是属于外包类系统，因此无法开源，目前挂在互站网上出售：https://www.huzhan.com/code/goods369780.html

&emsp;&emsp;（2）教育知识图谱：这个系统也是基于SSM开发的教育知识图谱构建工具，目前处于开发阶段，网址是：http://47.100.79.2/gitedutol/admin/login
### SSM部署

&emsp;&emsp;在eclipse集成工具中新建项目，在web.xml中配置项目环境，添加springmvc-config.xml配置，由于SSM是基于注解的MVC，因此需要在该文件内注册控制器（controller），调用注解包（annotation）以及处理handler，同时需要配置前端viewer的地址前后缀。另外还需要配置applicationContext.xml，该文件主要用于绑定数据源，以及声明一些数据模型和应用参数。

&emsp;&emsp;在创建完项目后，运行将项目添加至tomcat管理列表中，并启动tomcat，项目便正是的部署在本地服务器（Localhost）上。
### 编写第一个Controller

&emsp;&emsp;SSM框架中，当用户发起访问请求时，首先是由之前配置文件中所定义的将request抛给控制器Controller，因此需要编写Controller。Controller主要接受用户发起的请求，并在此设计业务逻辑。以用户登录注册为例，首先创建一个类UserController.class并使用@Controller注解声明当前类为控制器，其次编写控制器方法，通过注解@RequestMapping方式限定接受的请求源路径，并获取传入的参数。

&emsp;&emsp;控制器是连接视图Viewer和模型Model的中间件，因此需要获取发送请求的视图ModelAndView，一般情况下根据业务要求，还会获得当前的HttpSession对象以及HttpRequest或HttpResponse。

&emsp;&emsp;控制器依然可以作为包括Get方法或Post方法的请求，也支持AJAX异步请求，请求方法必须在@RequestMapping参数列表中申明。

&emsp;&emsp;控制器函数依然需要返回值，通常对于非异步请求，Controller函数的返回值是一个视图ModelAndView对象，亦即返回整个包含数据模型的视图，具体的业务操作都是在Controller函数体中通过业务逻辑和数据库访问共同完成的，因此再返回视图时，需要传输业务需要的参数。例如执行登录操作，控制器将接受指定Post方法的请求，且传入带有用户名和密码的参数，在控制器中首先进行判空检验，然后通过自动注入的数据访问服务实现数据库增删改查，判断当前用户与密码是否匹配，如果匹配成功则任务用户登录成功，通过返回指定JSP文件（例如用户中心）的视图并传递“登录成功”参数，同时将当前用户写入Session中；若登录失败，则返回另一个指定JSP的视图文件（例如返回当前的登录界面），并传递“登录失败”参数。
### 定义数据模型——JB
&emsp;&emsp;数据模型是Java的数据核心，通常称之为JavaBean（JB），其是一个包含若干私有属性和公有函数的可继承类，通常JavaBean的私有属性与数据库中对应数据表的各个字段名称即数据类型保持一致，公有函数则是基于私有属性的一系列操作，包括get()和set()方法，用于对这个数据模型进行增删改查。因此整个业务均是以JB展开的。

&emsp;&emsp;换句话说，如果需要对用户表User修改某一个用户的密码，则首先获得当前用户的数据UserDomain，其包含这个用户的所有信息，以及对这些信息操作的get和set方法，其次调用set方法重置这个密码，最后通过控制器与数据库访问将这个数据传入到数据库中。
### 数据库操作
&emsp;&emsp;SSM框架最有优势的一点是方便快捷的数据库操作，其不需要程序员花大量时间编写数据库连接和配置语句，而只关心SQL语句的设计与编写，这边是MyBatis的优势所在。MyBatis有两种策略，一种基于XML的数据库执行，一种是基于@注解的数据库执行。XML注解方法通常将执行的数据表操作放置XML标签中，具体的SQL语句则作为XML的值，MyBatis获得XML文件后通过XPath解析获得相应的数据库操作，再通过底层与数据库创建连接；基于@注解的方式相比XML要简单，只需要创建关于数据库操作的类，并编写一些类SQL语句即可，例如执行查询操作，只需编写@Select(“select * from user”)即可表示当前的数据操作，相应的函数则为传入的参数，返回值则是对应的JavaBean。

&emsp;&emsp;通常MyBatis是无法直接被Controller调用的，需要通过服务接口实现。因此这里需要定义具体操作的接口类Interface以及对应的实现类Implement。接口类是申明所有对数据库进行操作的业务方法，在接口中无须实现具体的业务逻辑，对应的实现类中则需要实例化所有接口中的方法，并在此处需要获得MyBatis中的数据。通常Implement除了可以获得MyBatis中的数据外，也可以在此编写简单的业务逻辑。需要说明的是，Controller获得数据库的数据需要将Implement实现类自动注入到Controller中，且只能调用对应的方法，间接的操作数据库。
## SSM的未来
&emsp;&emsp;众所周知，在SSM框架出来之前，一直统治者JavaWeb开发领域的是Structs，SSH框架，以及后来的SpringBoot，而SSM框架则是有机结合了Spring、SpringMVC以及改进了Hibernate的MyBatis，它的面向切面的编程和控制反转机制开拓了新的开发思想，当然SSM框架也是三年前的产物，现如今Dubbo、SpringCloud，以及一些支持分布式的FastdfsSpring boot、Dubbo+zookeeper等，时刻更新着开发框架。不过以Java自身的优势（框架完全以Jar包为配置源、自带的JVM等），SSM框架在近些年来依然不会落伍，就好像现阶段依然有许多应用是基于SSH、甚至是Spring+Structs2等框架开发，因此各个框架均有适用于具体业务背景的地方，SSM也因如此。

&emsp;&emsp;最后附上我们的SSM项目开发的系统界面
![教育知识图谱构建工具](https://github.com/wjn1996/Spring-SpringMVC-MyBatis-Community/blob/master/images/%E5%9B%BE%E7%89%871.png)
![教育知识图谱构建工具](https://github.com/wjn1996/Spring-SpringMVC-MyBatis-Community/blob/master/images/%E5%9B%BE%E7%89%872.png)
![图像标注管理系统](https://github.com/wjn1996/Spring-SpringMVC-MyBatis-Community/blob/master/images/%E6%88%AA%E5%8F%96%E5%9B%BE%E7%89%87%E5%B9%B6%E6%A0%87%E6%B3%A8.jpg)
![图像标注管理系统](https://github.com/wjn1996/Spring-SpringMVC-MyBatis-Community/blob/master/images/%E7%AE%A1%E7%90%86%E5%91%98%E6%9F%A5%E8%AF%A2%E4%BB%BB%E5%8A%A1%E5%AE%8C%E6%88%90%E6%83%85%E5%86%B5.jpg)

