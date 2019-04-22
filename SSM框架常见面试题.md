#                             SSM框架常见面试题

**1.什么是Spring框架？**

spring是为java应用程序开发提供基础性服务的一套框架，其目的是用于简化企业级应用程序开发，它使得程序员只需要关心业务需求。常见的配置方式有三种：**基于XML的配置、基于注解的配置、基于Java的配置。**

主要有以下几个模块组成：

Spring Core：核心类库，提供IOC服务；

Spring Context：提供框架式的Bean访问方式(可以使用配置文件)以及企业级功能(JNDI,定时任务等)；

Spring AOP：AOP服务；

Spring DAO：对JDBC抽象，简化了数据访问异常处理；

Spring ORM：提供对现有ORM框架的支持；

Spring Web：提供了基本的面向Web的综合特性，例如多方文件上传；

Spring MVC：提供了面向Web应用的Model-View-Controller实现。



**2. Spring的优点？**

1）<font color="red">低侵入式设计</font>，代码污染极低；

2）<font color="red">IOC</font>将对象之间的依赖关系交由框架处理，<font color="red">降低组件的耦合性</font>；

3）<font color="red">AOP支持允许将一些通用任务如安全、事务、日志等进行集中式管理，从而提供了更好的复用；</font>

4）<font color="red">对主流的框架提供了很好的集成支持。</font>



**3. 什么是DI或者IOC？请解释下Spring IOC。**

IOC：即“控制反转”，是一种设计思想，是指<font color="red">创建对象权的控制的转移</font>，以前创建对象的主动权和创建时机是由自己把控的，而现在这种权力转移到容器，<font color="red">对象与对象之间松散耦合，也利于功能复用，</font>更重要的是使得程序的整个体系结构变得非常灵活。DI(依赖注入)和控制反转<font color="red">是同一个概念的不同角度描述，所谓依赖注入就是应用程序依赖于IoC容器，在运行时需要IoC容器来动态提供对象需要的外部资源。</font>



**Java中依赖注入有以下三种实现方式：构造器注入、Setter方法注入、接口注入。**



**4、BeanFactory和ApplicationContext有什么区别？**

​        BeanFactory 可以理解为含有bean集合的工厂类。BeanFactory 包含了种bean的定义，以便在接收到客户端请求时将对应的bean实例化。**BeanFactory还能在实例化对象时生成协作类之间的关系。**此举将bean自身与bean客户端的配置中解放出来。BeanFactory还包含了bean生命周期的控制，调用客户端的初始化方法和销毁方法。

从表面上看，application context如同bean factory一样具有bean定义、bean关联关系的设置，根据请求分发bean的功能。但application context在此基础上还提供了其他的功能。

<font color="CornflowerBlue">A.提供了支持国际化的文本消息;</font>

<font color="CornflowerBlue">B.统一的资源文件读取方式;</font>

<font color="CornflowerBlue">C.已在监听器中注册的bean的事件。</font>

**<font color="Blue">以下是三种较常见的 ApplicationContext 实现方式 </font>：**

1、ClassPathXmlApplicationContext：从classpath的XML配置文件中读取上下文，并生成上下文定义。应用程序上下文从程序环境变量中取得。 ApplicationContext context = new ClassPathXmlApplicationContext(“bean.xml”);

2、FileSystemXmlApplicationContext ：由文件系统中的XML配置文件读取上下文，需要指定完整路径。ApplicationContext context = new FileSystemXmlApplicationContext(“bean.xml”);

3、XmlWebApplicationContext：由Web应用的XML文件读取上下文，在web.xml中配置。




**5. Spring AOP**

OOP引入封装、继承、多态等概念来建立一种对象层次结构，OOP允许开发者定义纵向的关系，但并不适合定义横向的关系，例如日志功能。日志代码往往横向地散布在所有对象层次中，而与它对应的对象的核心功能毫无关系对于其他类型的代码，如安全性、异常处理和透明的持续性也都是如此，这种散布在各处的无关的代码被称为横切（cross cutting），在OOP设计中，它导致了大量代码的重复，而不利于各个模块的重用。

