#  CSS学习总结（2）

##  一、CSS 背景(background)

CSS 可以添加背景颜色和背景图片，以及来进行图片设置。

| background-color                                            | 背景颜色         |
| ----------------------------------------------------------- | ---------------- |
| background-image                                            | 背景图片地址     |
| background-repeat                                           | 是否平铺         |
| background-position                                         | 背景位置         |
| background-attachment                                       | 背景固定还是滚动 |
| 背景的合写（复合属性）                                      |                  |
| background:背景颜色 背景图片地址 背景平铺 背景滚动 背景位置 |                  |

###  背景图片(image)

语法： 

```css
background-image : none | url (url) 
```

参数： 

none : 　无背景图（默认的）
url : 　使用绝对或相对地址指定背景图像 

background-image 属性允许指定一个图片展示在背景中（只有CSS3才可以多背景）可以和 background-color 连用（**解释怎么个连用法：** 如果图片不重复地话，图片覆盖不到地地方都会被背景色填充。） 如果有背景图片平铺，则会覆盖背景颜色。

小技巧：  我们提倡 背景图片后面的地址，url不要加引号。

###   背景平铺（repeat）

语法： 

```css
background-repeat : repeat | no-repeat | repeat-x | repeat-y 
```

参数： 

repeat :    背景图像在纵向和横向上平铺（默认的）设置背景图片时，默认把图片在水平和垂直方向平铺以铺满整                            个元素。

no-repeat : 　背景图像不平铺  

repeat-x : 　背景图像在横向上平铺

repeat-y : 　背景图像在纵向平铺 

#### 参数详解案例：如下

（1）先看看repeat-y : 　背景图像在纵向平铺 ，如下图

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190522201014.png)

（2）设置背景图片时，默认把图片在水平和垂直方向平铺以铺满整个元素，如下图。

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190522201128.png)

（3）no-repeat : 　背景图像不平铺  ，那就只会显示一个图像

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190522203750.png)



### 背景位置(position)

语法： 下面的两种写法都可以，两种写法都可以表示背景图片的位置             注意：position 后面是x坐标和y坐标

```css
background-position : length || length   举例：background-position :60px  50px
或者
background-position : position || position   举例：background-position :center top
```

参数： 

length : 　百分数   或者   由浮点数字和单位标识符组成的长度值。请参阅长度单位 
position : 　top | center | bottom | left | center | right 

说明： 

设置或检索对象的背景图像位置。**必须先指定background-image属性**。默认值为：(0% 0%)。
如果只指定了一个值，该值将用于横坐标。纵坐标将默认为50%。第二个值将用于纵坐标。

注意：

1. position 后面是x坐标和y坐标。 可以使用方位名词或者 精确单位。
2. 如果和精确单位和方位名字混合使用，则必须是x坐标在前，y坐标后面。比如 background-position: 15px top;   则 15px 一定是  x坐标   top是 y坐标。

实际工作用的最多的，就是背景图片居中对齐了。



### 背景附着

语法： 

```css
background-attachment : scroll | fixed 
```

参数： 

scroll : 　背景图像是随对象内容滚动
fixed : 　背景图像固定 

说明： 

设置或检索背景图像是随对象内容滚动还是固定的。 （**白话解释：** 就是当该网页的内容较长的时候，我们会通过滑轮往下滑来浏览全部内容，但是如果background-attachment : fixed，则背景图片不会移动，不管我们的滑轮怎么滑，屏幕的背景图像始终不变）



###  背景简写

因为背景属性值太多，如下表所示，所以我们要来一个背景简写

| background-color                                            | 背景颜色         |
| ----------------------------------------------------------- | ---------------- |
| background-image                                            | 背景图片地址     |
| background-repeat                                           | 是否平铺         |
| background-position                                         | 背景位置         |
| background-attachment                                       | 背景固定还是滚动 |
| 背景的合写（复合属性）                                      |                  |
| background:背景颜色 背景图片地址 背景平铺 背景滚动 背景位置 |                  |

