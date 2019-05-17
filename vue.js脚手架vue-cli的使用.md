# vue.js脚手架vue-cli的使用

**vue-cli**这个构建工具大大降低了webpack的使用难度，支持热更新，有**webpack-dev-server**的支持，相当于启动了一个请求服务器，给你搭建了一个测试环境，只关注开发就OK。

## 一、安装vue-cli

首先要确保安装了webpack

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190505165501.png)

执行结果截图如下：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190505165535.png)

安装成功：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190505165553.png)

安装完成之后输入 vue -V（注意这里是大写的“V”），如下图，如果出现相应的版本号，则说明安装成功。

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190505165627.png)

打开C:\Users\Andminster\AppData\Roaming\npm目录下可以看到：

 ![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190505170307.png)

打开node_modules也可以看到：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190505165709.png)

到这里vue-cli就算是安装完了



## 二、用vue-cli来构建项目

### **①** 我首先在D盘新建一个文件夹（dxl_vue）作为项目存放地，然后使用命令行cd进入到项目目录输入：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190505165732.png)

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190505165754.png)

输入命令后，会跳出几个选项让你回答：

- Project name (baoge)：     -----项目名称，直接回车，按照括号中默认名字（注意这里的名字不能有大写字母，如果有会报错Sorry, name can no longer contain capital letters），阮一峰老师博客[为什么文件名要小写](https://link.jianshu.com?t=http://www.ruanyifeng.com/blog/2017/02/filename-should-be-lowercase.html) ，可以参考一下。
- Project description (A Vue.js project)：  ----项目描述，也可直接点击回车，使用默认名字
- Author ()：       ----作者，输入dongxili
   接下来会让用户选择：
- Runtime + Compiler: recommended for most users    运行加编译，既然已经说了推荐，就选它了
   Runtime-only: about 6KB lighter min+gzip, but templates (or any Vue-specificHTML) are ONLY allowed in .vue files - render functions are required elsewhere   仅运行时，已经有推荐了就选择第一个了
- Install vue-router? (Y/n)    是否安装vue-router，这是官方的路由，大多数情况下都使用，这里就输入“y”后回车即可。
- Use ESLint to lint your code? (Y/n)      是否使用ESLint管理代码，ESLint是个代码风格管理工具，是用来统一代码风格的，一般项目中都会使用。
   接下来也是选择题Pick an ESLint preset (Use arrow keys)            选择一个ESLint预设，编写vue项目时的代码风格，直接y回车
- Setup unit tests with Karma + Mocha? (Y/n)  是否安装单元测试，我选择安装y回车
- Setup e2e tests with Nightwatch(Y/n)?     是否安装e2e测试 ，我选择安装y回车

回答完毕后上图就开始构建项目了。

### **②** 配置完成后，可以看到目录下多出了一个项目文件夹baoge，然后cd进入这个文件夹：

**安装依赖**：  （根据上面的输入选项的回答信息install相关的包）

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190505165832.png)

( 如果安装速度太慢。可以安装淘宝镜像，打开命令行工具，输入：
 `npm install -g cnpm --registry=https://registry.npm.taobao.org`
 然后使用`cnpm`来安装 )

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190505165853.png)

npm install ：安装所有的模块，如果是安装具体的哪个个模块，在install 后面输入模块的名字即可。而只输入install就会按照项目的根目录下的package.json文件中依赖的模块安装（这个文件里面是不允许有任何注释的），每个使用npm管理的项目都有这个文件，是npm操作的入口文件。因为是初始项目，还没有任何模块，所以我用npm install 安装所有的模块。安装完成后，目录中会多出来一个node_modules文件夹，这里放的就是所有依赖的模块。

然后现在，baoge文件夹里的目录是这样的：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190505165919.png)

解释下每个文件夹代表的意思(仔细看一下这张图）：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190505165940.png)



##  三、启动项目 

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190505170006.png)

如果浏览器打开之后，没有加载出页面，有可能是本地的 8080 端口被占用，需要修改一下配置文件 config里的index.js

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190505170024.png)

还有，如果本地调试项目时，建议将build 里的`assetsPublicPath`的路径前缀修改为 ' ./ '（开始是 ' / '），因为打包之后，外部引入 js 和 css 文件时，如果路径以 ' / ' 开头，就会从本地去找，在本地是无法找到对应文件的（服务器上没问题）。所以**如果需要在本地打开打包后的文件**，就得修改文件路径。

我的端口没有被占用，直接成功（服务启动成功后浏览器会默认打开一个“欢迎页面”）：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190505170043.png)

**注意：在进行vue页面调试时，一定要去谷歌商店下载一个vue-tool扩展程序。**



##   四、vue-cli的webpack配置分析

-  从`package.json`可以看到开发和生产环境的入口。

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190505170102.png)

- 可以看到dev中的设置，**build/webpack.dev.conf.js**，该文件是开发环境中webpack的配置入口。

- 在webpack.dev.conf.js中出现**webpack.base.conf.js**，这个文件是开发环境和生产环境，甚至测试环境，这些环境的公共webpack配置。可以说，这个文件相当重要。

- 还有**config/index.js 、build/utils.js  、build/build.js**等，具体请看这篇介绍：
   [https://segmentfault.com/a/1190000008644830](https://link.jianshu.com?t=https%3A%2F%2Fsegmentfault.com%2Fa%2F1190000008644830)



##  五、打包上线

注意，自己的项目文件都需要放到 src 文件夹下。
在项目开发完成之后，可以输入 `npm run build` 来进行打包工作。

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190505170123.png)

另外补充一些常见问题解答如下图：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190505170142.png)

打包完成后，会生成 dist 文件夹，如果已经修改了文件路径，可以直接打开本地文件查看。
项目上线时，只需要将 dist 文件夹放到服务器就行了。



本文转载自链接：<https://www.jianshu.com/p/32beaca25c0d>