AOP技术恰恰相反，它利用一种称为"横切"的技术，将那些影响了多个类的公共行为封装到一个可重用模块，并将其命名为"Aspect"，即切面。所谓"切面"，简单说就是那些与业务无关，却为业务模块所共同调用的逻辑或责任封装起来，便于减少系统的重复代码，降低模块之间的耦合度，并有利于未来的可操作性和可维护性。

使用"横切"技术，AOP把软件系统分为两个部分：**<font color="red">核心关注点</font>**和**<font color="red">横切关注点</font>**。业务处理的主要流程是核心关注点，与之关系不大的部分是横切关注点。横切关注点的一个特点是，他们经常发生在核心关注点的多处，而各处基本相似，比如权限认证、日志、事务。AOP的作用在于分离系统中的各种关注点，将核心关注点和横切关注点分离开来





**6. spring bean的生命周期**

**<font color="red">1） 实例化Bean</font>**

对于BeanFactory容器，当客户向容器请求一个尚未初始化的bean时，或初始化bean的时候需要注入另一个尚未初始化的依赖时，容器就会调用createBean进行实例化。对于ApplicationContext容器，当容器启动结束后，便实例化所有的bean。容器通过获取BeanDefinition对象中的信息进行实例化。并且这一步仅仅是简单的实例化，并未进行依赖注入。实例化对象被包装在BeanWrapper对象中，BeanWrapper提供了设置对象属性的接口，从而避免了使用反射机制设置属性。

**<font color="red">2）设置对象属性（依赖注入）</font>**

实例化后的对象被封装在BeanWrapper对象中，并且此时对象仍然是一个原生的状态，并没有进行依赖注入。紧接着，Spring根据BeanDefinition中的信息进行依赖注入。并且通过BeanWrapper提供的设置属性的接口完成依赖注入。

**<font color="red">3） 处理Aware接口</font>**

紧接着，Spring会检测该对象是否实现了xxxAware接口，并将相关的xxxAware实例注入给bean。

3.1）如果这个Bean实现了BeanNameAware接口，会调用它实现的  (String beanId)方法，此处传递的是Spring配置文件中Bean的ID。

3.2）如果这个Bean实现了BeanFactoryAware接口，会调用它实现的setBeanFactory()，传递的是Spring工厂本身。

3.3)如果这个Bean实现了ApplicationContextAware接口，会调用setApplicationContext(ApplicationContext)方法，传入Spring上下文。

**<font color="red">4）BeanPostProcessor</font>**

当经过上述几个步骤后，bean对象已经被正确构造，但如果你想要对象被使用前再进行一些自定义的处理，就可以通过BeanPostProcessor接口实现。

**<font color="red">5）InitializingBean与init-method</font>**

在Bean的全部属性设置成功后执行的初始化方法，这一阶段也可以在bean正式构造完成前增加我们自定义的逻辑，但它与前置处理不同，由于该函数并不会把当前bean对象传进来，因此在这一步没办法处理对象本身，只能增加一些额外的逻辑。若要使用它，我们需要让bean实现该接口，并把要增加的逻辑写在该函数中。然后Spring会在前置处理完成后检测当前bean是否实现了该接口，并执行afterPropertiesSet函数。

**<font color="red">6）DisposableBean和destroy-method</font>**

和init-method一样，通过给destroy-method指定函数，就可以在bean销毁前执行指定的逻辑。



![img](https://mmbiz.qpic.cn/mmbiz_png/N6ROOeR7rfzQ1JRwmlq9wzd6B1o3DPfmnN5aibmG52FjD1BC9cIyAq8A4K52xpYWY1cjslkrOBdBOBAuqUntInQ/640?wx_fmt=png)





**7. Spring Bean的作用域之间有什么区别？**

1）singleton：默认，每个容器中只有一个bean的实例

2）prototype：为每一个bean请求提供一个实例

3）request：为每一个网络请求创建一个实例

4）session：与request范围类似，确保每个session中有一个bean的实例，在session过期后，bean会随之失效。

5)  global-session：global-session和Portlet应用相关