**background属性的值的书写顺序官方并没有强制标准的。为了可读性，建议大家如下写：**

**background:背景颜色 背景图片地址 背景平铺 背景滚动 背景位置**

```css
background: transparent url(image.jpg) repeat-y  scroll 50% 0 ;
```



### 背景透明(CSS3)

CSS3支持背景半透明的写法语法格式是:

```css
background: rgba(0,0,0,0.3);  //前3个参数值代表了背景颜色，最后一个参数是alpha 透明度  取值范围                                   //0~1之间
```

最后一个参数是alpha 透明度  取值范围 0~1之间

 注意：  背景半透明是指盒子背景半透明， 盒子里面的内容不受影响。



## 二、盒子模型（CSS重点）

其实，CSS就三个大模块：  盒子模型 、 浮动 、 定位，其余的都是细节。要求这三部分，无论如何也要学的非常精通。  

所谓盒子模型就是把HTML页面中的元素看作是一个矩形的盒子，也就是一个盛装内容的容器。每个矩形都由元素的内容、内边距（padding）、边框（border）和外边距（margin）组成。

###  盒子边框（border）

边框就是那层皮。  橘子皮。。柚子皮。。橙子皮。。。

语法：      border-width：边框的粗细

```css
border : border-width || border-style || border-color 
```

举例：border: 1px solid blue;

border-style：边框属性—设置边框样式

边框样式用于定义页面中边框的风格，常用属性值如下：

```
none：没有边框即忽略所有边框的宽度（默认值）

solid：边框为单实线(最为常用的)

dashed：边框为虚线  

dotted：边框为点线

double：边框为双实线
```

**dashed  虚线  如下：**

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190523103847.png)

**dotted    点线  如下**

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190523103952.png)

**double  双实线 如下：**

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190523104017.png)

还可以给某条边设置样式：如下

```
        border-top: 1px solid red; /*上边框*/
		border-bottom: 2px solid green;
		border-left: 1px solid blue;
		border-right: 5px solid pink;
```

### 表格的细线边框

我们先看案例：

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
	table {
		width: 500px;
		height: 300px;
		border: 1px solid red;
	}
	td {
		border: 1px solid red;
		text-align: center; //让每个表格里的文字居中
	}
	</style>
</head>
<body>
	<table>
		<tr>
			<td>天王盖地虎</td>
			<td>天王盖地虎</td>
			<td>天王盖地虎</td>
		</tr>
		<tr>
			<td>宝塔镇河妖</td>
			<td>宝塔镇河妖</td>
			<td>宝塔镇河妖</td>
		</tr>
		<tr>
			<td>小鸡炖蘑菇</td>
			<td>小鸡炖蘑菇</td>
			<td>小鸡炖蘑菇</td>
		</tr>
	</table>
</body>
</html>
```

运行效果截图： 发现了一个问题，双重边框问题。原因就在于，table有边框，td也有边框。  别怕，下面继续看解决方案！

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190523105644.png)

修改原html文件代码，如下： 在table标签中加了cellpadding="0" cellspacing="0"这两个属性值，目的就是为了合让table的边框和td的边框之间的空隙为0，往下看运行结果

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
	table {
		width: 500px;
		height: 300px;
		border: 1px solid red;
	}
	td {
		border: 1px solid red;
		text-align: center;
	}
	/*table, td {
		border-collapse: collapse;  /*合并相邻边框*/
	}*/
	</style>
</head>
<body>
	<table cellpadding="0" cellspacing="0">
		<tr>
			<td>天王盖地虎</td>
			<td>天王盖地虎</td>
			<td>天王盖地虎</td>
		</tr>
		<tr>
			<td>宝塔镇河妖</td>
			<td>宝塔镇河妖</td>
			<td>宝塔镇河妖</td>
		</tr>
		<tr>
			<td>小鸡炖蘑菇</td>
			<td>小鸡炖蘑菇</td>
			<td>小鸡炖蘑菇</td>
		</tr>
	</table>
</body>
</html>
```

