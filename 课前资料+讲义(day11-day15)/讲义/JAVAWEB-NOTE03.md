[TOC]

unit08-JSP、EL、JSTL
===================

JSP概述
-------

### 什么是JSP

JSP和Servlet都是由SUN公司提供的动态Web资源开发技术。

JSP看起来像一个HTML，但和HTML不同的是，JSP中可以书写Java代码，可以通过Java代码展示动态的数据。

```
静态Web资源：任何人在任何条件下访问时，看到的都是相同的效果，这样的资源叫做静态Web资源。（html、css、js等）
动态Web资源：不同的人，在不同的条件下访问时，看到的都是不同的效果，这样的资源叫做动态Web资源。（Servlet、jsp、php、.NET等）
```

JSP本质上是一个Servlet程序

**思考1：为什么要学习JSP?**

- Servlet是一段Java程序，适合处理业务逻辑，但是Servlet不适合向浏览器输出一个html网页。
- html可以作为页面返回，但是html是一个静态Web资源，无法展示动态数据。
- 而JSP也是页面的开发技术，也可以作为页面返回，并且JSP中可以书写Java代码，可以通过Java代码展示动态的数据。
- 因此，JSP的出现即解决了Servlet不适合输出页面的问题，也解决了HTML无法展示动态数据的问题！

**思考2：为什么说JSP本质是一个Servlet?**

在JSP第一次被访问时，会翻译成一个Servlet程序。访问JSP后看到的html网页，其实是翻译后的Servlet执行的结果。（也就是说，访问JSP后看到的网页，是JSP翻译后的Servlet输出到浏览器的。）

### JSP执行过程

**访问服务器中的JSP文件，其执行过程为**:

1. 当浏览器请求服务器中的某一个JSP文件（例如：localhost/09-jsp/test.jsp），服务器会根据请求资源的路径去寻找该文件：
2. 如果找到了，JSP翻译引擎会将JSP翻译成一个Servlet程序（JSP----> xxx.java----> xxx.class）,然后Servlet程序再执行，执行的结果是向浏览器输出一个HTML网页！
3. 如果没有找到，服务器将会响应一个404页面，通知浏览器请求的资源不存在。

**访问服务器中的HTML文件，其执行过程为**:

1. 当浏览器请求服务器中的某一个HTML文件时（例如：localhost/09-jsp/test.html），服务器会根据请求资源的路径去寻找该文件：
2. 如果找到了，服务器会将html文件的内容作为响应实体发送给浏览器，浏览器再解析html并显示在网页上。
3. 如果没有找到，服务器将会响应一个404页面，通知浏览器请求的资源不存在。



### 修改JSP模版

修改JSP模版步骤: 点击菜单栏中的 window --> Preferences，出现如下窗口:

![](JAVAWEB-NOTE03.assets/4ff5d1a10bfe557a7ab921485a8ec375.png)

点击edit编辑JSP模版，修改为如下:

```jsp
<%@ page language="java" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8"/>
<title></title>
</head>
<body>
${cursor}
</body>
</html>
```

JSP语法
-------

### 模版元素

模板元素是指写在JSP中的html内容

或者除了JSP特有内容以外的其他内容称之为模板元素

模板元素在翻译后的Servlet中，被out.write原封不动的发送给浏览器，由浏览器负责解析并显示。

![image-20200331114124004](JAVAWEB-NOTE03.assets/image-20200331114124004.png)

### JSP表达式

格式：`<%=  常量、变量、表达式 %>`

作用：计算表达式的结果，将结果输出到浏览器中，由浏览器负责解析并显示。

```jsp
<%= "Hello JSP..." %>
<% String name = "林青霞"; %>
<%= name %>
<%= 100+123 %>
<%= Math.random() %>
```

### JSP脚本片段

格式：`<% 若干Java语句 %>`

作用：在翻译后的Servlet中，将脚本片段中的Java语句复制粘贴到Servlet的对应的位置执行。例如：

在JSP中代码如下：

```jsp
<!-- 在页面上输出5行"hello JSP" -->
<%
	for(int i=0; i<5; i++){
		out.write("Hello JSP...<br/>");
	}
%>
```

在翻译后的Servlet中代码如下：

```java
...
for(int i=0; i<5; i++){
	out.write("Hello JSP...<br/>");
}
...
```

另外，在某一个脚本片段中的Java代码可以是不完整的，但是在JSP中所有的脚本片段加在一起，必须是完整符合Java语法。例如，在JSP中代码如下：

```jsp
<% for(int i=0;i<5;i++){ %>
		Hello JSP~~~<br/>
<% } %>
```

在翻译后的Servlet中：

```java
for(int i=0;i<5;i++){ 
  out.write("\r\n");
  out.write("\t\t\tHello JSP~~~<br/>\r\n");
  out.write("\t");
} 
```

### JSP注释

格式：<%-- JSP注释内容 --%>

作用：（1）为代码添加解释说明  （2）将一些暂时不需要执行的代码注释掉。

在JSP翻译时，注释内容不会参与翻译，而是直接被丢弃



面试题：考察JSP中的JSP注释、Java注释、html注释

```jsp
<%-- 
<% out.write( "aaaaa<br/>" ); %>
 --%>
<% //out.write( "bbbbb<br/>" ); %>
<!-- 
<% out.write( "ccccc<br/>" ); %>
 -->
```

问题：（1）上面输出的三行内容，在访问时，会显示哪一行内容？

第一行被JSP注释给注释了，JSP注释的内容不会参与翻译，也不会执行，更不会发送给浏览器，也不会在浏览器上显示。

第二行被Java注释给注释了，放在脚本片段中的内容会参与翻译，会将其中的Java代码复制到翻译后的Servlet中，但由于代码被注释了，所以不会执行，也不会发送给浏览器，更不会在浏览器上显示。

第三行被html注释给注释了，html注释在JSP中是模板元素，注释本身会发送给浏览器，注释中的脚本片段会参与翻译，其中的java代码也会执行，也会将内容（ccccc）发送给浏览器，但由于发送到浏览器后的ccccc被html注释包裹，因此也不会显示在浏览器上。

（2）上面输出的三行内容，哪一行会发送到浏览器中？（不管是否显示）

其中第三行内容会发送到浏览器中，但不会显示，因为前后有html注释。

### JSP指令

指令的格式：`<%@ 指令名称 若干属性声明... %>`

指令的作用：用于指挥JSP解析引擎如何将一个JSP翻译成一个Servlet程序。

**1、page指令**：用于声明JSP的基本属性信息（比如JSP使用的编码，JSP使用的开发语言等）

```jsp
<%@ page language="java"%>
-- language属性用于指定当前JSP使用的开发语言，目前只有java语言支持

<%@ page import="java.util.Date"%>
-- import属性用于导入包，如果不导入包，在使用其他包下的类时，就需要在类名前面加上包路径，例如： java.util.Date date = new java.util.Date();

<%@ page pageEncoding="UTF-8"%>
-- pageEncoding属性是用于指定当前JSP使用的编码，Eclipse工具会根据这个编码保存JSP文件。
保证pageEncoding属性指定的编码和JSP文件保存时使用的编码相同,可以避免JSP文件出现乱码!
```

**2、taglib指令**：用于引入JSTL标签库或者其他的自定义标签库

后面讲解JSTL标签库时会讲解！

JSP标签技术 
------------

在JSP页面中写入大量的java代码会导致JSP页面中html代码和java代码混杂在一起，会造成jsp页面结构的混乱，导致后期难于维护，并且代码难以复用。

于是在JSP的2.0版本中，sun提出了JSP标签技术，推荐使用标签来代替JSP页面中java代码，并且推荐，JSP2.0以后不要在JSP页面中出现任何一行java代码。

### EL表达式

格式：`${ 常量/表达式/变量 }`	（放在EL中的变量得先存入域中，才可以获取变量的值）

作用：（1）计算放在其中的表达式的结果，将结果输出在当前位置。

（2）主要作用：用于从域对象中获取数据，将获取到的数据输出在当前位置。

域对象：pageContext、request、session、application

**1、获取常量、表达式、变量的值（变量得先存入域中）**

```jsp
${ "hello el" } 
hello el	<br/>
${ 100+123 }
${ 12*12 > 143 ? "yes" : "no" } <br/>

<% 
	String name = "马云";
	request.setAttribute( "name123" , name );
%>
${ name123 }
<%= request.getAttribute("name123") %>
<%-- 在EL表达式中书写变量,底层会根据变量名到四个作用域中寻找该名称的属性值
	如果找到对应的属性值, 就直接返回, 输出到当前位置; 如果找不到就接着寻找
	直到找完四个作用域, 最后还找不到就什么都不输出!
	到四个作用域中寻找的顺序为: pageContext->request->session->application
 --%>
```

**2、获取作用域中数组或集合中的元素**

Servlet中的代码：

```java
//声明一个数组, 为数组添加元素, 并将数组存入到域中
String[] names = {"刘德华", "郭富城", "张学友", "黎明" };
request.setAttribute( "names", names );
//将请求转发到jsp, 在JSP中获取域中的数组中的元素
request.getRequestDispatcher( "/02-el.jsp" ).forward(request, response);
```

JSP中的代码：

```jsp
<%-- 获取从Servlet转发带过来的域中的数组中的元素 --%>
${ names[0] } <%-- 刘德华 --%>
${ names[1] } <%-- 郭富城  --%>
${ names[2] } <%-- 张学友 --%>
${ names[3] } <%-- 黎明 --%>
```
**3、获取作用域中map集合中的元素**

Servlet中的代码：

