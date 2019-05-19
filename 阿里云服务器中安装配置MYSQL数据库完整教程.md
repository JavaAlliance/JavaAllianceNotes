# 阿里云服务器中安装配置MYSQL数据库完整教程

##  一、确保服务器系统处于最新状态（这步操作可有可无）



 **第一步：** 确保服务器系统处于最新状态（这步操作可有可无）

```
[root@localhost ~]# yum -y update
```

如果显示以下内容说明已经更新完成

```
Replaced:
grub2.x86_64 1:2.02-0.64.el7.centos grub2-tools.x86_64 1:2.02-0.64.el7.centos
Complete!
```

 **第二步：** 重启服务器（这步操作也可以跳过）
[root@localhost ~]# reboot

**第三步：** 首先检查是否已经安装，如果已经安装先删除以前版本，以免安装不成功

[root@localhost ~]# rpm -qa | grep mysql
或
[root@localhost ~]# yum list installed | grep mysql

如果安装了的话，就使用下面这条命令删除原先的mysql,举例如下：

```
rpm -e  --nodeps        mysql-libs-5.1.73-5.e16_6.i686  
```

​          

**第四步：** 下载MySql安装包

```
rpm -ivh http://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm
```

或

```
[root@localhost ~]# rpm -ivh http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm
```

**第五步：** 安装MySql

```
[root@localhost ~]# yum install -y mysql-server
或
[root@localhost ~]# yum install mysql-community-server
```

如果显示以下内容说明安装成功
Complete!

**第六步：** 设置开机启动mysql

```
 systemctl enable mysqld.service
```

**第七步：** 检查是否已经安装了开机自动启动

```
systemctl list-unit-files | grep mysqld
```

如果显示以下内容说明已经完成自动启动安装
mysqld.service enabled

**第八步：** 设置开启服务

```
systemctl start mysqld.service
```

**第九步：** 查看MySql默认密码

```
grep 'temporary password' /var/log/mysqld.log   
```

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190519133729.png)

**第十步：** 登陆MySql，输入用户名和密码

```
mysql -uroot -p       //密码也就是第九步里面查看到的默认密码
```

**第十一步：** 修改当前用户密码    注意看下面的报错

```
mysql>SET PASSWORD = PASSWORD('alliance');  //但是这样会报错的，具体错误看下面
```

注：直接复制粘贴上边的命令，会报错，错误如下

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190519135025.png)

解决方案如下：

原因：mysql为了安全，有自己的策略要求，如果我们想将其设置为我们常用的root或者123456这样的密码，需要修改策略要求，具体命令如下：

1.设置密码的验证强度等级，设置 validate_password_policy 的全局参数为 LOW 即可，
输入设值语句 “ set global validate_password_policy=LOW; ” 进行设值

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190519140604.png)

然后再次在mysql命令行下执行

```
mysql>SET PASSWORD = PASSWORD('alliance'); 
```

 ![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190519142214.png)

成功！



**第十二步：** 开启远程登录，授权root远程登录（**解释：不要以为阿里云服务器可以远程登录root用户，就以为我们也可以以mysql的root用户身份远程登录mysql数据库**）

```
mysql>GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'alliance' WITH GRANT OPTION;   //这里的alliance要换成你自己mysql数据库的密码
```

**第十三步：** 命令立即执行生效

```
mysql>flush privileges;//// 这一步一定要做，不然无法成功！ 这句表示从mysql数据库的grant表中重新加载权限数， 因为MySQL把权限都放在了cache中，所以在做完更改后需要重新加载。
```

Mysql是为了安全考虑，初始的时候并没有开启Root用户（**解释：mysql的root用户和我们云服务器的root用户是两个不同的，分开的**）的远程访问权限，Root（**解释：这里是指mysql的root用户，而不是云服务器的root用户**）只能本地localhost，127.0.0.1访问，但是我们操作起来实在是不方便，下面我们就使用Xshell连接Linux服务器操作Mysql给Root用户（**解释：这里是指mysql的root用户，而不是云服务器的root用户**）添加远程访问权限。（**解释：**上面的第十二步，就是授权其他工具可以远程以mysql的root用户的身份登录mysql（解释：这里是指mysql的root用户，而不是云服务器的root用户））



## 下面我们来看实操篇吧

我们先试用Xshell连接我们的远程Linux服务器：

  ①先远程登录云服务器

 ②然后输入mysql -u root -p命令，回车会出现 Enter password: 然后将我们的root用户密码输入进去再次回车：

然后要切换到mysql数据库

-> use mysql

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190519150457.png)

③接下来我们可以查看一下现有用户及连接权限

   -> select  User,authentication_string,Host from user

mysql是为了安全考虑所以初始的时候远程是不能访问的，只能本地localhost，127.0.0.1访问。（**就相当于远程只能登录mysql这个工具，却不能以某种用户的身份访问里面的数据**（我们平时做项目都是以root用户的身份访问指定数据库数据））



④下面我们就再添加一个root用户，密码暂时为空，允许任意Ip访问'%'   

```
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'root用户的密码' WITH GRANT OPTION;
```

 然后我们刷新一下mysql的权限

-> flush privileges;   // 这一步一定要做，不然无法成功！ 这句表示从mysql数据库的grant表中重新加载权限数据

​                           因为MySQL把权限都放在了cache中，所以在做完更改后需要重新加载。

执行完这两步，再次查询用户表命令：select  User,authentication_string,Host from user

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190519154500.png)

 发现多了一个用户，该用户所有的主机都可以访问，此时再次用navicat远程访问连接成功！如果还是连接不上的话，可以检查以下几项：

​    1.防火墙状态是否关闭u

​    2.阿里云服务器后台也要设置，如下图，点击“安全组配置”，开放指定端口号

   ![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190519154815.png)

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190519155050.png)

开放了指定的端口号之后，navicat就可以远程通过3306端口号来访问云服务器上的mysql数据库了（身份是mysql的root用户）

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190519155132.png)