运行结果截图：发现边框的厚度很厚，原因就在于table和td的边框拼接在一起了，挨在一起，所以厚度肯定是两者厚度之和！  为了让厚度变成我们所期望的，继续往下看解决方法

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190523110033.png)

再次修改代码如下： 在style里添加了一个

table, td {
		border-collapse: collapse;  /*合并相邻边框*/
	}       然后往下看运行效果截图：

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
	table {
		width: 500px;
		height: 300px;
		border: 1px solid red;
	}
	td {
		border: 1px solid red;
		text-align: center;
	}
	table, td {
		border-collapse: collapse;  /*合并相邻边框*/
	}
	</style>
</head>
<body>
	<table cellpadding="0" cellspacing="0">
		<tr>
			<td>天王盖地虎</td>
			<td>天王盖地虎</td>
			<td>天王盖地虎</td>
		</tr>
		<tr>
			<td>宝塔镇河妖</td>
			<td>宝塔镇河妖</td>
			<td>宝塔镇河妖</td>
		</tr>
		<tr>
			<td>小鸡炖蘑菇</td>
			<td>小鸡炖蘑菇</td>
			<td>小鸡炖蘑菇</td>
		</tr>
	</table>
</body>
</html>
```

运行效果截图：终于达到我们的期望了，OK!

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190523110407.png)

###  内边距（padding）

 padding属性用于设置内边距。  是指 边框与内容之间的距离。

![1558582921345](C:\Users\19643\AppData\Roaming\Typora\typora-user-images\1558582921345.png)

padding-top:上内边距

padding-right:右内边距

padding-bottom:下内边距

padding-left:左内边距

注意：  后面跟几个数值表示的意思是不一样的。



| 值的个数 | 表达意思                                                     |
| -------- | ------------------------------------------------------------ |
| 1个值    | padding：上下左右边距 比如padding: 3px; 表示上下左右都是3像素 |
| 2个值    | padding: 上下边距 左右边距 比如 padding: 3px 5px; 表示 上下3像素 左右 5像素 |
| 3个值    | padding：上边距 左右边距 下边距 比如 padding: 3px 5px 10px; 表示 上是3像素 左右是5像素 下是10像素 |
| 4个值    | padding:上内边距 右内边距 下内边距 左内边距 比如: padding: 3px 5px 10px 15px; 表示 上3px 右是5px 下 10px 左15px 顺时针 |



### 外边距（margin）

margin属性用于设置外边距。  设置外边距会在元素之间创建“空白”， 这段空白通常不能放置其他内容。

margin-top:上外边距

margin-right:右外边距

margin-bottom:下外边距

margin-left:上外边距

margin:上外边距 右外边距  下外边距  左外边

取值顺序跟内边距相同。

#### 外边距实现盒子居中

可以让一个盒子实现水平居中，需要满足一下两个条件：

1. 必须是块级元素。     （可以使用加display: inline-block;转换成块级元素）
2. 盒子必须指定了宽度（width）

然后就给**左右的外边距都设置为auto**，就可使块级元素水平居中。

实际工作中常用这种方式进行网页布局，示例代码如下：

```css
.header{ width:960px; margin:0 auto;}   //这里的 margin:0 auto;有两个参数，第一个参数是0，表示上                        //下外边距都为0，第二个参数是auto,表示左右外边距是自动，就可使块级元素水平居中
```

####  文字盒子居中图片和背景区别

1. 文字水平居中是  text-align: center
2. 盒子水平居中  左右margin 改为 auto 

```css
text-align: center; /*  文字居中水平 */
margin: 10px auto;  /* 盒子水平居中  左右margin 改为 auto 就可以了 */
```

#### 清除元素的默认内外边距

为了更方便地控制网页中的元素，制作网页时，可使用如下代码清除元素的默认内外边距： 

```css
* {
   padding:0;         /* 清除内边距 */
   margin:0;          /* 清除外边距 */
}
```

注意：  行内元素是只有左右外边距的，是没有上下外边距的。 内边距，在ie6等低版本浏览器也会有问题。

我们尽量不要给行内元素指定上下的内外边距就好了。

####  外边距合并

使用margin定义块元素的垂直外边距时，可能会出现外边距的合并。



#### 相邻块元素垂直外边距的合并

当上下相邻的两个块元素相遇时，如果上面的元素有下外边距margin-bottom，下面的元素有上外边距margin-top，则他们之间的垂直间距不是margin-bottom与margin-top之和，而是两者中的较大者。这种现象被称为相邻块元素垂直外边距的合并（也称外边距塌陷）。如下图所示;

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190523131128.png)

#### 嵌套块元素垂直外边距的合并

对于两个嵌套关系的块元素，如果父元素没有上内边距及边框，则父元素的上外边距会与子元素的上外边距发生合并，合并后的外边距为两者中的较大者，即使父元素的上外边距为0，也会发生合并。

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190523131427.png)

解决方案：  

1. 可以为父元素定义1像素的上边框或上内边距。

2. 可以为父元素添加overflow:hidden。                                              

   **如下图所示：**

   ![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190523132003.png)

#### content宽度和高度

使用宽度属性width和高度属性height可以对盒子的大小进行控制。

width和height的属性值可以为不同单位的数值或相对于父元素的百分比%，实际工作中最常用的是像素值。

大多数浏览器，如Firefox、IE6及以上版本都采用了W3C规范，符合CSS规范的盒子模型的总宽度和总高度的计算原则是：

```
  /*外盒尺寸计算（元素空间尺寸）*/
  Element空间高度 = content height + padding + border + margin
  Element 空间宽度 = content width + padding + border + margin
  /*内盒尺寸计算（元素实际大小）*/
  Element Height = content height + padding + border （Height为内容高度）
  Element Width = content width + padding + border （Width为内容宽度）
