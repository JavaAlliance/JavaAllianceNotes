**1.插值表达式写法**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190416162233547.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
**2.vue.js里的“插值表达式”的闪烁问题的解决**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190416162638453.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
效果图:第一眼是下面这样
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190416162719266.png)
然后转换成
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190416162816921.png)
以上表述的就是“闪烁问题”。   接下来说一下插值表达式的“闪烁问题”的解决方案

第一种方案：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190416163713212.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)

第二种方案：  直接使用v-text指令，这个指令的作用就是说“如果msg还没有解析到，那就不显示，等到msg解析到了才显示在浏览器上”
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190416164121956.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
但是v-cloak指令和v-text指令的作用是不是完全一样没有什么区别呢？当然不是，看下图中讲解它们的区别
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190416164549858.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
当msg里面含有<h1这种标签时，会怎么样呢？ 看下面2张图即可
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190416165339593.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190416165511762.png)
但是如果我要把上面msg里面的<h1解析成标签，而不是普通文本，该怎么办呢？   用v-html标签即可，见下面2张图即可
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190416170149970.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190416165939147.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
**3.v-bind指令的使用** 当然了v-bind指令也可以不写，直接写成：也可以，如下图所示，效果见下面第2张图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190416171411180.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190416171642279.png)

**4.v-on指令的用法**  v-on指令也有缩写，@，效果是一样的![在这里插入图片描述](https://img-blog.csdnimg.cn/2019041617380277.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190416173240377.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
效果图如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190416173346549.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
**5.制作“跑马灯”效果（类似于放广告的电子板文字不断的向一个方向移动）**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190416180422799.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190416180648190.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
上图还有一种写法，解决this问题，通过=>说明里面的this是和setInterval()函数外面的this一致
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190416180853332.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
效果图如下：下面的文字不断地向左移动
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190416180718467.png)
上面讲述了如何开启“跑马灯”效果，但是有一个问题就是多点几次“浪起来”按钮的话，就相当于设置了多个叠加的定时器，这样一来，跑马灯的速度就会变快，所以如何解决这个问题以及如何停止“跑马灯”效果呢？接下来我们一起学习如下

①先给“低调”按钮绑定一个stop方法
![在这里插入图片描述](https://img-blog.csdnimg.cn/201904161824264.png)
②在data里面添加一个intervalId属性变量，然后让setInterval()方法的返回值（定时器的id）赋给该属性变量
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190416183219515.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
停止定时器的代码如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190416183440565.png)

**6.常见事件修饰符介绍**
   ①.stop修饰符（阻止冒泡：见下图）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417091443468.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
要想解决这个问题，外面可以使用.stop修饰符来阻止冒泡
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417091611501.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
②.prevent修饰符（阻止默认行为）
举例：下图中的a链接里面有href，点击a链接会默认跳转到href指定的地址，而我们又给这个a链接添加了click事件，所以如果我们想取消a链接的默认跳转机制，就需要使用.prevent修饰符
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417100303206.png)
③.capture修饰符（捕获事件）
举例如下：当我们点击div里面的input按钮时，先执行外层的div的点击事件，然后再执行里层的input按钮的点击事件（**从外到里**）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417103318730.png)
④.self修饰符（作用就是当我们点击div里的input按钮时不会触发div的点击事件，只有当我们点击div本身时才触发div的点击事件，这才是self的含义）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417103837258.png)
**7、v-model指令的使用**
可以实现数据的双向绑定，就是说修改input框里的内容时，vue对象的data.msg值也跟着变，而v-bind却没有双向绑定的功能
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019041711050368.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
**8、vue中通过属性绑定为元素设置class**
先写css如下
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417130601996.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417131726560.png)
或者是按下面这样写也可以
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417132223421.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
**9.v-for指令的四种使用方式**
①v-for指令循环普通数组
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417133653392.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
运行效果图如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417133720596.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
②v-for指令遍历对象数组
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417134101629.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
运行效果图如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417134120213.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
③v-for指令遍历对象
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417135618722.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
运行效果截图如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417135805673.png)
④v-for指令遍历数字
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417140228590.png)
运行效果截图如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417140412387.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
**9、v-for中key的使用**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417143330742.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
如果不用上述的key，就会出现下面这样的问题
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417143513297.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417143621161.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
上面的添加是在list的头部添加的，代码如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417143831217.png)
如果我们要在list的尾部进行添加的话，代码如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019041714373322.png)
**10、v-if和v-show的使用及特点**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417145239919.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417145420808.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)











