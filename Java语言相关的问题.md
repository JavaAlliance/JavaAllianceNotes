



#                  Java语言相关的问题

## **1. String**

<font color="red">A.new String("abc");创建了几个对象？</font>

<font color="red">1 个或2 个对象。如果常量池中原来有”abc”，那么只创建一个对象(在java堆上)；如果常量池中原来没有字符串”abc”，那么就创建 2 个对象。</font>

B.String的StringPool是一个<font color="red">固定大小(jdk1.6中默认大小1009，jdk1.7中长度可变)的Hashtable，存放在方法区中，若String Pool中的String非常多，就会造成Hash冲突严重</font>，从而导致链表会很长，进而降低String使用的效率。

字符串的长度：int length()；

字符抽取：charAt(int index)； getChars():一次抽取多个字符。 getBytes():将字符存储在字节数组里。

substring(int startIndex):返回从startIndex开始到调用字符串结束的子串的一个副本。

**substring(int startIndex,int endIndex):指定起点和终点，返回这中间的字符串，不包括终点。**

concat():连接两字符串，与“+”运算符相同。

replace(char originial,char replacement):用一个字符在调用字符串中所有出现另一个字符的地方进行替换。

**trim():返回调用字符串对象的一个副本，但是所有起始和结尾的空白符都被删除了(字符中间的空白符未被删除)。**

toLowerCase():将所有字符改为小写

toUpperCase():将所有字符改为大写

append():将任何数据类型的字符串表示连接到调用的StringBuffer对象的末尾。

insert(int index，String str)：将一个字符串插入到另一个字符串中指定位置。

**reverse():颠倒StringBuffer对象中的字符**

delete(int startIndex,int endIndex),delete(int loc):调用对象中删除一串字符。



## 2.Java 中的基本数据类型占据几个字节？

<font color="red">byte，boolean(1字节)；**char,short(2字节)；**int,float(4字节)；long,double(8字节)</font>

对应封装类为：Integer ,Double ,Long ,Float, Short,Byte,Character,Boolean，需要注意的是使用泛型的时候必须使用包装类，或者说泛型参数只能是引用数据类型。

<font color="red">基本数据类型和对应的包装类的区别?</font>

当基本数据类型变量和包装类的对象作为类的实例变量时，<font color="red">默认初始值是不同的</font>。包装类的对象默认初始值是 null，基本数据类型变量的默认初始值为对应的0值。另外，<font color="red">包装类是不可变类。</font>

<font color="red">Java中原子操作类的实现：</font>Java提供的原子类是靠sun基于CAS实现的，以AtomicInteger为例，它提供了get和set方法，compareAndSet方法（如果该方法成功执行，那么将实现与读取/写入一个volatile变量相同的内存效果），以及原子的添加、递增和递减等方法。

***Integer 的缓存策略：***

在Java1.5中，为Integer的操作引入**缓存策略**来节省内存和提高性能。整型对象Integer在内部实现了缓存和重用。该规则适用于整数区间 -128 到 +127。这种缓存策略仅在自动装箱时有用，使用构造器创建的Integer对象不能被缓存。

<font color="red">Integer a = 10; //自动装箱；</font>

**<font color="red">Integer b = Integer.valueOf(10); //主动装箱；</font>**

其他缓存的对象：这种缓存行为不仅适用于Integer对象。针对所有整数类型的类都有类似的缓存机制。

Byte，Short，Long 有固定范围: -128 到 127。对于 Character, 范围是 0 到 127。除了 Integer 可以通过参数改变范围外，其它的都不行。**包装类作为参数传递时，仍是按值传递。**



## **3. java运算符优先级记忆口诀**

<font color="red">单目乘除为关系，逻辑三目后赋值。</font>

单目：单目运算符+ –(负数) ++ -- 等 

乘除：算数单目运算符* / % + - 

位：位移单目运算符<< >> 

关系：关系单目运算符> < >= <= == != 

逻辑：逻辑单目运算符&& || & | ^ 

三目：三目单目运算符A > B ? X : Y 

后：无意义，仅仅为了凑字数 

赋值：赋值=。



## **4. 强制类型转换的一些规矩**

1）对< int 的数据类型（byte,char,short）进行运算时，首先会把这些类型的变量值强制转为 int 类型，对 int 类型的值进行运算，最后得到的值也是int 类型的。

<font color="red">例子: short s1=1; s1=s1+1;</font>

<font color="red">编译出错。正确的写法是 short s1 = 1; s1 = (short) (s1 + 1);</font>

**<font color="red">例子: short s1 = 1;s1 += 1; </font>** **编译通过。**

**2）基本数据类型和 boolean 类型是不能相互转换的。因此不要再程序中使用！，容易导致错误。**

3）char 类型的数据转为高级类型时，会转换为对应的 ASCII 码。

自动转换按从低到高的顺序转换。不同类型数据间的优先关系如下：

