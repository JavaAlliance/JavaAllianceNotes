#                           Vue.js学习总结第四天

## 一、动画中transition-group的使用

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190425212631.png)

transition-group 的主要作用是给其子元素设定一个统一的过渡样式（统一的动画样式），而不需要给每单
个元素都用 transition 包裹起来

和 transition 标签不一样，transition-group 不是一个虚拟 DOM，会真实渲染在 DOM
树中。默认会是 span 标签，如下图所示，很明显在<ul于<li之间还有span标签

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190425212656.png)

另外，我们也可以通过 <ul is="transition-group" 这样的写
法或者<tag="ul"来设定标签。修改成下图所示，但是要把原先写在外层的<ul删掉，这个时候transition-group会自动渲染成ul标签包裹在li标签的外面

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190425212736.png)



##  二、创建全局的vue组件

①第一种方式：使用Vue.extends()创建

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190425212806.png)

②第二种方式：   实质上是第一种方式的变版，类似于“匿名内部类”一样，直接在Vue.component()的第二个参数上写Vue组件定义

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190425212827.png)

③第三种方式：通过id选择器方式

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190425212851.png)



##  三、使用component定义私有组件

先在绑定#app2这个div的vue对象里面定义私有组件

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190425212909.png)

在#app2中引用私有组件

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190425212924.png)



##    四、Vue组件中可以定义data属性和method属性

  首先说vue组件中定义data属性

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190425212942.png)

再说vue组件中定义method属性，下面的例子中组件的data属性的function里面return的是一个我们定义的全局的对象dataObj，这样的话就会产生一个问题，如果我们多次引用该组件时，这多个引用副本共享data属性里面的数据dataObj,如果要让每个副本都是独立的，可以把下图的return dataObj修改成return {count :0},这样的话，每个副本都是从count=0开始，各做各的，独立的。

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190425213003.png)

##   五、使用Vue提供的<component标签来实现组件切换

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190425225302.png)

##   六、组件切换---应用切换动画和mode方式
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset='utf-8'>
        <title></title>
        <!-- 引入vue.js -->
        <script src='https://cdn.jsdelivr.net/npm/vue/dist/vue.js'></script>
        <style>
            .v-enter-active,    //动画进入过程
            .v-leave-active {    //动画离开过程
                transition: all .8s ease;//“all”代表被transition包裹的所有元素一起移动
            }    
            .v-enter, .v-leave-to{  //整体动画过程：从右边往左边移动，移动过程中从透明逐渐变成不透明     //（因为初始坐标是  translateX(100px)），//然后离开时从左边往右边 
                           //移动过程中从不透明逐渐变成透明直至消失，  动画开始的起点和消失之后的终点                                    //是一个点 
                transform: translateX(100px); //动画进入时的起点，和动画离开后消失的终点
                opacity: 0;//不透明度为0，说明进入时和离开后都是消失状态
            }
        </style>
    </head>
    <body>
        <div id='app'>
            <a href="" @click.prevent="componentId='login'">登录</a>
            <a href="" @click.prevent="componentId='register'">注册</a>
            <!-- component是一个占位符，:is属性，可以用来指定要展示的组件名称 -->
            <transition mode="out-in">    //这里的mode=“out-in”的作用是让组件替换时（一个组件离                                   //开，一个组件进入），按顺序显示动画，而不是说“一个组件的离开和另                                  //一个组件的进入”的动画一起显示
                <component :is="componentId"></component>
            </transition>           
        </div>  
    </body>
    <script>
        Vue.component('login',{
            template:'<h3>登录组件</h3>'
        })
        Vue.component('register',{
            template:'<h3>注册组件</h3>'
        })
        // 实例化vue对象
        let vm = new Vue({
            // 绑定对象
            el:'#app',
            data:{
                flag:true,
                componentId:'login'
            },
            methods:{
                
            }
        })
    </script>
</html>
```