```java
//声明一个map集合,为集合添加元素, 并将map集合存入到域中
Map map = new HashMap();
map.put( "name" , "尼古拉斯赵四" );
map.put( "age" ,  28 );
map.put( "addr", "中国" );
request.setAttribute( "map1", map );
//将请求转发到jsp, 在JSP中获取域中的数组中的元素
request.getRequestDispatcher( "/02-el.jsp" ).forward(request, response);
```

JSP中的代码：

```jsp
${ map1.name } <%-- 尼古拉斯赵四 --%>
${ map1.age } <%-- 28 --%>
${ map1.addr } <%-- 中国 --%>
```

**4、获取作用域中JavaBean对象的属性值**

```
Bean：指可重用的组件
JavaBean：指Java中可重用的组件
业务Bean：专门用于处理业务逻辑（例如：处理注册请求时，在将用户的注册信息保存到数据库之前，需要对注册薪资进行校验）
实体Bean：是专门用于封装数据的（例如：User user = new User() ...）
```

Servlet中的代码：

```java
//声明一个User对象, 为对象的属性赋值, 并将User对象存入到域中
User u1 = new User();
u1.setName( "刘德华" );
u1.setAge( 18 );
u1.setAddr( "中国香港" );
request.setAttribute( "user" , u1 );

//将请求转发到jsp, 在JSP中获取域中的数组中的元素
request.getRequestDispatcher( "/02-el.jsp" ).forward(request, response);
```

JSP中的代码：

```jsp
<%-- 
${ user.getName() }
${ user.getAge() }
${ user.getAddr() } --%>
<hr/>
<%-- user.name 底层调用的仍然是 getName()方法--%>
${ user.name }
<%-- user.age 底层调用的仍然是 getAge()方法--%>
${ user.age }
<%-- user.addr 底层调用的仍然是 getAddr()方法--%>
${ user.addr }
```



### JSTL标签库

JSTL标签库是为JavaWeb开发人员提供的一套标准通用的标签库；

JSTL标签库和EL配合使用可以取代JSP中大部分的Java代码；

在使用JSTL标签库之前需要完成：

- 导入JSTL的开发包

![image-20200401090204772](JAVAWEB-NOTE03.assets/image-20200401090204772.png)

- 在使用JSTL标签库的JSP中引入JSTL（taglib指令）

![image-20200401090301088](JAVAWEB-NOTE03.assets/image-20200401090301088.png)

----

**其中常用的标签如下:**

`1、<c:set></c:set>` -- 用于往域中添加属性，或者修改域中已有的属性值

**c:set 标签属性总结:**

```
(1)var -- 指定存入作用域中的属性名称
(2)value -- 指定存入作用域中属性的值
(3)scope -- 指定将属性存入哪一个作用域中,默认值是page,表示pageContext域
可取值: a)page表示pageContext域 b)request表示request域
	c)session表示session域 d)application表示ServletContext域
```

代码示例：

```jsp
<%-- request.setAttribute("name", "张三"); --%>
<c:set var="name" value="张三" scope="request"/>
${ name }

<% String job = "java开发工程师"; %>
<c:set var="job" value="<%= job %>" scope="request"/>
${ job }

<c:set var="name" value="张三丰" scope="request"/>
${ name }
```

`2、<c:if></c:if>` -- 构造简单的 if…else…结构语句

**c:if 标签属性总结:**

```
test属性 -- 指定一个布尔表达式，当表达式的结果为true时，将会执行（输出）c:if标签中的内容，如果表达式结果为false,将不会输出c:if标签中的内容
```
代码示例：往域中存入一个成绩, 根据成绩判断成绩所属的等级

```jsp
<c:if test="${ 3>5 }">yes</c:if>
<c:if test="${ 3<=5 }">no</c:if>
<hr>
<!-- 根据成绩判断成绩所属的等级 -->
<c:set var="score" value="-35"/>
<c:if test="${ score>=80 and score<=100 }">您的成绩属于: 优秀!</c:if>
<c:if test="${ score>=60 and score<80 }">您的成绩属于: 中等!</c:if>
<c:if test="${ score>=0 and score<60 }">您的成绩属于: 不及格!</c:if>
<c:if test="${ score<0 or score>100 }">您的成绩有误!</c:if>
```

`3、<c:forEach></c:forEach>` -- 对集合或数组等中元素进行循环遍历或者是执行指定次数的循环.

(1) 遍历域中数组或集合中的元素

```

```

(2) 遍历域中map集合中的元素

```

```

(3) 遍历0\~100之间的整数，将是3的倍数的数值输出到浏览器中

```

```

**c:forEach 标签属性总结:**

```
(1)items: 指定需要遍历的集合或数组
(2)var: 指定用于接收遍历过程中的每一个元素
(3)begin: 指定循环从哪儿开始
(4)end: 指定循环到哪儿结束
(5)step: 指定循环时的步长, 默认值是1
(6)varStatus: 用于表示循环遍历状态信息的对象, 这个对象上有如下属性:
	first属性: 表示当前遍历是否是第一次, 若是, 则返回true;
	last属性: 表示当前遍历是否是最后一次, 若是, 则返回true;
	count属性: 记录当前遍历是第几次
```
代码示例：

```jsp

```
## JSP作业

`03-31作业：`

(1) 完成<<Servlet、request、response练习题>>

(2) 完成<<day11-jsp作业>>中的练习题

(3)练习课上讲的c:set标签和c:if标签



unit09-Maven
===========

**今日学习目标：**

1. 了解什么是Maven及Maven的作用

2. 掌握Maven安装及整合到Eclipse中

3. 掌握如何使用Maven构建Java项目和Web项目

4. 掌握使用Eclipse导入现有的Maven项目

5. 了解Maven的三种仓库(本地仓库、镜像仓库（私服）、中央仓库（公服）)

6. 了解Maven如何管理依赖（即管理jar包）


Maven介绍
---------

### Maven是什么?

![](JAVAWEB-NOTE03.assets/29cb6062127316fbe9e659ec05108844.png)

**Maven**: 翻译为"专家"、"内行"，是**Apache**下的一个纯Java开发的一个开源项目。

**Maven**是一个项目管理工具，使用Maven可以来管理企业级的Java项目开发及依赖的管理。

使用**Maven**开发，可以简化项目配置，统一项目结构。总之，Maven可以让开发者的工作变得更简单。

--------------

什么是依赖管理？要明白依赖管理，首先要知道什么是依赖？

一个Java项目中往往会依赖一些第三方的jar包。比如JDBC程序中要依赖数据库驱动包，或者在使用c3p0连接池时，要依赖c3p0的jar包等。这时我们称这些Java项目依赖第三方jar包。

而所谓的依赖管理，其实就是对项目中所有依赖的jar包进行规范化管理。

### 为什么要使用Maven?

传统的项目（工程）中管理项目所依赖的jar包完全靠人工进行管理，而人工管理jar包可能会产生诸多问题。

**1、不使用Maven，采用传统方式管理jar包的弊端：**

(1)在一些大型项目中会使用一些框架，比如SSM或者SSH框架，而框架中所包含的jar包非常多（甚至还依赖其他第三方的jar包），如果这些jar包我们手动去网上寻找，有些jar包不容易找到，比较麻烦。

(2)传统方式会将jar包添加到工程中，比如Java工程中将jar包放在工程根目录或者放在自建的lib目录下；JavaWeb工程会将jar包放在:/WEB-INF/lib目录下，这样会导致项目文件的体积暴增（例如，有些项目代码本身体积可能仅仅几兆，而加入jar包后，工程的体积可能会达到几十兆甚至百兆）。

(3)在传统的Java项目中是将所有的jar包统一拷贝的同一目录中，可能会存在jar包文件名称冲突的问题！

(4)在进行项目整合时，可能会出现jar包版本冲突的问题。

(5)在传统java项目中通过编译（手动编译或者在eclipse保存自动编译）、测试（手动在main函数中测试、junit单元测试）、打包部署(手动打war包/手动发布)、运行(手动启动tomcat运行)，最终访问程序。

**2、使用Maven来管理jar包的优势：**

(1)Maven团队维护了一个非常全的Maven仓库(中央仓库)，其中几乎包含了所有的jar包，使用Maven创建的工程可以自动到Maven仓库中下载jar包，方便且不易出错。

另外，在Maven构建的项目中，如果要使用到一些框架，我们只需要引入框架的核心jar包，框架所依赖的其他第三方jar包，Maven也会一并去下载。

(2)在Maven构建的项目中，不会将项目所依赖的jar包拷贝到每一个项目中，而是将jar包统一放在仓库中管理，在项目中只需要引入jar包的位置(坐标)即可。这样实现了jar包的复用。

(3)Maven采用坐标来管理仓库中的jar包，其中的目录结构为【公司名称+项目/产品名称+版本号】，可以根据坐标定位到具体的jar包。即使使用不同公司中同名的jar包，坐标不同（目录结构不同），文件名也不会冲突。

(4)Maven构建的项目中，通过pom文件对项目中所依赖的jar包及版本进行统一管理，可避免版本冲突。

(5)在Maven项目中，通过一个命令或者一键就可以实现项目的编译（mvn complie）、测试（mvn test）、打包部署（mvn deploy）、运行（mvn install）等。

还有发布到tomcat服务器中运行: mvn tomcat7:run。如果想实现上面的所有过程，只需要记住一个命令：mvn install

总之，使用Maven遵循规范开发有利于提高大型团队的开发效率，降低项目的维护成本，大公司都会优先使用Maven来构建项目.

Maven安装
---------

### 下载、安装Maven

1、官方下载地址：http://maven.apache.org/download.cgi

![](JAVAWEB-NOTE03.assets/53a1d2f3f17b247cd6d1690974484a38.png)

2、下载绿色版，解压之后就可以使用。

![](JAVAWEB-NOTE03.assets/f053c9311f77b5a8d9f905fa3859e2c7.png)