**8.Spring框架中的单例Beans$是线程安全的么$？**

Spring框架并没有对单例bean进行任何多线程的封装处理。关于<font color="red">单例bean的线程安全和并发问题需要开发者自行去搞定。</font>但实际上，大部分的Spring bean<font color="red">并没有可变的状态</font>(比如Service类和DAO类)，所以在某种程度上说Spring的单例bean是线程安全的。<font color="red">如果你的bean有多种状态的话（比如 View Model 对象），就需要自行保证线程安全。最浅显的解决办法就是将多态bean的作用域由“singleton”变更为“prototype”.</font>



**9. spring的自动装配？**

自动装配：在spring中，对象无需自己查找或创建与其关联的其他对象，容器负责把需要相互协作的对象引用赋予各个对象，使用**<font color="blue">autowire来配置自动装载模式。</font>**

xml中5种自动装配模式，基于配置的方式。

1）no：默认的方式是不进行自动装配，通过手工设置ref 属性来进行装配bean。

2）byName：通过参数名自动装配，如果一个bean的name 和另外一个bean的 property 相同，就自动装配。

3）byType：通过参数的数据类型自动自动装配。

4）construct：构造函数进行装配，并且构造函数的参数通过byType进行装配。

5）autodetect ：如果有构造方法，通过 construct的方式自动装配，否则使用 byType的方式自动装配。

基于注解的方式自动装配：

除了bean配置文件中提供的自动装配模式，还可以使用@Autowired注解来自动装配指定的bean。在使用@Autowired注解之前需要在按照如下的配置方式在Spring配置文件进行配置才可以使用<context:annotation-config />。在启动spring IoC时，容器自动装载了一个AutowiredAnnotationBeanPostProcessor后置处理器，当容器扫描到@Autowied、@Resource或@Inject时，就会在IoC容器自动查找需要的bean，并装配给该对象的属性。

在使用@Autowired时，首先在容器中查询对应类型的bean, 　　　　如果查询结果刚好为一个，就将该bean装配给@Autowired指定的数据；　　　　如果查询的结果不止一个，那么@Autowired会根据名称来查找；　　　　如果上述查找的结果为空，那么会抛出异常。解决方法时，使用required=false。

<font color="red">当出现装配歧义性(即一个接口出多个实现类)</font>，解决方法：

1）首先加上注解@Qualifier用来注入指定名字的实例（通过在实现接口的类上通过value属性去命名不同的名称,对于@Repository、@Service 和 @Controller 和 @Component四个注解都有类似value属性可以设置）。

2）因为一个接口存在两个以上的实现类,也可以通过标识首选哪个bean,来解决歧义性问题。

@Component @Primary public class UserServiceImpl implements IUserService{}

@Autowired可用于：构造函数、成员变量、Setter方法

注：<font color="red">@Autowired和@Resource之间的区别</font>

(1)、<font color="red">@Autowired默认是按照类型装配注入的</font>，默认情况下它要求依赖对象必须存在（可以设置它required属性为false）。

(2)、<font color="red">@Resource默认是按照名称来装配注入的</font>，只有当找不到与名称匹配的bean才会按照类型来装配注入。

@Autowired一般会和组件扫描搭配实现自动配置

组件扫描需要配置<context:component-scan base-package="cn.itcast" />(包含了<context:annotation-config />)，之后spring就会自动寻找@Component、@Service、@Controller、@Repository注解的类，并把这些类放入Spring容器管理



**10. Spring框架中有哪些不同类型的事件？**

Spring 提供了以下5种标准的事件：

1）上下文更新事件（ContextRefreshedEvent）：在调用ConfigurableApplicationContext 接口中的refresh()方法时被触发。

2）上下文开始事件（ContextStartedEvent）：当容器调用ConfigurableApplicationContext的Start()方法开始/重新开始容器时触发该事件。

3）上下文停止事件（ContextStoppedEvent）：当容器调用ConfigurableApplicationContext的Stop()方法停止容器时触发该事件。

