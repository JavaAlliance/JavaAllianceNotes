# 在linux服务器上安装jdk

**准备**（括号里为本文所用）**：**

​    1、服务器（阿里云服务器，CentOS 7.3 64位 ）；

​    2、Linux上的jdk压缩包（jdk-8u161-linux-x64.tar.gz）；

​    3、服务器远程连接工具（Xshell 5）；

 

### **步骤：**

①打开连接工具xshell，远程连接云服务器

② yum install lrzsz ，安装lrzsz，用于往服务器上传文件（**阿里云服务器默认安装了yum**）

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190519221820.png) 

​       ④ cd /usr ，切换目录到usr

​        mkdir JavaJDK，创建文件夹JavaJDK，存放jdk压缩包

​        cd JavaJDK，进入JavaJDK文件夹

​        rz ，弹出如下窗口，选取要上传的文件,然后把jdk-linux版本的上传

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190519222935.png)

查看文件夹，压缩包已存在，说明上传成功了

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190520083920.png)



⑤解压压缩包，解压   

```
 tar -zxvf jdk-8u151-linux-x64.tar.gz //解压到原目录
```

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190520085713.png)

然后使用vim /etc/profile修改配置文件 ，在文件最前面添加如下内容（注意JAVA_HOME一定要是你jdk压缩包解压后的目录）    ，注意，export一定要顶格，最前面才行

```
export JAVA_HOME=/root/usr/JavaJDK/jdk1.8.0_151
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export  PATH=${JAVA_HOME}/bin:$PATH
```

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190520091131.png)

配置完后要执行source /etc/profile命令，刷新该配置文件

到此，jdk已安装完成

​        java -version ，若出现如下显示，则说明jdk安装成功

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190520091249.png)

