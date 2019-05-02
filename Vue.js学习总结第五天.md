

# Vue.js学习总结第五天

##   一、父组件向子组件传值

解释：主要是通过v-bind属性绑定，具体讲解看下面第2张图

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset='utf-8'>
        <title></title>
        <!-- 引入vue.js -->
        <script src='https://cdn.jsdelivr.net/npm/vue/dist/vue.js'></script>
    </head>
    <body>
        <div id='app'>
            <!-- 父组件，可以在引用子组件的时候，通过 属性绑定（v-bind）的形式，
             把 需要的传递给 子组件的数据，以属性绑定的形式，传递到子组件内部，供子组件使用 -->
            <com1 v-bind:parentmsg="msg"></com1>
        </div>   
    </body>
    <script>
        // 实例化vue对象
        const vm = new Vue({
            // 绑定对象
            el:'#app',
            data:{
                msg:"1213-父组件中的数据"
            },
            methods:{              
            },
            components:{
            //结论：经过演示，发现子组件中，默认无法访问到父组件中的data 上的数据和methods中的方法
                com1:{
                    data(){//注意：子组件中的data数据，并不是通过父组件传递过来的，而是子组件自身
                    	//私有的，比如：子组件通过ajax发送请求从而得到服务端传回来的数据，
                    	//都可以放到data身上
                    	//data上的数据都“可读可写的”
                        return {
                            title:'子组件title'
                        }
                    },
                    template:"<h1 @click='change'>这是子组件——{{parentmsg}}</h1>",        
                    //注意：组件中的所有props中的数据，都是通过父组件传递给子组件的
                    //props中的数据，都是只读的，无法重新赋值
                    props:['parentmsg'], // 把父组件传递过来的parentmsg属性，先在 props 数组中，定义一下，
                                          //这样，才能使用这个数据
                    methods:{
                        change(){
                            this.parentmsg="被修改了"
                        }
                    }
                }
            }           
        })
    </script>
</html>
```

![1556252122194](C:\Users\19643\AppData\Roaming\Typora\typora-user-images\1556252122194.png)



## 二、父组件向子组件传递方法function

解析：父组件向子组件传递方法主要是通过v-on指令完成的

问题：父子组件之间是如何相互通信的？

答：子组件调用父组件的方法，这个方法里面有赋值操作，把子组件需要传递给父组件的值赋给父组件的某一个属性参数即可，这样就相当于子组件和父组件互通数据了，下例已经很详细了

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
           //父组件向子组件传递 方法，使用的是事件绑定机制;v-on,当我们自定义了 一个事件属性之后，
           //那么子组件就能够通过某些方式来调用传递进去的这个方法了，下面的@是v-on的简写
            <com2 @show="show"></com2>  //根据等号右边的“show”去父组件vue对象中去找show方法，找               </div>                             // 到之后赋给变量show       
        <template id="tmp2">
            <div>
                <h1>这是子组件</h1>
                <p @click="myclick">点击我调用show方法</p>
            </div>
        </template>
    </body>
    <script>
        // 定义了一个字面量类型的 组件模板对象
        var com2 = {
            template:'#tmp2', //通过指定一个 ID,表示加载该元素
            data(){
                return {
                    sonmsg:{name:'小头儿子',age:8}
                }
            },
            methods:{
                myclick(){
                	//当点击子组件的按钮的时候，如何拿到父组件传递过来的func方法，并调用这个方法??                    
                    console.log("子组件事件")
                    this.$emit("show",'123','456') //emit英文原意：触发，调用，发射
                }
            }
        }
        // 实例化vue对象
        var vm = new Vue({
            // 绑定对象
            el:'#app',
            data:{  
                dataMsg:null   //当子组件调用本vue对象的show方法时，拿来接收从子组件传过来的值
            },
            methods:{
                show(data,data2){
                    this.dataMsg=data
                    console.log("该方法是父组件的show方法" + data + '--' + data2)                            }
            },
            components:{
                com2    //引用上面定义的com2组件对象
            }
        })
    </script>
</html>
```



## 三、组件案例：实现评论的发表和自动刷新列表

代码部分如下：

