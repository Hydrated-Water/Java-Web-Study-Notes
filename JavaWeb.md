# 概述





本笔记始建于2025/03/25，用于总和JavaWeb所有必须的技术栈

对于相对复杂的知识体系，本笔记将尽可能按“使用方法”到“原理细节”的顺序进行内容组织，做到先知其然，后知其所以然的阶段性系统性学习

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
    - Lombok
    - Maven
    - Tomcat
    - Linux
    - Docker
  - Spring 基础
    - Spring MVC 入门
    - Spring Boot 入门
    - Spring Boot 进阶
  - 数据库
    - MySQL
    - MyBatis
    - MyBatis-Plus
    - Redis
  - 工具类库
    - Lombok









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





### Lombok

==TODO 空的章节==





### Maven

可在Apache Maven官网获得更详细的[文档](https://maven.apache.org/guides/)



#### 概述

Maven是由Apache托管的用于简化Java项目构建流程的开源工具

##### 为什么使用Maven

- 传统项目依赖与多项目间依赖手动管理困难
- 不同开发环境的项目结构互相冲突
- 手动和非统一的项目构建、测试、打包、发布流程混乱

##### Maven的功能

- 依赖管理

  统一的便捷的项目依赖管理与项目间依赖管理

- 项目构建

  统一的编译、测试、打包和发布流程

- 项目结构

  统一的便于管理、扩展、维护的项目结构

##### Maven模型

![Maven模型](JavaWeb笔记图片/Maven模型.png)



#### 部署

##### 安装

Windows环境无需安装守护进程时仅解压即可

##### 配置

- 环境变量

  - `JAVA_HOME`：用于Maven运行所需的Java环境
  - `MAVEN_HOME`：用于Maven自身及其他依赖Maven的应用
  - `PATH`：用于命令行，值应为`%MAVEN_HOME%\bin`

- 全局配置文件`%MAVEN_HOME%\conf\settings.xml`

  - 本地仓库`settings.localRepository`

    默认值`${user.home}/.m2/repository`

  - 镜像仓库`settings.mirrors`

    Maven默认远程中央仓库地址`repo.maven.apache.org/maven2/`

    使用如下配置增加阿里云镜像仓库

    ```xml
    <mirror>
        <id>nexus-aliyun</id>
        <mirrorOf>central</mirrorOf>
        <name>Nexus aliyun</name>
        <url>http://maven.aliyun.com/nexus/content/groups/public</url>
    </mirror>
    ```

- 用户配置文件`settings.xml`

  与全局配置文件格式相同，用于覆盖全局配置文件配置，位于本地仓库路径下

  当本地仓库路径未修改时默认路径即为`${user.home}/.m2/settings.xml`



#### 目录结构

Maven提供了一系列标准的项目目录结构

- `src`/    源文件
  - `main`/
    - `java`/
    - `resources`/
    - `webapp`/
  - `test`/
    - `java`/
    - `resources`/
- `target`/    构建输出
- `pom.xml`    POM声明



#### 命令

##### 原型

可使用原型（Archetype）快速创建标准的目录结构的项目

参考`mvn archetype:generate`

##### 构建

在Maven项目根目录（`pom.xml`所在文件夹）可执行若干构建命令

- `mvn clean`：清除构建输出`target/`
- `mvn compile`：编译源代码文件并输出`.class`文件与资源文件到`target/`
- `mvn test`：编译测试源代码文件并输出`.class`文件与资源文件到`target/`，执行测试代码并输出结果报告到`target/surefire-reports`
- `mvn package`：编译、测试源代码与资源文件并打包为`.jar`文件输出到`target/`
- `mvn install`：编译、测试、打包源代码与资源文件并存储到本地仓库



#### IDEA整合

IDEA通常使用自带Maven，如果需要使用自行配置的Maven应通过IDEA设置进行修改，注意IDEA支持的Maven版本存在滞后

在IDEA运行/调试配置中可新增Maven运行/调试，用于在运行/调试时强制使用Maven构建插件

在使用IDEA新建项目时可选择Maven Archetype使用Maven原型功能快速创建标准目录结构的项目



#### POM

POM即项目对象模型（Project Object Model），是Maven识别、解析、管理项目的依据，由`pom.xml`进行声明

##### 内容

`pom.xml`支持的内容如下：

```xml
<project
         xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    
    <!-- POM版本，值应为"4.0.0"，Maven目前仅支持该版本 -->
    <modelVersion>4.0.0</modelVersion>
    
    <!-- The Basics -->
    
    <!-- 项目的坐标 groupId:artifactId:version -->
    <groupId>...</groupId>
    <artifactId>...</artifactId>
    <version>...</version>
    <!-- 项目打包（生命周期"package"）方式，与项目类型相关，默认值"jar"，常见值如"jar"、"war"、"rar"、"pom"等，IDEA会识别该值以判断项目类型 -->
    <packaging>...</packaging>
    <!-- 依赖声明 -->
    <dependencies>...</dependencies>
    <!-- 用于声明继承的父POM -->
    <parent>...</parent>
    <!-- 用于继承中父POM进行依赖管理 -->
    <dependencyManagement>...</dependencyManagement>
    <!-- 用于聚合模块的子模块声明 -->
    <modules>...</modules>
    <!-- 子模块声明 -->
    <properties>...</properties>
    
    <!-- Build Settings -->
    
    <build>...</build>
    <reporting>...</reporting>
    
    <!-- More Project Information -->
    
    <name>...</name>
    <description>...</description>
    <url>...</url>
    <inceptionYear>...</inceptionYear>
    <licenses>...</licenses>
    <organization>...</organization>
    <developers>...</developers>
    <contributors>...</contributors>
    
    <!-- Environment Settings -->
    <issueManagement>...</issueManagement>
    <ciManagement>...</ciManagement>
    <mailingLists>...</mailingLists>
    <scm>...</scm>
    <prerequisites>...</prerequisites>
    <repositories>...</repositories>
    <pluginRepositories>...</pluginRepositories>
    <distributionManagement>...</distributionManagement>
    <profiles>...</profiles>
    
</project>
```

POM的最小内容应包含：

- `project`根
- `modelVersion`
- `groupId`
- `artifactId`
- `version`

##### Maven坐标

Maven项目的唯一标识，由`groupId`、`artifactId`、`version`组成

##### 属性

POM中类似常量的概念，通过通过`${属性名}`访问，如`${project.version}`

- 自定义属性

  通过在POM中使用`properties`标签声明，如

  ```xml
  <properties>
      <myproject.version>${project.version}</project-version>
      <myproject.name>app</myproject.name>
      <foobar>12345</foobar>
  </properties>
  ```

- Project属性，属性名以`project.`开头，如`project.version`即POM中`version`标签声明的值

- Settings属性

  由Maven的`settings.xml`声明的属性值，属性名以`settings.`开头

- Java系统属性

  可通过`System.getProperties()`访问的属性，属性名通常以`java.`，如`java.home`

- 环境变量属性

  系统/Shell环境变量，属性名以`env.`开头，如`env.PATH`

  注意环境变量名必须为大写，包括在不区分大小写的Windows上以小写声明的环境变量

可使用命令`mvn help:system`查看支持的Java系统属性和环境变量

属性可以继承



#### 依赖

Maven项目通过在`pom.xml`声明依赖和在本地或远程仓库中存储的依赖来进行依赖管理

##### 依赖传递

若工件A**直接依赖**工件B，工件B直接依赖工件C，则默认情况下工件A**间接依赖**工件C

##### 依赖调解

项目的所有依赖组成依赖树，可通过命令`mvn dependency:tree`查看依赖树

当依赖树中存在多个`groupId`和`artifactId`相同的依赖时将发生依赖冲突，通过依赖调解解决冲突

规则如下：

- 同一`pom.xml`文件相同依赖后声明的优先
- 在依赖树上深度低者优先
- 在依赖树上同一深度先声明的优先

##### 可选依赖

通过声明某依赖的`denpendency.optional`值为`true` ，使得该项目的该依赖对依赖该项目的项目隐藏

简单来说，就是使当前项目的某依赖无法被传递

`optional`的默认值是`false`

```xml
<dependency>
    <groupId>com.example.web</groupId>
    <artifactId>lib</artifactId>
    <version>1.0</version>
    <optional>true</optional>
</dependency>
```

##### 排除依赖

通过对某依赖声明`denpendency.exclusions`的一个或多个`exclusion`，使得从该依赖传递的一个或多个间接依赖被排除

在`exclusion`中仅需声明`groupId`和`artifactId`

```xml
<dependency>
    <groupId>com.example.web</groupId>
    <artifactId>lib</artifactId>
    <version>1.0</version>
    <exclusions>
        <exclusion>
            <groupId>com.example.util</groupId>
            <artifactId>util</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

##### 依赖范围

通过`denpendency.scope`声明依赖的作用范围

`scope`的默认值为`compile`，可选的值有

- `compile`：参与所有编译、测试编译、测试执行和打包
- `provided`：参与编译、测试编译、测试执行，但不参与打包，用于目标运行环境已提供该依赖的情况，如Servlet API
- `runtime`：不参与编译、测试编译，但参与测试执行和打包，常用于仅在运行时所需的依赖，如JDBC
- `test`：参与测试编译、测试执行，不参与编译和打包
- `system`
- `import`

```xml
<dependency>
    <groupId>com.example.web</groupId>
    <artifactId>lib</artifactId>
    <version>1.0</version>
    <scope>compile</scope>
</dependency>
```

##### 依赖范围的传递

当某项目的某依赖声明的依赖范围，且该依赖对其依赖也声明了依赖范围，那么该项目对该间接依赖的依赖范围将通过以下规则获得

- `provided`和`test`声明的依赖无法被传递，因此声明了`provided`或`test`的间接依赖项将被忽略
- 最终的依赖范围将取两个依赖范围的交集

简单来说，如果A依赖B，B依赖C，那么C对于A的依赖范围如下表所示

| A声明B的范围 \ B声明C的范围 | compile  | provided | runtime  | test |
| --------------------------- | -------- | -------- | -------- | ---- |
| compile                     | compile  | 忽略     | runtime  | 忽略 |
| provided                    | provided | 忽略     | provided | 忽略 |
| runtime                     | runtime  | 忽略     | runtime  | 忽略 |
| test                        | test     | 忽略     | test     | 忽略 |



#### 生命周期与插件

Maven生命周期是指构建流程中的各个阶段的总和，每个阶段都需要一个特定的插件实现，每个阶段的执行必须依赖前一个阶段的执行结果

Maven共有三类生命周期：

- clean：用于清理
- default：用于编译、测试、打包、安装、部署等
- site：生成报告、发布站点等

![image-20250423162954695](JavaWeb笔记图片/Maven生命周期.png)

常用的生命周期阶段：

- clean 移除上一次构建生成的文件
- compile 编译项目源代码
- test 使用合适的单元测试框架运行测试
- package 将编译后的文件打包
- install 安装项目到本地仓库

通过执行特定的Maven命令即可通过特定的Maven插件执行指定的阶段

可通过在`pom.xml`中声明插件来实现更复杂的功能



#### 多模块

Maven支持项目以多模块的方式管理项目的编码、编译、测试、运行等构建流程

##### 原始

通过仓库和依赖管理实现多模块的构建管理，通过手动管理的方式按照合理顺序从底层到高层对模块进行构建和安装、发布

如对于依赖A-B、B-C、B-D、D-E，当D模块更新时，需要依次对D、B、A模块分别进行编译、测试、打包、安装、发布的构建流程，使得上层模块能正确使用下层已经更新了的模块

##### 聚合

通过Maven定义聚合模块，在聚合模块中声明其所有子模块，使得对聚合模块的构建等效于对其所有子模块的合理顺序的依次构建，实现多模块构建管理的自动化，即执行聚合模块的单个构建命令等效于对所有子模块执行有顺序的构建命令

构建顺序通常由Maven通过依赖树决定，使得依赖树中更深的子模块更早构建，如果子模块不互相依赖，那么构建顺序由模块的声明顺序决定

常见项目结构：

- `App/`
  - `SubApp1`
    - `src/`
    - `pom.xml` 子模块POM声明
  - `SubApp2`
    - `src/`
    - `pom.xml` 子模块POM声明
  - `pom.xml` 聚合模块POM声明

或

- `App/`
  - `pom.xml` 聚合模块POM声明
- `SubApp1`
  - `src/`
  - `pom.xml` 子模块POM声明
- `SubApp2`
  - `src/`
  - `pom.xml` 子模块POM声明

聚合模块声明：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">

  <modelVersion>4.0.0</modelVersion>

  <groupId>com.example.pom</groupId>
  <artifactId>pom</artifactId>
  <version>1.0</version>

  <!-- 聚合模块必须声明打包方式为"pom" -->
  <packaging>pom</packaging>

  <!-- 通过modules标签声明子模块，值为各子模块的相对目录 -->
  <modules>
    <!-- 该子模块位于聚合模块目录内 -->
    <module>Module A</module>
    <!-- 该子模块位于聚合模块目录旁 -->
    <module>../Module B</module>
  </modules>

</project>
```

##### 继承

Maven提供了继承的功能，可用于在多模块构建管理中，让多个子模块POM继承同一个父POM，以用于简化重复的配置，处理可能的依赖版本冲突等问题

- 在子模块的`pom.xml`中声明继承

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    
    <!-- 声明继承 -->
    <parent>
        <groupId>com.example.pom</groupId>
        <artifactId>parent</artifactId>
        <version>1.0</version>
        <!-- 非必须，Maven会先尝试从相对路径中获取，再尝试从仓库中获取-->
        <relativePath>../pom.xml</relativePath>
    </parent>

    <!-- 从父POM中继承groupId和version的值，因此此处可无需重复声明，或此处可覆盖父POM的值 -->
    <artifactId>app</artifactId>
    
    <dependencies>
        <dependency>
            <!-- 从父POM中继承version值，因此此处可无需重复声明 -->
            <groupId>com.example.lib</groupId>
            <artifactId>lib</artifactId>
        </dependency>
    </dependencies>

</project>
```

- 父POM应另属于一个新模块，声明父POM

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example.pom</groupId>
    <artifactId>parent</artifactId>
    <version>1.0</version>
    
    <!-- 必须声明打包方式为"pom" -->
    <packaging>pom</packaging>
    
    <!-- 通过该标签声明依赖版本，使得子POM在声明依赖时无需指定版本 -->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>com.example.lib</groupId>
                <artifactId>lib</artifactId>
                <version>1.0</version>
            </dependency>
            <dependency>
                <groupId>com.example.lib</groupId>
                <artifactId>lib2</artifactId>
                <version>2.0</version>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <!-- 对于build.plugins，同样可通过build.pluginManagement标签来声明plugin进行插件管理 -->
    
</project>
```

- 可被继承的POM标签包含

  ```markdown
  - groupId
  - version
  - description
  - url
  - inceptionYear
  - organization
  - licenses
  - developers
  - contributors
  - mailingLists
  - scm
  - issueManagement
  - ciManagement
  - properties
  - dependencyManagement
  - dependencies
  - repositories
  - pluginRepositories
  - build
    - plugin executions with matching ids
    - plugin configuration
    - etc.
  - reporting
  ```

- 不会被继承的POM标签包含

  ```markdown
  - artifactId
  - name
  - prerequisites
  ```

- 继承和聚合可同时使用，且通常共同使用；在一个多模块的项目中，父模块同时承担着通过继承功能管理多模块项目依赖和通过聚合功能管理多模块构建流程的功能



#### 版本管理

##### 版本号约定

![image-20250504201042471](C:\CRIM\Program\Web\Resources\Documentation\JavaWeb-all\JavaWeb笔记图片\版本号约定.png)



#### 更多

- 资源文件处理 `build.resources`
- 多环境 `profiles`
- 跳过测试
  - IDEA跳过测试选项
  - 命令参数`-D skipTests`
  - `build.plugin`中配置
- 远程仓库服务器的部署、配置与使用





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







## Spring 基础





### Spring MVC 入门

<u>知其然</u>



#### 概述

##### 介绍

Spring MVC全称Spring Web MVC，是基于Servlet API的Web框架

##### Hello World

构建Spring Boot Hello World应用，参考[Spring Quick Start](https://spring.io/quickstart)或更详细的[Spring Boot First Application](https://docs.spring.io/spring-boot/tutorial/first-application/index.html)

编写表现层控制器类代码，注解`@Controller`声明该类为控制类，使用请求匹配注解`@RequestMapping("/helloworld")`使方法绑定到路径`/helloworld`，使用注解`@ResponseBody`使方法返回值作为响应体数据内容，方法返回一个String类型的JSON格式文本

```java
@Controller
public class HelloWorldController {
    
    @RequestMapping("/helloworld")
    @ResponseBody
    public String helloWorld(){
        return "{'data':'Hello World!'}";
    }
    
}
```

向预定路径发送GET请求

```http
GET http://localhost:8080/helloworld
```

得到响应结果

```bash
HTTP/1.1 200 
Content-Type: text/plain;charset=UTF-8
Content-Length: 23
Date: Wed, 31 Jul 2024 06:53:33 GMT
Keep-Alive: timeout=60
Connection: keep-alive

{'data':'Hello World!'}
```



#### 路由

##### 概述

可以通过`@RequestMapping`等注解声明端点（endpoint）映射，以使控制器方法匹配特定的HTTP方法、URL、请求参数、请求头等

##### 请求方法映射

使用注解`@RequestMapping`的`method`属性限定匹配一个或多个请求方法，或使用便捷的注解如`@GetMapping`、`@PostMapping`、`@PutMapping`、`@DeleteMapping`等注解实现相同的效果

```java
@RequestMapping(value = "/bar2", method = {RequestMethod.GET, RequestMethod.POST})
public String bar2(@RequestParam(required = false) List<String> info, Account account) {
    return "GET : " + Objects.toString(info) + ":" + Objects.toString(account);
}

@DeleteMapping(value = "/bar2")
public String bar21(@RequestParam(required = false) List<String> info, Account account) {
    return "DEL : " + Objects.toString(info) + ":" + Objects.toString(account);
}
```

可以使用

如果请求方法不同，两个控制器方法可以绑定到同一个请求路径上

##### 请求路径映射

###### 基础

使用注解`@RequestMapping`可为控制器类和方法设定请求路径映射

当标注在类上时，设定为此控制类所有请求映射路径的前缀

```java
@Controller
@RequestMapping("/foo")
public class FooBarController {
    
    @RequestMapping("/bar")
    @ResponseBody
    public String bar(){
        return "{'data':'FooBar'}";
    }
    
    @RequestMapping("/baz")
    @ResponseBody
    public String baz(){
        return "{'data':'FooBaz'}";
    }
}
```

请求

```http
GET http://localhost:8080/foo/bar
```

```http
GET http://localhost:8080/foo/baz
```

注意`@RequestMapping("/")`与`@RequestMapping("")`的区别，在不同版本的Spring MVC中的处理方式可能不同

###### 模式

模式是通过`@RequestMapping`等方式对控制器方法或类声明的进行请求路径匹配的规则

Spring MVC支持两种路径匹配方案：

- `PathPattern` 更高效的匹配方案
- `AntPathMatcher` 原始的匹配方案

其中`PathPattern`已于Spring MVC 6.0起默认使用

`PathPattern`支持的匹配模式：

- 单字符匹配 `?`

  如`/foobar/te?t`，用于匹配除`/`等特殊字符以外的任意一个字符

- 路径段多字符匹配 `*`

  如`/foobar/*.html`，用于匹配除`/`等特殊字符以外的零到多个字符

- 多路径段匹配 `**`

  如`/foobar/**`，用于匹配包括`/`字符在内的多个字符组成的子路径，对于`/foobar/**`，包括`/foobar`、`/foobar/`、`/foobar//`等在内的畸形路径均可能匹配，但具体的特性受匹配方案、Spring MVC版本和配置等多方面影响

  不可以使用如`/foobar/**/test`等形式

- 路径段路径变量 `{variable}`

  如

  ```java
  @RequestMapping("/foobar/{value}.do/index.html")
  @ResponseBody
  public String foobar(HttpServletRequest request, @PathVariable("value") String value) {
      return request.getRequestURI() + "\n" + value;
  }
  ```

  用于匹配单个路径段中的至少一个字符并进行解析和类型转换以赋值给变量

- 路径段正则表达式匹配的路径变量 `{variable:[a-z]+}`

  如`/foobar/{value:[0-9]+}.do/index.html`，用于用于匹配单个路径段中符合正则的至少一个字符并进行解析和类型转换以赋值给变量

- 多路径段路径变量 `{*variable}`

  如`/foobar/{*value}`，用于匹配0到多个字符组成的子路径以赋值给变量，对于`/foobar/{*value}`，包括`/foobar`、`/foobar/`、`/foobar//`等在内的畸形路径均可能匹配，且这些匹配结果的路径变量的值应为空字符串、`/`、`//`，但具体的特性受匹配方案、Spring MVC版本和配置等多方面影响

  不可以使用如`/foobar/{*value}/test`等形式

- Properties值注入 `${variable}`

  如`/${foobar.name}.html`，用于使用外部配置文件等配置值来决定匹配的路径

对于可能发生重叠的请求路径匹配模式，Spring MVC会尝试匹配更精确的模式，具体的模式比较规则由匹配方案决定，如`PathPattern`会通过分析模式的复杂度和长度来决定，如果`PathPattern`无法决定将会抛出异常，如对于请求路径`/foobar/foo/bar`和两个匹配模式`/foobar/{foo}/bar`、`/foobar/foo/{bar}`

##### 请求参数/请求头匹配

可以使用`@RequestMapping`的其他属性来匹配或过滤请求，与上述的请求方法和请求路径匹配相同，它们也同样可以同时作用于整个控制器类或单个控制器方法

详细的使用方法可参考源码文档注释或[Spring Docs](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-requestmapping.html)

- `consumes` 匹配或过滤一个或多个`Content-Type`

  如`@PostMapping(path = "/foobar", consumes = {"text/plain", "application/*"})`匹配多个`Content-Type`值或`@PostMapping(path = "/foobar", consumes = "!text/plain")`匹配除`text/plain`以外的所有`Content-Type`值

- `params` 匹配或过滤一个或多个查询参数

  如`params = {"name", "!value", "code=1", "msg!=error"}`匹配包含参数`name`，不包含参数`value`，参数`code`值为`1`且参数`msg`值不为`error`的请求

- `headers` 匹配或过滤一个或多个请求头

  如`headers = {"Token", "!Authorization", "Content-Type=text/*", "referer!=https://www.example.com/"}`

##### 动态路由注册

可在配置`@Configuration`中通过`setHandlerMapping`方法以代码的形式动态地注册控制器方法



#### 请求与响应

##### 概述

在控制器方法中常常需要处理请求的方式、路径、参数、标头和请求体等数据，并返回一定的数据

虽然路由可以匹配请求方式、路径等，但大多数动态的数据需要通过某种方式声明控制器方法参数来主动获取

响应可通过设置某些类型的控制器方法参数的值，或返回某些类型的值来直接或间接的产生

##### 请求数据基础

通过简单地或带注释地声明控制器方法的参数来获取请求参数和请求体数据

请求参数常指URL中的查询参数和表单（`application/x-www-form-urlencoded`和`multipart/form-data`）数据

请求中的对应数据在转换成方法参数时遵循一定的规则和配置，简单来说就是有一批转换器类用于执行字符串与特定类型（原始类型、包装类型、集合、POJO类型等）之间的数据类型转换，详情见下文数据转换

由于原始类型不能为`null`，在请求中无对应数据时更容易触发更多类型转换时的异常，通常不建议使用原始类型方法参数或包含原始类型的类型的方法参数来接收请求数据

POJO类在Spring中的要求并不严格，它可以包含一些其他的方法等内容，且POJO类可以嵌套，但是其主要的属性必须包含Getter和Setter方法，这里所谓主要的属性指那些用于接受输入或提供输出数据的属性，即参与了序列化和反序列化的属性。一般的，推荐使用`lombok`简化POJO声明

###### 简单方法参数

直接声明方法参数用于获取请求参数（查询参数和表单数据）

如

```java
@RequestMapping("/foobar")
@ResponseBody
public String foobar(Integer id, String name) {
    return id + ":" + name;
}
```

使用POJO类型参数获取请求参数，如上述代码等效于

```java
@Data
public class User {
    private String name;
    private Integer id;
}
```

```java
@RequestMapping("/foobar")
@ResponseBody
public String foobar(User user) {
    return user.getId() + ":" + user.getName();
}
```

可使用嵌套的POJO类型参数，如

```java
@Data
public class Account {
    private User user;
    private String uuid;
}
```

```java
@RequestMapping("/bar")
@ResponseBody
public String bar(Account account) {
    return Objects.toString(account);
}
```

使用`.`符号索引被嵌套的POJO类型的属性，如对上述代码的查询参数如下

```
?user.id=1&user.name=李华&uuid=114514
```

###### 请求参数获取

使用`@RequestParam`显式声明此方法参数获取请求参数（查询参数和表单数据）

如

```java
@RequestMapping("/bar")
@ResponseBody
public String bar(@RequestParam("id") Integer id,
                   @RequestParam(value = "username", required = false) String name) {
    return id + ":" + name;
}
```

`@RequestParam`的常用属性

- `value` / `name` 请求参数键名，默认为方法参数名
- `required` 是否必须（非`null`值），默认为`true`，但即使为`true`也不可避免空字符串、空数组等异常数据
- `defaultValue` 请求参数默认值，默认为`null`

通常不应该对POJO类型的方法参数声明`@RequestParam`，这是因为这会使得一个单独的请求参数的值转换到POJO对象，但这种转换通常因没有合适的转换器或规则、配置而失败

可以使用不声明参数键名的`@RequestParam`标注`Map<String, String>`或`MultiValueMap<String, String> params`以用于获取全部的请求参数（查询参数和表单数据）

如

```java
@RequestMapping("/foo")
@ResponseBody
public String foo(@RequestParam Map<String, String> params) {
    return "";
}
```

```java
@RequestMapping("/foo")
@ResponseBody
public String foo(@RequestParam MultiValueMap<String, String> params) {
    return "";
}
```

其中`MultiValueMap<String, String>`本质上是`Map<String, List<String>>`，但Spring MVC默认无法处理`Map<String, List<String>>`、`Map<String, String[]`等类型

`Map`和`MultiValueMap`在接收全部参数时值的类型在绝大多数情况下应为`String`

###### 请求头获取

使用`@RequestHeader`声明方法参数使其获取请求头，与`@RequestParam`使用方法类似，包含`value`/`name`、`required`、`defaultValue`属性，可用于标注`Map<String, String>`或`MultiValueMap<String, String> params`以获取全部请求头

如

```java
@RequestMapping("/foo")
@ResponseBody
public String foo(
    @RequestHeader(value = "Cookie", required = false) String cookie, 
    @RequestHeader(value = "User-Agent", required = false) String userAgent) {
    return "";
}
```

```java
@RequestMapping("/foo")
@ResponseBody
public String foo(@RequestHeader MultiValueMap<String, String> headers) {
    return "";
}
```

###### 请求体获取

使用注解`@RequestBody`声明方法参数获取请求体数据（除表单数据外）

如获取`application/json`类型的请求体数据并转换为嵌套的POJO对象

```java
@Data
public class User {
    private Long id;
    private String name;
    private Permission[] permissions;
}
```

```java
@RequestMapping("/bar")
@ResponseBody
public String bar(@RequestBody User user) {
    return Objects.toString(user);
}
```

##### 数据转换

==TODO 不完善的章节==

常见特殊数据转换规则

1. 请求参数中重复的键，如`?name=李华&name=川建国`转换为字符串形式时通常以`,`符号为分隔，如：`"李华,川建国"`
2. 请求参数中以`,`符号分割的值可以转换为数组或列表，如`?name=李华,川建国`

##### 方法参数及其注解

==TODO 不完善的章节==

###### 表

这里列出支持的方法参数及其注解声明，大多用于各种情况下的请求或与该请求有关的数据的获取，也可能用于响应相关

| 方法参数及注解                                               | 描述                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| `WebRequest`, `NativeWebRequest`                             | Generic access to request parameters and request and session attributes, without direct use of the Servlet API. |
| `jakarta.servlet.ServletRequest`, `jakarta.servlet.ServletResponse` | Choose any specific request or response type — for example, `ServletRequest`, `HttpServletRequest`, or Spring’s `MultipartRequest`, `MultipartHttpServletRequest`. |
| `jakarta.servlet.http.HttpSession`                           | Enforces the presence of a session. As a consequence, such an argument is never `null`. Note that session access is not thread-safe. Consider setting the `RequestMappingHandlerAdapter` instance’s `synchronizeOnSession` flag to `true` if multiple requests are allowed to concurrently access a session. |
| `jakarta.servlet.http.PushBuilder`                           | Servlet 4.0 push builder API for programmatic HTTP/2 resource pushes. Note that, per the Servlet specification, the injected `PushBuilder` instance can be null if the client does not support that HTTP/2 feature. |
| `java.security.Principal`                                    | Currently authenticated user — possibly a specific `Principal` implementation class if known.Note that this argument is not resolved eagerly, if it is annotated in order to allow a custom resolver to resolve it before falling back on default resolution via `HttpServletRequest#getUserPrincipal`. For example, the Spring Security `Authentication` implements `Principal` and would be injected as such via `HttpServletRequest#getUserPrincipal`, unless it is also annotated with `@AuthenticationPrincipal` in which case it is resolved by a custom Spring Security resolver through `Authentication#getPrincipal`. |
| `HttpMethod`                                                 | 请求的HTTP方法                                               |
| `java.util.Locale`                                           | The current request locale, determined by the most specific `LocaleResolver` available (in effect, the configured `LocaleResolver` or `LocaleContextResolver`). |
| `java.util.TimeZone` + `java.time.ZoneId`                    | The time zone associated with the current request, as determined by a `LocaleContextResolver`. |
| `java.io.InputStream`, `java.io.Reader`                      | For access to the raw request body as exposed by the Servlet API. |
| `java.io.OutputStream`, `java.io.Writer`                     | For access to the raw response body as exposed by the Servlet API. |
| `@PathVariable`                                              | URL路径参数，参见请求路径映射                                |
| `@MatrixVariable`                                            | URI矩阵参数                                                  |
| `@RequestParam`                                              | 请求参数，包括查询参数和表单数据                             |
| `@RequestHeader`                                             | 请求头                                                       |
| `@CookieValue`                                               | 请求头Cookie                                                 |
| `@RequestBody`                                               | 请求体                                                       |
| `HttpEntity<B>`                                              | 请求头和请求体                                               |
| `@RequestPart`                                               | For access to a part in a `multipart/form-data` request, converting the part’s body with an `HttpMessageConverter`. See [Multipart](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-methods/multipart-forms.html). |
| `java.util.Map`, `org.springframework.ui.Model`, `org.springframework.ui.ModelMap` | For access to the model that is used in HTML controllers and exposed to templates as part of view rendering. |
| `RedirectAttributes`                                         | Specify attributes to use in case of a redirect (that is, to be appended to the query string) and flash attributes to be stored temporarily until the request after redirect. See [Redirect Attributes](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-methods/redirecting-passing-data.html) and [Flash Attributes](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-methods/flash-attributes.html). |
| `@ModelAttribute`                                            | For access to an existing attribute in the model (instantiated if not present) with data binding and validation applied. See [`@ModelAttribute`](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-methods/modelattrib-method-args.html) as well as [Model](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-modelattrib-methods.html) and [`DataBinder`](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-initbinder.html).Note that use of `@ModelAttribute` is optional (for example, to set its attributes). See “Any other argument” at the end of this table. |
| `Errors`, `BindingResult`                                    | For access to errors from validation and data binding for a command object (that is, a `@ModelAttribute` argument) or errors from the validation of a `@RequestBody` or `@RequestPart` arguments. You must declare an `Errors`, or `BindingResult` argument immediately after the validated method argument. |
| `SessionStatus` + class-level `@SessionAttributes`           | For marking form processing complete, which triggers cleanup of session attributes declared through a class-level `@SessionAttributes` annotation. See [`@SessionAttributes`](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-methods/sessionattributes.html) for more details. |
| `UriComponentsBuilder`                                       | For preparing a URL relative to the current request’s host, port, scheme, context path, and the literal part of the servlet mapping. See [URI Links](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-uri-building.html). |
| `@SessionAttribute`                                          | For access to any session attribute, in contrast to model attributes stored in the session as a result of a class-level `@SessionAttributes` declaration. See [`@SessionAttribute`](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-methods/sessionattribute.html) for more details. |
| `@RequestAttribute`                                          | For access to request attributes. See [`@RequestAttribute`](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-methods/requestattrib.html) for more details. |
| 其他                                                         | If a method argument is not matched to any of the earlier values in this table and it is a simple type (as determined by [BeanUtils#isSimpleProperty](https://docs.spring.io/spring-framework/docs/6.2.7/javadoc-api/org/springframework/beans/BeanUtils.html#isSimpleProperty-java.lang.Class-)), it is resolved as a `@RequestParam`. Otherwise, it is resolved as a `@ModelAttribute`. |

###### `MultipartFile`

在处理`multipart/form-data`类型的包含文件部分的表单数据时，常使用`@RequestParam`和`MultipartFile`及其集合类型（即使文件部分数据总在请求体中出现）

与`@RequestParam`的使用方法类似，这些集合类型包括`List<MultipartFile>`、`Map<String, MultipartFile>`、`MultiValueMap<String, MultipartFile>`

这是因为`multipart/form-data`表单请求的请求体中包含多个部分，每个部分的标头通常包含`name`字段，其值即表单的键名，对于文件部分，其标头往往包含如`filename`等字段以提供更多信息，而不同部分的标头的`name`字段值可能相同

因此当表单的一项值为多个文件或表单的多个文件项的键名相同时，`MultipartFile`类型的方法参数只会获取第一个文件，而`List<MultipartFile>`和`MultiValueMap<String, MultipartFile>`可以获取全部

示例

```java
@RequestMapping("/foo")
@ResponseBody
public String foo(
        @RequestParam(value = "comment") String comment,
        @RequestParam(value = "file") MultipartFile file,
        @RequestParam(value = "file2") List<MultipartFile> file2
) {
    return "";
}
```

```java
@RequestMapping("/foo")
@ResponseBody
public String foo(@RequestParam MultiValueMap<String, MultipartFile> params) {
    return "";
}
```

尝试使用`String`类型获取`MultipartFile`数据将发生异常，而`MultipartFile`类型接收文本数据时值为`null`，但这是在Spring MVC 6中的测试结果，不同版本可能特性不同

因此使用`Map<String, MultipartFile>`、`MultiValueMap<String, MultipartFile>`和不声明参数名称的`@RequestParam`作为方法参数时只会仅接受全部的文件数据

###### `@DateTimeFormat`

可使用`@DateTimeFormat`注解获取指定格式的时间日期参数，不符合格式的参数默认情况下引发400错误

如

```java
@RequestMapping("/bar")
@ResponseBody
public String bar(@RequestParam @DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss") Date date) {
    return Objects.toString(date);
}
```

##### 方法返回值及其注解

###### 表

这里列出支持的控制器方法返回值类型及注解，通常用于响应生成

其中部分注解除可标注方法外，也可标注整个控制器类，使得该注解对所有控制器方法生效，如`@ResponseBody`（但它通常被`@RestController`替代）

| 方法返回值及注解                                             | 描述                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| `@ResponseBody`                                              | 声明返回值及响应体                                           |
| `HttpEntity<B>`, `ResponseEntity<B>`                         | 前者为响应头和响应体，后者为完整的响应                       |
| `HttpHeaders`                                                | 无响应体的响应                                               |
| `ErrorResponse`                                              | To render an RFC 9457 error response with details in the body, see [Error Responses](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-ann-rest-exceptions.html) |
| `ProblemDetail`                                              | To render an RFC 9457 error response with details in the body, see [Error Responses](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-ann-rest-exceptions.html) |
| `String`                                                     | A view name to be resolved with `ViewResolver` implementations and used together with the implicit model — determined through command objects and `@ModelAttribute` methods. The handler method can also programmatically enrich the model by declaring a `Model` argument (see [Explicit Registrations](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-requestmapping.html#mvc-ann-requestmapping-registration)). |
| `View`                                                       | A `View` instance to use for rendering together with the implicit model — determined through command objects and `@ModelAttribute` methods. The handler method can also programmatically enrich the model by declaring a `Model` argument (see [Explicit Registrations](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-requestmapping.html#mvc-ann-requestmapping-registration)). |
| `java.util.Map`, `org.springframework.ui.Model`              | Attributes to be added to the implicit model, with the view name implicitly determined through a `RequestToViewNameTranslator`. |
| `@ModelAttribute`                                            | An attribute to be added to the model, with the view name implicitly determined through a `RequestToViewNameTranslator`.Note that `@ModelAttribute` is optional. See "Any other return value" at the end of this table. |
| `ModelAndView` object                                        | The view and model attributes to use and, optionally, a response status. |
| `FragmentsRendering`, `Collection<ModelAndView>`             | For rendering one or more fragments each with its own view and model. See [HTML Fragments](https://docs.spring.io/spring-framework/reference/web/webmvc-view/mvc-fragments.html) for more details. |
| `void`                                                       | A method with a `void` return type (or `null` return value) is considered to have fully handled the response if it also has a `ServletResponse`, an `OutputStream` argument, or an `@ResponseStatus` annotation. The same is also true if the controller has made a positive `ETag` or `lastModified` timestamp check (see [Controllers](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-caching.html#mvc-caching-etag-lastmodified) for details).If none of the above is true, a `void` return type can also indicate “no response body” for REST controllers or a default view name selection for HTML controllers. |
| `DeferredResult<V>`                                          | Produce any of the preceding return values asynchronously from any thread — for example, as a result of some event or callback. See [Asynchronous Requests](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-ann-async.html) and [`DeferredResult`](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-ann-async.html#mvc-ann-async-deferredresult). |
| `Callable<V>`                                                | Produce any of the above return values asynchronously in a Spring MVC-managed thread. See [Asynchronous Requests](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-ann-async.html) and [`Callable`](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-ann-async.html#mvc-ann-async-callable). |
| `ListenableFuture<V>`, `java.util.concurrent.CompletionStage<V>`, `java.util.concurrent.CompletableFuture<V>` | Alternative to `DeferredResult`, as a convenience (for example, when an underlying service returns one of those). |
| `ResponseBodyEmitter`, `SseEmitter`                          | Emit a stream of objects asynchronously to be written to the response with `HttpMessageConverter` implementations. Also supported as the body of a `ResponseEntity`. See [Asynchronous Requests](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-ann-async.html) and [HTTP Streaming](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-ann-async.html#mvc-ann-async-http-streaming). |
| `StreamingResponseBody`                                      | Write to the response `OutputStream` asynchronously. Also supported as the body of a `ResponseEntity`. See [Asynchronous Requests](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-ann-async.html) and [HTTP Streaming](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-ann-async.html#mvc-ann-async-http-streaming). |
| Reactor and other reactive types registered via `ReactiveAdapterRegistry` | A single value type, for example, `Mono`, is comparable to returning `DeferredResult`. A multi-value type, for example, `Flux`, may be treated as a stream depending on the requested media type, for example, "text/event-stream", "application/json+stream", or otherwise is collected to a List and rendered as a single value. See [Asynchronous Requests](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-ann-async.html) and [Reactive Types](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-ann-async.html#mvc-ann-async-reactive-types). |
| 其他                                                         | If a return value remains unresolved in any other way, it is treated as a model attribute, unless it is a simple type as determined by [BeanUtils#isSimpleProperty](https://docs.spring.io/spring-framework/docs/6.2.7/javadoc-api/org/springframework/beans/BeanUtils.html#isSimpleProperty-java.lang.Class-), in which case it remains unresolved. |



#### 模型与视图

模型和视图技术常用于

==TODO 不完善的章节==





### Spring Boot 入门

<u>知其然</u>



#### 概述

##### 介绍

Spring Boot是Spring提供的用于快速构建可独立运行的Spring应用程序的开发框架，具有入门简单、开箱即用、约定大于配置等特点

##### Hello World

构建Spring Boot Hello World应用，参考[Spring Quick Start](https://spring.io/quickstart)或更详细的[Spring Boot First Application](https://docs.spring.io/spring-boot/tutorial/first-application/index.html)

##### 指南

可以在Spring官网中检索入门[指南](https://spring.io/guides)，这些指南面向新手，以各种特定的企业应用需求进行分类以用于检索

##### 支持

Spring Boot 3.4.5需要：

- Java 17+
- Spring Framework 6.2.6+
- Maven 3.6.3+ 或 Gradle 7.6.4+ / 8.4+

并提供嵌入式Servlet（5.0+ / 6.0）容器支持：

- Tomcat 10.1
- Jetty 12.0
- Undertow 2.3

而对于Spring Boot 2，通常仅需Java 8





### Spring Boot 进阶

<u>知其所以然</u>



#### 独立应用程序原理

##### 概述

Spring Boot使用了多种技术，使得开发者可以将应用程序打包成单个`jar`文件，通过简单的命令`java -jar example.jar`启动，或者将应用程序打包成单个`war`文件，以支持简单命令启动`java -jar example.war`或部署到Tomcat服务器（仅需较少的配置而不是大量的变更）

对于开发者而言，使用上述技术仅需在`pom.xml`配置构建插件，如

```xml
<build>
	<plugins>
		<plugin>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-maven-plugin</artifactId>
		</plugin>
	</plugins>
</build>
```

##### 文件结构

单个可执行`jar`文件的打包通常需要处理同包同名`class`文件冲突。传统上常见的技术为`Shaded jar`（又称`Fat jar`或`Uber jar`），它将所有需要的类文件打包到同一`jar`文件中，并通过重命名包路径来解决冲突。

而Spring Boot通过嵌套的`jar`文件来解决这一问题，它的文件结构通常如下：

对于`jar`文件

```markdown
example.jar
 |
 +-META-INF    # JAR规范中的元数据文件夹
 |  +-MANIFEST.MF    # JAR规范中的清单文件
 +-org    # 依照JAR规范存储的class文件
 |  +-springframework
 |     +-boot
 |        +-loader
 |           +-<spring boot loader classes> # Spring Boot启动类
 +-BOOT-INF    # 非JAR规范存储的文件，需由Spring Boot类加载器
    +-classes
    |  +-mycompany
    |     +-project
    |        +-YourClasses.class    # 包含main方法的类
    +-lib
       +-dependency1.jar    # 外部依赖项，需由Spring Boot类加载器加载
       +-dependency2.jar
```

对于`war`文件

```markdown
example.war
 |
 +-META-INF
 |  +-MANIFEST.MF
 +-org
 |  +-springframework
 |     +-boot
 |        +-loader
 |           +-<spring boot loader classes>
 +-WEB-INF    # Servlet规范中的文件夹
    +-classes
    |  +-com
    |     +-mycompany
    |        +-project
    |           +-YourClasses.class    # 包含main方法的类
    +-lib    # 存储无论何时都需要的外部依赖项
    |  +-dependency1.jar
    |  +-dependency2.jar
    +-lib-provided    # 存储在独立运行时需要，部署到Web容器时不需要的外部依赖项
       +-servlet-api.jar
       +-dependency3.jar
```

##### 启动与类加载

JAR规范并未定义嵌套jar文件的启动和加载，且Spring Boot的特有`jar`文件结构使得其中的大部分类文件需显式地通过类加载器加载，因此Spring Boot有自己的启动和加载机制。

Spring Boot定义了`org.springframework.boot.loader.launch.Launcher`及其三个子类`JarLauncher`、`WarLauncher`和`PropertiesLauncher`来作为实际的启动类，并加载所需要的类加载器和类

由Spring Boot构建插件自动生成的`MANIFEST.MF`清单文件常包含内容如下：

对于`jar`文件

```markdown
Main-Class: org.springframework.boot.loader.launch.JarLauncher
Start-Class: com.mycompany.project.MyApplication
```

对于`war`文件

```
Main-Class: org.springframework.boot.loader.launch.WarLauncher
Start-Class: com.mycompany.project.MyApplication
```

其中`Main-Class`是JAR规范规定的入口类声明，而`Start-Class`是Spring Boot自定义的字段

`NestedJarFile`类是Spring Boot支持从嵌套的`jar`文件中加载类的核心支持，它继承至`java.util.jar.JarFile`，它无需解压外层`jar`文件到磁盘或将整个`jar`文件读入内存处理，即可读取内层`jar`中的类文件，这是因为它依靠类文件在`jar`压缩文件中的偏移量来寻找需要的类文件

```markdown
myapp.jar
+-------------------+-------------------------+
| /BOOT-INF/classes | /BOOT-INF/lib/mylib.jar |
|+-----------------+||+-----------+----------+|
||     A.class      |||  B.class  |  C.class ||
|+-----------------+||+-----------+----------+|
+-------------------+-------------------------+
 ^                    ^           ^
 0063                 3452        3980
```

如上所示，类A、B、C的类文件的查找都依靠统一的偏移量

##### 流程

综上所述，整个流程可以概述如下：

1. Spring Boot Maven或Gradle打包插件将应用打包成具有特定格式的`jar`或`war`文件
2. 使用`java -jar`命令启动后，JVM运行在`MANIFEST.MF`中声明的Spring Boot启动类
3. 启动类加载Spring Boot类加载器和`NestedJarFile`，从对应的文件夹和嵌套的`jar`文件中加载开发者编写编译的类文件和外部依赖项的类文件
4. 开发者编写的主类被通过反射地方式执行，Spring Boot应用正式启动

##### 局限性

上述技术虽然提供了诸多便利，但仍存在一定的局限性：

- 被嵌套的内层`jar`文件不能被再次压缩，而是“仅存储”（外层`jar`文件中的其他内容和内层`jar`文件中的内容可以被正常压缩），否则`NestedJarFile`将无法定位内层`jar`文件中的类文件
- 被嵌套的内层`jar`文件中的类不能正常使用`ClassLoader.getSystemClassLoader()`，它应该使用`Thread.getContextClassLoader()`，因此依赖系统类加载器的功能将无法使用，如 `java.util.Logging`







## 数据库





### MyBatis 入门



#### 概述

MyBatis是一款持久层框架，用于简化JDBC的开发，其通过XML或注解来配置数据库中记录到POJO对象的映射

MyBatis的前身是Apache开源项目iBatis，现在托管于Github



#### Hello World

准备Spring Boot开发环境及项目、MySQL数据库及表和记录

引入MyBatis依赖和MySQL驱动依赖，如

```xml
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>3.0.4</version>
</dependency>

<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter-test</artifactId>
    <version>3.0.4</version>
    <scope>test</scope>
</dependency>


<dependency>
    <groupId>com.mysql</groupId>
    <artifactId>mysql-connector-j</artifactId>
    <scope>runtime</scope>
</dependency>
```

在`application.properties`中配置数据源，如

```properties
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/world
spring.datasource.username=root
spring.datasource.password=password
```

按照表结构声明对应的POJO类，如

```java
package com.crim.web.lab.springmvclab.web.entity;

import lombok.Data;


/**
 * CREATE TABLE `city` (
 * `ID` int NOT NULL AUTO_INCREMENT,
 * `Name` char(35) NOT NULL DEFAULT '',
 * `CountryCode` char(3) NOT NULL DEFAULT '',
 * `District` char(20) NOT NULL DEFAULT '',
 * `Population` int NOT NULL DEFAULT '0',
 * PRIMARY KEY (`ID`),
 * KEY `CountryCode` (`CountryCode`),
 * CONSTRAINT `city_ibfk_1` FOREIGN KEY (`CountryCode`) REFERENCES `country` (`Code`)
 * ) ENGINE=InnoDB AUTO_INCREMENT=4080 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci
 */
@Data
public class City {
    
    private Integer id;
    
    private String name;
    
    private String countryCode;
    
    private String district;
    
    private Integer population;
    
}
```

声明Mapper接口，如

```java
package com.crim.web.lab.springmvclab.web.mapper;

import com.crim.web.lab.springmvclab.web.entity.City;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Select;

import java.util.List;


@Mapper
public interface CityMapper {
    
    @Select("select * from city")
    List<City> getAllCities();
    
}
```

进行测试，如

```java
@Autowired
CityMapper cityMapper;

@Test
void test() {
    System.out.println(cityMapper.getAllCities());
}
```



#### 配置

##### 日志

在`application.properties`配置MyBatis日志输出以在日志中查看SQL的执行和结果，如

```properties
# 使com包下的日志输出级别为debug
logging.level.com=debug
# 使用Spring Boot的SLF4J作为日志输出
mybatis.configuration.log-impl=org.apache.ibatis.logging.slf4j.Slf4jImpl
```

通常info级别不会输出SQL语句，debug级别将输出SQL语句及查询结果，trace级别将输出查询结果集

##### IDEA

通过配置IDEA使其更适合基于MyBatis开发

- 配置SQL方言

  用于SQL语法自动检测和语法提示

- 配置数据源

  用于SQL中的表、字段等的自动检测和语法提示

- 安装MyBatisX插件

  用于MyBatis自动检测、语法提示和快捷跳转

##### 连接池

通过配置连接池可以使持久层进行性能优化或支持更多的功能

- 无配置

  Spring Boot 默认使用Hikari连接池

- 配置Druid连接池

  Druid内容参见Druid章节或[官方文档](https://github.com/alibaba/druid)

  引入boot starter依赖，对于Spring Boot 2 如下

  ```xml
  <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid-spring-boot-starter</artifactId>
      <version>1.2.24</version>
  </dependency>
  ```

  对于Spring Boot 3 如下

  ```xml
  <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid-spring-boot-3-starter</artifactId>
      <version>1.2.24</version>
  </dependency>
  ```

  在`application.properties`中配置数据源

  ```properties
  spring.datasource.druid.url= 
  # 或spring.datasource.url= 
  
  spring.datasource.druid.username= 
  # 或spring.datasource.username=
  
  spring.datasource.druid.password= 
  # 或spring.datasource.password=
  
  spring.datasource.druid.driver-class-name= 
  #或 spring.datasource.driver-class-name=
  ```



#### 基于注解

##### 概述

MyBatis支持通过在Mapper方法上声明注解的方式实现方法与SQL语句之间的映射

示例

```java
@Select("select name from city where id = #{id}")
String getCityNameById(Integer id);
```

##### 参数映射

可以使用`#{param}`或`${param}`的形式将需要注入的变量按名称绑定到参数中

其中前者使用预编译的占位符，而后者为直接的字符串拼接

被绑定的参数可以是直接声明的方法参数、方法参数POJO类型中声明的属性或`Map`中存储的键值对

示例

```java
@Select("select CountryCode from city where ID=#{id} and Name=#{name}")
String getCountryCodeByIdAndName(Integer id, String name);
```

```java
@Select("select CountryCode from city where ID=#{id} and Name=#{name}")
String getCountryCodeByIdAndName2(City city);
```

```java
@Select("select CountryCode from city where ID=#{id} and Name=#{name}")
String getCountryCodeByIdAndName3(Map<String, Object> params);
```

`${param}`用于无法使用预编译的情况，如`like`字符串模糊匹配、表名、字段名等

示例

```java
// 由于存在SQLi，常用concat('%',#{string},'%')的形式替代
@Select("select CountryCode from city where Name like '%${string}%'")
```

##### 结果映射

可以通过声明方法返回值类型的方式将查询结果集映射到单个值、POJO对象、键值对、POJO的列表、键值对的列表等

示例

```java
@Select("select name from city where id = #{id}")
String getCityNameById(Integer id);
```

```java
@Select("select name, countrycode from city where id = #{id}")
City getCityById(Integer id);
```

```java
@Select("select name, countrycode from city where id = #{id}")
Map<String, Object> getCityById2(Integer id);
```

```java
@Select("select * from city")
List<City> getAllCities();
```

```
@Select("select * from city")
List<Map<String, Object>> getAllCities2();
```

INSERT、DELETE和UPDATE语句的结果也可以映射至整型或长整型、布尔型（及其包装类型）返回值

示例

```java
@Update("update user set phone_number=#{phoneNumber} where id=#{id}")
int updatePhoneNumber(User user);
```

```java
@Update("update user set phone_number=#{phoneNumber} where name=#{name}")
boolean updatePhoneNumber(String name, String phoneNumber);
```

INSERT语句如果使用了数据库自动生成主键，可以通过声明`@Options`注解的`useGeneratedKeys`为`true`以及`keyProperty`为POJO类型属性名的方式进行主键返回，如果有多个ID，则使用`,`符号分隔，当表的主键不是第一个字段或不止一个字段时，应声明`keycolumn`属性

示例

```java
@Options(useGeneratedKeys = true, keyProperty = "id")
@Insert("insert into user(name,phone_number) values(#{name},#{phoneNumber})")
int insertUser2(User user);
```

##### 列名映射

在SELECT语句中，如果需要将结果集映射到POJO对象时，可能会出现表列名与POJO属性名不符的情况

如

```java
@Data
public class User {
    private Integer id;
    private String name;
    private String phoneNumber;
}
```

```mysql
CREATE TABLE `user` (
    `id` int NOT NULL AUTO_INCREMENT,
    `name` char(16) NOT NULL,
    `phone_number` char(16) DEFAULT NULL,
    PRIMARY KEY (`id`)
)
```

在进行查询时，未被匹配的列数据将被忽略，使得`User.phoneNumber`字段值总为`null`

###### 别名

通过SQL语句的别名功能进行手动映射

示例

```java
@Select("select id, name, phone_number as phoneNumber from user")
List<User> getAllUsers();
```

###### `@Results`

通过`@Results`注解进行手动映射

示例

```java
@Results({
    @Result(property = "phoneNumber", column = "phone_number")
})
@Select("select id, name, phone_number from user")
List<User> getAllUsers();
```

###### 下划线-驼峰命名自动转换

通过在`application.properties`中配置开启MyBatis下划线命名与驼峰命名自动转换功能

```properties
mybatis.configuration.map-underscore-to-camel-case=true
```



#### 基于XML映射

##### 概述

MyBatis支持在XML文件中声明Mapper类及其方法与SQL语句之间的映射，因此这些XML文件也被成为“XML映射文件”

在默认配置下，XML映射文件的声明应至少遵循如下规范，否则映射可能失败

- XML文件应与所对应的Mapper源码Java文件同包同名

  对于Maven项目，`java`文件夹中的Mapper源码Java文件所在的包应与`resources`文件夹中对应XML映射文件所在的包相同

- XML映射文件包含应包含的头为

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE mapper
          PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
          "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
  ```

- XML映射文件的`mapper`根元素应包含`namespace`属性，其值应为所对应的Mapper类的全限定名

- 每个`select`、`update`、`insert`、`delete`标签的`id`属性的值应为所对应的Mapper类方法名，`resultType`应为对应Mapper类方法返回值中一条记录的类型，当返回值为集合时，`resultType`应当是集合的类型而不是集合，具体的声明方式参见下文

- Mapper中的每一个方法都应该存在映射（即使不在XML映射文件中），但XML映射文件中可以存在冗余的声明

示例：

`src/main/java/com/example/mapper/UserMapper.java`

```java
package com.example.mapper;

import com.example.entity.User;
import org.apache.ibatis.annotations.Mapper;

import java.util.List;


@Mapper
public interface UserMapper {
    
    List<User> getUsers();
    
}

```

`src/main/resources/com/example/mapper/UserMapper.xml`

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.example.mapper.UserMapper">

    <select id="getUsers" resultType="com.example.entity.User">
        select * from user
    </select>

</mapper>
```

##### 元素及其属性一览

在`mapper`中可声明的子元素如下：

- `cache` – 该命名空间的缓存配置
- `cache-ref` – 引用其它命名空间的缓存配置
- `resultMap` – 描述如何从数据库结果集中加载对象
- ~~`parameterMap` – 老式风格的参数映射。此元素已被废弃，并可能在将来被移除！请使用行内参数映射~~
- `sql` – 可被其它语句引用的可重用语句块
- `insert` – 映射插入语句
- `update` – 映射更新语句
- `delete` – 映射删除语句
- `select` – 映射查询语句

`select`元素中可声明的属性如下：

- `id`
- `parameterType`
- ~~`parameterMap`~~
- `resultType`
- `resultMap`
- `flushCache`
- `useCache`
- `timeout`
- `fetchSize`
- `statementType`
- `resultSetType`
- `databaseId`
- `resultOrdered`
- `resultSets`
- `affectData`

`insert`和`update`元素中可声明的属性如下：

- `id`
- `parameterType`
- ~~`parameterMap`~~
- `flushCache`
- `timeout`
- `statementType`
- `useGeneratedKeys`
- `keyProperty`
- `keyColumn`
- `databaseId`

`delete`元素中可声明的属性如下：

- `id`
- `parameterType`
- ~~`parameterMap`~~
- `flushCache`
- `timeout`
- `statementType`
- `databaseId`

##### 参数映射

可以使用`#{param}`或`${param}`的形式将需要注入的变量按名称绑定到参数中

其中前者使用预编译的占位符，而后者为直接的字符串拼接

被绑定的参数可以是直接声明的方法参数、方法参数POJO类型中声明的属性或`Map`中存储的键值对

可以使用`parameterType`属性显式声明参数类型，无特殊情况时也可忽略该属性

示例

```java
int selectCountByName(String name);
    
int selectCountByNameAndPhoneNumber(User user);
    
int selectCountByNameOrPhoneNumber(Map<String, Object> params);
```

```xml
<select id="selectCountByName" parameterType="string" resultType="int">
    select count(*) from user where name=#{name}
</select>

<select id="selectCountByNameAndPhoneNumber" parameterType="com.crim.web.lab.springmvclab.web.entity.User" resultType="Integer">
    select count(*) from user where name=#{name} and phone_number=#{phoneNumber}
</select>

<select id="selectCountByNameOrPhoneNumber" parameterType="map" resultType="Integer">
    select count(*) from user where name=#{name} or phone_number=#{phoneNumber}
</select>
```

或

```xml
<select id="selectCountByName" resultType="int">
    select count(*) from user where name=#{name}
</select>

<select id="selectCountByNameAndPhoneNumber" resultType="Integer">
    select count(*) from user where name=#{name} and phone_number=#{phoneNumber}
</select>

<select id="selectCountByNameOrPhoneNumber" resultType="Integer">
    select count(*) from user where name=#{name} or phone_number=#{phoneNumber}
</select>
```

##### 结果映射

可在`select`元素声明`resultType`将结果映射到指定类型

结果可以映射到单个值、POJO对象、键值对、POJO的列表、键值对的列表等

但当结果集是多个记录时，`resultType`仅需声明单个记录的类型，且无需泛型声明

示例

```java
String getUserNameById(Integer id);
    
User getUserById(Integer id);
    
List<User> getUsers();
    
Map<String, Object> getUserMapById(Integer id);
    
List<Map<String, Object>> getUsersMap();
```

```xml
<select id="getUserNameById" resultType="String">
	select name from user where id=#{id}
</select>

<select id="getUserById" resultType="com.crim.web.lab.springmvclab.web.entity.User">
	select * from user where id=#{id}
</select>

<select id="getUsers" resultType="com.crim.web.lab.springmvclab.web.entity.User">
	select * from user
</select>

<select id="getUserMapById" resultType="Map">
	select * from user where id=#{id}
</select>

<select id="getUsersMap" resultType="Map">
	select * from user
</select>
```

无特殊情况时`resultType`可省略，MyBatis会自动从Mapper方法中获取返回值类型

示例

```xml
<select id="getUserNameById">
	select name from user where id=#{id}
</select>

<select id="getUserById">
	select * from user where id=#{id}
</select>

<select id="getUsers">
	select * from user
</select>

<select id="getUserMapById">
	select * from user where id=#{id}
</select>

<select id="getUsersMap">
	select * from user
</select>
```

元素`update`、`delete`、`insert`不能声明`resultType`属性，但在Mapper方法中可以声明返回值为整型或长整型、布尔型（及其包装类型）

示例

```java
Long updatePhoneNumber(Integer id, String phoneNumber);
boolean updatePhoneNumber2(Integer id, String phoneNumber);
```

```xml
<update id="updatePhoneNumber">
    update user set phone_number=#{phoneNumber} where id=#{id}
</update>

<update id="updatePhoneNumber2">
    update user set phone_number=#{phoneNumber} where id=#{id}
</update>
```

INSERT语句如果使用了数据库自动生成主键，可以通过声明`insert`元素的`useGeneratedKeys`属性值为`true`以及`keyProperty`为POJO类型属性名的方式进行主键返回，如果有多个ID，则使用`,`符号分隔，当表的主键不是第一个字段或不止一个字段时，应声明`keycolumn`属性

示例

```java
int insertUser(User user);
```

```xml
<insert id="insertUser" useGeneratedKeys="true" keyProperty="id" keyColumn="id">
    insert into user(name,phone_number) values(#{name},#{phoneNumber})
</insert>
```





### MyBatis 进阶



#### 高级结果映射

##### 概述

对于较为复杂的映射，如有复杂映射关系的多个表的连接查询，通过Java和SQL代码将查询结果映射到特定结构的Map或POJO是较为繁琐和困难的，但是，MyBatis提供了诸多特性，用于大幅地简化复杂查询下的结果映射

在XML映射文件中，可通过在`mapper`中声明子元素`resultMap`，并在`select`元素中通过属性`resultMap`进行引用，以实现较为复杂的结果映射

`select`元素中`resultType`和`resultMap`属性只能声明一个

以下示例展示了使用`resultMap`将结果映射到自定义键名的Map、以及解决列名与POJO属性名不匹配的问题：

```java
Map<String, Object> getUserMapById(Integer id);
    
User getUserById(Integer id);
```

```xml
<resultMap id="userResultMap" type="map">
    <id property="id" column="id"/>
    <result property="name" column="name"/>
    <result property="phone" column="phone_number"/>
</resultMap>

<resultMap id="userResult" type="com.crim.web.lab.springmvclab.web.entity.User">
    <id property="id" column="id"/>
    <result property="name" column="name"/>
    <result property="phoneNumber" column="phone_number"/>
</resultMap>

<select id="getUserMapById" resultMap="userResultMap">
    select * from user where id=#{id}
</select>

<select id="getUserById" resultMap="userResult">
    select * from user where id= #{id}
</select>
```

##### 元素及属性一览

==TODO==

`resultMap`

属性

- `id`
- `type`
- `autoMapping`

##### 直接映射

在`resultMap`元素中声明`id`或`result`子元素，将指定的查询结果集的列映射到POJO（或嵌套的POJO）对象属性或Map（或嵌套的Map）键

可以将同一个结果集列多次映射到不同的POJO属性或Map键

`id`和`result`的属性共用，它们的区别在于`id`用于在缓存或关联映射中作为对象的标识符以优化性能

可声明的属性如下：

- `property` 被映射的POJO属性名或Map键名，对于嵌套的属性或键，使用`.`符号索引
- `column` 查询结果集的列名
- `javaType` Java类全限定名或类型别名，用于映射后的值类型，如果`resultMap`的`type`为键值对，通常需要声明该属性以显式确定值类型
- `jdbcType`
- `typeHandler`

以下示例分别展示了将查询结果映射到嵌套的POJO对象和自定义值类型的嵌套的`HashMap`中，且同一个列均被重复映射了两次：

```xml
<mapper namespace="com.example.ExampleMapper">

    <resultMap id="cityMap1" type="java.util.HashMap">
        <id property="id" column="ID" javaType="Integer"/>
        <result property="name" column="Name" javaType="String"/>
        <result property="country.code" column="CountryCode" javaType="String"/>
        <result property="countryCode" column="CountryCode" javaType="String"/>
        <result property="district" column="District" javaType="String"/>
        <result property="population" column="Population" javaType="Long"/>
    </resultMap>

    <resultMap id="city1" type="com.example.entity.City">
        <id property="id" column="ID"/>
        <result property="name" column="Name"/>
        <result property="country.code" column="CountryCode"/>
        <result property="countryCode" column="CountryCode"/>
        <result property="district" column="District"/>
        <result property="population" column="Population"/>
    </resultMap>

    <select id="getCityById" resultMap="city1">
        select * from city where id = #{id}
    </select>

    <select id="getCityMapById" resultMap="cityMap1">
        select * from city where id = #{id}
    </select>

</mapper>
```

```java
@Data
public class City {
    /// 表字段
    // Key
    private Integer id;
    private String name;
    private String countryCode;    
    private String district;
    private Integer population;
    /// 关联字段
    private Country country;
}
```

```java
@Data
public class Country {
    /// 表字段
    // Key
    private String code;
    private String name;
    private String continent;
    private String region;
    private Double surfaceArea;
    private Integer indepYear;
    private Integer population;
    private Double lifeExpectancy;
    private Double gnp;
    private Double gnpOld;
    private String localName;
    private String governmentForm;
    private String headOfState;
    private Integer capital;
    private String code2;
    /// 关联字段
    private City capitalCity;
    private City[] cities;
    private CountryLanguage[] languages;
    private CountryLanguage[] officialLanguages;
    private CountryLanguage[] unofficialLanguages;
}
```

```java
@Data
public class CountryLanguage {
    /// 表字段
    // Key 1
    private String countryCode;
    // Key 2
    private String language;
    private String isOfficial;    
    private Double percentage;
    /// 关联字段
    // CountryCode关联国家
    private Country country;
    // 所有使用了该语言的国家
    private Country[] countries;
}
```

```java
@Mapper
public interface ExampleMapper {
    City getCityById(Integer id);
    Map<String, Object> getCityMapById(Integer id);
}
```

##### 关联映射

如果需要将连接查询（或可视为连接查询的查询功能）结果集映射到嵌套的POJO或Map中，在代码实现上较为繁琐，不过MyBatis提供了关联映射用于解决这一问题

MyBatis的关联映射通过`resultMap`的子元素`association`和`collection`实现

其中`association`用于解决一对一映射问题，典型情景是将连接查询的结果集映射到一个或多个包含POJO类型属性的嵌套POJO对象中，如上文的`City`类和`City.country`属性

而`collection`用于解决一对多问题，典型情景是连接查询的结果集映射到包含POJO的集合属性的一个或多个POJO对象中，如上文的`CountryLanguage`类和`CountryLanguage.countries`属性

`association`有三种工作模式，基于嵌套查询

无论哪种工作模式`association`中可声明的公共属性有：

==TODO==

##### 基于嵌套查询的关联映射

基于嵌套查询的关联映射的核心思想是通过声明`select`语句之间的关系，使得不同`select`语句分别执行，它们查询结果集按照一对一的关系进行关联映射

基于嵌套查询的`association` 额外声明的属性：

- `column` 必须，作为参数传递给目标`select`的结果集列名，如果目标`select`需要多个参数，应使用如`{param1=column1,param2=column2}`的格式
- `select` 必须，目标`select`元素的`id`
- `fetchType` 可选，值为`lazy`懒加载或`eager`急加载，详情参见工作原理及优化

基于嵌套查询的关联映射的工作模式类似于SQL中的左连接，如果目标`select`查询结果集为空集，那么最终Mapper方法返回值中关联属性的值为null

###### 基础

以下示例展示了通过ID查询一个`City`及该`City`所属的`Country`，并将该`Country`写入到属性`City.country`中

通过`association`的`property`声明需要填入数据的POJO关联属性名，并通过`javaType`声明该POJO关联属性的类型，通过`select`声明该POJO关联属性的值数据来源于指定的`select`，并将`column`中声明的结果集列传递到指定的`select`中

示例代码如下：

```xml
<select id="getCityAndCountryByCityId" resultMap="cityAndCountry">
    select * from city where id = #{cityId}
</select>

<resultMap id="cityAndCountry" type="com.example.entity.City">
    <id property="id" column="ID"/>
    <result property="name" column="Name"/>
    <result property="countryCode" column="CountryCode"/>
    <result property="district" column="District"/>
    <result property="population" column="Population"/>
    <association property="country" javaType="com.example.entity.Country" column="CountryCode" select="getCountryByCode"/>
</resultMap>

<select id="getCountryByCode" resultType="com.example.entity.Country">
    select * from country where code = #{code}
</select>
```

MyBatis在执行Mapper方法`getCityAndCountryByCityId`，会先执行`id`为`getCityAndCountryByCityId`的`select`中的SQL语句，在执行`id`为`getCountryByCode`的`select`中的SQL语句

###### 多层嵌套关联

可以多次嵌套，用于多个`select`的结果集的互相映射

以下示例展示了通过ID获取一个`City`，以及`City.country`属性所关联的`Country`，以及`Country.capitalCity`所关联的`City`，总共需要执行三次SQL语句

示例代码如下：

```xml
<select id="getCityAndCountryAndCapital" resultMap="cityAndCountryAndCapital">
    select id,name,countryCode from city where id = #{cityId}
</select>

<resultMap id="cityAndCountryAndCapital" type="com.example.entity.City">
    <id property="id" column="ID"/>
    <result property="name" column="Name"/>
    <result property="countryCode" column="CountryCode"/>
    <association property="country" javaType="com.example.entity.Country" column="CountryCode" select="getCountryAndCapital"/>
</resultMap>

<select id="getCountryAndCapital" resultMap="countryAndCapital">
    select code,name,capital from country where code = #{code}
</select>

<resultMap id="countryAndCapital" type="com.example.entity.Country">
    <id property="code" column="Code"/>
    <result property="name" column="Name"/>
    <result property="capital" column="Capital"/>
    <association property="capitalCity" javaType="com.example.entity.City" column="Capital" select="getCityById"/>
</resultMap>

<select id="getCityById" resultType="com.example.entity.City">
    select * from city where id = #{id}
</select>
```

注意不应通过在`association`元素中声明`association`子元素来实现多层嵌套查询

###### 多参数

如果目标`select`需要多个参数，应使用如`{param1=column1,param2=column2}`的格式对`association`的`column`属性进行声明，注意不匹配或不正确的声明可能仍正常执行

以下示例用于获取一个`Country`，及`Country.capitalCity`所关联的`City`，示例展示了在目标`select`中需要两个参数的情景，在`association`的`column`属性中将查询结果集的列与参数进行绑定

示例代码如下：

```xml
<select id="getCountryAndCapitalCityByCode" resultMap="countryAndCapitalCity">
    select code, name, capital from country where code = #{code}
</select>

<resultMap id="countryAndCapitalCity" type="com.crim.web.lab.springmvclab.web.entity.Country">
    <id property="code" column="Code"/>
    <result property="name" column="Name"/>
    <result property="capital" column="Capital"/>
    <association property="capitalCity" javaType="com.example.entity.City" column="{id=Capital,countryCode=Code}" select="getCityByIdAndCountryCode"/>
</resultMap>

<select id="getCityByIdAndCountryCode" resultType="com.example.entity.City">
    select * from city where id = #{id} and CountryCode = #{countryCode}
</select>
```

###### 工作原理及优化

由于基于嵌套查询的实质是是将SQL语句分开执行，在数据处理的映射阶段再进行关联，这导致这种模式极易发生N+1查询问题

示例：

```xml
<select id="getCityAndCountryPage" resultMap="cityAndCountry">
    select * from city limit #{start},#{size}
</select>

<resultMap id="cityAndCountry" type="com.example.entity.City">
    <id property="id" column="ID"/>
    <result property="name" column="Name"/>
    <result property="countryCode" column="CountryCode"/>
    <result property="district" column="District"/>
    <result property="population" column="Population"/>
    <association property="country" javaType="com.example.Country" column="CountryCode" select="getCountryByCode" fetchType="eager"/>
</resultMap>

<select id="getCountryByCode" resultType="com.example.entity.Country">
    select * from country where code = #{code}
</select>
```

上述代码在执行时，MyBatis将先从`city`表中查询N条记录，再通过N条记录的`Code`字段值另外执行N条SQL语句以从`country`表中查询

这显然将大幅降低性能，因此MyBatis提供了延迟加载（懒加载）功能

`association`元素提供了一个属性`fetchType`，其值为`lazy`或`eager`，用于控制该关联查询的工作模式是懒加载还是急加载

在急加载模式下，Mapper方法执行过程中MyBatis就将执行N+1条SQL语句，以立即完成查询结果集的关联映射

但在懒加载模式下，MyBatis通过动态代理技术，对POJO、`HashMap`等对象进行代理，Mapper方法执行过程仅执行了1条SQL语句，关联映射尚未完成，而当对应的关联属性需要获取时，SQL语句才开始执行，对应的查询结果集才被映射到该关联属性上，这可以减少Mapper方法的执行时间

但这意味着如果使用了懒加载模式，但在Mapper执行完毕后立刻开始遍历查询结果，将导致总体的性能比急加载更低

为了更好的性能，推荐使用其他工作模式的关联映射，详情见下文

```
Tips: 笔者在实际的性能测试中（Spring Boot3、MyBatis 3、Alibaba Druid 1.2、MySQL 8.0），上述代码在懒加载和急加载两种场景下（不包含对结果集的遍历）的性能表现相近，甚至在1000条数据查询下，急加载比懒加载性能表现更优，通过分析数据库日志等手段后推测，主要是因为一级缓存将需要执行的SQL语句数量大幅降至200条，且连接池技术也极大的优化了查询性能，至于为什么懒加载仅一条SQL语句也较急加载性能更差，笔者尚未得知原因，可能是动态代理的黑魔法导致的？
```

##### 基于嵌套结果映射的关联映射

基于嵌套结果映射的核心思想是将多个`resultMap`或`association`嵌套声明或关联起来，使得其结构与相应的嵌套POJO或Map对应，以用于将SQL连接查询的结果集映射到对应的数据结构

基于嵌套结果映射的`association`可以声明的属性有：

- `resultMap` 可选，被嵌套的结果映射`resultMap`的ID
- `columnPrefix` 可选，列前缀，详情参见下文
- `notNullColumn` 可选，非空列，详情参见下文
- `autoMapping` 可选，自动映射，详情参见下文

###### 基础

通过在`association`中声明更多的`id`或`result`子元素使得连接查询中的若干列可以被映射到被嵌套的POJO对象或Map中

以下示例展示了通过这种方式将连表查询的查询结果集映射到`Country`对象以及`Country.capitalCity`属性中

```xml
<select id="getCountryAndCapitalCityPage" resultMap="countryAndCapitalCity">
    select country.code       as countryCode,
           country.name       as countryName,
           country.Population as countryPopulation,
           country.capital    as capitalCityId,
           city.ID            as capital,
           city.name          as capitalCityName,
           city.Population    as capitalCityPopulation
    from country
             left join city on country.capital = city.id
    limit #{start},#{size}
</select>

<resultMap id="countryAndCapitalCity" type="com.example.entity.Country">
    <id property="code" column="countryCode"/>
    <result property="name" column="countryName"/>
    <result property="population" column="countryPopulation"/>
    <result property="capital" column="capitalCityId"/>
    <association property="capitalCity" javaType="com.example.entity.City">
        <id property="id" column="capital"/>
        <result property="name" column="capitalCityName"/>
        <result property="population" column="capitalCityPopulation"/>
    </association>
</resultMap>
```

可以通过`association`的`resultMap`属性，复用其他`resultMap`中的子元素声明，如上述示例与以下示例等效，它使用了`resultMap`属性将`association`元素复用了另一个`resultMap`元素

```xml
<select id="getCountryAndCapitalCityPage" resultMap="countryAndCapitalCity">
    select country.code       as countryCode,
           country.name       as countryName,
           country.Population as countryPopulation,
           country.capital    as capitalCityId,
           city.ID            as capital,
           city.name          as capitalCityName,
           city.Population    as capitalCityPopulation
    from country
            left join city on country.capital = city.id
    limit #{start},#{size}
</select>

<resultMap id="countryAndCapitalCity" type="com.entity.entity.Country">
    <id property="code" column="countryCode"/>
    <result property="name" column="countryName"/>
    <result property="population" column="countryPopulation"/>
    <result property="capital" column="capitalCityId"/>
    <association property="capitalCity" javaType="com.entity.entity.City" resultMap="capitalCity"/>
</resultMap>

<resultMap id="capitalCity" type="com.entity.entity.City">
    <id property="id" column="capital"/>
    <result property="name" column="capitalCityName"/>
    <result property="population" column="capitalCityPopulation"/>
</resultMap>
```

###### 多层嵌套关联

结果映射的嵌套也可以是多层的，以用于超过两层的POJO或Map嵌套的数据结构的映射

可通过在`association`元素中声明`association`子元素来实现多层嵌套，如以下示例展示了通过这种方式将连接查询映射到一个`City`对象以及`City.country`属性、以及`City.country.capitalCity`属性

```xml
<select id="getCityAndCountryAndCapitalCityPage" resultMap="cityAndCountryAndCapitalCity">
    select city1.ID           as cityID,
           city1.name         as cityName,
           city1.Population   as cityPopulation,
           country.code       as countryCode,
           country.name       as countryName,
           country.Population as countryPopulation,
           country.capital    as capitalCityId,
           city2.ID           as capital,
           city2.name         as capitalCityName,
           city2.Population   as capitalCityPopulation
    from city as city1
             left join country on city1.CountryCode = country.code
             left join city as city2 on country.capital = city2.id
    limit #{start},#{size}
</select>

<resultMap id="cityAndCountryAndCapitalCity" type="com.example.entity.City">
    <id property="id" column="cityID"/>
    <result property="name" column="cityName"/>
    <result property="population" column="cityPopulation"/>
    <association property="country" javaType="com.example.entity.Country">
        <id property="code" column="countryCode"/>
        <result property="name" column="countryName"/>
        <result property="population" column="countryPopulation"/>
        <result property="capital" column="capitalCityId"/>
        <association property="capitalCity" javaType="com.example.entity.City">
            <id property="id" column="capital"/>
            <result property="name" column="capitalCityName"/>
            <result property="population" column="capitalCityPopulation"/>
        </association>
    </association>
</resultMap>
```

多层嵌套同样可以是多层复用`resultMap`元素，如下示例与上述示例等效

```xml
<resultMap id="cityAndCountryAndCapitalCity" type="com.example.entity.City">
    <id property="id" column="cityID"/>
    <result property="name" column="cityName"/>
    <result property="population" column="cityPopulation"/>
    <association property="country" javaType="com.example.entity.Country" resultMap="countryAndCapitalCity"/>
</resultMap>
<resultMap id="countryAndCapitalCity" type="com.example.entity.Country">
    <id property="code" column="countryCode"/>
    <result property="name" column="countryName"/>
    <result property="population" column="countryPopulation"/>
    <result property="capital" column="capitalCityId"/>
    <association property="capitalCity" javaType="com.example.entity.City" resultMap="capitalCity"/>
</resultMap>

<resultMap id="capitalCity" type="com.example.entity.City">
    <id property="id" column="capital"/>
    <result property="name" column="capitalCityName"/>
    <result property="population" column="capitalCityPopulation"/>
</resultMap>
```

多层嵌套功能相当强大，使得不同工作模式的`association`、`collection`以及`resultMap`元素可以互相嵌套关联，以用于复杂数据结构的映射，以及映射的代码复用、性能优化

考虑到多层嵌套的性能优化是个庞大的话题，受实际的MyBatis映射逻辑、缓存、连接池、数据库、SQL等等多因素影响，依赖开发者的综合考量和实践测试，此处不做展开说明

###### 列前缀

###### 非空列

###### 工作原理及优化





## 工具类库





### BeanUtils

==TODO 空的章节==