低 ---------------------------------------------> 高

byte,short,char-> int -> long -> float -> double



## **5. Java程序初始化过程**

<font color="red">先父类静态内容（先静态变量再静态代码块），然后子类静态内容；对于非静态内容，先初始化父类的非静态变量、非静态代码块然后再执行构造函数(构造函数在后面执行)随后在子类同样如此执行。</font>



## 6.Object 有哪些公用方法？

<font color="red">1.clone() ：创建并返回此对象的一个副本，调用时必须调super.clone()。使用该方法时必须注意深浅复制的区别。</font>

<font color="red">2.equals(Object obj)：指示其他某个对象是否与此对象的地址“相等”。</font>

<font color="red">hashCode()：返回该对象的哈希码值。</font>

<font color="red">3.wait() /notify() ：唤醒在此对象监视器上等待的单个线程。</font>

<font color="red">notifyAll()：唤醒在此对象监视器上等待的所有线程。</font>

<font color="red">4.toString()：返回该对象的字符串表示。</font>

**<font color="red">5.finalize()：当垃圾回收器确定不存在对该对象的更多引用时，由对象的垃圾回收器调用此方法。并且在下一次垃圾回收时，真正回收对象占用的内存空间。</font>**

<font color="red">6.getClass() ：返回此 Object 的运行时类。</font>

<font color="red">注意：main函数作为Java程序的入口方法，所以JVM在运行时先查找main方法。</font>



## **7. String、StringBuffer 与 StringBuilder 的区别**

1）可变与不可变

String 对象是不可变的；StringBuilder 与 StringBuffer 对象是可变的。因此在每次对 String 类型进行改变的时候，都会生成一个新的 String 对象，然后将指针指向新的 String 对象，所以经常改变内容的字符串最好不要用 String ，因为每次生成对象都会对系统性能产生影响，特别当内存中无引用对象多了以后， JVM 的 GC 就会开始工作，性能就会降低。

<font color="red">2）是否多线程安全</font>

<font color="red">String 和StringBuffer 是线程安全的。 StringBuilder并没有对方法进行加同步锁，所以是非线程安全的。</font>

<font color="red">3）初始化方式的不同</font>

<font color="red">StringBuffer 和 StringBuilder 只能用构造函数的形式来初始化。String 除了用构造函数进行初始化外，还可以直接赋值。</font>



## **8. java面向对象的三个特征**

