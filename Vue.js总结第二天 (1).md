**1.品牌案例---完成品牌列表的添加功能**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417165032239.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
代码如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417165221582.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417164816983.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
下面是添加add的代码  （注意push()是从尾部插入的）
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019041716495819.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
再来看删除del()方法
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417165945879.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
**2.根据关键字实现对数组的过滤**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417183220382.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417183056730.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
**3、Vue中全局过滤器的基本使用**（所谓全局的意思就是说“每个vue对象都可以使用（解释：假设每个vue对象绑定一个div框），那么全局的意思就是说每个div里面都可以使用到该全局过滤器”），下文第4点我们会讲“**私有过滤器**”
①过滤器只有一个参数时
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417184925328.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417185048906.png)
②接下来看过滤器中有两个参数的情况
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418093318406.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
运行效果截图如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418093611396.png)
③一个对象调用多个过滤器的情况
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418093958634.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418094029874.png)
运行效果截图如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418094338806.png)
案例：写一个时间过滤器
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418104251978.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019041810442415.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
解释：下面的data.getMonth()为什么要加1，是因为月份是从0开始算的
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418104223759.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
运行效果截图如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019041810450410.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
但是上面的代码有一些问题，如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418110649109.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
解决方案如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418110901862.png)
代码修改成如下
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418111024454.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
这样修改后就没有问题了


**4、定义私有过滤器**（在vue对象的filters属性里面写）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418110013724.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
**5、自定义按键修饰符**
vue官方提供了下列按键
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418112328322.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
可以看到vue没有提供其他的按键了，如果我们想要使用其他按键该怎么办呢？
比如下面的f2按键，就算写了@keyup.f2=“add”，当我们按下f2时，也不能执行我们自定义的add方法
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418112441209.png)
这个时候我们可以这样做：百度查找js里面按键对应的键码，然后以@keyup.键码  的形式使用，比如@key.113=“add”
 还有一种方法就是我们可以自己来配置按键
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/201904181129599.png)
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418113024523.png)
 这样配置了以后，按键f2就能够起作用了
 
 **6、自定义全局指令**
 举例：以自定义v-focus指令为例
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418152742434.png)
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418152626908.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
 但如果是定义样式，就可以放在bind里面
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190419100911999.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
接下来我们再看bind里面拿到我们传递的值的情况
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190419101022582.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
举例如下：
在input框中使用我们自定义的v-color指令，并给指令传值为blue
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190419101310443.png)
解释：下面的function（）的第二个参数是形参，用于接收我们传递的参数的，可以任意取名字啊
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190419101626219.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
运行效果截图如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190419101706455.png)
**7、定义私有指令**
总结：跟我们自定义私有过滤器一样，都在vue对象内部定义的
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190419103213883.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
扩展：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190419104311253.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
**8、Vue的生命周期的一些钩子函数**
看不懂下面的图没关系，跳过去看下文讲解

vue生命周期图示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190419114349752.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019041911013463.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
先讲讲beforeCreate()和created()
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190419105744874.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
接下来再讲讲beforeMount()   ，Mount这个单词的意思是“挂载”，beforeMount的意思就是“在渲染到页面之前执行”，先看代码，然后下面有运行效果截图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190419111812370.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190419112036745.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
运行效果截图如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190419112207143.png)
接下来再看一下mounted()的作用
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190419114645912.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
运行效果截图如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190419114758242.png)
再接下来我们学习一下beforeUpdate（）的使用
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019041913393710.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190419133733330.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
运行效果截图如下:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190419132937993.png)
接下来继续看updated()的使用
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190419135716369.png)
运行效果截图如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190419135831811.png)
**总结归纳：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190419135437712.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)
最后再说一下beforeDestroy()和destroyed()吧
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190419141256600.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMjQxOTU3,size_16,color_FFFFFF,t_70)




 