4）上下文关闭事件（ContextClosedEvent）：当ApplicationContext被关闭时触发该事件。容器被关闭时，其管理的所有单例Bean都被销毁。

5）请求处理事件（RequestHandledEvent）：在Web应用中，当一个http请求（request）结束触发该事件。

如果一个bean实现了ApplicationListener接口，当一个ApplicationEvent 被发布以后，bean会自动被通知。



**11 .spring的设计模式？**

1)工厂模式，BeanFactory用来创建对象的实例。

2) 代理模式，在Aop实现中用到了JDK的动态代理。

3) 单例模式，这个比如在创建bean的时候。

4) 模板方法，用来解决代码重复的问题。比如. RestTemplate, JmsTemplate, JpaTemplate。



**12. spring中的BeanFactory与ApplicationContext的作用有哪些？**

\1. BeanFactory负责读取bean配置文档，管理bean的加载，实例化，维护bean之间的依赖关系

\2. ApplicationContext除了提供上述BeanFactory所能提供的功能之外，还提供了更完整的框架功能：

a. 国际化支持

b. 资源访问：Resource rs = ctx. getResource(”classpath:config.properties”), “file:c:/config.properties”

c. 事件传递：通过实现ApplicationContextAware接口



**SpringMVC**

**1.SpringMVC的工作流程？**

![img](https://mmbiz.qpic.cn/mmbiz_png/N6ROOeR7rfzQ1JRwmlq9wzd6B1o3DPfmibcZ8YlTDFnVyg55fWpBqykPwia7aJ64iauvmlaltOHAlqYicyRkpDtBvA/640?wx_fmt=png)

1、用户发送请求至前端控制器DispatcherServlet；



2、DispatcherServlet收到请求调用HandlerMapping处理器映射器； 

3、处理器映射器找到具体的处理器，生成处理器对象及处理器拦截器(如果有则生成)一并返回给DispatcherServlet；

4、DispatcherServlet调用HandlerAdapter处理器适配器；

5、HandlerAdapter经过适配调用具体的处理器(Controller，也叫后端控制器)； 

6、Controller执行完成返回ModelAndView；

7、HandlerAdapter将controller执行结果ModelAndView返回给DispatcherServlet；

8、DispatcherServlet将ModelAndView传给ViewReslover视图解析器；

9、ViewReslover解析后返回具体View；

10、DispatcherServlet根据View进行渲染视图（即将模型数据填充至视图中）；

11、DispatcherServlet响应用户。



**2. SpringMVC常用的注解有哪些？**

   @RequestMapping:用于请求url映射。

   @RequestBody：注解实现接收http请求的json数据，将json转换为java对象。

   @ResponseBody：注解实现将conreoller方法返回对象转化为json对象响应给客户。



**3. 如何解决get和post乱码问题？**

   解决post请求乱码：我们可以在web.xml里边配置一个CharacterEncodingFilter 过滤器。设置为utf-8。

   解决get请求乱码有两种方法个：

​       1）修改tomcat配置文件添加编码和工程编码一致；

​        2) 另一种方法对参数进行重新编码；

String username = new String(Request.getParameter("userName").getBytes("ISO8859-1"),"utf-8")。

**4. springmvc的优点？**

1）可以支持各种视图技术,而不仅仅局限于JSP；

2）与Spring框架集成（如IoC容器、AOP等）；

3)   清晰的角色分配:前端控制器(dispatcherServlet) , 请求到处理器映射（handlerMapping),  处理器适配器（HandlerAdapter), 视图解析器（ViewResolver）。



**5. 什么是springmvc?**

SpringMVC是一种基于Java的实现了MVC设计模式的请求驱动类型的轻量级Web框架，即使用了MVC架构模式的思想，将web层进行职责解耦，基于请求驱动指的就是使用请求-响应模型。MVC: Model View Controller 模型-视图-控制器 。

视图：展示给用户的视图；

模型：表示数据和业务处理规则。模型返回的数据是独立的，这样的一个模式能够为多个视图提供数据，这样一来模型的代码只需要写一次便能够被多个视图重用，减少了代码的重复性；