**<font color="red">封装：就是将数据和方法封装到类中，并且只让可信的类或者对象对此进行操作，对不可信的类进行信息隐藏。</font>/**

好处是信息隐藏和模块化，提高安全性。封装的主要作用在于对外隐藏内部实现细节，增强程序的安全性。

**继承：子类可以继承父类的成员变量和成员方法。继承可以提高代码的复用性（继承和组合，在新类里创建原有类的对象）。**

继承的特性：1.单一继承。2.子类只能继承父类的非私有成员变量和方法。

**<font color="red">多态：当同一个操作作用在不同</font>对象<font color="red">时，会产生不同的结果。</font>**

多态的实现原理（实现机制）有 2 种方式来实现多态：**一种是编译时多态，另外一种是运行时多态；编译时多态是通过方法的重载来实现的，运行时多态是通过方法的重写来实现的**。方法的重载，指的是同一个类中有多个同名的方法，但这些方法有着不同的参数。在编译时就可以确定到底调用哪个方法。方法的重写，子类重写父类中的方法。父类的引用变量不仅可以指向父类的实例对象，还可以指向子类的实例对象。当父类的引用指向子类的对象时，只有在运行时才能确定调用哪个方法。特别注意：只有类中的方法才有多态的概念，类中成员变量没有多态的概念，private修饰的方法不可以被覆盖，而且不能依靠访问权限来实现重载。

使用静态分派实现编译时多态：对于Human man=new Man();来说Human是man的静态类型，Man是man的动态类型，所谓静态类型是指在编译阶段就能够确定的类型，而方法重载就是通过参数的静态类型来确定调用那个方法来执行。

使用动态分派实现运行时多态：方法重写是根据对象的实际类型来确定调用方法，如果在实际类型中没有符合的方法则从下往上在父类中寻找。



## **9.  接口与抽象类的区别**

<font color="red">语法层面上的区别：</font>

　　<font color="red">1）抽象类可以提供非抽象方法的实现细节；</font>

　<font color="red">　2）抽象类中的成员变量可以是各种类型的，而接口中的成员变量只能是public static final类型；</font>

　　**<font color="red">3）接口中不能含有静态代码块以及静态方法，而抽象类可以有静态代码块和静态方法；</font>**

　　<font color="red">4）一个类只能继承一个抽象类，而一个类却可以实现多个接口；</font>

设计层面上的区别：

　　<font color="red">1）抽象类是对整个类整体进行抽象，包括属性、行为；接口却是对类局部（行为）进行抽象。继承是 "is-A"的关系，而接口实现则是 "has-A"的关系。</font>如果一个类继承了某个抽象类，则子类必定是抽象类的种类，而接口实现则是有没有、具备不具备的关系。

　　2）抽象类作为很多子类的父类，<font color="red">是一种模板式设计。而接口是一种行为规范</font>，表明实现某个接口的类具有某种方法。



## **10 .内部类（静态内部类、成员内部类、局部内部类、匿名内部类）**

<font color="red">   1.静态内部类只能访问外部类的静态内容，非静态内部类可以访问静态内容与非静态内容。</font>

2. 创建内部类实例

创建静态内部类的实例（注意前面还是要加外部类的名字的）：

OuterClass.NestedStaticClass printer = new OuterClass.NestedStaticClass();

为了创建非静态内部类，我们需要外部类的实例：

OuterClass outer = new OuterClass();

OuterClass.InnerClass inner = outer.new InnerClass();

一步创建的内部类实例

OuterClass.InnerClass innerObject = new OuterClass().new InnerClass();

局部内部类：在外部类的方法中定义的类，其作用的范围是所在的方法内。它不能被 public,private,protected 来修饰。它只能访问方法中定义为 final类型的局部变量，匿名内部类不能有构造函数，在 Java 8 之前，如果匿名内部类需要访问外部类的局部变量，则必须使用 final 来修饰外部类的局部变量。



## 11.反射

定义：反射机制是在运行时，<font color="red">对于任意一个类，都能获悉这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意一个方法</font>。在 java 中，只要给定类的名字，那么就可以通过反射机制来获得类的所有信息。

**有 3 种方法可以得到 Class 对象：**

**1.Class.forName(“类的路径”);**

**2.类名.class**

**3.对象名.getClass()**

反射的优缺点：

<font color="red">优点：运行时动态获取类的实例，大大提高程序的灵活性。</font>

<font color="red">缺点：1）使用反射会降低程序的性能； 2）破坏了类的封装性，相对来说不安全。</font>



## **12. final** 

1.当一个类被 final 修饰的时候，<font color="red">表示该类不能被继承</font>。类中方法默认被final 修饰。

2.当 final 修饰基本数据类型的变量时，表示该值在被初始化后不能更改；当 final 修饰引用类型的变量时，表示该引用在初始化之后不能再指向其他的对象。**final 修饰的变量必须被初始化。可以在定义的时候初始化，也可以在构造函数中进行初始化。**

3.当 final 修饰方法时，表示这个方法不能被子类重写。

使用 final 方法的优点：

<font color="red">第一、把方法锁定，防止任何继承类修改它的实现；</font>

<font color="red">第二、提高程序的执行效率。</font>当要使用一个被声明为 final 的方法时，直接将方法主体插入到调用处，而不进行方法调用（如果过多的话，这样会造成代码膨胀）。



## **13. hash冲突的解决方法**

<font color="red">开放地址法</font>：如果产生hash冲突，<font color="red">则通过一个增长序列，在冲突点之前或者之后寻找空余的点；</font>

 <font color="red">再哈希法</font>：当发生冲突时，对原来哈希之后的结果再进行hash，直到不产生冲突；

 <font color="red">链地址法</font>：如果发生哈希冲突，则在冲突点上采用链表的形式，数据直接放在该链表的后面。



## **14. Java新特性**

<font color="red">1.接口中可以使用两个关键字static、</font>**<font color="red">default，使用这两个关键字可以修饰接口中的方法，这个方法就可以具体实现。</font>**

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190424104533.png)

<font color="red">表面上更加混淆了接口和抽象类的界限，但本质上可以获得一个很清晰的信号，抽象类将慢慢成为历史，带默认实现的接口将成为未来的主流。</font>

<font color="red">2.Lambda表达式</font>

<font color="red">      “如果某个方法入参中需要一个单一功能的接口，可以将该接口需要实现的方法以入参形式直接指向具体实现”</font>

​       单一功能的接口，首先必须是interface，其次这个interface只能有一个必须override的方法。以入参形式，指的是(a,b)或者(String a,String b)，有没有类型无所谓，JVM自己会帮我们找。指向，就是->符号。

基本语法:(parameters) -> expression或(parameters) ->{ statements; }



## **15. Java基础特性**

1.面向对象，编写程序更容易，不同于C++既支持面向对象又支持面向过程

2.平台无关性，作为解释性语言，由JVM隔离了各个平台之间的差异，C++为编译性语言

3.提供了丰富的API，同时支持web应用开发

4.不需要同C++一样进行内存管理，而且不支持多继承



## **16.static关键字**

**作用：为某特定数据类型或对象分配单一的内存空间，而与创建对象的个数无关；将某个方法或者属性与类关联起来，在不创建对象的情况下就能进行调用。实例变量属于对象，只有实例被创建后才会分配内存空间。**

**final定义的变量不能被修改，static定义的可以被修改。**