![](JAVAWEB-NOTE03.assets/9ab44a6351a1fd7320ae28eea7a0fa6b.png)

原则: 安装的路径中不要有中文和空格!!

3、若要下载旧版本Maven，可以访问：

```
https://archive.apache.org/dist/maven/maven-3/
```

![](JAVAWEB-NOTE03.assets/2125c89c33f029357b460c5f98edaed5.png)

![](JAVAWEB-NOTE03.assets/4465c83cfebe53f76a6e25e5e919402d.png)

Maven的相关配置
-------------

在开发中更多是通过Eclipse+Maven来构建Maven项目，所以这里我们需要将Maven配置到Eclipse开发工具中。

在将安装好的Maven工具配置的Eclipse开发工具中之前，需要做一些相关的配置。

### 配置本地仓库位置

本地仓库：其实就是本地硬盘上的某一目录，该目录中会包含maven项目中所需要的所有jar包及插件。当所需jar包在本地仓库没有时，从网络上下载下来的jar包也会存放在本地仓库中。

因此本地仓库其实就是一个存放jar包的目录，我们可以指定Maven仓库的位置。

如果不指定，maven本地仓库的默认位置是在c盘，在：

`C:/Users/{当前用户}/.m2/repository`，例如:

![](JAVAWEB-NOTE03.assets/d9d738475e447449e535cf7e6a735be8.png)

可以保持默认，当然也可以修改本地仓库的位置到别的盘符路径。

修改方法：找到[MAVEN_HOME]/conf/目录中的配置文件settings.xml，修改maven仓库的路径。

![image-20200401104914602](JAVAWEB-NOTE03.assets/image-20200401104914602.png)

配置该目录后，以后通过maven下载的jar包将会保存在配置的目录下。

**以上内容可以总结为：**

1. 什么是本地仓库？
2. 本地仓库的默认位置在哪儿？
3. 配置和不配置本地仓库有什么区别？

### 配置镜像仓库(私服)

当maven项目中需要依赖jar包时，如果本地仓库中没有，就会到远程仓库去下载jar包。

如果不配置镜像仓库，默认连接的是中央仓库，由于中央仓库面向的是全球用户，所以在下载jar包时，速度可能会比较慢，效率会比较低。

可以在settings.xml文件中配置连接达内镜像仓库（前提是在达内教室，连接的是达内内网）或者连接阿里云镜像仓库（需要有外网）。

1、如果连接的是达内内网，可以连接达内镜像仓库（如果不配置，默认连接中央仓库，没有外网，连接不了中央仓库，会导致jar包无法下载）。

需要做的是，在settings.xml文件中的`<settings>`标签下的`<mirrors>`标签内部添加如下配置，配置达内镜像仓库：

```xml
<mirror>
    <id>nexus-tedu</id>
    <name>Nexus tedu</name>
    <mirrorOf>central</mirrorOf>
    <url>http://maven.tedu.cn/nexus/content/groups/public/</url>
</mirror>
```

2、如果在家里、在公司连接的是外网，是无法连接达内的镜像仓库，可以选择什么都不配置，默认连接中央仓库，或者可以配置连接阿里云镜像仓库（不要使用手机热点网络连接），配置如下：

配置阿里云镜像仓库：

```xml
<mirror>
	<id>nexus-aliyun</id>
	<name>Nexus aliyun</name>
	<mirrorOf>central</mirrorOf>
	<url>http://maven.aliyun.com/nexus/content/groups/public/</url>
</mirror>
```

**镜像仓库配置总结：**

1. 当所需jar包在本地仓库没有时，会从网络上下载。配置镜像仓库其实就是配置，去网络中哪个位置下载jar包到本地。

2. 如果在公司，并且公司有搭建镜像服务器，推荐使用公司的镜像服务器下载jar包，速度会更快。（如果在达内，使用的是内网，只能配置达内的镜像仓库。否则，没有外网也连接不了中央仓库，下载jar包会失败！）

3. 如果在家里，使用的是外网，可以不配置镜像仓库，默认连接中央仓库下载jar包，或者配置阿里云的镜像仓库。连接阿里云服务器下载jar包。（注意，如果配置阿里云镜像服务器，**不可使用手机热点网络！**）


### 配置JDK版本

通过 Maven创建的工程，JDK版本默认是JDK1.5，每次都需要手动改为更高的版本。

这里可以通过修改maven的settings.xml文件, 达到一劳永逸的效果。

配置方式为：打开 `{maven根目录}/conf/settings.xml` 文件并编辑，在 settings.xml文件内部的 `<profiles>` 标签内部添加如下配置：

```xml
<profile>
    <id>development</id>
    <activation>
    	<jdk>1.8</jdk>
    	<activeByDefault>true</activeByDefault>
    </activation>
    <properties>
    	<maven.compiler.source>1.8</maven.compiler.source>
    	<maven.compiler.target>1.8</maven.compiler.target>
  <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
    </properties>
</profile>
```



### 将Maven配置到Eclipse中

将Maven工具配置到Eclipse中，就可以通过Eclipse和自己安装的Maven创建Maven项目了。

**1、window右键--> Preferences：**

![](JAVAWEB-NOTE03.assets/95e8c0d77e32611aa01c83fdfe9115bc.png)

**2、点击Maven选项，在右侧选项中勾选 "Download Artifact Sources"：**

![](JAVAWEB-NOTE03.assets/b8de69f1e0238506740120c3ac2761b2.png)

**3、点击add将自己安装的Maven添加进来：**

![](JAVAWEB-NOTE03.assets/c75f99240250846b1009f595627fd48b.png)

**4、添加自己安装的Maven：**

![](JAVAWEB-NOTE03.assets/1e470e51fcaec075f1d1e3475978325f.png)

![](JAVAWEB-NOTE03.assets/90ca91b942da7c2a58b6e38d234200c3.png)

**一定要注意:**自己安装的Maven不要放在桌面上(容易丢失，并且路径中往往有空格)，maven的安装路径中也不要包含中文和空格!!

![](JAVAWEB-NOTE03.assets/88bd59974c2d8f430ffab34336bb9f33.png)

**5、将默认的maven切换为自己配置的maven：**

![](JAVAWEB-NOTE03.assets/3c3426a658642a9ca1c41e67e790f006.png)

**6、设置maven的settings文件的位置：**

![](JAVAWEB-NOTE03.assets/3290041ab768b3e7a6ee02ffd40f8c9e.png)

**7、测试是否配置成功：**window---> show view ---> other中搜索"maven"，点击下面的选框中的选项

![](JAVAWEB-NOTE03.assets/facabede667d2edbead383cd5d725a75.png)

在弹出的窗口中，查看自己配置的本地仓库和远程仓库镜像:

![](JAVAWEB-NOTE03.assets/e5fbbd21f05c86804f0656638784cba8.png)

Maven的项目构建
-------------

通过Maven构建Java项目分为两种方式：

（1）创建Maven的简单Java工程以及创建Maven的简单Web工程

（2）创建使用模板的Java工程以及创建使用模板的Web工程

在利用Maven构建项目时分两种，第一种是：创建简单工程**（Create a simple
project）**，即在创建时勾选前面的框。

![](JAVAWEB-NOTE03.assets/8888dd2dcf76aff253b16f4559f6df39.png)

*(不勾选前面的框，即创建使用骨架（其实就是模版）创建Maven工程)*

另，在创建简单工程时，还分为创建**Java工程**和**JavaWeb工程**。下面分别进行演示。

### 创建简单工程—Java工程

**1、空白处右键New ---> Maven Project：**

![](JAVAWEB-NOTE03.assets/a21856795d5e9f4c4f63603c68711cbb.png)

**2、在弹出的窗口中，勾选前面的框，创建一个简单工程（即不使用骨架），进入下一步。**

![](JAVAWEB-NOTE03.assets/d8b1436aea89ddf1c2ee001333a9f56c.png)

**3、在弹出的窗口中，填写内容(Package选择jar，即创建java工程)，点击完成即可。**

![](JAVAWEB-NOTE03.assets/eb0b7c41231cc90448d85150fd76ce95.png)

**在上述内容中，必填的内容有四项：**

(1)**Group Id** -- 组的名称，通常填写公司名称（比如com.tedu）或者组织名称(org.apache..)

(2)**Artifact Id** -- 项目名称或者模块名称

(3)**Version** -- 项目的版本，创建的项目默认是0.0.1-SNAPSHOT快照，也叫非正式版，正式版是RELEASE)

(4)**Package** -- 项目的类型：jar表示创建的是Java工程，war表示创建的是web工程，pom表示创建的是父工程（当然相对的还有子工程）或者聚合工程，pom目前我们不讨论。

填写完毕后，点击完成即可完成创建简单Java工程

**4、切换工程视图为包视图：window --> show view，在弹出的窗口中搜索：**

![](JAVAWEB-NOTE03.assets/0578e41f1fad77ea0ed8509afd2d11d9.png)

### 创建简单工程—Web工程

**1、空白处右键New ---> Maven Project：**

![](JAVAWEB-NOTE03.assets/a21856795d5e9f4c4f63603c68711cbb.png)

**2、在弹出的窗口中，勾选前面的框，创建一个简单工程（即不使用骨架），进入下一步。**

![](JAVAWEB-NOTE03.assets/d8b1436aea89ddf1c2ee001333a9f56c.png)

**3、在弹出的窗口中，填写内容(Package选择war，即创建web工程)，点击完成即可。**

![](JAVAWEB-NOTE03.assets/e6f4239c8e4caba3a12fd2852be782bc.png)

**4、创建完成后pom.xml文件会报错，说找不到web.xml文件，例如：**

