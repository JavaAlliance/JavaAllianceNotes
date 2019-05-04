#  Vue.js学习总结第七天

##  一、使用vue实例的render方法渲染组件

在讲render方法之前，先引入一个例子 ,如下面代码部分：

```html
<html>
    <head>
        <meta charset='utf-8'>
        <title></title>
        <!-- 引入vue.js -->
        <script src='https://cdn.jsdelivr.net/npm/vue/dist/vue.js'></script>
    </head>
    <body>
        <div id='app'>
        	<p>123456</p>
           <login></login>
        </div>    
    </body>
    <script>
        var login={
        	template:'<h1>这是登录组件</h1>'
        }
        //创建Vue实例
        var vm=new Vue({
        	el:"#app",
        	data:{},
        	methods:{},
        	components:{
        		login
        	}
        })
    </script>
</html>
```

运行结果截图：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504205732.png)

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504205749.png)

以上是使用components的效果，再div里面使用<login引用组件，不会对div标签有影响（没有让div标签消失）

再看一下render方法的使用代码, 在vue实例里面写render属性

```html
<html>
    <head>
        <meta charset='utf-8'>
        <title></title>
        <!-- 引入vue.js -->
        <script src='https://cdn.jsdelivr.net/npm/vue/dist/vue.js'></script>
    </head>
    <body>
        <div id='app'>              
           <p>123456</p>
        </div>    
    </body>
    <script>
        var login={
        	template:'<h1>这是登录组件</h1>'
        }
        //创建Vue实例
        var vm=new Vue({
        	el:"#app",
        	data:{},
        	methods:{},  //下面的function(createElements)中createElements是形参，它是一个方法，调用该方法可以把传入的组件模板（本例为login组件）渲染为html，然后替换本vue实例中el所代表的div元素，
            render:function(createElements){
          	return createElements(login)
          }
        })
    </script>
</html>
```

解释：在本例中render方法的作用就是说把<div id='app'这个模块连带着div标签删掉，然后再把login组件放到<div id='app'的位置，相当于是进行了替换，

运行效果截图： 可以看到虽然在div标签里面写了"" <p123456</p"" 但是最终没有显示，从下面第2张图中可以看到，div标签已经消失了，说明了login组件完全代替div标签里的内容（包括div标签在内一起被删除了），整体替换了

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504205810.png)

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504205823.png)

##   二、在webpack中导入Vue包

我们在普通html页面导入时很简单，直接写下面一行代码完事了

```
 <script src='https://cdn.jsdelivr.net/npm/vue/dist/vue.js'></script>
```

但是如果在webpack中使用Vue.js，按如下步骤即可，

**第一步：** 在项目根目录下cmd控制台执行npm i vue -s 来安装vue的包

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504205850.png)

**第二步：** 在main.js中使用import  Vue from 'vue'从node_modules文件夹（该文件夹是使用webpack工具之后生成的）中导入安装的vue包

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504205905.png)

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504205927.png)

但是这样是有问题的，引入的vue包不是完整的，而是vue包的一部分，我们可以按上图中说的去找node_modules包下的vue文件夹，然后找vue文件夹下的package.json配置文件，然后找到main属性，发现main属性是dist/vue.runtime.common.js，意思就是说“当我们使用import  Vue from 'vue'的时候，实质上是导入的/node_modules/vue/dist/vue.runtime.common.js”

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504205946.png)

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504210002.png)

 所以我们的解决办法很简单，第一种是 直接在mian.js里导入具体文件

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504210019.png)

 第二种是在webpack.config.js里添加resolve节点进行如下配置

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504210036.png)

然后在main.js里面不做任何修改，直接按原来的import Vue form 'vue'写就好

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504210050.png)

记得修改完以后要重新启动项目



##  三、webpack中vue-loader的使用

我们一般把components组件代码写在html页面里面，但是如果我们想把这些components组件代码抽取出来单独形成一个文件，文件的后缀名是以.vue结尾的，比如下图，以login.vue为例，以.vue结尾的文件里面只能有三部分，分别是<template,<script,<style，这三部分里面都是写跟components有关的配置，共同组成了一个完整的组件component模块(**解释：**我们这里的一个以.vue结尾的文件，就相当于我们平时经常在new vue对象时在vue对象里面写component属性绑定的一个组件)
 ![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504210113.png)


然后在main.js中通过下面代码引入该文件

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504210130.png)

默认webpack无法直接打包.vue文件，需要安装相应的loader：到项目根路径下进入cmd控制台然后输入下面代码

```
npm i vue-loader vue-template-compiler -D
```

然后在webpack.config.js中，在module节点下的rules数组中，添加一个新的匹配规则，匹配.vue结尾的文件

```
{test:/\.vue$/,use:'vue-loader'}
```

但是就这样去配置了以后，重启webpack,运行之后还是报错

这个时候我们只能使用另一种方法了，使用render函数

先在main.js里面导入我们写的login.vue文件，然后写vue实例对象，并在实例对象里面写render方法，然后在index.html里面导入bundle.js(mian.js被webpack打包后的文件)，之后启动webpack运行访问index.html就可以看到运行结果正是我们期望的，      

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504210155.png)

总结一点：在webpack中，如果想要通过vue，把一个组件放到页面中去展示，可以通过vue中的render方法去实现

梳理：webpack中如何使用vue

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504210214.png)



##  四、export default和export的使用方式

export-default和export都是向外暴露的作用

先说export-default吧，看下例子，我们新建一个test.js文件，然后在文件里面定义info对象，然后使用export-default info来暴露该对象

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504210231.png)

这个时候我们在main.js中使用import导入该test.js文件，用m222变量接收test.js中export default暴露的对象

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504210247.png)

然后使用webpack打包部署运行，main.js中的console.log(m222)能够在浏览器端打印出来，如下图所示：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504210259.png)

**注意：**不能在一个test.js中写多个export default 暴露多个对象，只运行写一次export default 



虽然我们不能写多个export default ，但是我们可以写多个export呀，如下图，暴露多个变量

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504210316.png)

然后在main.js里面做接收

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504210331.png)

运行结果： 可以看到main.js里面的两行console.log()都能够正常地打印出来了

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504210343.png)



##  五、在webpack中使用vue-router

首先在项目的根目录下cmd进入控制台，然后执行命令

```
npm i vue-router -s
```

安装vue-router包到node-modules文件夹下

然后在main.js中头部写下面3行代码，从node-modules文件夹中导入Vue包和VueRouter包，然后把Vue和VueRouter关联起来

 ![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504210401.png)

然后就可以直接使用vue-router路由了



## 六、以.vue结尾的文件中的style标签中的lang属性和scoped属性

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504210418.png)
