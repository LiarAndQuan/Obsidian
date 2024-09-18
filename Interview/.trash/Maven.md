## 一. 基本配置

>Maven的核心配置文件在"\\conf\\settings.xml"

### 1.指定本地仓库

>本地仓库默认值：用户目录/.m2/repository

````xml
<localRepository>指定的仓库地址</localRepository>
````

![[Attachments/Images/Pasted image 20230112160709.png]]

### 2.配置镜像仓库

>Maven 下载 jar 包默认访问境外的中央仓库，而国外网站速度很慢
>改成阿里云提供的镜像仓库，访问国内网站，可以让 Maven 下载 jar 包的时候速度更快
>配置的方式如下:

```xml
<mirror>
	<id>nexus-aliyun</id>
	<mirrorOf>central</mirrorOf>
	<name>Nexus aliyun</name>
	<url>http://maven.aliyun.com/nexus/content/groups/public</url>
</mirror>
```

![[Attachments/Images/Pasted image 20230112161736.png]]

### 3.配置 Maven 工程的基础 JDK 版本

>如果按照默认配置运行,Java 工程使用的默认 JDK 版本是 1.5,修改配置的方式是：将 profile 标签整个复制到 settings.xml 文件的 profiles 标签内

```xml
<profile>  
  <id>jdk-17</id>  
  <activation>  
    <activeByDefault>true</activeByDefault>  
    <jdk>17</jdk>  
  </activation>  
  <properties>  
    <maven.compiler.source>17</maven.compiler.source>  
    <maven.compiler.target>17</maven.compiler.target>  
    <maven.compiler.compilerVersion>17</maven.compiler.compilerVersion>  
  </properties>  
</profile>
```

### 4.配置环境变量

> Maven 是一个用 Java 语言开发的程序，它必须基于 JDK 来运行，需要通过 JAVA_HOME 来找到 JDK 的安装位置

首先需要配置JAVA_HOME和MAVEN_HOME为根路径 :

![[Attachments/Images/Pasted image 20230112162357.png]]

然后需要在path中添加环境变量 : 

![[Attachments/Images/Pasted image 20230509173800.png]]

在命令行中输入mvn -v 检查是否配置完成 : 

![[Attachments/Images/Pasted image 20230112162538.png]]

## 二. GAVP坐标

Maven 中的 GAVP 是指 GroupId、ArtifactId、Version、Packaging 等四个属性的缩写，其中前三个是必要的，而 Packaging 属性为可选项。这四个属性主要为每个项目在maven仓库中做一个标识

**GAV遵循以下规则：**

1. GroupID 格式：com.{公司/BU }.业务线.\[子业务线]，最多 4 级

	说明：{公司/BU} 例如：alibaba/taobao/tmall/aliexpress 等 BU 一级；子业务线可选
	正例：com.taobao.tddl 或 com.alibaba.sourcing.multilang

2. ArtifactID 格式：产品线名-模块名。语义不重复不遗漏，先到仓库中心去查证一下

	正例：tc-client / uic-api / tair-tool / bookstore

3. Version版本号格式推荐：主版本号.次版本号.修订号

	主版本号：当做了不兼容的 API 修改，或者增加了能改变产品方向的新功能
	次版本号：当做了向下兼容的功能性新增（新增类、接口等）
	修订号：修复 bug，没有修改方法签名的功能加强，保持 API 兼容性
	正例： 初始→1.0.0  修改bug → 1.0.1  功能调整 → 1.1.1等

**Packaging定义规则：**

​	指示将项目打包为什么类型的文件，idea根据packaging值，识别maven项目类型

1. packaging 属性为 jar（默认值），代表普通的Java工程，打包以后是.jar结尾的文件。

2. packaging 属性为 war，代表Java的web工程，打包以后.war结尾的文件。

3. packaging 属性为 pom，代表不会打包，用来做继承的父工程。

**坐标和仓库中 jar 包的存储路径之间的对应关系 : **

````xml
<groupId>javax.servlet</groupId>
<artifactId>servlet-api</artifactId>
<version>2.5</version>
````

>这样的一个jar包在本地仓库的存储路径为`Maven本地仓库根目录\javax\servlet\servlet-api\2.5\servlet-api-2.5.jar`

## 三. Pom文件