![](JAVAWEB-NOTE03.assets/ce00e7568eb6f3632ae14f0053e87451.png)

**手动添加(拷贝)即可，例如:**

![](JAVAWEB-NOTE03.assets/9714b18c4003841a3bbf23062539f491.png)

**5、创建Servlet程序，测试运行环境。**

![](JAVAWEB-NOTE03.assets/0185f08055d050b93aa6ed68f920bff8.png)

上面的错误是因为运行环境中缺少Servlet的jar包，将tomcat运行环境添加过来即可！

**!!缺少Servlet运行环境解决方案：**

在项目上点击鼠标右键，选择 "Properties" ---> "Targeted Runtimes":

![](JAVAWEB-NOTE03.assets/e321e3569a1e0dc2d6a7ddc2d953204f.png)

或者，在项目中的pom.xml文件中的根标签下添加Servlet的jar包的坐标，引入Servlet的jar包，如下：

```xml
<dependencies>
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>servlet-api</artifactId>
        <version>2.5</version>
        <scope>provided</scope>
    </dependency>
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>jsp-api</artifactId>
        <version>2.0</version>
        <scope>provided</scope>
    </dependency>
</dependencies>
```

添加后保存pom文件。若还报错，在项目上右键选择 **"Maven"** ---> **"UpdateProject…"** 更新工程即可！

**6、实现Servlet程序**

![](JAVAWEB-NOTE03.assets/1bcc65d4c935093c915409cb865902bf.png)

