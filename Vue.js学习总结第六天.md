# Vue.js学习总结第六天

## 一、使用children属性实现路由的嵌套 

通俗地说就是组件里面也有路由规则，大组件里面套小组件，根据路由情况有选择的展示某些小组件

使用步骤：

①在父组件里面写 <router-link to="/account/login"时，一定要带上父组件的路由

②在路由规则对象定义里要给相应父组件对象映射规则下加上children属性，children属性里面写子路由信息

```html
<html>
    <head>
        <meta charset='utf-8'>
        <title></title>
        <!-- 引入vue.js -->
        <script src='https://cdn.jsdelivr.net/npm/vue/dist/vue.js'></script>
        <!-- 引入路由模块 -->
        <script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
    </head>
    <body>
        <div id='app'>  //当我们点击“Account”时， <router-view>会展示account组件对象
            <router-link to="/account">Account</router-link>
            <router-view></router-view>
        </div>
        <template id="tmp1">
            <div>
                <h1>这是account组件</h1> //以“/account/login”可以实现在父路由为account的情况下，去访问父路由account下的子路由login
                <router-link to="/account/login">登录</router-link> | 
                <router-link to="/account/register">注册</router-link>
                <router-view></router-view>
            </div>
        </template>
    </body>
    <script>
        var account = {
            template:"#tmp1"
        }
        var login = {
            template:"<h1>登录组件</h1>"
        }
        var register = {
            template:"<h1>注册组件</h1>"
        }
        var router = new VueRouter({
            routes:[
                {
                	path:'/account',
                	component:account,
   //使用children属性，实现子路由，同时，子路由的path前面，不要带/，否则永远以根路径
    //开始请求，这样不方便我们用户去理解url地址
                    children:[ 
                    {path:'login',component:login},//path开头不要加“/”
                    {path:'register',component:register}
                ]
                   },            
            ]
        })
        // 实例化vue对象
        let vm = new Vue({
            // 绑定对象
            el:'#app',
            data:{
                
            },
            methods:{
                
            },
            router
        })
    </script>
</html>
```



##  二、路由-命名视图实现经典布局

通俗的话就是说让<router-view有针对性，某些<router-view只展示某些组件

使用步骤：

①给<router-view设置name值，

②在router路由对象里面配置 path规则

```html
<html>
    <head>
        <meta charset='utf-8'>
        <title></title>
        <!-- 引入vue.js -->
        <script src='https://cdn.jsdelivr.net/npm/vue/dist/vue.js'></script>
        <!-- 引入路由模块 -->
        <script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
        <style>
            *{
                margin:0;
                padding: 0;
            }
            .header{
                background: red;
                height: 100px;
            }
            .container{
                display:flex;
                height: 500px;
            }
            .left{
                background: lightblue;
                flex:2;
            }
            .main{
                background: lightsalmon;
                flex:8;
            }
        </style>
    </head>
    <body>
        <div id='app'>
            <router-view></router-view>
            <div class="container">  //如果给router-view指定了name值，那么展示指定的组件，到router路由对象里面找与之对应的组件（根据path规则找）
                <router-view name="left"></router-view>
                <router-view name="main"></router-view>
            </div>           
        </div>    
    </body>
    <script>
        let header = {
            template:"<h1 class='header'>Header头部区域</h1>"
        }
        let leftBox = {
            template:"<h1 class='left'>leftBox侧边栏区域</h1>"
        }
        let mainBox = {
            template:"<h1 class='main'>MainBox主体区域</h1>"
        }
        let router = new VueRouter({
            routes:[
                {path:'/',components:{ //要注意到这里的component带了s，components
                    'default':header,//给上面<router-view>指定默认展示的组件
                    'left':leftBox,
                    'main':mainBox
                }},
            ]
        })
        // 实例化vue对象
        let vm = new Vue({
            // 绑定对象
            el:'#app',
            data:{               
            },
            methods:{                
            },
            router  //其实是router:router的简写
        })
    </script>
</html>
```

运行效果截图：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190502100715.png)



##  三、使用watch监听文本框数据的变化

watch的作用：监听绑定的文本框的数据的变化，一旦变化，就立刻触发watch后的function

使用步骤：