```

注意：

1、宽度属性width和高度属性height仅适用于块级元素，对行内元素无效（ img 标签和 input除外）。

2、计算盒子模型的总高度时，还应考虑上下两个盒子垂直外边距合并的情况。

3、如果子元素没有知道宽度或高度，那么它将默认和父元素一样宽，一样高

#### 盒子模型布局稳定性

开始学习盒子模型，同学们最大的困惑就是， 分不清内外边距的使用，什么情况下使用内边距，什么情况下使用外边距？

答案是：  其实他们大部分情况下是可以混用的。  就是说，你用内边距也可以，用外边距也可以。 你觉得哪个方便，就用哪个。

但是，总有一个最好用的吧，我们根据稳定性来分，建议如下：

按照 优先使用  宽度 （width）  其次 使用内边距（padding）    再次  外边距（margin）。   

```
  width >  padding  >   margin   
```

原因：

1. margin 会有外边距合并 还有 ie6下面margin 加倍的bug（讨厌）所以最后使用。
2. padding  会影响盒子大小， 需要进行加减计算（麻烦） 其次使用。
3. width   没有问题（嗨皮）我们经常使用宽度剩余法 高度剩余法来做。

###  圆角边框(CSS3)

从此以后，我们的世界不只有矩形。radius 半径（距离）

语法格式：

```css
border-radius: 左上角  右上角  右下角  左下角;
```

圆角边框图如下所示：当div盒子的weight和height相等，且border-radius:50%时，就会变成圆

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190523144002.png)

### 盒子阴影

如下图：该盒子在水平方向和垂直方向都有影子

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190523145634.png)

语法格式：

```css
box-shadow:水平阴影长度 垂直阴影长度 阴影的模糊距离 阴影尺寸大小 阴影颜色  内/外阴影；
```

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190523145355.png)

