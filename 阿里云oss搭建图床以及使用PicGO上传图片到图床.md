# 阿里云oss搭建图床以及使用PicGO上传图片到图床



最近喜欢上了使用markdown来写博客，可是markdown的图片却是本地的，如果我要发博客到GitHub上，那么就不行了，GitHub上是不能存图片的。于是干脆弄了个图床，本地截图的时候上传到图床，markdown中的代码结果也是图床里的，这样就解决了这个问题了。

## 一、开通OSS服务    oss是对象存储服务的意思

   首先购买OSS服务（在阿里云上买）地址：https://www.aliyun.com/product/oss/

有两种方式，

第一种是”按量收费“，只需要开通OSS即可，然后就完事了（默认是第一种方式”按量收费“），就可以直接用OSS服务了，不需要付款什么的，按量收费，从我们的阿里云账户直接扣钱，边用边扣的模式。

还有一种是”买套餐包月包年“，需要我们到页面付款一次结清

举例：然后选择折扣套餐。
这里我选择40g的。

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190423230307.png)

后续选择确认支付。



空间已经买了，但是流入流出的流量还没买。这里列一下流量的价格。
下面的是按流量计费的，因为使用频率较低，所以选择了按量付费。（并没有太贵，我买了好久都没花超过两块钱，虽然我存的东西少，日常就是存博文的图片）(**好像流量太低的话是不会扣费的，会进行累计？**，反正我弄了好久，偶尔才收到扣我三毛钱的消息。)

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190423230532.png)



## 二、然后在控制台新建Bucket

bucket的作用：相当于一个存储图片的”屋子“。然后下图就是配置”屋子“的信息，我们通过这个”屋子“的信息把图片上传到这个”屋子“里面

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190423230708.png)

创建bucket之后，我们的存储空间就创建成功了。



#              使用PicGo实现快速上传图片

PicGo是一个开源项目，可以用来快捷上传图片到图床。

可以去https://github.com/Molunerfinn/PicGo这个地址去下载picGo

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190423231239.png)

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190423231331.png)##





## 一、配置图床

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190423231443.png)

- accesskey和accesskeySecret可以从阿里云控制台获取。
- 存储空间名是bucket的名字。
- 存储区域到阿里云OSS控制台去找，例如下图中存储区域就是 oss-cn-shenzhen
- ![1556032541014](C:\Users\19643\AppData\Roaming\Typora\typora-user-images\1556032541014.png)
- 存储路径是存储图片的位置，要求以/结尾。
- 自定义域名可以不填写。



## 二、开始上传

配置了之后，在截图了之后，可以点击剪贴板图片上传，也可以使用快捷键快速上传剪贴板图片（快捷键在PICGO设置中可以设置，）

![](https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190423231816.png)

上传之后，会自动拷贝链接格式，粘帖得到的结果是链接格式。上面我们选择的是markdown，所以粘帖结果会是markdown引用图床图片的结果。