①在vue对象里面指定watch属性，然后在watch属性里对需要绑定的元素（vue对象的data属性里有很多数据都可以被监听）做监听配置

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
            <!-- 分析 1、监听到文本框的改变  -->
            <h2>第一个</h2>
            <input type="text" v-model="firstname" @keyup="getFullname"> + 
            <input type="text" v-model="lastname" @keyup="getFullname"> = 
            <input type="text" v-model="fullname">
        </div>   
    </body>
    <script>
        // 实例化vue对象
        let vm = new Vue({
            // 绑定对象
            el:'#app',
            data:{
                firstname:'',
                lastname:'',
                fullname:'',
            },
            methods:{
                getFullname(){
                    this.fullname = this.firstname + '-' + this.lastname
                }
            },
            // 使用watch可以监视 data 中指定数据的变化，然后触发这个 watch 中对应的function处理函数
            watch:{
                'firstname':function(newVal,oldVal){
                    console.log("new:"+newVal+"--old:"+oldVal)
                    this.fullname = this.firstname + '-' + this.lastname
                },
                'lastname':function(newVal){
                    this.fullname = this.firstname + '-' + newVal
                }
            },
        })
    </script>
</html>
```

运行效果截图：

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190502100739.png)



##  四、使用watch监听路由地址的改变

目的：我们点击不同的按钮就会在本页面进行不同的路由，路由地址会产生变化，这个时候我们可以对路由地址做监听

使用步骤：

①在vue对象里面写watch属性，并对其进行配置"$route.path":function(newVal,oldVal)

```html
<html>
    <head>
        <meta charset='utf-8'>
        <title></title>
        <!-- 引入vue.js -->
        <script src='https://cdn.jsdelivr.net/npm/vue/dist/vue.js'></script>
        <!-- 引入路由模块 -->
        <script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
    </head>
    <body>
        <div id='app'>
            <router-link to="/login">登录</router-link>
            <router-link to="/register">注册</router-link>
            <router-view></router-view>
        </div>  
    </body>
    <script>
         // 创建组件模板对象
         var login = {
            template:"<h1>登录组件</h1>",      
        }
        var register = {
            template:"<h1>注册组件</h1>",
        }
        var router = new VueRouter({
            routes:[
                {path:'/',redirect:'login'},
                {path:'/login',component:login},
                {path:'/register',component:register}
            ]
        })
        // 实例化vue对象
        let vm = new Vue({
            // 绑定对象
            el:'#app',
            data:{                
            },
            methods:{               
            },
            router,
            watch:{   //$route.path 是路由路径，下面是做对路由路径变化的监听配置
                //下面的newVal是指新的路由路径，oldVal是指旧的路由路径
                "$route.path":function(newVal,oldVal){
                    console.log("new:"+newVal+"--old:"+oldVal)
                    if(newVal == "/login"){
                        console.log("欢迎进入登录页面")
                    }else if(newVal == "/register"){
                        console.log("欢迎进入注册页面")
                    }
                }
            }
        })
    </script>
</html>
```

运行效果截图：
![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190502101229.png)


##  五、computed计算属性的使用

效果：每次我们修改firstname或者lastname 文本框时，fullname文本框也会跟着变化

使用步骤：

 ①在vue对象中写computed属性：然后配置相应的“计算属性”，比如本例里面配置“fullname”属性，然后与<div id下的fullname文本框绑定， 然后在computed里给fullname配置相应的function即可

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
            <!-- 分析 1、监听到文本框的改变  -->
            <input type="text" v-model="firstname" > + 
            <input type="text" v-model="lastname" > = 
            <input type="text" v-model="fullname">
            <p>{{fullname}}</p>  //这里虽然引用了3次fullname,但是fullname对应的function里使用到                   //的数据都没有变化，所以不管我们怎么引用fullname都不会触发fullname对应的function
            <p>{{fullname}}</p>
            <p>{{fullname}}</p>
        </div>   
    </body>
    <script>
        // 实例化vue对象
        var vm = new Vue({
            // 绑定对象
            el:'#app',
            data:{
                firstname:'',
                lastname:'',
            },
            methods:{          
            },
            computed:{ //在computed中，可以定义一些属性，这些属性可以与我们的input标签绑定，
            	 //我们在这里给属性添加方法，作用就是“只要该属性function里用到的某一个data数据发生变化，
            	 //都会重新触发该方法返回新值，然后展示到绑定的input标签中去”，我们把之称为“计算属性”
            	 //注意1：“计算属性”，在引用的时候一定不要加()去调用，直接把它当作普通属性去使用就好，
            	 //注意2：“计算属性”的求值结果，会被缓存起来，方便下次直接使用；只要该计算属性方法里用                   //到的数据都没有发生变化，就不会重新触发该方法调用
            	'fullname':function(){
            		console.log('ok'); //一旦firstname或者lastname发生变化，立马触发这个                       //function，然后刷新fullname文本框的值
            		return this.firstname+'-'+this.lastname;
            	}
            }
        })
    </script>
</html>
```

运行结果截图：  解释一下控制台的两个ok分别是我们输入firstname和lastname而触发fullname对应的function，而我们在上面代码中虽然多次写了<p{{fullname}}</p 但是由于fullname ：function里用到的数据没有发生变化，所以不会触发fullname ：function

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190502100821.png)
