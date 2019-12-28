# 《01(Servlet)Servlet快速入门_》

创建module时

Java EE version: Java EE7

Version:如果这个版本是2.5 必须勾选create web.xml 如果是3.0以上可以不勾选

勾选：create web.xml

![](img\创建web项目步骤.png)

上面只是建好一个module而已必须要把它放到tomcat去访问 所以去调菜单见下图



![](img\配置服务器操作.png)

![](img\项目放进服务器操作.png)

**做完以上步骤启动Tomcat 这个项目也不会发布到Tomcat里面去**然后看下面操作点击file-project structure选中WEB02:war exploded(要操作的目标)的outdirectory输出路径放到Tomcat目录的webapps里面去把这个文件改为WEB02  例:Tomcat/webapps/WEB详情见图片

F:\code\out\artifacts\WEB02_war_exploded

![](img\输出目录.png)



以上就完成了idea 部署发布项目操作

**Servlet快速入门步骤**

- 定义类实现接口Servlet
- 重写接口中的抽象方法
- ![image-20191204134059541](C:\Users\Aaron\AppData\Roaming\Typora\typora-user-images\image-20191204134059541.png)
- （通知tomcat运行程序）写web.xml配置文件（tomcat不知道类是什么所以在web.xml进行一个配置）

![](img\image-20191204134059541.png)

name第一个随便起

servlet-class类全路径

第一个name随便起 第二个name跟第一个一样即可

配置浏览器地址

![](img\图标识别.png)



# 《02(Servlet)Servlet概述_》

Servlet运行在服务端的Java小程序,是sun公司提供一套规范，用来处理客户端请求、响应给浏览器的动态资源.

Servlet是十三规范中的一个规范，Servlet是JavaEE技术平台的规范，只能运行在WEB服务器（Tomcat）

作用:接收请求,进行响应

Servlet是**JavaWeb三大组件之一**（Servlet、Filter、Listener）最为重要

**Servlet的任务有:**

1：获取请求数据

2：处理请求

3 ：完成响应

广义的Servlet:Servlet接口所有实现类(一般说的都是广义的)

狭隘的Servlet:专门指的是Servlet接口（光一个接口是运行不了的）

《03(Servlet)Servlet执行过程原理_》

浏览器请求http://localhost:8080/WEB02/quick是tomcat引擎接收的(而不是web项目去接收的)

# 《03(Servlet)Servlet执行过程原理_》

![](img\Servlet程序执行原理.bmp)

**Tomcat引擎：**

Reponse对象根据service方法一结束就判断写完数据了

1:接收并解析客户端请求 /WEB02/quick  （不要域名端口号）请求拿到访问的地址

2:（Tomcat引擎里面）创建两个对象request，response

3:在webapps找到访问的资源 com.itheima.servlet.QuickServlet

(/quit找到配置文件上面的第2个名字name--通过第二个name找到上面第一个name--第一个name通过name找到了class)

![](img\web配置文件解析.png)

![](img\servlet执行流程.png)

4:Tomcat利用反射技术创建类对象例如:

```java
Class.forName("com.itheima.servlet.QuickServlet").newInstance();
```