>POM：Project Object Model，项目对象模型
>和 POM 类似的是：DOM（Document Object Model），文档对象模型
>它们都是模型化思想的具体体现
>POM 理念集中体现在 Maven 工程根目录下 pom.xml 这个配置文件中

**pom.xml的解读**

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">  
  <modelVersion>4.0.0</modelVersion>  
  
  <!-- 当前Maven工程的坐标 -->  
  <groupId>quan</groupId>  
  <artifactId>pro01</artifactId>  
  <version>1.0-SNAPSHOT</version>  
  
  <!-- 当前Maven工程的打包方式，可选值有下面三种： -->  
  <!-- jar：表示这个工程是一个Java工程  -->  
  <!-- war：表示这个工程是一个Web工程 -->  
  <!-- pom：表示这个工程是“管理其他工程”的工程 -->  
  <packaging>jar</packaging>  
  
  <name>pro01</name>  
  <url>http://maven.apache.org</url>  
  
  <properties>  
    <!-- 工程构建过程中读取源码时使用的字符集 -->  
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>  
  </properties>  
  
  <!-- 当前工程所依赖的jar包 -->  
  <dependencies>  
    <!-- 使用dependency配置一个具体的依赖 -->  
    <dependency>  
      <!-- 在dependency标签内使用具体的坐标依赖我们需要的一个jar包 -->  
      <groupId>junit</groupId>  
      <artifactId>junit</artifactId>  
      <version>3.8.1</version>  
      <!-- scope标签配置依赖的范围 -->  
      <scope>test</scope>  
    </dependency>  
  </dependencies>  
</project>
```

## 四. 依赖

### 1.依赖的范围

> <\scope><\/scope>
> 标签的可选值：**compile**/**test**/**provided**/system/runtime/**import**

| 依赖范围    | 描述                                                                                                                                                                                                             |
|:-------------- |:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **compile**  | 编译依赖范围，scope 元素的缺省值。使用此依赖范围的 Maven 依赖，对于三种 classpath 均有效，即该 Maven 依赖在上述三种 classpath 均会被引入。例如，log4j 在编译、测试、运行过程都是必须的。                         |
| **test**     | 测试依赖范围。使用此依赖范围的 Maven 依赖，只对测试 classpath 有效。例如，Junit 依赖只有在测试阶段才需要。                                                                                                       |
| **provided** | 已提供依赖范围。使用此依赖范围的 Maven 依赖，只对编译 classpath 和测试 classpath 有效。例如，servlet-api 依赖对于编译、测试阶段而言是需要的，但是运行阶段，由于外部容器已经提供，故不需要 Maven 重复引入该依赖。 |
| runtime      | 运行时依赖范围。使用此依赖范围的 Maven 依赖，只对测试 classpath、运行 classpath 有效。例如，JDBC 驱动实现依赖，其在编译时只需 JDK 提供的 JDBC 接口即可，只有测试、运行阶段才需要实现了 JDBC 接口的驱动。         |
| system       | 系统依赖范围，其效果与 provided 的依赖范围一致。其用于添加非 Maven 仓库的本地依赖，通过依赖元素 dependency 中的 systemPath 元素指定本地依赖的路径。鉴于使用其会导致项目的可移植性降低，一般不推荐使用。          |
| import       | 导入依赖范围，该依赖范围只能与 dependencyManagement 元素配合使用，其功能是将目标 pom.xml 文件中 dependencyManagement 的配置导入合并到当前 pom.xml 的 dependencyManagement 中。                                   |

**compile与test的对比**

|         | main目录(空间) | test目录(空间) | 开发过程(时间) | 部署到服务器(时间) |
|:-------:|:--------------:|:--------------:|:--------------:|:------------------:|
| compile |      有效      |      有效      |      有效      |        有效        |
|  test   |      无效      |      有效      |      有效      |        无效        |

**compile 和 provided 对比**

|          | main目录(空间) | test目录(空间) | 开发过程(时间) | 部署到服务器(时间) |
|:--------:|:--------------:| -------------- |:--------------:|:------------------:|
| compile  |      有效      | 有效           |      有效      |        有效        |
| provided |      有效      | 有效           |      有效      |        无效        |
|          |                |                |                |                    |

**总结 :** 
- compile：通常使用的第三方框架的 jar 包这样在项目实际运行时真正要用到的 jar 包都是以 compile 范围进行依赖的。比如 SSM 框架所需jar包
- test：测试过程中使用的 jar 包，以 test 范围依赖进来。比如 junit
- provided：在开发过程中需要用到的“服务器上的 jar 包”通常以 provided 范围依赖进来。比如 servlet-api、jsp-api。而这个范围的 jar 包之所以不参与部署、不放进 war 包，就是避免和服务器上已有的同类 jar 包产生冲突，同时减轻服务器的负担  

### 2.依赖的传递性

> A 依赖 B，B 依赖 C，那么在 A 没有配置对 C 的依赖的情况下，A 里面能不能直接使用 C ?

这个问题取决于 B 依赖 C 时使用的依赖范围
-  B 依赖 C 时使用 compile 范围：可以传递
-  B 依赖 C 时使用 test 或 provided 范围：不能传递，所以需要这样的 jar 包时，就必须在需要的地方明确配置依赖才可以

### 3.依赖冲突

![[Attachments/Images/Pasted image 20230112201041.png]]

> 配置依赖的排除其实就是阻止某些 jar 包的传递。因为这样的 jar 包传递过来会和其他 jar 包冲突,需要在dependency中加入exclusions标签排除依赖,配置方式如下:

```xml
<dependency>
	<groupId>com.atguigu.maven</groupId>
	<artifactId>pro01-maven-java</artifactId>
	<version>1.0-SNAPSHOT</version>
	<scope>compile</scope>
	<!-- 使用excludes标签配置依赖的排除	-->
	<exclusions>
		<!-- 在exclude标签中配置一个具体的排除 -->
		<exclusion>
			<!-- 指定要排除的依赖的坐标（不需要写version） -->
			<groupId>commons-logging</groupId>
			<artifactId>commons-logging</artifactId>
		</exclusion>
	</exclusions>