**7、测试访问：**打开浏览器访问地址：[http://localhost/MavenSimpleProJavaweb/HelloServlet](http://localhost:8080/MavenSimpleProJavaweb/HelloServlet)

![](JAVAWEB-NOTE03.assets/7c9682115d7e105ccc587d511be921e1.png)

### Maven的目录结构

使用Maven创建的工程我们称它为Maven工程，Maven工程具有一定的目录规范，对目录结构有严格的要求，一个Maven工程要具有如下目录结构：

![image-20200225092639227](JAVAWEB-NOTE03.assets/image-20200225092639227.png)

下面以Maven的Web项目为例，介绍Maven项目中的目录结构：

```
Maven项目名称(Web项目)
  |-- src/main/java(源码目录)：用于存放程序/项目所需要的java源码文件
  |-- src/main/resources(源码目录)：用于存放程序/项目所需要的配置文件
  |-- src/test/java(源码目录)：用于存放测试程序的java源文件
  |-- src/test/resources(源码目录)：用于存放测试程序所需要配置文件
  |-- src/main/webapp：(Web应用的根目录，作用类似于WebContent)
  				 				|-- WEB-INF：(受保护的目录)
  				 							|-- web.xml：(Web应用的核心配置文件)
  |-- target/classes(类目录)：源码目录中的资源经过编译后，会输出到类目录下。
  |-- pom.xml：Maven项目中非常重要的文件，将来项目需要任何jar包或插件，都可以通过pom文件来导入这些jar包或插件。
```

导入已有的Maven项目
-------------------

现将后面通过SSM框架实现的<<永和大王门店管理系统>>（Maven）项目导入到我们的Eclipse开发环境中。

在导入项目时我们通常会通过 "File" --> "Import..." 来导入项目，但是这样能会产生环境问题=，例如：如果项目本身自带的环境和我们当前使用的开发环境不一致，就会产生问题。可以按照下面的方式来进行导入。

下面是导入的步骤：

**1、创建一个新的Maven工程（JavaWeb工程）**

确保已经配置好Maven的环境后，在Eclipse中创建一个新的Maven工程（Javaweb工程），新工程的名字和所导入的工程的名字可以相同也可以不同，例如：

![](JAVAWEB-NOTE03.assets/ed42db71791984bdce6bbe04c3147120.png)

**2、解压yonghe-ssm.zip压缩包**

将下发的 yonghe-ssm 项目解压出来，解压后的结构如下：

![image-20200326195041536](JAVAWEB-NOTE03.assets/image-20200326195041536.png)

打开 yonghe-ssm 目录：

![image-20200326195147418](JAVAWEB-NOTE03.assets/image-20200326195147418.png)

**3、将解压后的目录中的src目录选中并复制。**

![](JAVAWEB-NOTE03.assets/b9bf1d703dc2546c114b1a7832113233.png)

**4、点击新创建的Maven工程右键粘贴，将复制的src目录粘贴到新建的工程中**

![](JAVAWEB-NOTE03.assets/fdc57a6cab8a89d634112760063efe18.png)

**5、打开解压后的目录中的pom.xml文件, 复制其中的内容：**

![](JAVAWEB-NOTE03.assets/32b2d2e712857eb26fae448eecf7ce26.png)

如果复制后项目或pom文件仍然报错, 可以更新Maven工程

**更新Maven工程：**在项目上右键选择 **"Maven"** ---> **"Update
Project…"**，在弹出的窗口中直接点击OK即可！

**6、执行SQL脚本文件，导入数据**

打开cmd，连接mysql数据库，执行**yonghedb.sql**中的SQL语句，创建数据库、表及插入记录。

**7、部署项目到服务器并启动服务器，访问测试**

在正确完成上面的操作后，打开浏览器访问如下地址：<http://localhost/yonghe-ssm/index>
，可以看到如下界面：

![](JAVAWEB-NOTE03.assets/770fe679ff527efa1f80fc4e57a96f79.png)

Maven的依赖管理
--------

### 依赖(jar包)管理

依赖管理即jar包的管理，那么通过Maven创建的工程是如何管理jar包的？

**1、在Maven项目中如何引入jar包？**

在Maven创建的项目中，如果需要引用jar包，只需要在项目的pom.xml文件中添加jar包的坐标(GroupID + ArtifactID + Version)即可将jar包引进项目中，之后就可以在项目中使用所引入的jar包了。

例如，现在我们在pom.xml文件中，添加servlet的jar包的坐标如下：

```xml
<dependency>
	<groupId>javax.servlet</groupId>
	<artifactId>servlet-api</artifactId>
	<version>2.5</version>
</dependency>
```

在pom.xml文件中，添加mysql驱动包的坐标如下：

```xml
<dependency>
	<groupId>mysql</groupId>
	<artifactId>mysql-connector-java</artifactId>
	<version>5.1.32</version>
</dependency>
```

**2、项目中引入的jar包存放在哪里？**

那么问题来了，在pom文件中添加的servlet的jar包坐标，添加的仅仅是一个jar包对应的坐标，而这个jar包到底存放在哪里呢?

了解Maven管理jar包的规则之后，我们可以找到jar包的存放位置是在**本地仓库**（仓库默认是在：C:\\Users\\{当前用户}\\.m2\\repository）的 /javax/servlet/servlet-api/2.5/目录下，例如：

![](JAVAWEB-NOTE03.assets/a9335bd016fc4426edf4e06a0a53ef73.png)

**总结：**

- 在Maven中，所有的jar包全都存放在本地仓库的目录中，如果项目中需要使用某一个jar包，直接在项目的pom.xml文件中通过坐标(GroupID + ArtifactID + Version)引入指定位置的jar包即可。
- 这样可以将项目中所有使用的jar包集中在一个目录（本地仓库）中统一进行管理，需要时通过坐标直接引入即可，而不是在每个项目中都拷贝一份，减少了项目体积，也节省了磁盘空间。
- 将来如果别人需要导入你的项目，只需要将项目（当然包括pom.xml文件）代码整体传给对方，无需将jar包发送给对方，对方在配置Maven的环境后，Maven会自动根据项目中pom.xml文件里配置的坐标，引入（或下载后再引入）对应的jar包。

**3、如果引入的jar包在本地仓库中没有呢？**

- 如果是刚配置的Maven环境，本地仓库中还没有太多jar包，此时在pom文件中通过坐标引入jar包，而本地仓库中没有这个jar包，这时会怎么样呢？

- 若本地仓库没有所需要的jar包，则会到镜像仓库（也叫私服）或者到中央仓库（也叫公服）中下载。下面我们就来介绍Maven的这三种仓库。


### Maven三种仓库

在上面所提到的本地仓库、镜像仓库、中央仓库是用来Maven用来更好的管理jar包的所采用的一种方式。下面来了解Maven的**三种仓库**，以及三种仓库之间的潜在联系。

通过maven构建的项目，会通过项目中的pom.xml文件从远程仓库下载，并保存到本地仓库

![](JAVAWEB-NOTE03.assets/6d744a6b627f8edded770b46bb5e316e.png)

**本地仓库**：默认的本地仓库位置在：c:/\${user.dir}/.m2/repository，其中\${user.dir}表示windows下的用户目录。本地仓库的作用是，用于保存（存储）从私服或者从中央仓库下载下来的jar包（或插件）。当项目中需要使用jar包和插件时，优先从本地仓库查找。

如果本地仓库中没有所需的jar包，可以到私服或者到中央仓库中下载后再保存到本地仓库。

**镜像仓库**：镜像仓库也叫做私服（Nexus），私服一般由公司搭建并维护（也可以自己搭建）。比如达内有搭建自己的私服服务器（http://maven.tedu.cn/nexus/content/groups/public/），以及阿里云私服服务器（http://maven.aliyun.com/nexus/content/groups/public/）。

如果项目中使用到的jar包或者插件本地仓库中没有，则可以到私服中下载，如果私服中有就直接将jar包保存到本地仓库中；而如果私服中也没有所需的jar包，就到中央仓库（公服）上下载所需要的jar包，下载之后先在私服上保存一份，最后再保存到本地仓库。

**中央仓库**：中央仓库也叫做公服，在maven软件中内置了一个仓库地址（http://repo1.maven.org/maven2）它就是中央仓库，服务于整个互联网，由Maven团队自己搭建并维护，里面存储了非常全的jar包，它包含了世界上大部分流行的开源项目的jar包。

那么我们在使用Maven构建的Java项目，项目中所使用的jar包会来自哪里呢？例如，通过Maven先后构建项目A和项目B，在项目中都需要依赖第三方jar包：

- 如果项目A中需要依赖第三方jar包，只需要在项目下的pom文件中引入jar包在**本地仓库**中的坐标即可使用。如果本地仓库没有所需要的jar包，则会连接私服（需要提前配置）下载所需jar包到本地仓库供项目使用。
- 如果私服上也没有所需的jar包，则会连接中央仓库下载所需要的jar包保存到私服，再将jar包从私服下载至本地仓库，供项目使用。
- 如果没有配置私服，则默认连接中央仓库下载所需要的jar包到本地仓库中供项目使用

- 当项目B也需要依赖第三方jar包时，先到本地仓库中查找所需jar包，如果有则直接引用而无需再次下载，如果仍有部分jar包本地仓库中没有，则同上，即连接私服下载所需jar包到本地仓库。若私服中也没有所需jar包，则连接中央仓库下载jar包到私服，再从私服下载jar包到本地仓库中，供项目使用。



### 添加依赖:方式一

使用maven插件的索引功能快速添加jar包

这种方式需要**本地仓库中已经包含了该jar包，否则搜索不到**!!!

1、如果本地仓库中有我们需要的jar包，可以在项目中的pom.xml文件中空白处右键-->  Maven --> Add Dependency在弹出的窗口中添加所需要的依赖(jar包)，如图：

![](JAVAWEB-NOTE03.assets/33fe1d9ff8f2d46631335a13ee2d6b51.png)

**2、**添加依赖示例：添加spring的jar包的坐标到项目中

(1) 在项目中的pom.xml文件中右键 -> Maven -> Add Dependency，在弹出的窗口中输入
"spring":

![](JAVAWEB-NOTE03.assets/5b98e92822f738cda80091a2d4709bb8.png)

选中要添加的jar包（坐标会自动填写），点击OK即可完成添加

![](JAVAWEB-NOTE03.assets/172e2faf10ff88ed41dfd5fff6e05939.png)

(2)如果搜索不到jar包（保证本地仓库中已经下载了该jar包），可以尝试重建索引。

在

![](JAVAWEB-NOTE03.assets/facabede667d2edbead383cd5d725a75.png)

"Maven Repositories" 视图窗口中可以看到如下内容：

![](JAVAWEB-NOTE03.assets/fe8dfdbc79b3219c161e97c1f650b2a5.png)

在"Local Repositories"上右键选择 "Rebuild Index" 即可重建索引。

完成后，再尝试搜索jar包进行添加。

### 添加依赖:方式二

**1、直接在pom.xml文件中的<dependencies>标签内部添加。例如：在pom.xml文件中添加如下配置，就可以将junit单元测试的jar包引入到项目中来了。**

添加依赖：

```xml
<dependencies>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.9</version>
        <scope>test</scope>
    <dependency>
<dependencies>
```

**2、手动添加依赖需要指定所依赖jar包的坐标，但是在大部分情况下，我们是不知道jar包的坐标的。可以通过访问如下网址，在互联网上搜索查询：**

```
http://mvnrepository.com
```

或者在公司镜像仓库中搜索查询:

```
http://maven.tedu.cn/nexus
```

**3、示例：添加c3p0的jar包的坐标到项目中**

- 访问上面其中的一个网址，在搜索框中搜索 "c3p0"

- 在搜索出来的内容中，选择所需要的版本并点击版本，查看该版本的c3p0
  jar包所对应的坐标：
- 将坐标直接拷贝到项目的pom.xml文件中即可：

<img src="JAVAWEB-NOTE03.assets/image-20200225120117127.png" alt="image-20200225120117127" style="zoom:67%;" />

![image-20200225120916520](JAVAWEB-NOTE03.assets/image-20200225120916520.png)

![image-20200225120728549](JAVAWEB-NOTE03.assets/image-20200225120728549.png)

**4、将上图中右侧选中的坐标拷贝到pom.xml文件中:**

<img src="JAVAWEB-NOTE03.assets/image-20200225121143892.png" alt="image-20200225121143892" style="zoom:67%;" />

FAQ:Maven常见问题
-------------

### 常见Maven环境问题

问题描述：

- 创建Maven项目时报如下错误：

![image-20200315204732042](JAVAWEB-NOTE03.assets/image-20200315204732042.png)

![image-20200315204802766](JAVAWEB-NOTE03.assets/image-20200315204802766.png)

- 或者创建Maven项目目录结构不全（比如只有src目录），如下图：

  ![image-20200315205928911](JAVAWEB-NOTE03.assets/image-20200315205928911.png)

- 导入已有的Maven项目，项目运行不了（jar没有下载完全）

此时是因为maven的环境被破坏了，导致Maven基础运行环境不全，无法创建Maven项目，或者无法下载所需要的jar包。解决方法：

```
(1)在项目的pom文件中敲一个空白行,再保存文件，目的是让maven检测到pom文件发生变化，再根据pom文件中的配置到本地仓库中寻找对应的jar包，如果没有相应的jar包，maven会重新下载。
(2)如果上面的方式不行,可以尝试在项目上,右键---> Maven ---> Update Project...,强制更新项目,此时maven也会检查pom文件，在本地仓库中有没有相应的jar包。
(3)如果上面的方式仍然没有解决问题，检查当前网络环境是否能连接上所配置的镜像仓库。（比如在家里使用外网，无法连接达内的镜像仓库，或者使用手机热点网络无法连接阿里云的镜像仓库等）
(4)如果网络能够连接上所配置的镜像仓库，到本地仓库的目录下，将本地仓库中所有的目录都删除，删除时，eclipse正在使用本地仓库中的资源文件，所以会阻止删除，此时将eclipse关闭，再将本地仓库中的所有目录删除，重启eclipse。
(5)启动eclipse后,再将上面的第(1)步和第(2)步再做一遍!
```

问题描述2：每天第一次打开Eclipse发现之前创建的Maven工程报错（比如项目上有叉号或者叹号，但项目之前是OK的），解决方法：在菜单栏中找到 Project ---> Clean....

![image-20200315210421557](JAVAWEB-NOTE03.assets/image-20200315210421557.png)



### 找不到jar包问题

在项目中通过坐标引入了jar包（或者插件），并且本地仓库中也存在对应的jar包，但是项目还是报错，提示内容说找不到。

解决方法：如果引入的jar包，在本地仓库中存在，但是还是提示找不到，可以将本地仓库中jar包或插件的所在目录整个删除(如果删除时提示文件正在被占用，关闭eclipse再删除即可)，重新保存pom.xml文件，并更新工程，让maven再次下载上面的jar包即可！

未下载完全示例：

![](JAVAWEB-NOTE03.assets/71d763b5d1bea2be9037ab549f3a6033.png)

正常下载完全示例：

![](JAVAWEB-NOTE03.assets/c4988ae458e47fe30d6fb6ab1ebf9522.png)

### 拷贝Maven仓库

如果因为网络环境的问题，导致jar包无法下载，也可以将别人下载好的（完整的）Maven的本地仓库拷贝过来，放在自己配置的本地仓库中。因为Maven可以支持拷贝别人的仓库。

扩展:Maven项目结构
-----------------------

使用Maven创建的工程我们称它为Maven工程，Maven工程具有一定的目录规范，对目录结构有严格的要求，一个Maven工程要具有如下目录结构：

![](JAVAWEB-NOTE03.assets/9de5e6454964c4be0b3284fd4845ce85.png)

Maven项目目录介绍：

1）/src/main/java -- 主目录下的Java目录，用于存放项目中的.java文件

2）/src/main/resources –- 主目下的资源目录，存放项目中的资源文件(如框架的配置文件)

3）/src/test/java -- 测试目录下的Java目录，用于存放所有单元测试类的.java文件，如Junit测试类

4）/src/test/resources -- 测试目录下的资源目录，用于存放测试类所需资源文件(如框架的配置文件)

5）/target -- 项目输出目录，编译后的class文件、及项目打成的war包等会输出到此目录中

6）/pom.xml -- maven项目的核心配置文件，文件中通过坐标来管理项目中的所有jar包和插件。

## maven作业

1、练习将Maven整合到Eclipse中（可以切换一个新的工作空间进行练习）

2、练习使用Maven创建简单的Java工程

3、练习使用Maven创建简单的JavaWeb工程

4、练习导入已有的Maven工程

5、Maven的三种仓库每种仓库的作用，以及三种仓库之间的联系

6、Maven项目中是如何管理jar包的？

```
(1)Maven项目是如何引入jar包
(2)Maven项目中引入的jar包存放在哪里
(3)Maven项目引入的jar包在本地仓库中没有会怎样
```

unit10-Cookie、Session
=====================

什么是会话
----------

什么是会话：当浏览器发请求访问服务器开始，一直到访问服务器结束，浏览器关闭为止，这期间浏览器和服务器之间产生的所有请求和响应加在一起，就称之为浏览器和服务器之间的一次会话。

在一次会话中往往会产生一些数据，而这些数据往往是需要我们保存起来的，如何保存会话中产生的这些数据呢？

- 比如在购物过程中，将商品加入购物车，其实就是将商品信息保存到数据库中。（不讨论）
- 如果在没有登录时，将商品加入购物车，其实就是将商品信息保存到了cookie或session中。

可以使用cookie或者session保存会话中产生的数据。

如何将会话中产生的数据保存到cookie或者是session中？

cookie原理及应用
------

### cookie的工作原理

![](JAVAWEB-NOTE03.assets/35f0bd1bfc616e3bfcc9e96c189444ee.png)

1. Cookie是将会话中产生的数据保存在客户端，是客户端技术。
2. Cookie是基于两个头进行工作的：分别是Set-Cookie响应头和Cookie请求头
3. 通过Set-Cookie响应头将cookie从服务器端发送给浏览器，让浏览器保存到内部；而浏览器一旦保存了cookie，以后浏览器每次访问服务器时，都会通过cookie请求头，将cookie信息再带回服务器中。在需要时，在服务器端可以获取请求中的cookie中的数据，从而实现某些功能。

### cookie的API及应用

**1、创建Cookie对象**

```java
Cookie c = new Cookie(String name, String value);
// 创建cookie的同时需要指定cookie的名字和cookie要保存的值
```

**2、将Cookie添加到response响应中**

```java
response.addCookie( Cookie c );
// 将cookie添加到响应中，由服务器负责将cookie信息发送给浏览器，再由浏览器保存到内部（可以多次调用该方法，添加一个以上的cookie）
```

**3、获取请求中的所有cookie对象组成的数组**

```java
Cookie[] cs = request.getCookies();
// 获取请求中携带的所有cookie组成的cookie对象数组，如果请求中没有携带任何cookie，调用该方法会返回null。
```

**4、删除浏览器中的Cookie**

```java
// cookie的API中没有提供直接删除cookie的方法，可以通过别的方式间接删除cookie
// 删除名称为cart的cookie：可以向浏览器再发送一个同名的cookie（即名称也叫做cart），并设置cookie的最大生存时间为零，由于浏览器是根据cookie的名字来区分cookie，如果前后两次向浏览器发送同名的cookie，后发送的cookie会覆盖之前发送的cookie。而后发送的cookie设置了生存时间为零，因此浏览器收到后也会立即删除！
```

代码示例：

```java
//创建一个名称为cart的cookie
Cookie c = new Cookie("cart", "");
//设置cookie的最大生存时间为零
c.setMaxAge( 0 );
//将cookie添加到响应中,发送给浏览器
response.addCookie( c );
out.write( "成功删除了名称为cart的cookie..." );
```

**5、Cookie的常用方法**

```java
cookie.getName(); // 获取cookie的名字
cookie.getValue(); // 获取cookie中保存的值
cookie.setValue(); // 设置/修改cookie中保存的值(没有setName方法,因为cookie的名字无法修改)
cookie.setMaxAge(); //设置cookie的最大生存时间
```

**6、setMaxAge方法**：设置cookie的最大生存时间

```
如果不设置该方法，cookie默认是会话级别的cookie，即生存时间是一次会话。当浏览器关闭，会话结束时，cookie也会被销毁（cookie默认存在浏览器的内存中，当浏览器关闭，内存释放，cookie也会随着内存的释放而销毁。）
如果设置了该方法，cookie将不会保存到浏览器的内存中，而是以文件形式保存到浏览器的临时文件夹中（也就是硬盘上），这样再关闭浏览器，内存释放，保存到硬盘上的cookie文件不会销毁，再次打开浏览器，还可以获取硬盘上的cookie信息。
```

代码示例：

```java
//创建一个Cookie对象，将商品信息保存到cookie中
Cookie cookie = new Cookie( "cart", prod  );
//设置cookie的最大生存时间, 单位:秒
cookie.setMaxAge( 60*60*24 );
//将cookie对象添加到response响应中
response.addCookie( cookie );
```

### 案例:使用cookie模拟购物车

**1.index.html**

```html
...
<body>
	<h3>点击下面的商品链接, 可以将商品加入购物车</h3>
	<!-- 
	http://localhost/day13-cookie/CartServlet
	http://localhost/day13-cookie/index.html
	 -->
	<p><a href="CartServlet?prod=iphone11">iphone11</a></p>
	<p><a href="CartServlet?prod=vivonex3">vivonex3</a></p>
	<p><a href="CartServlet?prod=xiaomishouji">xiaomishouji</a></p>
	<p><a href="CartServlet?prod=huaweip30">huaweip30</a></p>
	<p><a href="CartServlet?prod=海尔洗衣机">海尔洗衣机</a></p>
	
	<h3>点击下面的支付链接, 可以对购物车中的商品进行结算</h3>
	<a href="PayServlet">支付</a>
</body>
...
```

**2.CartServlet**

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response)
		throws ServletException, IOException {
	response.setContentType("text/html;charset=utf-8");
	PrintWriter out = response.getWriter();
	//-------------------------------------------------------
	//获取请求中携带的商品信息(将要加入购物车的商品信息)
	String prod = request.getParameter( "prod" );
	//创建一个Cookie对象，将商品信息保存到cookie中
	Cookie cookie = new Cookie( "cart", prod  );
	//设置cookie的最大生存时间, 单位:秒
	cookie.setMaxAge( 60*60*24 );
	//将cookie对象添加到response响应中
	response.addCookie( cookie );
	//做出回应
	out.write( "成功将"+prod+"加入了购物车....." );
}
```

**3.PayServlet**

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response)
		throws ServletException, IOException {
	response.setContentType("text/html;charset=utf-8");
	PrintWriter out = response.getWriter();
	//------------------------------------------------------
	//获取请求中的所有cookie信息(null/cookie数组)
	Cookie[] cs = request.getCookies();
	//遍历所有cookie数组, 找出名称为cart的cookie
	String prod = null;
	if( cs != null ) {
		for (Cookie c : cs) {
			if( "cart".equals( c.getName() ) ) {
				//获取cookie中保存的商品信息
				prod = c.getValue();
			}
		}
	}
	//为商品进行结算
	if( prod == null ) {
		out.write( "您还没有将商品加入购物车...." );
	}else {
		out.write( "成功为"+prod+"支付了1000.00元...." );
	}
}
```

