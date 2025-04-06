# 概述





本笔记始建于2025/03/25，用于总和JavaWeb所有必须的技术栈

内容组织结构如下：

- 前端
  - 基础
    - HTML
    - CSS
    - JavaScript
  - 框架
    - Ajax
    - Vue
    - React
- 后端
  - 基础
    - Java SE
    - Java EE
  - 工具链
    - Maven
    - Tomcat
    - Linux
    - Docker
  - Spring基础
    - Spring Framework
    - SpringBoot
  - 数据库
    - MySQL
    - MyBatis
    - MyBatis-Plus
    - Redis
  - 云
    - SpringCloud









# 前端









# 后端







## 基础





### Java SE



#### JDBC

##### 概述

Java数据库连接，（Java Database Connectivity，简称JDBC）是SUN公司提供的一套规范，由一系列类和接口组成。

数据库厂商通过实现该规范，使得Java程序可以通过统一的接口访问不同的数据库驱动。

##### 流程

- 加载数据库驱动

  添加项目依赖，如

  ```xml
  <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>8.0.33</version>
  </dependency>
  ```

  使用反射加载（可选的）：

  ```java
  Class.forName("com.mysql.jdbc.driver");
  ```

- 新建数据库连接

  通过`java.sql.DriverManager`和`java.sql.Connection`新建并获取数据库连接

  ```java
  Connection connection = DriverManager.getConnection(url, user, password);
  ```

- 创建并执行SQL语句

  通过`java.sql.Statement`执行SQL语句，使用`java.sql.ResultSet`检索查询结果

  ```java
  Statement statement = connection.createStatement();
  ResultSet resultSet = statement.executeQuery(sql);
  ```

- 释放资源

  ```java
  resultSet.close();
  statement.close();
  connection.close(); // 将关闭事务
  ```

##### API

概述各主要类、接口的功能

`DriverManager`

- 注册驱动

  通常的驱动实现（如mysql）会在类加载阶段自动注册驱动

-  创建数据库连接

`Connection`

- 创建SQL语句对象

  如`Statement`、`PreparedStatement`

- 管理事务

`Statement`

- 执行SQL语句
- 批处理SQL语句

`PreparedStatement`

- `Statement`的含预编译功能子类

`CallableStatement`

- `PreparedStatement`的含数据库存储过程或函数调用功能子类

`ResultSet`

- 用于结果的存储、遍历、查询

##### 预编译与参数化

使用`PreparedStatement`用于解决SQL注入问题，流程参考如下：

- 使用`?`占位符替代原参数
- 创建预编译SQL语句
- 设置参数
- 执行语句

```java
String sql = "SELECT * FROM CITY WHERE CountryCode = ?";
PreparedStatement preparedStatement = connection.prepareStatement(sql);
preparedStatement.setString(1, "CHN");
ResultSet resultSet = preparedStatement.executeQuery();
```

##### 事务

- 使用`connection.setAutoCommit(false)`关闭自动事务提交（自行管理事务时使用）
- 使用`connection.commit()`提交事务
- 使用`connection.rollback()`回滚事务

##### 资源释放

JDBC中的`Connection`、`ResultSet`等会占用大量的数据库、系统或内存资源，手动管理时应当及时释放





### Java EE



#### 数据库连接池

##### 概述

连接池通过管理和复用少数几个数据库连接`java.sql.Connection`，使得

- 数据库连接的生命周期管理代码分离以降低耦合
- 减少创建和销毁数据库连接所花费的时间
- 减少维护数据库连接所花费的资源

`javax.sql.DataSource`/`jakarta.sql.DataSource`是`java.sql.DriverManager`的替代接口，为连接池和分布式事务提供了支持

可通过自行实现`DataSource`或使用现有库来使用数据库连接池功能

##### Druid

使用硬编码初始化

```java
DruidDataSource dataSource = new DruidDataSource();
dataSource.setDriverClassName("com.mysql.cj.jdbc.Driver");
dataSource.setUrl("jdbc:mysql://localhost:3306/world");
dataSource.setUsername("root");
dataSource.setPassword("123456");
```

使用配置文件初始化

- `/src/main/resouces/db.properties`

  ```properties
  driverClassName=com.mysql.cj.jdbc.Driver
  url=jdbc:mysql://localhost:3306/world
  username=root
  password=123456
  ```

- 核心代码

  ```java
  Properties properties = new Properties();
  properties.load(this.getClass().getClassLoader().getResourceAsStream("db.properties"));
  DataSource dataSource = DruidDataSourceFactory.createDataSource(properties);
  ```