</dependency>
```

### 4.依赖的继承与聚合

>Maven工程之间，A 工程继承 B 工程 ,  本质上是 A 工程的 pom.xml 中的配置继承了 B 工程中 pom.xml 的配置

操作步骤如下 :

1.  创建父工程,修改打包方式为pom, ``<packaging>pom</packaging>``

2.  在父工程目录下创建子工程,会自动生成module标签, 这就是module与父项目的聚合

> 聚合的好处
> 1. 一键执行 Maven 命令：很多构建命令都可以在“总工程”中一键执行. 
> 以 mvn install 命令为例：Maven 要求有父工程时先安装父工程；有依赖的工程时，先安装被依赖的工程。我们自己考虑这些规则会很麻烦。但是工程聚合之后，在总工程执行 mvn install 可以一键完成安装，而且会自按照正确的顺序执行
> 2. 配置聚合之后，各个模块工程会在总工程中展示一个列表，让项目中的各个模块一目了然

```xml
<modules>  
	<module>pro04-maven-module</module>
	<module>pro05-maven-module</module>
	<module>pro06-maven-module</module>
</modules>
```

3. 子工程会自动创建parent标签

```xml
<!-- 使用parent标签指定当前工程的父工程 -->
<parent>
	<!-- 父工程的坐标 -->
	<groupId>com.atguigu.maven</groupId>
	<artifactId>pro03-maven-parent</artifactId>
	<version>1.0-SNAPSHOT</version>
</parent>

<!-- 子工程的坐标 -->
<!-- 如果子工程坐标中的groupId和version与父工程一致，那么可以省略 -->
<!-- <groupId>com.atguigu.maven</groupId> -->
<artifactId>pro04-maven-module</artifactId>
<!-- <version>1.0-SNAPSHOT</version> -->
```

4. 在父工程中配置依赖的统一管理

```xml
<!-- 使用dependencyManagement标签配置对依赖的管理 -->
<!-- 被管理的依赖并没有真正被引入到工程 -->
<dependencyManagement>
	<dependencies>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-core</artifactId>
			<version>4.0.0.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-beans</artifactId>
			<version>4.0.0.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>4.0.0.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-expression</artifactId>
			<version>4.0.0.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-aop</artifactId>
			<version>4.0.0.RELEASE</version>
		</dependency>
	</dependencies>
