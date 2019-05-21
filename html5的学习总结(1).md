#    HTML5的学习总结(1)

##  一、什么是HTML5？

1.支持Html5的浏览器包括[Firefox](http://baike.baidu.com/view/3279.htm)（火狐浏览器），[IE9](http://baike.baidu.com/view/2298486.htm)及其更高版本，[Chrome](http://baike.baidu.com/view/1835504.htm)（谷歌浏览器），[Safari](http://baike.baidu.com/view/110484.htm)，Opera等；国内的傲游浏览器（Maxthon），以及基于IE或[Chromium](http://baike.baidu.com/view/404073.htm)（Chrome的工程版或称实验版）所推出的[360浏览器](http://baike.baidu.com/view/1949679.htm)、[搜狗浏览器](http://baike.baidu.com/view/2083809.htm)、[QQ浏览器](http://baike.baidu.com/view/2610930.htm)、[猎豹浏览器](http://baike.baidu.com/view/8467425.htm)等国产浏览器同样具备支持HTML5的能力

2.HTML5的设计目的是为了在移动设备上支持多媒体。新的语法特征被引进以支持这一点，如video、audio和canvas 标记。HTML5还引进了新的功能，可以真正改变用户与文档的交互方式

###  （1） 回顾一下以前学过的知识：

####  1.1 块级元素：

​           块级元素独占一行，从页面布局和外观显示来看，块级元素所处行的后面无法再放任何的内容，（**也就是说一行里面最多只可能存在一个“块级元素”**）
一个网页的布局就类似于一篇报纸的排版。首先将网页分成大的模块。然后在模块里面再分成小的模块。所以块级元素多用于布局。

#### 1.2 行级元素：

只占一小块空间，在后面可以继续放内容行级元素也称为行内标签，内联标签（**也就是说一行里面可以存在多个行级元素**），使用块级元素将网页结构搭建好了后，使用行级元素来排版。

#### 1.3 行级元素和块级元素的区别：

**块级元素独占一行，比较霸道。而行级元素却可以多个元素共享一行。**
块级元素支持高和宽，行级不支持高和宽。行级元素的高和宽由内容来决定。
块级元素常常作为容器，可以容纳其他的行级元素和块级元素。行级元素一般用来组织内容。即容纳文字、图片、超链接。

#### 1.4 块级元素有哪些？

div、p、h、hr、table 、ul、ol、form...

#### 1.5 行级元素有哪些？

a、span、label、input、textarea、br、img...

###  怎么将行级元素转换为块级元素呢？

**解答：**下面的main标签是行级元素（**也许你会有疑问：既然main是行级元素，那为什么下面的代码里面还写width，height属性呢？那是因为里面加了display:block,已经变成块级元素了，所以可以加宽高属性**），只需要在里面加上display: block;即可转换成块级元素

```html
main{
           /*将行级元素转换为块级元素*/
            display: block;
            width: 100%;
            height: 500px;
            background-color: #ccc;
        }
```

### （2）如何处理兼容性问题呢？

html5shiv.min.js:在默认情况下，IE8及以下的IE版本不支持HTML5,引入这个文件就可以做到兼容



## 二、HTML新增input标签的 type类型

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<form action="">
    用户名：<input type="text" name="userName"> <br>
    密码：<input type="password" name="userPwd"> <br>
    <!--email提供了默认的电子邮箱的完整验证：要求必须包含@符号，同时必须包含服务器名称，如果不能满足验证，则会阻止当前的数据提交-->
    邮箱：<input type="email"> <br>
    <!--tel它并不是来实现验证。它的本质目的是为了能够在移动端打开数字键盘。意味着数字键盘限制了用户只能输入数字。  如何查看效果：qq发送文件>>手机端使用qq来接收>>使用手机浏览器查看-->
    电话：<input type="tel"> <br>
    <!--验证只能输入合法的网址：必须包含http://，否则不通过-->
    网址：<input type="url"> <br>
    <!--number：只能输入数字(包含小数点)，输入不了其他的字符
    max:最大值
    min:最小值
    value:默认值-->
    数量：<input type="number" value="60" max="100" min="0"> <br>
    <!--search：可以提供更人性化的输入体验-->
    请输入商品名称：<input type="search"> <br>
    <!--range:范围-->
    范围：<input type="range" max="100" min="0" value="50"> <br>
    颜色：<input type="color"> <br>
    <!--日期时间相关-->
    <!--time:时间：时分秒-->
    时间：<input type="time"> <br>
    <!--date：日期：年月日-->
    日期：<input type="date"> <br>
    <!--datetime:大多数浏览器不能支持datetime,所以一般用下面datetime-local那个.用于屏幕阅读器-->
    日期时间：<input type="datetime"><br>
    <!--datetime-local:日期和时间-->
    日期时间：<input type="datetime-local"> <br>
    月份：<input type="month"> <br>
    星期：<input type="week">
    <!--提交-->
    <input type="submit">
</form>
</body>
</html>
```

看运行效果截图如下

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190521152743.png)



##   三、HTML5表单新增的其他属性

 属性：placeholder，autofocus，autocomplete，required，pattern，multiple，form，具体应用如下

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<form action="" id="myForm">
    <!--placeholder：提示文本，提示占位-->
    <!--autofocus:自动获取焦点-->
    <!--autocomplete:自动完成：on:打开  off:关闭
    1.必须成功提交过:提交过才会记录
    2.当前添加autocomplete的元素必须有name属性-->
    用户名：<input type="text" name="userName" placeholder="请输入用户名" autofocus autocomplete="on"> <br>
    <!--tel并不会实现验证，仅仅是在移动端能够弹出数字键盘-->
    <!--required:必须输入，如果没有输入则会阻止当前数据提交-->
    <!--pattern:正则表达式验证
    *:代表任意个
    ?:代表0个或1个
    +：代表1个或多个-->
    手机号：<input type="tel" name="userPhone" required pattern="^(\+86)?1\d{10}$"> <br>
    <!--multiple：可以选择多个文件-->
    文件：<input type="file" name="photo" multiple> <br>
    <!--email:有默认验证  在email中，multiple允许输入多个邮箱地址，以逗号分隔-->
    邮箱：<input type="email" name="email" multiple><br>   
    <!--提交：-->
    <input type="submit"> <br>
</form>
<!--form:指定表单id,那么在将来指定id号的表单进行数据提交的时候，也会将当前表单元素的数据一起提交-->
地址：<input type="text" name="address" form="myForm">
</body>
</html>
```

对上面autocomplete属性的解释：就相当于有如下图所示的提示功能

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190521162408.png)