## session原理及应用

### session的工作原理

<img src="JAVAWEB-NOTE03.assets/9f99b30f96234a6bba48e37d6852a080.png" style="zoom:67%;" />

1. Session是将会话中产生的数据保存在服务器端，是服务器端技术
2. Session是一个域对象，session中也保存了一个map集合，往session中存数据，其实就是将数据保存到session的map集合中。
3. 通过session.setAttribute()方法可以将数据保存到session中，通过session.getAttribute()方法可以将数据从session中取出来。

### session是一个域对象

获取session对象：

```
request.getSession() 
// 获取一个session对象；如果在服务器内部有当前浏览器对应的session，则直接返回该session对象；如果没有对应session，则会创建一个新的session对象再返回；
```

Session是一个域对象，因此session中也提供了存取数据的方法。

```java
session.setAttribute(String attrName, Object attrValue); 
// 往session域中添加一个域属性，属性名只能是字符串类型，属性值可以是任意类型。
session.getAttribute(String attrName);
// 根据属性名获取域中的属性值，返回值是一个Object类型
```

Session域对象的三大特征：

**(1)生命周期：**

**创建session：**第一次调用request.getSession()方法时，会创建一个session对象。（当浏览器在服务器端没有对应的session时，调用request.getSession()方法服务器会创建一个session对象。）

**销毁session：**

1. 超时销毁：默认情况下，当超过30分钟没有访问session，session就会超时销毁。（30分钟是默认时间，可以修改，但不推荐修改）

2. 自杀：调用session的invalidate方法时，会立即销毁session。

3. 意外身亡：当服务器非正常关闭时（硬件损坏，断电，内存溢出等导致服务器非正常关闭），session会随着服务器的关闭而销毁；

   当服务器正常关闭，在关闭之前，服务器会将内部的session对象序列化保存到服务器的work目录下，变为一个文件。这个过程叫做session的钝化（序列化）；再次将服务器启动起来，钝化着的session会再次回到服务器，变为服务器中的对象，这个过程叫做session的活化（反序列化）。

**(2)作用范围：**在一次会话范围内（获取到的都是同一个session对象）

**(3)主要功能：**在整个会话范围内实现数据的共享



### 案例:使用session模拟购物车

**1、index.html**

```html
<body>
	<h3>点击下面的商品链接, 可以将商品加入购物车</h3>
	<!-- 
	http://localhost/day13-cookie/CartServlet
	http://localhost/day13-cookie/index.html
	 -->
	<p><a href="CartServlet?prod=iphone11">iphone11</a></p>
	<p><a href="CartServlet?prod=vivonex3">vivonex3</a></p>
	<p><a href="CartServlet?prod=xiaomishouji">xiaomishouji</a></p>
	<p><a href="CartServlet?prod=huaweip30">huaweip30</a></p>
	<p><a href="CartServlet?prod=海尔洗衣机">海尔洗衣机</a></p>
	
	<h3>点击下面的支付链接, 可以对购物车中的商品进行结算</h3>
	<a href="PayServlet">支付</a>
</body>
```

**2、CartServlet**

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response)
		throws ServletException, IOException {
	response.setContentType("text/html;charset=utf-8");
	PrintWriter out = response.getWriter();
	//------------------------------------------------------
	//获取将要添加到购物车的商品信息
	String prod = request.getParameter( "prod" );
	//获取一个session对象，将商品信息保存到session中
	HttpSession session = request.getSession();
	//设置session的超时时间为30秒
	//session.setMaxInactiveInterval( 30 );
	session.setAttribute( "cart" , prod );
	//做出响应
	out.write( "成功将 [ "+prod+" ] 加入了购物车~~~" );
}
```

**3、PayServlet**

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response)
		throws ServletException, IOException {
	response.setContentType("text/html;charset=utf-8");
	PrintWriter out = response.getWriter();
	//获取一个session对象(之前的session)
	HttpSession session = request.getSession();
	//从session中获取到要结算的商品信息
	String prod = (String)session.getAttribute( "cart" );
	//对商品进行结算,做出回应
	out.write( "成功为 [ "+prod+" ] 支付了2000.00元~~~~" );
}
```