</dependencyManagement>
```

5. 子工程中引用那些被父工程管理的依赖

```xml
<!-- 子工程引用父工程中的依赖信息时，可以把版本号去掉。	-->
<!-- 把版本号去掉就表示子工程中这个依赖的版本由父工程决定。 -->
<!-- 具体来说是由父工程的dependencyManagement来决定。 -->
<dependencies>
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-core</artifactId>
	</dependency>
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-beans</artifactId>
	</dependency>
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-context</artifactId>
	</dependency>
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-expression</artifactId>
	</dependency>
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-aop</artifactId>
	</dependency>
</dependencies>
```

6. 可以在父工程中声明自定义属性,在需要使用的地方引用自定义的属性名

```xml
<!-- 通过自定义属性，统一指定Spring的版本 -->
<properties>
	<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
	
	<!-- 自定义标签，维护Spring版本数据 -->
	<quan.spring.version>4.3.6.RELEASE</quan.spring.version>
</properties>
<dependencies>
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-core</artifactId>
		<version>${atguigu.spring.version}</version>
	</dependency>
<dependencies/>
```

## 五. 常用命令

|    命令     |            描述             |
|:-----------:|:---------------------------:|
| mvn compile |  编译项目，生成target文件   |
| mvn package | 打包项目，生成jar或war文件  |
|  mvn clean  | 清理编译或打包后的项目结构  |
| mvn install |  打包后上传到maven本地仓库  |
| mvn deploy  | 只打包，上传到maven私服仓库 |
|  mvn site   |          生成站点           |
|  mvn test   |        执行测试源码         |

## 六. build配置

项目构建是指将源代码、依赖库和资源文件等转换成可执行或可部署的应用程序的过程，在这个过程中包括编译源代码、链接依赖库、打包和部署等多个步骤。

默认情况下，构建不需要额外配置，都有对应的缺省配置。当然了，我们也可以在pom.xml定制一些配置，来修改默认构建的行为和产物！

例如：

1.  指定构建打包文件的名称，非默认名称
2.  制定构建打包时，指定包含文件格式和排除文件
3.  打包插件版本过低，配置更高版本插件

构建配置是在pom.xml / build标签中指定！

**指定打包命名**

```xml
<!-- 默认的打包名称：artifactid+verson.打包方式 -->
<build>
  <finalName>定义打包名称</finalName>
</build>  
```

**指定打包文件**

如果在java文件夹中添加java类，会自动打包编译到classes文件夹下！

但是在java文件夹中添加xml文件，默认不会被打包！

默认情况下，按照maven工程结构放置的文件会默认被编译和打包！

除此之外、我们可以使用resources标签，指定要打包资源的文件夹要把哪些静态资源打包到 classes根目录下！

应用场景：mybatis中有时会将用于编写SQL语句的映射文件和mapper接口都写在src/main/java下的某个包中，此时映射文件就不会被打包，如何解决

```xml
<build>
    <!--设置要打包的资源位置-->
    <resources>
        <resource>
            <!--设置资源所在目录-->
            <directory>src/main/java</directory>
            <includes>
                <!--设置包含的资源类型-->
                <include>**/*.xml</include>
            </includes>
        </resource>
    </resources>
</build>
```

**配置依赖插件**

dependencies标签下引入开发需要的jar包！我们可以在build/plugins/plugin标签引入插件！

常用的插件：修改jdk版本、tomcat插件、mybatis分页插件、mybatis逆向工程插件等等！

```xml
<build>
  <plugins>
      <!-- java编译插件，配jdk的编译版本 -->
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <configuration>
          <source>1.8</source>
          <target>1.8</target>
          <encoding>UTF-8</encoding>
        </configuration>
      </plugin>
      <!-- tomcat插件 -->
      <plugin>
        <groupId>org.apache.tomcat.maven</groupId>
        <artifactId>tomcat7-maven-plugin</artifactId>
         <version>2.2</version>
          <configuration>
          <port>8090</port>
          <path>/</path>
          <uriEncoding>UTF-8</uriEncoding>
          <server>tomcat7</server>
        </configuration>
      </plugin>
    </plugins>
</build>
```

## 七. Maven的生命周期

> 为了让构建过程自动化完成，Maven 设定了三个生命周期，生命周期中的每一个环节对应构建过程中的一个操作, 三个生命周期彼此是独立的
> 在任何一个生命周期内部，执行任何一个具体环节的操作，都是从本周期最初的位置开始执行，直到指定的地方

三个生命周期 :

![[Attachments/Images/Pasted image 20230112213259.png]]

