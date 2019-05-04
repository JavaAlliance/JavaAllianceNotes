#   webpack-dev-server使用方法

首先来回顾以下webpack的内容

首先，我们来看看基本的webpack.config.js的写法

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

配置文件提供一个入口和一个出口，webpack根据这个来进行js的打包和编译工作。虽然webpack提供了webpack –watch的命令来动态监听文件的改变并实时打包，输出新bundle.js文件，这样文件多了之后打包速度会很慢，此外这样的打包的方式不能做到hot replace，即每次webpack编译之后，你还需要手动刷新浏览器。

webpack-dev-server其中部分功能就能克服上面的2个问题。

原始文件作出改动后，webpack-dev-server会实时的编译，但是最后的编译的文件并没有输出到目标文件夹，即上面配置的:

```javascript
output:{
		path:path.join(__dirname,'./dist'),//指定打包好的文件，输出到哪个目录中去
		filename:'bundle.js' //这是指定 输出的文件的名称
	}
```

**注意：你启动webpack-dev-server后，你在目标文件夹中是看不到编译后的文件的,实时编译后的文件都保存到了内存当中。因此很多同学使用webpack-dev-server进行开发的时候都看不到编译后的文件** 



## 一、安装webpack_dev_server

webpack_dev_server的作用其实就是**“热部署”**

注意：一定要在本项目中安装webpack才行（不加-g，在项目根目录出现node_modules文件夹，内含webpack及其依赖包）

```
npm install webpack
```

在本地项目中安装完webpack后，再在终端中进入项目目录下，敲下

```
npm install webpack-dev-server --save-dev
```

回车来安装webpack_dev_server,

之后再启动webpack_dev_server

```
.\node_modules\.bin\webpack-dev-server
```

或者是npm run dev命令也行，为了避免出现webpack和webpack-dev-server之间的版本兼容性问题，这里举例按下图配置版本是没有问题的

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504213818.png)

命令行执行成功了

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504213840.png)

到此webpack_dev_server热部署已经启动了，部署的服务器地址在localhost:8080/上，项目也就部署到该地址上了

以本例来说：main.js做以下修改，然后ctrl+s保存，会看到cmd控制台会再次编译成功，截图见下面第2张图

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504213939.png)

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504213956.png)

然后我们再次访问src下的index.html发现颜色还是red，没有变成blue。继续看下面是如何解决此问题！

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504214013.png)

我们再去看index.html文件，发现引入的js文件是本项目的dist/bundle.js是磁盘上确确实实存在的文件，而我们webpack_dev_server的帮我们重新打包生成的bundle.js文件并没有存放到实际的物理磁盘上（因为是重新编译打包，所以是覆盖原文件bundle.js）；而是直接存放在电脑的内存中，所以我们这里的问题很简单解决，直接把src="../dist/bundle.js"改成src="/bundle.js"即可，src="/bundle.js"中的bundle.js不是从本项目导入的，而是从电脑内存导入的(**解释：因为是webpack_dev_server部署服务器(使用内存空间)地址在localhost:8080,所以可以直接使用"/"来访问，所以“/bundle.js”是从内存导入的**)，

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504214039.png)

修改成src="/bundle.js"之后，就没有问题了，每次我们修改main.js里的代码，只需要ctrl+s保存，然后webpack_dev_server自动编译（热部署），浏览器端就会即时展示新的应用效果

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504214057.png)



## 二、webpack_dev_server的常用命令参数

我们可以自定义设置webpack_dev_server启动的一些参数，而不采用它默认的

**第一种配置方式**：

下面的--hot参数是“热加载”的意思，就是说我们每次修改要编译的源文件（对修改css文件可以，但是对js文件好像不行）然后ctrl+s保存后，浏览器不重新刷新，而是直接异步加载

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504214113.png)



**第二种配置方式：**

把原来在package.json中dev:后面的参数部分给删掉，只留下“webpack-dev-server”部分

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504214135.png)

再修改webpack.config.js，添加deServer:属性和plugins属性，按下图配置，同时要在webpack.config.js开头添加const webpack=require('webpack')即可

  ![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504214152.png)


 ![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504214210.png)



## 三、html-webpack-plugin的两个基本作用

因为我们的index.html是存放在磁盘上的，我们如果想把index.html页面也存放到内存中（也用webpack-dev-server部署）去该怎么办呢？这个时候就需要用到html-webpack-plugin插件了

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504214228.png)

在plugins属性中写new htmlWebpackPlugin()对象，然后指定原文件，和要在内存中生成的目标页面的名字即可，这样OK了
![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504214244.png)



## 四、loader---配置处理css样式表的第三方loader

如果我们直接在main.js里面引入css文件然后用webpack-dev-server打包部署的话，会报错，所以我们需要解决这个问题、    注意：webpack默认只能打包处理 JS类型的文件，无法处理其他的非JS类型的文件

解决问题，

**第一步：**先在项目的cmd控制台下执行

```
npm i style-loader css-loader -D
```

来安装css加载器

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504214316.png)

具体思路见下图：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504214332.png)



**第二步:** 在webpack.config.js里面新增一个module节点，然后进行下图配置

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504214350.png)

到此为止经过配置以后，webpack-dev-server就可以对css文件打包部署了

**总结：** webpack处理第三方文件类型的过程

1.发现这个要处理的文件不是JS文件，然后就去配置文件中，查找有没有对应的第三方loader规则

2.如果能够找到对应的规则，就会调用对应的loader处理这种文件类型

3.在调用loader的时候，是从后往前调用的，比如本例中就是先调用css-loader，再调用style-loader

4.当最后的一个loader调用完毕，会把处理的结果直接交给webpack进行打包合并，最终输出到bundle.js中去



## 五、loader---配置处理less样式表的第三方loader

比如再main.js中引入less文件如下，webpack肯定不能直接对此编译和打包部署，所以需要我们在本地项目中安装less-loader，

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504214408.png)

在项目的cmd控制台中输入npm i less-loader -D安装less-loader

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504214425.png)

然后再在项目的cmd控制台中输入npm i less -D安装less

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504214441.png)

然后再在webpack.config.js中的module里配置以.less为结尾的文件的规则，loader的执行规则是从右到左，所以下面配置写的是最右边是less-loader,然后处理完交给css-loader，再然后把处理结果交给style-loader，最后把最终结果交给webpack打包部署。 可以看到loader配置是从右到左的

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504214512.png)



##  六、loader---配置处理sass样式表的第三方loader

跟上面一样的套路做法，如果再main.js中引入.scss文件

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504214527.png)

解决方法：

①在项目的cmd控制台中输入npm i sass-loader -D安装sass-loader

②然后再输入npm i node-sass -D安装node-sass

③然后在webpack.config.js中的module里配置以.scss为结尾的文件的规则即可

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504214545.png)