## 总结:两者的区别

Cookie和session都属于会话技术，都可以保存会话中产生的数据，但由于cookie和session的工作原理和特点不同，因此两者的应用场景也不一样。

**Cookie的特点：**

1. cookie是将会话中产生的数据保存在**浏览器客户端**, 是客户端技术（JS可以访问cookie）

2. cookie是将数据保存在客户端浏览器，容易随着用户的操作导致cookie丢失或者被窃取，因此cookie中保存的数据不太稳定，也不太安全。

3. 但cookie将数据保存在客户端，对服务器端没有太多影响，可以将数据保存很长时间。

4. 总结：因此cookie中适合存储需要长时间保存、但对安全性要求不高的数据。
5. 浏览器对cookie的大小和个数都有限制，一般推荐每一个站点给浏览器发送的cookie数量不超过20个，每一个cookie的大小不超过1kb。
6. Cookie的应用：实现购物车、记住用户名、30天内自动登录等。

**Session的特点**

1. session是将会话中产生的数据保存在**服务器端**，是服务器端技术
2. session将数据存在服务器端的session对象中，相对更加的安全，而且更加稳定。不容易随着用户的操作而导致session中的数据丢失或者是被窃取。
3. 但session是服务器端的对象，在并发量较高时每一个浏览器客户端在服务器端都要对应一个session对象，占用服务器的内存空间，影响效率。
4. 总结：因此session中适合存储对安全性要求较高，但不需要长时间保存的数据。
5. Session的应用：保存登录状态、保存验证码

## 扩展内容

### cookie中保存中文数据的问题

以下问题是针对Tomcat8.0及8.0以下的版本，在Tomcat8.5及8.5以后的版本中已经解决了该问题！

HTTP协议中规定了请求信息和响应信息中不能包含中文数据！

因此通过浏览器向服务器发送中文数据时，浏览器会将中文数据进行URL编码，编码为下面这种格式：

![image-20200225174146534](JAVAWEB-NOTE03.assets/image-20200225174146534.png)

http://localhost/day13-cookie/index.html?user=%E5%BC%A0%E9%A3%9E%E9%A3%9E

将中文数据转成下面这种格式，叫做URL编码：

`张飞飞 ---> URL编码 ---> %E5%BC%A0%E9%A3%9E%E9%A3%9E`

将下面这种格式再次转回中文数据，叫做URL解码：

`%E5%BC%A0%E9%A3%9E%E9%A3%9E ---> URL解码---> 张飞飞`



**问题：当cookie中保存中文数据，将cookie添加到响应中时，会报一个500异常，如下：**

![image-20200226095315873](JAVAWEB-NOTE03.assets/image-20200226095315873.png)

**解决方法是：**

将存入cookie中的先进行URL编码，再存入Cookie中，例如：

![image-20200225174242538](JAVAWEB-NOTE03.assets/image-20200225174242538.png)

从cookie取出来的数据是进行URL编码后的数据，在使用之前需要进行URL解码，例如：

![image-20200225174252978](JAVAWEB-NOTE03.assets/image-20200225174252978.png)

### 获取不到之前的session的问题

将商品保存到session中后，关闭浏览器再打开浏览器，访问服务器，此时获取不到之前的session。因为session是基于Cookie工作的。

在服务器创建一个session后，会为session分配一个独一无二的编号，称之为session的id，在此次响应时，服务器会将session的id以一个名称为JSESSIONID的cookie发送给浏览器保存到浏览器内部。

由于保存sessionid的cookie默认是会话级别的cookie，在浏览器关闭后，cookie会跟着销毁，sessionid也丢失了。因此下次访问服务器，没有session的id就获取不到之前的session。也获取不到session中的商品信息

解决方法：我们可以创建一个名称为JSESSIONID的cookie，其中保存session的ID，并设置cookie的最大存活时间，让cookie保存到硬盘上（即使浏览器关闭，cookie也不会销毁），这样下次访问服务器时，还可以将sessionid带给服务器，服务器可以通过sessionid获取到之前的session。 从session中获取到商品信息

<img src="JAVAWEB-NOTE03.assets/9e195f495be5cc6e805c1185fcc753ad.png" style="zoom:67%;" />



`04-02作业：`

1、描述Cookie保存数据的原理

2、描述session保存数据的原理

3、练习课上案例:使用cookie技术实现购物功能

4、练习课上案例:使用session技术实现购物功能




unit11-MySQL数据库事务
===============

**今日目标:**

- 了解事务的作用

- 掌握事务的四大特性(面试)

- 了解事务的三个并发读问题

- 掌握mysql开启和结束事务

- 了解事物的四个隔离级别


事务及四大特性
--------------

### 什么是事务

数据库事务(Database Transaction)，是指作为单个逻辑工作单元执行的一系列操作，要么完全地执行，要么完全地不执行。

简单的说：事务就是将一堆的SQL语句(通常是增删改操作)绑定在一起执行，要么都执行成功，要么都执行失败，即都执行成功才算成功，否则就会恢复到这堆SQL执行之前的状态。

下面以银行转账为例，张三转100块到李四的账户，这至少需要两条SQL语句：

-   给张三的账户减去100元；

```sql
update 账户表 set money=money-100 where name='张三';
```

-   给李四的账户加上100元。

```sql
update 账户表 set money=money+100 where name='李四';
```

如果在第一条SQL语句执行成功后，在执行第二条SQL语句之前，程序被中断了（可能是抛出了某个异常，也可能是其他什么原因），那么李四的账户没有加上100元，而张三却减去了100元，在现实生活中这肯定是不允许的。

如果在转账过程中加入事务，则整个转账过程中执行的所有SQL语句会在一个事务中，而事务中的所有操作，要么全都成功，要么全都失败，不可能存在成功一半的情况。

也就是说给张三的账户减去100元如果成功了，那么给李四的账户加上100元的操作也必须是成功的；否则，给张三减去100元以及给李四加上100元都是失败的。

### 事务的四大特性

事务的四大特性(ACID)是：

(1)**原子性**（Atomicity）：事务中所有操作是不可再分割的原子单位。事务中所有操作要么全部执行成功，要么全部执行失败。

(2)**一致性**（Consistency）：事务执行后，数据库状态与其它业务规则保持一致。如转账业务，无论事务执行成功与否，参与转账的两个账户金额之和在事务前后应该是保持不变的。

```
张三：1000	1000-500=500		1000
李四：1000	1000+500=1500 		1000
```

(3)**隔离性**（Isolation）：隔离性是指在并发操作中，不同事务之间应该隔离开来，使每个并发中的事务不会相互干扰。也就是说，在事中务查看数据更新时，数据所处的状态要么是另一事务修改它之前的状态，要么是另一事务修改它之后的状态，事务不会查看到中间状态的数据。例如：在A事务中，查看另一B事务(正在修改张三的账户金额)中张三的账户金额，要查看到B事务之前的张三的账户金额，要么查看到B事务之后张三的账户金额。

```
事务1: 查询A、B账户金额之和（1000+1000）
事务2: A转账给B 500元
		A - 500 = 500
		B + 500 = 1500
		
```

(4)**持久性**（Durability）：一旦事务提交成功，事务中所有的数据操作都必须被持久化到数据库中，即使提交事务后，数据库马上崩溃，在数据库重启时，也必须能保证通过某种机制恢复数据。

```
开启事务---A给B转账500元
A: 1000 - 500 = 500	(成功了)	在日志中记录,事务成功,A账户金额更新为500
B: 1000 + 500 = 1500 (成功了)	在日志中记录,事务成功,B账户金额更新为1500
结束事务---回滚/提交
```

MySQL中的事务
-------------

在默认情况下，MySQL每执行一条SQL语句，都是一个单独的事务。因为底层在执行SQL语句之前会自动开启事务，在SQL语句执行完后，会立即结束事务！

如果需要在一个事务中包含多条SQL语句，那么需要手动开启事务和结束事务。

-   开启事务：start transaction；

-   结束事务：commit（提交事务）或 rollback（回滚事务）。

在执行SQL语句之前，先执行 strat transaction，这就开启了一个事务（事务的起点），然后可以去执行多条SQL语句，最后要结束事务，**commit**表示提交，即事务中的多条SQL语句所做出的影响会持久化到数据库中。或者**rollback**，表示回滚，即回滚到事务的起点，之前做的所有操作都被撤消了！

下面演示A账户给B账户转账的例子：

**准备数据：**

```sql
-- 1、创建数据库jt_db数据库(如果不存在才创建)
create database if not exists jt_db charset utf8;
use jt_db; -- 选择jt_db数据库
-- 2、在 jt_db 库中创建 acc 表(银行账户表),要求有id(主键),name(姓名),money(账户金额)
drop table if exists acc;
create table acc(
    id int primary key auto_increment,
    name varchar(50),
    money double
);
-- 3、往 acc 表中, 插入2条记录
insert into acc values(null,'A',1000);
insert into acc values(null,'B',1000);
-- 查询acc表中的所有记录
select * from acc;
```

下面分别演示事务开启及执行一系列SQL之后，回滚事务、提交事务及中断操作的效果。

-- **rollback（回滚事务）**：

```mysql
-- 查询acc账户表中A和B的金额
select * from acc;
-- 开启事务
start transaction;
-- 开始转账，A账户减去100元
update acc set money=money-100 where name='A';
-- 查询acc账户表中A和B的金额
select * from acc;
-- B账户增加100元
update acc set money=money+100 where name='B';
-- 查询acc账户表中A和B的金额
select * from acc;
-- 回滚事务
rollback;
-- 再次查询acc账户表中A和B的金额
select * from acc;
```