*5:调用对象中的方法 service,传递参数 request，response(**注意是Tomcat引擎调用的service方法 而不是我们调用的我们没有写任何代码去调用service**)`*

6:数据写在了response对象的缓冲区(没有直接写到浏览器)

7:service方法结束，数据从response对象缓冲区取出,组装一个HTTP的响应信息，统一回传浏览器

# 《04(Servlet)Servlet对象生命周期1_》

Servlet对象什么时候生，什么时候死

生命周期相关的三个方法,init,service,destroy

​    1.第一次调用时，将执行初始化方法：init(ServletConfig)

​    2.每一次调用，都将执行service(ServletRequest，ServletResponse)方法

​    3.服务器关闭，或项目移除：destroy()方法

- init(ServletConfig config)Servlet对象的初始化方法,对象被创建的时候调用
- service(Request，Response)客户端访问一次，执行一次
- destory（）Servlet对象销毁之前时候调用
- Servlet对象什么时候被销毁：

  ​           1:停止tomcat服务

  ​           2:WEB项目从服务器移除
- Servlet对象什么时候被创建? 
  - Servlet默认第一次访问的时候,对象被创建只在对象创建的时候调用一次(之后再怎么刷新都不会再调用了)
  - ![](img\浏览器访问创建对象.png)
  - Tomcat服务器启动的时候创建对象 需要修改web.xml(一般不配置)  添加<load-on-startup>5</load-on-startup><!--里面写任意整数--><!--    配置的是Servlet对象启动优先级    可以写任意整数:数字越小,Servlet启动优先级越高-->
  - ![](img\配置webxml启动tomcat创建独享演示.png)

​                 

**init()只会被调用一次成功后 不会被调第二次了**

# 《05(Servlet)重启动服务器_》



apache-tomcat-8.5.32\conf\context.xml

 <WatchedResource>WEB-INF/web.xml</WatchedResource><!--监视web.xml有改变就重启服务-->

# 《06(Servlet)生命周期2_》

见---《04(Servlet)Servlet对象生命周期1_》



![](img\idea移除项目.png)

# 《07(Servlet)web.xml配置详解_》



```xml
 <!--
      Servlet的详细配置
    -->
    <servlet>
        <!--name，定义名字，随意-->
        <servlet-name>path</servlet-name>
        <!--class 配置类的全名，给Tomcat利用反射技术创建类对象使用的-->
        <servlet-class>com.itheima.servlet.PathServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <!--name 必须和上面的name相同-->
        <servlet-name>path</servlet-name>
        <!--url-pattern 配置的是浏览器访问的虚拟路径 /开头
        路径不存在tomcat的webapps里面的 ，给浏览器用的只要浏览器输入的是/path就能对上 tomcat反射建对象
        -->
        <url-pattern>/path</url-pattern>
    </servlet-mapping>
```



url-pattern的配置

- 完全匹配

    /abc:浏览器地址栏必须写/abc

- 目录匹配

   /aaa/bbb/*：浏览器地址栏可以写 /aaa/bbb/任意

- 后缀名匹配

  *.abc:浏览器地址可以写 任意.abc

  目录匹配和后缀名匹配不能同时使用有冲突

# 《08(Servlet)全局的web.xml配置_》(了解)

Tomcat目录中/conf/web.xml是全局配置文件，所有的WEB项目都使用

自己写的web.xml只有你自己的项目使用

当全局配置web.xml和自己配置的web.xml冲突了，就听自己的，没有就听全局的



```xml
<servlet>
    <servlet-name>default</servlet-name>
    <servlet-class>org.apache.catalina.servlets.DefaultServlet</servlet-class>
    <init-param>
        <param-name>debug</param-name>
        <param-value>0</param-value>
    </init-param>
    <init-param>
        <param-name>listings</param-name>
        <param-value>false</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup><!--tomcat启动那个对象就被创建-->
</servlet>
```




只要都没匹配上就会跟这个匹配上

![ m](img\全局webxml配置1.png)

如果没匹配到这个接盘侠响应给浏览器404见上图



看全局web.xml

```xml
<session-config>
    <session-timeout>30</session-timeout><!--session超时时间 30分钟到session就会销毁没有-->
</session-config>
```


![](img\全局webxml2.png)

![](img\全局webxml3.png)





# 《09(Servlet)IDEA创建Servlet_》



![](img\idea创建Servlet.jpg)



```
/**
 * 继承的方式创建
 * 继承HttpServlet
 */
