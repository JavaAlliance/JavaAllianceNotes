#                                                  Vue.js学习总结第三天

##    一、vue-resouce发起post、get、jsoup请求  

​      我们没学过vue.js之前只知道jQuery的ajax(),现在我们来看一下如何用Vue发起ajax请求

​                 **前提条件：** 首先我们需要导入包

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190424145536.png)

###                    ①vue的ajax方式的get请求

​    如果是回调成功，那么得到的result.body.status就等于0，表示成功了

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190424145608.png)

​              

###                    ②vue的ajax方式的post请求

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190424145629.png)

​       

###               ③vue的ajax方式的jsoup请求

​                                     （jsoup方式和get方式不同的地方就是“jsoup可以在ajax里面实现跨域请求”）

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190424145655.png)

​                                   jsoup的详解如下：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190424145715.png)

​         



## 二、Vue配置全局数据接口的根路径

​     先阐述问题：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190424145735.png)

解决方案如下：配置全局的数据接口的根域名

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190424145752.png)

然后再修改原路径即可，系统运行时会自动进行拼接

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190424145835.png)



## 三、Vue配置全局emulateJSON选项

阐述问题：每次写请求时都要在后面加上emulateJSON:true，写起来会很麻烦的

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190424145857.png)

解决办法：配置全局启用emulateJSON选项

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190424145922.png)

然后我们就可以不用每次都在请求里面写emulateJSON:true了

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190424145937.png)




##   四、Vue的过度和动画

opacity：不透明度，模糊，不清晰，当opacity=0时，就是“隐形的”，当opacity=1时就是“完全的实心”

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190424145959.png)

v-enter,v-enter-to,v-leave,v-leave-to都是时间点

v-enter: 定义进入过渡的开始状态。在元素被插入时生效，在下一个帧移除。
v-enter-active: 定义进入过渡的结束状态。在元素被插入时生效，在 transition/animation 完成之后移除。
v-leave: 定义离开过渡的开始状态。在离开过渡被触发时生效，在下一个帧移除。
v-leave-active: 定义离开过渡的结束状态。在离开过渡被触发时生效，在 transition/animation 完成之后移除。

**接下来看一个例子：**

先看代码部分如下：    讲解：可以看到在.v-enter， .v-leave-to里面写了一个 transform: translateX(100px);就是说“进入的起点”和“离开的终点”的位置都在（100px，0）处，然后要注意transition组件必须要包在一个v-if或v-show的外面，不然的话不起作用      

效果就是：“当我们点击input按钮时，文字先从右边往左边移动，一边移动一边从”隐形“变得”可见“（这是enter过程），当我们再次点击input按钮时，文字从左往右移动，一边移动一边从”可见“变成”隐形“
![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190424150022.png)

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190424150039.png)



利用钩子函数实现半场动画

```html
<html>
    <head>
        <meta charset='utf-8'>
        <title></title>
        <!-- 引入vue.js -->
        <script src='https://cdn.jsdelivr.net/npm/vue/dist/vue.js'></script>
        <style>
            .ball{
                width: 20px;//
                height: 20px;//设置球的宽度和高度
                border-radius: 50%;//设置球的半径比例
                background-color: red;//设置球的颜色
            }
        </style>
    </head>
    <body>
        <div id='app'>
            <input type="button" value="快到碗里来" @click="flag = !flag">
            <transition  /* v-on:before-enter表示动画入场之前，此时动画尚未开始，可以在beforeEnter中设置元素开始之前的起始样式 */ 
                v-on:before-enter="beforeEnter"  
                v-on:enter="enter"  /*v-on：enter表示动画开始之后的样式*/
                @after-enter="afterEnter"/*动画结束之后*/
            >
                <div class="ball" v-if="flag"></div>
            </transition>
        </div>
    
    </body>
    <script>
        // 实例化vue对象
        let vm = new Vue({
            // 绑定对象
            el:'#app',
            data:{
                flag:false
            },
            methods:{
                // 动画钩子函数：el,表示 要执行动画的那个DOM元素，是一个原生的js对象
                beforeEnter(el){
                    //berareEnter表示动画入场之前，此时，动画尚未开始，可以在beforeEnter中设置开始动画之前的样式
                    el.style.transform = "translate(0,0)"  //设置小球的初始坐标位置
                },
                enter(el,done){//这里的done 其实就是afterEnter
                    // 表示动画 开始之后的样式，这里，可以设置小球完成动画之后的样式，结束状态
                    el.offsetWidth //得写这个，会强制动画刷新
                    el.style.transform = "translate(150px,450px)"
                    el.style.transition = 'all 1s ease' 
                    done()  //这里的done 其实就是afterEnter，官方文档说这个必须调用一下
                },
                afterEnter(el){
                    // 动画完成之后
                    console.log("ok")
                    this.flag = false//修改flag来控制小球是否显示
                }
            }
        })
    </script>
</html>
```

小疑问：我们可以看到在transtion标签里头总有一个v-if或者v-show,这是触发动画开始的必要条件当v-if或者v-show=true时开始触发动画，也就是说v-if=“flag”里面的flag为true或flase影响很大，而在上述afterEnter()方法里写了this.flag=false,之后就不展示动画了所以小球消失，也可以理解为v-if=“flase”时，这个元素消失了（这两种解释都可以）