```html

<html>
    <head>
        <meta charset='utf-8'>
        <title></title>
        <!-- 引入vue.js -->
        <script src='https://cdn.jsdelivr.net/npm/vue/dist/vue.js'></script>
        <!-- 最新版本的 Bootstrap 核心 CSS 文件 -->
        <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">
    </head>
    <body>
        <div id='app'>
            <cmt-box @func="loadComments"></cmt-box>//这里是父组件向子组件传递function
            <!-- 评论列表 -->
            <ul class="list-group">
                <li class="list-group-item" v-for="item in list" :key="item.id">
                    <span class="badge">评论人： {{item.user}}</span>
                    {{item.content}}
                </li>
            </ul>
            <!-- 评论列表结束 -->
        </div>
        <template id="tmp1">
            <div>
                <div class="form-group">
                    <label>评论人：</label>
                    <input type="text" class="form-control" v-model="user">
                </div>
                <div class="form-group">
                    <label>评论内容：</label>
                    <textarea class="form-control" v-model="content"></textarea>
                </div>
                <div class="form-group">
                    <input type="button" class="btn btn-primary" value="发表评论" @click="postComment">
                </div>                    
            </div>
            
        </template>
    </body>
    <script>
        var commentBox = {
            template:'#tmp1',
            data(){
                return {
                    user:'',
                    content:''
                }
            },
            methods:{
                postComment(){
                    if(this.user == '' || this.content == ''){
                        alert("请填写正确内容")
                        return
                    }
                    // 分析：发表评论的业务逻辑
                    // 1、评论数据存到哪里去？？ 存放到 localStorage 中
                    // 2、先组织出一个最新的评论数据对象
                    // 3、想办法，把 第二步中，得到的评论对象。保存到 localStorage中
                    //在HTML5中，新加入了一个localStorage特性，这个特性主要是用来作为本地存储来使用的，
                    //解决了cookie存储空间不足的问题(cookie中每条cookie的存储空间为4k)，localStorage
                    //中一般浏览器支持的是5M大小，这个在不同的浏览器中localStorage会有所不同。
                    // 3.1localStorage 只支持存放字符串数据，要先调用JSON.stringify
                    //JSON.stringify() 方法用于将 JavaScript 值转换为 JSON 字符串。
                    // 3.2在保存最新的评论数据之前，要先从localStorage获取到之前评论数据
                    // 3.3需要先判断localStorage中有无数据，若没有，返回"[]"
                    // 3.4发最新的列表数据，再次调用JSON。stringify转为数组字符串
                    var comment = {id:Date.now(),user:this.user,content:this.content}
                    // 从localStorage中获取评论
                    var list = JSON.parse(localStorage.getItem('cmts') || "[]")
                    list.unshift(comment)  //从list的头部插入
                    // 重新保存
                    localStorage.setItem('cmts',JSON.stringify(list))
                    this.user = ''
                    this.content = ''
                  this.$emit("func")//调用父组件传递过来的方法func,emit的英文是“触发，激活”的意思
                }
            }
        }
        // 实例化vue对象
        var vm = new Vue({
            // 绑定对象
            el:'#app',
            data:{
                list:[]
            },
            methods:{
                // 从本地的localStorage中，加载评论
                loadComments(){ //下面的||或运算符的意思是说“如果前者不为null，
                	            //那么就取前者为运算结果，否则取后者为运算结果”
                    let list = JSON.parse(localStorage.getItem('cmts') || "[]")
                    this.list = list
                }
            },
            components:{
                'cmt-box':commentBox
            },
            created() {
                this.loadComments()  
            },
        })
    </script>
</html>       })
    </script>
</html>
```

运行效果如下：

![1556337542396](C:\Users\19643\AppData\Roaming\Typora\typora-user-images\1556337542396.png)





## 四、使用ref获取DOM元素和组件

代码部分如下：    可以通过ref属性取得对应的Dom元素

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
            <input type="button" value="获取元素" @click="getElement">
            <h3 id="myh3" ref="myh3">哈哈哈，好逗啊</h3> //这里有一个ref属性，是方便取该Dom元素用的
            <hr>  //换行符
            <login ref="mylogin"></login>  //组件Dom元素也可以通过ref属性来取
        </div>
    
    </body>
    <script>
        var login = {
            template:"<h1>login组件</h1>",
            data(){
                return {
                    msg:"子组件msg"
                }
            },
            methods:{
                show(){
                    console.log("子组件show方法")
                }
            }
        }
        // 实例化vue对象
        var vm = new Vue({
            // 绑定对象
            el:'#app',
            data:{
                msg:"父组件msg"
            },
            methods:{
                getElement(){
                    // console.log(document.getElementById('myh3').innerText)
                    console.log(this.$refs.myh3.innerText)//取"myh3"这个Dom元素
                    console.log(this.$refs.mylogin.msg)
                }
            },
            components:{
                login
            }
        })
    </script>