public class MyServlet extends HttpServlet {
    /**
     * doGet方法对应了客户端浏览器的提交方式
     */
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.getWriter().print("RUNING!!!!!!!!!!!!!!!!");
    }

    /**
     * doPost方法对应了客户端浏览器的提交方式POST
     * 无论:是POST提交，还是GET,统一处理，都使用doGet
     */
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }
}
```



# ?《10(Servlet)继承HttpServlet原理_》



在访问的时候Tomcat引擎不认识什么叫post什么是get(tomcat只认识service方法)

servlet是JavaEE规范，Servlet接口中的标准方法service方法

![](img\继承httpservlet原理.bmp)

# 《11(Servlet)修改Servlet模版_》

![](img\servlet模板修改.png)



# 《12(Servlet)ServletConfig对象_》

ServletConfig对象，表示的是Servlet配置对象

有一个Servlet程序，出现对应的ServletConfig

- 获取ServletConfig对象
  
-   init方法参数(ServletConfig servletConfig)方法调用者是tomcat引擎，参数ServletConfig对象是由Tomcat创建，因此在父类中定义一个方法ServletConfig  getServletConfig()
  
- 获取ServletConfig对象

- ServletConfig对象能做什么

  -   能获取Servlet名字 String     getServletName()

    ```xml
     <servlet-name>config</servlet-name>                                       
    ```

    *获取Servlet的初始化参数String  getInitParamter(“键名”) 需要先在web.xml配置的init标签

    ```xml
        <servlet>
            <servlet-name>config</servlet-name>
            <servlet-class>com.itheima.servlet.ConfigServlet</servlet-class>
            <init-param>
                <param-name>heima</param-name><!--方法中的键-->
                <param-value>Java</param-value><!--方法中的值-->
            </init-param>
        </servlet>
    ```

    - 获取ServletContext对象 getServletContext() 返回Servlet上下文对象(对象是一个接口类型后面讲)

      # 

      # 

      

# 《13(Servlet)登录案例的流程_》

编程思想:不要一做案例就想代码怎么写，所以先把功能流程搞清楚在写，流程能跑通，代码就很容易了

![](img\(Servlet)登录案例的流程.bmp)

# 《14(Servlet)登录案例数据库表创建_》

**实现步骤**

- 创建数据库表MYSQL用关键字做名字加单引号防止阅读混淆

```mysql
USE web02;
SELECT * FROM USER;
SELECT * FROM USER WHERE username='tom' AND PASSWORD='123';
/*
    dbutils 使用哪个结果集(一共有九种方式其他5个用处不大)
    BeanHandler       查询结果一个JavaBean
    BeanListHandler   查询结果集是多个JavaBean，存储List集合
    ScanlarHandler    单项值查询(比如聚合函数count就适合这个结果集)
    ColumnListHandler 查询一个列数据存储集合List<Object>

*/
```

- 创建WEB项目

  - 添加必要的jar  c3p0需要一个配置文件xml放在src目录下
  - ![](img\需要的jar包dbutils_c3p0_数据库驱动.png)

  ![](img\c3p0-config_xml.png)

   

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <c3p0-config>
  	<default-config>
  	    <!-- 
  	         c3p0-config.xml
  	         本文件xml放在src目录下
  	         property标签,配置数据库连接四大信息 
  	         name属性,要配置什么,标签体,配置的实际内容
  	         改写自己的数据库信息
  	    -->
  		<property name="driverClass">com.mysql.jdbc.Driver</property>
  		<property name="jdbcUrl">jdbc:mysql://localhost:3306/web02</property>
  		<property name="user">root</property>
  		<property name="password">root</property>
  	</default-config>
  	
  </c3p0-config> 
  ```

  

  **新建工具类包utils**

  ```java
  package com.itheima.utils;
  
  import java.sql.Connection;
  import java.sql.ResultSet;
  import java.sql.SQLException;
  import java.sql.Statement;
  
  import javax.sql.DataSource;
  
  import com.mchange.v2.c3p0.ComboPooledDataSource;
  
  public class C3P0Utils {
       //创建一个类是DataSource接口的实现类dataSource
  	private static ComboPooledDataSource dataSource = new ComboPooledDataSource();
  
  	/**
  	 * 返回实现类对象
  	 */
  	public static DataSource getDataSource(){
  		return dataSource;
  	}
  	
  
  	public static Connection getConnection() throws SQLException{
  		return dataSource.getConnection();
  	}
  
  	/**
  	 * close释放资源的一个方法
  	 */
  	public static void close(ResultSet rs,Statement stat,Connection con){
  		if(rs!=null)
  			try {
  				rs.close();
  			} catch (SQLException e) {
  				e.printStackTrace();
  			}
  		
  		if(stat!=null)
  			 try{
  				 stat.close();
  			 }catch(Exception ex){}
  			
  			if(con!=null)
  				try{
  					 con.close();
  				 }catch(Exception ex){}
  	}
  }
  ```

  **写个javaBean把结果集装在JavaBean里面**

  ```java
  package com.itheima.domain;
  
  /**
   * javaBean
   * 结果集装在javaBean里面
   * 类名和数据库表名一样
   *
   */
  public class User {
      private String username;//用户名
      private String password;//密码
      //创建get/set方法
  
  
      public String getUsername() {
          return username;
      }
  
      public void setUsername(String username) {
          this.username = username;
      }
  
      public String getPassword() {
          return password;
      }
  
      public void setPassword(String password) {
          this.password = password;
      }
      //toString加不加都可以
  
      @Override
      public String toString() {
          return "User{" +
                  "username='" + username + '\'' +
                  ", password='" + password + '\'' +
                  '}';
      }
  }
  
  ```

  基本环境搭建好了

  - 创建自己的包