\-- **commit（提交事务）**：将上面的操作再做一次，最后将rollback替换为commit，即提交事务

```mysql
commit;
```

\-- **中断操作**：将上面的操作再做一次，最后将rollback替换为quit，即中断操作

```mysql
quit;
```

事务并发读问题
--------------

### 事务并发读问题

多个事务对相同的数据同时进行操作，这叫做事务并发。

在事务并发时，如果没有采取必要的隔离措施，可能会导致各种并发问题，破坏数据的完整性等。这些问题中，其中有三类是读问题，分别是：脏读、不可重复读、幻读。

(1)**脏读**（dirty read）：在一个事务中，读取到另一个事务未提交更新的数据，即读取到了脏数据；

例如：A给B转账100元但未提交事务，在B查询后，A做了回滚操作，那么B查询到了A未提交的数据，就称之为脏读。

```
提示：需要将数据库的事务隔离级别设置为最低，才能够看到脏读现象
事务1：开启事务; A - 100 = 900; B + 100 = 1100; (没有提交事务)
事务2：开启事务; 查询B账户的金额 1100, 这个过程叫做脏读, 1100就是一个脏数据
```

(2)**不可重复读**（unrepeatable read）：对同一记录的两次读取结果不一致，因为在两次查询期间，有另一事务对该记录做了修改（是针对修改操作）

例如：在事务1中，前后两次查询A账户的金额，在两次查询之间，另一事物2对A账户的金额做了修改（并且也提交了事务），此种情况可能会导致事务1中，前后两次查询的结果不一致。这就是不可重复读。

```
事务1：开启事务---
	第一次读取A账户的金额：1000
	第二次读取A账户的金额：900
事务2：开启事务---
	A账户 - 100 = 900;
	提交事务---
```

(3)**幻读（虚读）**（phantom read）：对同一张表的两次查询结果不一致，因为在两次查询期间，有另一事务进行了插入或者是删除操作(是针对插入或删除操作)；

```sql
事务1：开启事务---
	select * from acc where id=3;//不存在id为3的记录
	insert into acc value(3,'C',2000);
	select * from acc where id=3;//存在id为3的记录
事务2：开启事务---
	insert into acc value(3,'C',2000);
	提交事务---
```

注意:mysql默认的是不允许出现脏读和不可重复读，所以在下面演示之前需要设置mysql允许出现脏读、不可重复读等。

```mysql
set tx_isolation='read-uncommitted'; -- 设置mysql的事务隔离级别
```

**1、脏读示例：**

```sql
-- 在窗口1中，开启事务，执行A给B转账100元
set tx_isolation='read-uncommitted'; -- 允许脏读、不可重复读、幻读
use jt_db; -- 选择jt_db库
start transaction; -- 开启事务
update acc set money=money-100 where name='A';
update acc set money=money+100 where name='B';
-- 在窗口2中，开启事务，查询B的账户金额
set tx_isolation='read-uncommitted'; -- 允许脏读、不可重复读、幻读
use jt_db; -- 选择jt_db库
start transaction; -- 开启事务
select * from acc where name='B'; -- 出现脏数据
-- 切换到窗口1，回滚事务，撤销转账操作。
rollback; -- 回滚事务
-- 切换到窗口2，查询B的账户金额
select * from acc where name='B';
```

在窗口2中，B看到自己的账户增加了100元（此时的数据A操作事务并未提交），此种情况称之为"脏读"。

**2、不可重复读示例：**

```sql
-- 在窗口1中，开启事务，查询A账户的金额
set tx_isolation='read-uncommitted'; -- 允许脏读、不可重复读、幻读
use jt_db; -- 选择jt_db库
start transaction; -- 开启事务
select * from acc where name='A';
-- 在窗口2中，开启事务，查询A的账户金额减100
set tx_isolation='read-uncommitted'; -- 允许脏读、不可重复读、幻读
use jt_db; -- 选择jt_db库
start transaction; -- 开启事务
update acc set money=money-100 where name='A'; -- A账户减去100
select * from acc where name='A';
commit; -- 提交事务
-- 切换到窗口1，再次查询A账户的金额。
select * from acc where name='A'; -- 前后查询结果不一致
```

在窗口1中，前后两次对同一数据(账户A的金额)查询结果不一致，是因为在两次查询之间，另一事务对A账户的金额做了修改。此种情况就是"不可以重复读"

**3、幻读示例：**

```sql
-- 在窗口1中，开启事务，查询账户表中是否存在id=3的账户
set tx_isolation='read-uncommitted'; -- 允许脏读、不可重复读、幻读
use jt_db; -- 选择jt_db库
start transaction; -- 开启事务
select * from acc where id=3;
-- 在窗口2中，开启事务，往账户表中插入了一条id为3记录，并提交事务。
-- 设置mysql允许出现脏读、不可重复度、幻读
set tx_isolation='read-uncommitted';
use jt_db; -- 选择jt_db库
start transaction; -- 开启事务
insert into acc values(3, 'C', 1000);
commit; -- 提交事务
-- 切换到窗口1，由于上面窗口1中查询到没有id为3的记录，所以可以插入id为3的记录。
insert into acc values(3, 'C', 1000); -- 插入会失败!
```

在窗口1中，查询了不存在id为3的记录，所以接下来要执行插入id为3的记录，但是还未执行插入时，另一事务中插入了id为3的记录并提交了事务，所以接下来窗口1中执行插入操作会失败。

探究原因，发现账户表中又有了id为3的记录（感觉像是出现了幻觉）。这种情况称之为"幻读"

以上就是在事务并发时常见的三种并发读问题，那么如何防止这些问题的产生？

可以通过设置事务隔离级别进行预防。

### 事务隔离级别

事务隔离级别分四个等级，在相同数据环境下，对数据执行相同的操作，设置不同的隔离级别，可能导致不同的结果。不同事务隔离级别能够解决的数据并发问题的能力也是不同的。

```sql
set tx_isolation='read-uncommitted';
```

**1、READ UNCOMMITTED（读未提交数据）**

安全性最差，可能出现任何事务并发问题(比如脏读、不可以重复读、幻读等)

但性能最好（不使用!!）

**2、READ COMMITTED（读已提交数据）**（Oracle默认）

安全性较差

性能较好

可以防止**脏读**，但不能防止不可重复读，也不能防止幻读；

**3、REPEATABLE READ（可重复读）**（MySQL默认）

安全性较高

性能较差

可以防止**脏读**和**不可重复读**，但不能防止幻读问题；

**4、SERIALIZABLE（串行化）**

安全性最高，不会出现任何并发问题，因为它对同一数据的访问是串行的，非并发访问；

性能最差；(不使用!!)

MySQL的默认隔离级别为REPEATABLE READ，即可以防止脏读和不可重复读

### 设置隔离级别(了解)

0、MySQL查询当前的事务隔离级别

```sql
select @@tx_isolation;
```

**1、MySQL设置事务隔离级别（了解）**

(1) set tx_isolation='read-uncommitted';　

安全性最差，容易出现**脏读**、**不可重复读**、**幻读**，但性能最高

(2) set tx_isolation='read-committed';

安全性一般，可防止**脏读**，不能防止**不可重复读**、**幻读**

(3) set tx_isolation='repeatable-read';

安全性较好，可防止**脏读**、**不可重复读**，但不能防止**幻读**

(4) set tx_isolation='serialiable';

安全性最好，可以防止一切事务并发问题，但是性能最差。

**2、JDBC设置事务隔离界别**

JDBC中通过Connection提供的方法设置事务隔离级别：

```sql
Connection.setTransactionIsolation(int level)
```

参数可选值如下：

```sql
Connection.TRANSACTION_READ_UNCOMMITTED 1（读未提交数据）
Connection.TRANSACTION_READ_COMMITTED 2（读已提交数据）
Connection.TRANSACTION_REPEATABLE_READ 4（可重复读）
Connection.TRANSACTION_SERIALIZABLE 8（串行化）
Connection.TRANSACTION_NONE 0（不使用事务）
```

提示：在开发中，一般情况下不需要修改事务隔离级别

**3、JDBC中实现转账例子**

提示：JDBC中默认是自动提交事务，所以需要关闭自动提交，改为手动提交事务

也就是说, 关闭了自动提交后, 事务就自动开启, 但是执行完后需要手动提交或者回滚!!

(1)执行下面的程序，程序执行没有异常，转账成功！A账户减去100元，B账户增加100元。

(2)将第4步、5步中间的代码放开，再次执行程序，在转账过程中抛异常，转账失败！由于事务回滚，所以A和B账户金额不变。

```sql
public static void main(String[] args) throws SQLException {
    Connection conn = null;
    Statement stat = null;
    ResultSet rs = null;
    try {
        //1.获取连接
        conn = JdbcUtil.getConn();
        //2.关闭JDBC自动提交事务（默认开启事务）
        conn.setAutoCommit(false);
        //3.获取传输器
        stat = conn.createStatement();
        /* ***** A给B转账100元 ***** */
        //4.A账户减去100元
        String sql = "update acc set money=money-100 where name='A'";
        stat.executeUpdate(sql);
        //int i = 1/0; // 让程序抛出异常，中断转账操作
        //5.B账户加上100元
        sql = "update acc set money=money+100 where name='B'";
        stat.executeUpdate(sql);
        //6.手动提交事务
        conn.commit();
        System.out.println("转账成功！提交事务...");
    } catch (Exception e) {
    	e.printStackTrace();
    	//一旦其中一个操作出错都将回滚，使两个操作都不成功
    	conn.rollback();
   		System.out.println("执行失败！回滚事务...");
    } finally{
    	JdbcUtil.close(conn, stat, rs);
    }
}
```