连接的获取与归还

```java
Connection connection = dataSource.getConnection();
/*...*/
connection.close(); // 实质上归还了连接而不是立即关闭连接
```

##### C3P0

一款较早期的开源数据库连接池，使用方式类似于`Druid`







## 工具链





### Tomcat



#### 概述

Tomcat是Apache Jakarta的开源Web Servlet容器，实现了部分Java EE/Jakarta EE规范

- ≤ Tomcat 9：支持Java EE
- ≥ Tomcat 10：支持Jakarta EE

常用版本：

- Tomcat 9：支持Java EE 8
- Tomcat 10：支持部分Jakarta EE 10
- Tomcat 11：支持部分Jakarta EE 11

注意目标为Tomcat 9的Web应用无法发布在Tomcat 10上

支持的Java EE/Jakarta EE规范有：

- Servlet
- JSP
- WebSocket

若需要使用实现了所有Java EE/Jakarta EE规范的Java EE全栈服务器，可参考`WebSphere`、`WebLogic`等



#### Tomcat 9

可以在官方文档里参考或学习详细的使用方法：[Tomcat 9 文档](https://tomcat.apache.org/tomcat-9.0-doc/index.html)

##### 使用入门

- 安装和确保合适的Java环境

  - 推荐JDK 8运行环境

    较高或较低的版本均可能产生意外的问题

  - 配置`JAVA_HOME`环境变量

- 下载并解压合适的Tomcat包

  如`Core 64-bit Windows.zip`，包含核心jar程序和适用于Windows的已编译本机库、bat脚本等

- 修改或确认`/conf/server.xml`中的端口号（默认为8080）使其处于可用状态

  ```xml
  <!--默认的配置如下-->
  <Connector port="8080" protocol="HTTP/1.1"
                 connectionTimeout="20000"
                 redirectPort="8443"
                 maxParameterCount="1000"
                 />
  ```

- 解决可能的终端输出乱码问题

  可选的方案有：

  - 修改`/conf/logging.properties`中的`java.util.logging.ConsoleHandler.encoding`为本机默认编码
  - 修改启动脚本中的JVM参数

- 使用`/bin`目录下的`startup.bat`和`shutdown.bat`等脚本启动或关闭Tomcat服务器

- 访问http://localhost:8080确认服务器运行状况

##### Context

在Tomcat中，Context概指一个Web应用程序，在一个Tomcat可以同时存在多个Context

##### 目录结构

- `/bin`：脚本等可执行文件，用于启动、关闭、管理Tomcat服务器
- `/conf`：配置文件，核心配置文件为`server.xml`
- `/logs`：日志文件
- `/webapps`：Web应用程序存储目录
  - `/ROOT`：默认Web应用目录
    - `/WEB-INF`
      - `/classes`：Web应用的Java类文件和资源
      - `/lib`：Web应用Java程序依赖的Jar文件
      - `web.xml`：Web应用核心描述文件
    - `/static_resources_dir_1...`：更多的静态文件
    - `*.html、*.js、*.jsp...`：静态资源文件等
  - `/app_dir_1...`：更多的Web应用
- `/lib`：Web应用运行所需的Jar依赖文件包
- `/temp`：JVM临时文件目录
- `/work`：Tomcat的临时工作目录，JSP编译后的`.java`、`.class`等文件也存储于此

##### 部署Web应用

- 静态部署：在Tomcat启动前部署或更新Web应用
- 动态部署：在Tomcat运行时部署或更新Web应用，依赖Tomcat的配置和自动部署工具

可以使用以下方法部署待发布的Web应用

- 复制Web应用文件夹到`/webapps`或其子文件夹

- 复制WAR存档的Web应用到`/webapps`或其子文件夹

  Tomcat会自动解压该WAR存档，但更新Web应用时应删除原先的已解压的文件夹

- 使用Tomcat Manager进行图形化的管理

##### Manager

Tomcat Manager时一个Tomcat内置的Web服务，提供了以下图形化便捷功能：

- 远程部署、更新Web应用
- 管理Web应用
- 服务器资源和运行状态的统计
- SSL/TLS管理

等等

##### 其他功能

Tomcat支持的其他常用功能包括：

- WebSocket支持
- 虚拟主机
- Realm支持
- JNDI
- JSP支持
- SSL/TLS支持
- CGI
- 反向代理
- 负载均衡
- 集群支持
- Windows服务