写css javascript html 都写在web目录下

LoginServlet类编写

```java
package com.itheima.servlet;

import com.itheima.domain.User;
import com.itheima.utils.C3P0Utils;
import org.apache.commons.dbutils.QueryRunner;
import org.apache.commons.dbutils.handlers.BeanHandler;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.sql.SQLException;

/**
 * 实现登录的Servlet程序
 * 1:获取表单提交的数据
 * 2:作为数据表查询条件(查询数据库)
 * 3:获取查询后的结果集
 * 4:结果集判断，响应浏览器，成功还是失败
 *
 * 获取表单提交的数据(HttpServletRequest request(Tomcat创建的)直接使用接收就可以了)
 * request 接收客户端提交的数据
 * 方法:String getParameter("表单的name属性值")  返回值String就是表单填写的内容
 *
 * 写完测试:
 * sql语句有错误的话就会抛异常 变量user还是null
 * 要看看控制台没显示异常程序才写成功
 *
 */
public class LoginServlet extends HttpServlet {
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
       //1:获取表单提交的数据
        String username = request.getParameter("username");
        String password = request.getParameter("password");
        //System.out.println(name+"=========="+password);
        //2:作为数据表查询条件(查询数据库)
        //2.1:创建dbutils工具类的 核心类对象 QueryRunner
        //不写里面的参数玩事务(事务安全增删改) 查询没有事物所以加参数，把数据库接口DataSource就直接传过来
        QueryRunner qr = new QueryRunner(C3P0Utils.getDataSource());
        //拼写登录查询的SQL
        String sql="SELECT * FROM user WHERE username=? AND password=?";
        User user = null;//提升作用域给判断使用
        try {
            //3:执行查询语句,获取查询后的结果集(结果集要么就什么都查不着 要么就只有一个符合条件的语句)
            //用BeanHandler结果集 参数传入存储到JavaBean的calss对象泛型也是User 后面是?占位符用户名和密码
            //这里有sql异常抛不出去 因为doGet()方法是重写父类的 父类中不抛子类就没办法抛只能try
            user = qr.query(sql,new BeanHandler<User>(User.class),username,password);
        } catch (Exception e) {//抓一个大号异常
            e.printStackTrace();
        }
        //4:查询后的结果集进行判断
        //4.1:条件不匹配，查询不到任何数据，返回null
        //浏览器只能看到以下两个结果
        if(user == null){
            //查询不到数据，登录失败 打中文会乱码response缓冲区编码集不是utf-8
            response.getWriter().print("Login Err");
        }else {
            //4.2:查询数据匹配，登陆成功
            response.getWriter().print("Login Success");
        }
    }

    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }
}
```



C3P0-config.xml只要保证能连接数据库没问题用自己的就可以





















































































