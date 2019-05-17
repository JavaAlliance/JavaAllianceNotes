#    webpack使用教程（2）

##    一、webpack中url-loader的使用

举例：如下图在index.scss中添加background:url（）,然后在main.js中引入index.scss文件，如果直接使用webpack进行打包编译mian.js会报错，因为如果webpack要处理 非JS类型的文件，我们需要手动安装一些合适的第三方loader加载器

 ![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504212714.png)

  在这里我们安装url-loader 和file-loader 加载器

 ![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504212817.png)

然后在webpack.config.js文件的module里写匹配图片后缀名的规则吗，下图中说的base64的作用是把“图片转换成超级超级长的字符串，如下面第2张图”，

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504212843.png)

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504213012.png)

而如果出现一种情况：就是一个项目下有两个不同的存图片的文件夹，文件名一模一样，如下图所示

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504213035.png)

最后在main.js中引入之后，用webpack-dev-server打包部署，就有可能会产生同名覆盖（因为webpack-dev-server把图片打包部署之后的路径都是项目根目录，所以就会产生同名覆盖），为了避免这个问题，我们可以在webpack.config.js里module中的匹配图片的loader后面 name前加上一个[hash:8]，每张图片的hash值都不一样，所以我们可以取hash值前8位拼接原文件名以及原文件后缀名，这样的话就不会有重复的情况发生了

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504213059.png)

扩展一下字体文件也可以使用url-loader来加载，字体文件的匹配规则如下，以.ttf,.eof.svg.woff.woff2结尾的文件都是字体文件

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504213112.png)



## 二、webpack中的babel配置

在讲babel配置之前，先看下图所示的问题，在main.js中用到了ES6的新语法，结果直接运行使用时报错了！

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504213127.png)

在webpack中，默认只能处理一部分ES6的新语法，不能处理一些更高级的ES6语法或者ES7语法；这时候就需要借助于第三方的loader，来帮助webpack处理这些高级的语法，当第三方loader把高级语法转为低级的语法之后，会把结果交给webpack去打包到bundle.js中

   通过Babel可以帮我们将高级的语法转换为低级的语法

**使用步骤如下：**

①在webpack中，可以运行如下两套命令，安装两套包，去安装 Babel相关的loader功能

1.第一套包：npm i babel-core  babel-loader  babel-plugin-transform-runtime -D    //这是安装转换器（从高级语法向低级语法转换）

2第二套包：npm i babel-preset-env babel-preset-stage-0 -D    //这是安装转换的规则（根据什么对应关系从高级语法到低级语法的转换），假设英语是高级语言，汉语是低级语言，英语中的单词要翻译成汉语，需要有翻译对应关系，什么单词对应什么汉语



②打开webpack的配置文件webpack.config.js，在module节点下的rules数组中，添加一个新的匹配规则，匹配所有以.js结尾的文件，然后使用babel-loader加载器处理（之所以要用babel-loader加载器处理的原因是因为有可能以.js结尾的文件里面写了一些ES6更高级的语法或者是ES7高级语法）

 ![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504213147.png)

注意：在配置babel的loader规则的时候，必须把node_modules目录，通过exclude选项排除掉：原因有以下两点：

​       第一点 : 如果不排除node_modules,则Babel会把node_modules中所有的第三方 JS 文件都打包翻译，这样会非常消耗CPU，同时打包速度非常慢

​       第二点：哪怕最终Babel把所有node_modules中的 JS 转换完毕了，但是，项目也无法正常运行！



③ 在项目的根目录中，新建一个叫做 .babelrc的Babel配置文件，这个配置文件属于JSON格式，所以在写.babelrc配置的时候，必须符合JSON语法规范，不能写注释（JSON文件里是不能写注释的，否则运行容易出问题），字符串必须用双引号



④在项目的根目录下新建一个.babelrc的文件（必须符合JSON语法规范），然后在里面写下面的配置，preset对应的是“语法”的包，plugins对应的是插件的包，这些根据上面使用步骤①npm安装的包来写，

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504213223.png)



到此babel就配置成功了，现在再来启动运行webpack的时候就没有问题了，看下图就可以console.log(Person.info)不报错了

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190504213244.png)