</html>
```



## 五、路由vue-router的基本使用

路由的解释：根据不同的参数，展示不同的组件

使用步骤：①先创建路由规则对象routerObj

​                   ②把路由规则对象与Vue对象绑定

​                   ③在Vue对象所代表的dom组件里面使用<router-view 标签用来展示路由的结果

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
            <a href="#/login">login</a>  //注意带有"#"才能实现在本页面内路由，而不是去访问其他的页面
            <a href="#/register">register</a>
            <hr>
            //这是vue-router.js提供的元素，专门用来当作占位符的，将来根据路由规则，匹配到的组件就会展示
            //到这个router-view里面去
           <router-view></router-view>           
        </div>  
    </body>
    <script>
        // 创建组件对象
        var login = {
            template:"<h1>登录组件</h1>"
        }
        var register = {
            template:"<h1>注册组件</h1>"
        }
        // 创建路由对象
        // 当导入vue-route包之后，在window全局对象中，就有了一个路由的构造函数，叫做VueRouter
        var routerObj = new VueRouter({
            // 这个配置对象中的route表示【路由匹配规则】
            // 每个路由规则，都是一个对象，这个对象身上，有两个必须的属性
            // 属性1：path——表示监听 那个路由链接地址
            // 属性2:component——表示，如果路由是前面匹配到的path，则展示component属性对应的组件
            routes:[
                {path:'/',redirect:'/login'},//重定向到“/login”路由上去
                {path:'/login',component:login},//component必须是一个组件对象，不能是一个组件的引用名称
                {path:'/register',component:register}
            ],
             // 更改激活类（根据这个类可以改样式）
            linkActiveClass:'router-link-active'//默认就是这个哦
        })
        // 实例化vue对象
        let vm = new Vue({
            // 绑定对象
            el:'#app',
            data:{                
            },
            methods:{                
            },
            router:routerObj,//将路由规则对象，注册到vm实例上，用来监听URL地址变化，然后展示对应的组件
        })
    </script>
</html>
```

运行结果如下：  看浏览器地址栏后面带有一个"#"

![1556353229561](C:\Users\19643\AppData\Roaming\Typora\typora-user-images\1556353229561.png)

**问题一：**但是上面每次都要在<a href=“#”里面路径的前面写#号，很麻烦，有没有比较好的解决方法？
答：可以通过使用 router-link 组件来导航，例如

```html
<a href="#/login">login</a>
```

就可以修改成     

```
<router-link to="/login" tag="span">login</router-link>
```

通过传入 `to` 属性指定链接，<router-link默认会被渲染成一个 `<a>` 标签，这个可以在浏览器调试时看到

**问题二：**如何修改<router-link里的样式呢？

但是<router-link链接展示的字体会有默认样式，‘'router-link-active'，我们可以在重写这个样式里面的内容

再或者我们可以通过 linkActiveClass属性自己指定自定义的样式，我们可以先自定义样式，然后把linkActiveClass属性的值设置为我们自定义的即可，如上文代码部分



##  六、路由传参--使用query或者param方式传递参数

分别解释query和param方式的用法：

**第一种，用query方式：**

①在<router-link to="/login?id=10&name=shaohang" 中可以看到/login后面用？跟上一些列的参数键值对

②在login组件里面使用

```
{{$route.query.id}}---{{$route.query.name}}
```

来引用



**第二种，用param方式：**

①在<router-link to="/register/1/shy" 中可以看到/register后面用/连接每个参数，若有多个参数，可以继续加/连接

②  在router对象里面的写{path:'/register/:id/:name',component:register}，可以看到/register/后面的参数前都有“：”，这些参数用来接收从<router-link to="/register/1/shy传过来的值

②在register组件里面使用

```
{{$route.params.id}} -- {{$route.params.name}}
```

来使用传过来的值



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
        <div id='app'>  //这是用query方式传值，在下面的/login后面用？链接一系列的参数，用于访问组件                         //的时候把参数带过去
            <router-link to="/login?id=10&name=shaohang">登录</router-link>
            <router-link to="/register/1/shy">注册</router-link>  //这是使用param方式传值
            <router-view></router-view>  //根据路由的结果显示不同的组件，本例是显示login组件或者                                          //register组件
        </div>   
    </body>
    <script>
         // 创建组件模板对象
         var login = {     //取得通过路由方式访问该组件时带过来参数的值，都存在query对象里面
            template:"<h1>登录组件---{{$route.query.id}}---{{$route.query.name}}</h1>",
            // 组件的生命周期函数
            created(){
                console.log(this.$route)
                let id = this.$route.query.id
                console.log(id)
                // this.id = id
            }
        }
        var register = {
            template:"<h1>注册组件--{{$route.params.id}} -- {{$route.params.name}}</h1>",
            created(){
                console.log(this.$route.params.id)
            }
        }
        var router = new VueRouter({
            routes:[
                {path:'/',redirect:'login'},
                {path:'/login',component:login},
                //下面这种是利用param方式传参
                {path:'/register/:id/:name',component:register}//可以看到/register/后面链接的参数前面都有“：”，用于接收路由带过来的参数值
            ]
        })
        // 实例化vue对象
        var vm = new Vue({
            // 绑定对象
            el:'#app',
            data:{               
            },
            methods:{                
            },//下面写router其实是router:router的简写
            router
        })
    </script>
</html>
```

运行效果截图：

![1556428423536](C:\Users\19643\AppData\Roaming\Typora\typora-user-images\1556428423536.png)