# 部署springboot项目到云服务器的两种方式（jar+war）

springboot版本：2.0.3.RELEASE

云服务器：阿里云ECS CentOS 7.3 64位

IDE：IntelliJ IDEA

服务器远程连接工具：Xshell 5

# 方式一、以jar文件运行

### 第一步：添加maven依赖

Spring Boot 默认以jar包方式运行，

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190520092726.png)

可以在Maven配置如下插件，将Spring Boot 导出成可执行的jar文件。

```
<build>
	<plugins>   
		<plugin>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-maven-plugin</artifactId>
		</plugin>
	</plugins>
</build>
```

### 第二步：将项目打包

在工程目录下的运行命令行中运行mvn package：

\>mvn package

推荐使用IDE中图形化界面的操作（下图的第4步是为了跳过test测试操作）

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190520093311.png)

package会将Maven工程打包成一个可执行的jar文件存放在target目录下，在控制台中看到有如下输出则表示输出成功：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190520093456.png)



### 第三步：将jar文件放到服务器

 打包好的jar文件已存放在target目录下

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190520093719.png)

鼠标放在该项目上右键，之后点击show in Explorer,展示文件所在目录位置![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190520125719.png)

将jar文件放到服务器合适的目录下，使用rz命令上传

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190520125442.png)



### 第四步：在服务器运行jar文件

切换到jar文件所在目录，执行命令（前提是已[在服务器装好jdk](https://blog.csdn.net/weixin_39274753/article/details/80315256)）

```
java -jar yourProjectName.jar
```

### 第五步：浏览器访问

在浏览器输入地址访问，OK成功了

**注意：**注意和在本地电脑运行相比，只需将ip地址换成服务器的即可，端口号和路径名都是跟在本地电脑运行时一样的，即与yml配置文件一致

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190520101005.png)

至此，项目部署完成



# 方式二、以war方式部署

Spring Boot 默认自带了一个嵌入式的Tomcat服务器，可以以jar方式运行，更为常见的情况是需要将Spring Boot 应用打成一个war包，部署到Tomcat等服务器上(**解释：**因为SpringBoot里面自带tomcat插件，我们不用它自带的，所以我们先要将其移除，然后使用我们自己外来的tomcat）。

### 第一步：修改打包方式

这种情况下，需要将pom中的packaging改成war方式：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190520101254.png)

### 第二步：移除嵌入式tomcat插件

因为SpringBoot里面自带tomcat插件，我们不用它自带的，所以我们先要将其移除，然后用我们自己外来的tomcat

这里提供2种方式来移除（本文项目使用方式2）

 **方式1：** 需要将嵌入的Tomcat依赖方式改成provided（provided是指“编译、测试时将依赖的包加入本工程的classpath，运行时不加入，可以理解成运行时不使用Spring Boot 自带的Tomcat”）

```

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-tomcat</artifactId>
    <scope>provided</scope>
</dependency>
```

 

**方式2：** 在pom.xml里找到`spring-boot-starter-web`依赖节点，在其中添加如下代码，

```

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <!-- 移除嵌入式tomcat插件 -->
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```



### 第三步：移除了tomcat之后，要添加servlet-api的依赖

```
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>3.1.0</version>
    <scope>provided</scope>   
</dependency>
```

provided是指“编译、测试时将依赖的包加入本工程的classpath，运行时不加入，可以理解成运行时不使用t”



### 第四步：修改启动类，并重写初始化方法

只需要重新下面这个方法即可（至于为什么，先不管，反正这样配，原因应该是为了让该SpringBoot项目使用外来的服务器而不用SpringBoot自带的tomcat，外来的是什么服务器,那就是什么服务器）

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190520123500.png)

### 第五步：打包

操作步骤与jar方式的打包一致，打包完成后会在target目录下生成.war文件

###  第六步：部署

将war文件放到Tomcat的webapps目录下，启动Tomcat（在bin目录下执行./startup.sh），即可自动解压部署

### 浏览器访问：

####  因为外置tomcat的时候，访问的时候，默认是要带项目名才能访问的

**404问题：** 由于本SpringBoot项目是采用外部的tomcat，所以默认访问要带项目名，但是我们在编写程序的时候，代码里面访问都是不带项目名的，所以这个时候会报404错误，

**所以如果我们想要不写项目名，直接通过服务器的ip地址访问项目该怎么解决呢？**

解决办法（**先看下面的图，再看后面的链接文章**）是：<https://www.cnblogs.com/huaixiaonian/p/10521460.html>，里面提供了两种方法，很详细了

这里还是简单说一下吧：**就算是我们是用idea打包的，这里也是写eclipse**，

vrs是项目名

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190520124328.png)



**由于不使用Spring Boot 自带的Tomcat所以yml文件下的server配置不起作用**

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190520125037.png)

到此，项目部署完成

##  总结

1、jar包部署方式使用Spring Boot 自带的Tomcat，因为Spring Boot 应用自带Tomcat，所以可直接在服务器运行jar文件

2、war包部署方式则使用云服务器里的Tomcat，此时需要移除Spring Boot 自带的Tomcat插件

3、注意2种部署方式的访问路径差异

4、注意所用端口号是否已在安全组开放