整个网页的显示如下：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190521162616.png)



##  四、表单新增元素dataList的讲解

<datalist标签不仅可以选择，还应该可以输入，类似于下图所示：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190521164505.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<form action="">
    <!--专业：
    <select name="" id="">
        <option value="1">前端与移动开发</option>
        <option value="2">java</option>
        <option value="3">javascript</option>
        <option value="4">c++</option>
    </select>-->
    <!--不仅可以选择，还应该可以输入-->
    <!--建立输入框与datalist的关联  list="datalist的id号"-->
    专业：<input type="text" list="subjects"> <br>
    <!--通过datalist创建选择列表-->
    <datalist id="subjects">
        <!--创建选项值：value:具体的值 label:提示信息，辅助值-->
        <!--option可以是单标签也可以是双标签-->
        <option value="英语" label="不会"/>
        <option value="前端与移动开发" label="前景非常好"></option>
        <option value="java" label="使用人数多"></option>
        <option value="javascript" label="做特效"></option>
        <option value="c" label="不知道"></option>
    </datalist>
    
    网址：<input type="url" list="urls">
    <datalist id="urls">
        <!--如果input输入框的type类型是url,那么value值必须添加http://-->
        <option value="http://www.baidu.com" label="百度"></option>
        <option value="http://www.sohu.com"></option>
        <option value="http://www.163.com"></option>
    </datalist>
</form>
</body>
</html>
```

运行结果截图如下：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190521164541.png)



##  五、HTML5新增的表单事件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<form action="">     
    用户名：<input type="text" name="userName" id="userName"><br>   //type="tel"，以pattern                                                                //上文第四点已 经讲过              电话：<input type="tel" name="userPhone" id="userPhone" pattern="^1\d{10}$"> <br>
    <input type="submit">
</form>
<script>
    /*1.oninput:监听当前指定元素内容的改变：只要内容改变(添加内容，删除内容)，就会触发这个事件*/
    document.getElementById("userName").oninput=function(){
        console.log("oninput:"+this.value);
    }

    /*onkeyup:键盘弹起的时候触发：每一个键的弹起都会触发一次*/
    document.getElementById("userName").onkeyup=function(){
        console.log("onkeyup:"+this.value);
    }

    /*oninvalid:当验证不通过时触发*/
    document.getElementById("userPhone").oninvalid=function(){
        /*设置默认的提示信息*/
        this.setCustomValidity("请输入合法的11位手机号");
   }
</script>
</body>
</html>
```

运行效果截图如下：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190521194330.png)

**总结：** oninput是，只要内容变了就会触发。 而onkeyup只当键盘弹起才触发



##   六、HTML5的进度条

 <progress标签的作用就是进度条     ，不过感觉meter度量器好像和progress进度条没什么区别。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<!--max:最大值
value:当前进度值-->
<progress max="100" value="50"></progress>
<!--度量器：衡量当前进度值-->
    
<!--high:规定的较高的值
low:规定的较低的值
max:最大值
min:最小值
value:当前度量值-->
<meter max="100" min="0" high="80" low="40" value="30"></meter>
<meter max="100" min="0" high="80" low="40" value="60"></meter>
<meter max="100" min="0" high="80" low="40" value="100" name="level"></meter>
</body>
</html>
```

运行效果截图：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190521195807.png)
