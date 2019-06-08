#   在Linux上使用ActiveMQ

##  一、下载linux版的ActiveMQ

简单记一下，下载地址 <http://activemq.apache.org/download.html>

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190607223645.png)

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190607223724.png)

即可下载linux版的ActiveMQ



##  二、安装JDK（必须jdk7以上才行）

ActiveMQ部署其实很简单，和所有Java一样，要跑java程序就必须先安装JDK并配置好环境变量，这个很简单



在linux服务器上安装jdk：<https://blog.csdn.net/qq_40241957/article/details/90369063>



## 三、上传activemq-5.15.9-bin.tar.gz到linux服务器

通过rz命令上传到linux服务器上，然后使用tar -zxvf activemq-5.15.9-bin.tar.gz 来解压。



## 四、运行ActiveMQ

activemq启动分linux-x86-32和linux-x86-64

进入bin/linux-x86-64下,然后使用./activemq start命令启动

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190607224753.png)



## 五、启动成功后，访问

**区别：61616端口和8161端口，一个是服务端端口，一个是浏览器控制端端口**

activemq默认服务端端口号是61616，（官方文档：ActiveMQ's default port is 61616. ）  

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190607233230.png)

也可以在浏览器通过8161端口访问       ip是指linux服务器的地址

```
http://ip:8161/admin
```

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190607233546.png)

默认登录用户：admin 密码：admin

修改用户信息编辑 conf/jetty-realm.properties 即可