控制器：接受用户的输入并调用相应的模型和视图来完成用户的需求。



**6.SpringMVC怎么样设定重定向和转发的？**

在返回值前面加"forward:"就可以让结果转发,譬如"forward:user.do?name=method4" 在返回值前面加"redirect:"就可以让返回值重定向,譬如"redirect:http://www.baidu.com"。



**7、Spring MVC的主要组件？**

（1）前端控制器 DispatcherServlet（不需要程序员开发）

作用：接收请求、响应结果 相当于转发器，有了DispatcherServlet 就减少了其它组件之间的耦合度。

（2）处理器映射器HandlerMapping（不需要程序员开发）

作用：根据请求的URL来查找Handler

（3）处理器适配器HandlerAdapter

注意：在编写Handler的时候要按照HandlerAdapter要求的规则去编写，这样适配器HandlerAdapter才可以正确的去执行Handler。

（4）处理器Handler（需要程序员开发）

（5）视图解析器 ViewResolver（不需要程序员开发）

作用：进行视图的解析 根据视图逻辑名解析成真正的视图（view）

（6）视图View（需要程序员开发jsp）

View是一个接口， 它的实现类支持不同的视图类型（jsp，freemarker，pdf等等）

 

**8、Spring MVC的异常处理 ？**

答：可以将异常抛给Spring框架，由Spring框架来处理；我们只需要配置简单的异常处理器，在异常处理器中添视图页面即可。

 

**9、SpringMvc的控制器是不是单例模式,如果是,有什么问题,怎么解决？**

答：是单例模式,所以在多线程访问的时候有线程安全问题,不要用同步,会影响性能的,解决方案是在控制器里面不能写字段。

 

**10、SpingMvc中的控制器的注解一般用那个,有没有别的注解可以替代？**

答：一般用@Conntroller注解,表示是表现层,不能用别的注解代替。

 

**11、 @RequestMapping注解用在类上面有什么作用？**

答：是一个用来处理请求地址映射的注解，可用于类或方法上。用于类上，表示类中的所有响应请求的方法都是以该地址作为父路径。

 

**12、如果在拦截请求中,我想拦截get方式提交的方法,怎么配置？**

答：可以在@RequestMapping注解里面加上method=RequestMethod.GET。

 

**13、如果前台有很多个参数传入,并且这些参数都是一个对象的,那么怎么样快速得到这个对象？**

答：直接在方法中声明这个对象,SpringMvc就自动会把属性赋值到这个对象里面。

 

**14、SpringMvc中函数的返回值是什么？**

答：返回值可以有很多类型,有String, ModelAndView，但一般用String比较好。

 

**15、SpringMvc用什么对象从后台向前台传递数据的？**

答：通过ModelMap对象,可以在这个对象里面用put方法,把对象加到里面,前台就可以通过el表达式拿到。

**16、注解原理**

　　<font color="blue">注解本质是一个继承了Annotation的特殊接口，其</font>**<font color="red">具体实现类是Java运行时生成的动态代理类</font>**<font color="blue">。而我们通过反射获取注解时，返回的是Java运行时生成的动态代理对象。通过代理对象调用自定义注解的方法，会最终调用AnnotationInvocationHandler的invoke方法。该方法会从memberValues这个Map中索引出对应的值。而memberValues的来源是Java常量池。</font>



![img](https://mmbiz.qpic.cn/mmbiz_png/N6ROOeR7rfzxd7WUE6WqwlG1SkGDJDqnvMC7ZSicsx2RxASSlT5vpSicIotUrBgDb6aRVGfOPpudab5UYfr5e1icA/640?wx_fmt=png)



![img](https://mmbiz.qpic.cn/mmbiz_png/N6ROOeR7rfzxd7WUE6WqwlG1SkGDJDqnxf3q9Y9ibGdGGkxvTDs45H5ictgvtwAvCo7tpxicDENSe5pkceFwd3fhQ/640?wx_fmt=png)



