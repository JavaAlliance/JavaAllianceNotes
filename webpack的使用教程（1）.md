#  webpack的使用教程(1)

##   一、先安装Node.js包

**第一步:** 先去 [Node.js官网](https://nodejs.org/en/)下载安装Node.js,傻瓜式安装，一路next

虽然我们把Node.js安装在D盘了，但是除了这个目录，其实在C盘的另外一个目录下，还有一个附带生成的目录【C:\Users\用户名\AppData\Roaming\npm】，这个目录是用来存放你通过npm全局安装的包。比如，如果你通过“npm i nrm -g”全局下载nrm这个工具，那么下载的文件就会被保存到“【C:\Users\用户名\AppData\Roaming\npm】”这个目录下。

既然Node.js的安装目录都不想放到C盘，那么通过npm下载的包更不想放在C盘。我希望统一管理，把npm全局下载的包都保存到nodejs的根目录下，也就是“D:\nodejs”这个路径下。所以我们就必须做一些响应的设置了。接下来第二步就是解决这个问题的。

**第二步：** 修改默认的全局目录

​     有以下两种方案可以选择：

​      **方法一:**  到node安装目录[D:/nodejs]执行以下命令：

​        **前提条件:** 在node安装目录创建`/nodejs/node_global`、`/nodejs/node_cache`两个文件夹存放全局包

```
npm config set prefix D:/nodejs/node_global/ //全局包目录，就在node安装目录新建了个nodejs文件夹存放
npm config set cache D:/nodejs/node_cache/  //全局包缓存目录，就在node安装目录新建了个nodejs文件夹存放

```

​    **方法二：** 直接修改`C:/Users/[username]/.npmrc`文件的`cache`值和`prefix`值，文件如下：

```
prefix=D:\node\nodejs\node_global
cache=D:\node\nodejs\node_cache
registry=https://registry.npm.taobao.org/
```

**第三步：** 配置环境变量

计算机->属性->高级系统配置->环境变量->用户变量->编辑`path`,添加`global“目录如下 

```
PATH: D:\node\nodejs\node_global\;
```

　OK，到这里已经设置好了，你可以开心的使用nodejs，以及它附带的npm工具了

 

## 二、在Git或者cmd中输入下面这段代码, 通过全局先将webpack指令安装进电脑中

```
npm install webpack -g
```

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504211715.png)

接下来我们要通过npm命令安装nrm了  :

下面运行npm i nrm -g时可能会由于网速问题导致安装报错，可以把命令换成

```
npm install -g nrm --registry=https://registry.npm.taobao.org
```

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504211743.png)

第一步安装nrm

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504211805.png)

第二步：运行nrm ls 查看nrm给我们提供的镜像有哪些

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504211821.png)

可以看到上面有6个镜像，其中cnpm和taobao是咱们国内的，所以下载速度会快很多，所以我们不从国外的原版

```
npm ---- https://registry.npmjs.org/
```

路径上下载。使用nrm use taobao或者nrm use cnpm 命令进行切换即可！

**注意：** nrm只是单纯的提供了几个常用的下载包的URL地址，并能够让我们在这几个地址之间很方便地进行切换，但是我们每次装包的时候，使用的装包工具都是npm(**解释：** 就是说我们每次下载东西的时候，还是使用的npm命令，但是该命令会从nrm指定的地址去下载而已)



## 三、安装webpack

webpack安装的两种方式：

1.运行npm i webpack -g 全局安装webpack，这样就能在全局使用webpack的命令

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504211846.png)

2.在项目根目录中运行npm i webpack --save-dev 安装到项目依赖中



全局安装webpack-cli,相当于是webpack的客户端，一定要确保webpack-cli和webpack版本之间能够互相兼容

```
//因为上面的webpack是全局安装的,因此这里我们安装weback-cli也是需要全局安装的!
npm install --save-dev webpack-cli -g
```



##  四、webpack的使用

我们安装完了webpack之后，就要使用webpack了 :webpack的作用就是“能够处理JS的兼容问题，把高级的、浏览器不能识别的语法转为低级的，浏览器能够正常识别的语法

举例：我们在hubuilder里面新建一个项目，项目的结构如下： 其中dist目录是用来存放某些文件被webpack编译解析之后的结果（举例：因为有一些ES6语法写的js文件，可能浏览器解析不了，所以需要使用webpack对该js文件进行编译，之后我们使用编译之后的结果即可）

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504211916.png)

我们进入到项目所在位置执行cmd命令，然后执行npm  init -y,  之后会发现项目下多出来一个package.json文件

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504211930.png)

假设我们需要在项目的index.html里面用到jQuery包，那该怎么办呢？我们只需要通过npm工具安装jQuery即可

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504211944.png)

命令执行完之后再刷新项目，会发现多出来了两个文件

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504211956.png)

然后在main.js里写如下代码 

```javascript
//这个main.js是我们项目的JS入口文件

//导入jQuery
//import *** from *** 是ES6中导入模块的方式
import $ from 'jquery'  //这句话的意思是说“从项目中node_modules（如上图所标）中导入jQuery包，然后让“$”来接收
$(function(){
	$('li:odd').css('backgroundColor','lightblue')
	$('li:even').css('backgroundColor','red')
})
```

在index.html中写代码如下：

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<script src="main.js"></script>
	</head>
	<body>
		<ul>
			<li>这是第1个li</li>
			<li>这是第2个li</li>
			<li>这是第3个li</li>
			<li>这是第4个li</li>
			<li>这是第5个li</li>
			<li>这是第6个li</li>
			<li>这是第7个li</li>
			<li>这是第8个li</li>
			<li>这是第9个li</li>
			<li>这是第10个li</li>
		</ul>		
	</body>
</html>
```

最后运行在浏览器端运行index.html时，报语法错误如下，说明浏览器不能解析该main.js，原因是因为main.js中的代码涉及到了ES6的语法，浏览器比较低级，解析不了

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504212054.png)

为了解决这个问题，我们使用webpack这个前端构建工具把main.js做一下处理，生成一个bundle.js文件，然后我们使用这个bundle.js文件即可

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504212108.png)

然后我们把index.html里的

```
<script src="main.js"></script>
```

修改成

```
<script src="../dist/bundle.js"></script>
```

之后再运行就可以出真正的结果了，index.html运行结果截图如下：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504212132.png)



但是如果我们修改了main.js里面的内容，岂不是又要写一遍

```
webpack ./src/main.js  ./dist/bundle.js
```

每次都要在webpack后写源文件路径，编译后的路径，这样太麻烦了，所以我们需要对此进行配置（写进配置文件里面）

首先我们需要在项目的根目录下创建一个webpack.config.js文件

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504212158.png)

代码如下：

```javascript
const path=require('path')
//这个配置文件，其实就是一个JS文件，通过Node中的模块操作，向外暴露了一个配置对象
module.exports={
	//在配置文件中需要手动指定入口和出口
	entry:path.join(__dirname,'./src/main.js'),//入口，表示要使用webpack打包哪个文件
	output:{
		path:path.join(__dirname,'./dist'),//指定打包好的文件，输出到哪个目录中去
		filename:'bundle.js' //这是指定 输出的文件的名称
	}
}
```

这样就OK了，每当我们需要修改main.js后，只需要在项目路径下cmd进入控制台，然后使用webpack命令就可以完成编译打包输出了，不需要在webpack后面写 源文件路径 和输出结果路径了

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504212216.png)

举例，我们把main.js中的”red改成”blue“,然后在项目路径下，使用webpack命令，再去访问index.html时效果如下面第2张图

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504212235.png)

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504212250.png)



**webpack的总结：**当我们在控制台直接输入webpack命令执行的时候，webpack做了以下几步：

1.首先，webpack发现，我们并没有通过命令的形式给他指定入口和出口

2.webpack就会去项目的根目录中去查找一个叫做 “webpack.config.js”的配置文件

3.当找到配置文件后，webpack会去解析执行这个配置文件，当解析执行完配置文件后，就得到了配置文件中导出的配置对象

4.当webpack拿到配置对象后，就拿到了配置对象中，指定的入口和出口，然后进行打包构建

**扩展技能：**我们知道package.json里面的devDependencies下都是本项目所使用的依赖信息，而node_modules文件下是存放本项目下载的所有包（就是根据依赖信息来下载的）

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504212307.png)

一旦node_modules文件下的包出现残缺或者丢失，可以把项目的node_modules文件删除，然后再在项目的根路径下使用cmd进入控制台然后输入npm i 命令重新根据依赖下载包即可
