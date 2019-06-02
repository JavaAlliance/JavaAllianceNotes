#         SpringBoot整合ActiveMq教程

## 一、下载安装ActiveMq

 　  下载地址: http://activemq.apache.org/download.html,

​      可以下载windows版本，也可以下载linux版本

### 1.1 、window下 ActiveMQ安装

ActiveMQ部署其实很简单，和所有Java一样，要跑java程序就必须先安装JDK并配置好环境变量，这个很简单。然后解压下载的apache-activemq-5.10-20140603.133406-78-bin.zip压缩包到一个目录，得到解压后的目录结构如下图：

<img src="https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190602154344.png"/>

进入bin目录，发现有win32和win64两个文件夹，这2个文件夹分别对应windows32位和windows64位操作系统的启动脚本。

<img src="https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190602154421.png"/>

　　

我的实验环境是windows64位，就进入win64目录，会看到如下目录结构。

<img src="https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190602154508.png"/>

其中activemq.bat便是启动脚本，双击启动。

<img src="https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190602154638.png"/>

ActiveMQ默认启动到8161端口，启动完了后在浏览器地址栏输入：[http://localhost:8161/admin]

要求输入用户名密码，默认用户名密码为admin、admin，这个用户名密码是在conf/users.properties中配置的。输入用户名密码后便可看到如下图的ActiveMQ控制台界面了。

<img src="https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190602154837.png"/>

接下来，介绍一下Queues下的控制台

![1559462866960](C:\Users\19643\AppData\Roaming\Typora\typora-user-images\1559462866960.png)

Number Of Pending Messages 等待消费的消息 这个是当前未出队列的数量。可以理解为总接收数-总出队列数

Number Of Consumers 消费者 这个是消费者端的消费者数量

Messages Enqueued 进入队列的消息 进入队列的总数量,包括出队列的。 这个数量只增不减
Messages Dequeued 出了队列的消息 可以理解为是消费者消费掉的数量
（**解释：这个要分两种情况理解**
**在queues里它和进入队列的总数量相等(因为一个消息只会被成功消费一次),如果暂时不等是因为消费者还没来得及消费。**
**在 topics（类似于发布订阅模式，可以一个生产者对应多个消费者）里 它因为多消费者从而导致数量会比入队列数高。**）
简单的理解上面的意思就是
当有一个消息进入这个队列时，等待消费的消息是1，进入队列的消息是1。
当消息消费后，等待消费的消息是0，进入队列的消息是1，出队列的消息是1.
在来一条消息时，等待消费的消息是1，进入队列的消息就是2.

没有消费者时 Pending Messages 和 入队列数量一样
有消费者消费的时候 Pedding会减少 出队列会增加
到最后 就是 入队列和出队列的数量一样多

以此类推，进入队列的消息和出队列的消息是池子，等待消费的消息是水流。



### 1.2.Linux下的 ActiveMQ安装

安装过程比较简单, 在centos中, 解压出来, 就算是安装好了

运行方法：

<img src="https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190602154732.png"/>

运行起来后, 可以通过 ip:8161 来查看是否成功. 

<img src="https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190602155120.png"/>

点击红框中的链接, 会出现登录弹框, 账号密码默认都是admin.

<img src="https://javaalliance.oss-cn-shenzhen.aliyuncs.com/img/20190602155153.png"/>

## 二、在SpringBoot项目的pom.xml文件里面导入以下依赖

```
<!-- activeMq -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-activemq</artifactId>
</dependency>
<dependency>
    <groupId>org.apache.activemq</groupId>
    <artifactId>activemq-pool</artifactId>
</dependency>
```

##  三、在SpringBoot的application.properties里添加配置如下

```
#activeMq
spring.activemq.broker-url=tcp://127.0.0.1:61616
spring.activemq.user=admin
spring.activemq.password=admin
spring.activemq.in-memory=false
#true表示使用连接池
spring.activemq.pool.enabled=false
```

##  四、实际应用

先定义一些常量存放在一个类中：

```java
package com.alliance.service.mq;

/**
 * @Auther: 19643
 * @Date: 2019/6/2 13:46
 * @Description:
 */
public class MQQueueType {
    public static final String MQ_DEFAULT = "MQ_DEFAULT";
   //在该类里面可以继续添加我们自定义的常量
}

```

**消息的生产者：**

```java
package com.alliance.service.mq;

import org.apache.activemq.command.ActiveMQQueue;
import org.springframework.jms.core.JmsMessagingTemplate;
import org.springframework.stereotype.Service;

import javax.annotation.Resource;

/**
 * @Auther: 19643
 * @Date: 2019/6/2 13:44
 * @Description:
 */
@Service
public class MessageProducerServiceImpl {
    @Resource
    private JmsMessagingTemplate jmsMessagingTemplate;

  /*  *//**
     * 向默认队列发送消息
     *
     * @param msg
     *//*
    public void sendMsgD(String msg) {
        jmsMessagingTemplate.convertAndSend(new ActiveMQQueue(MQQueueType.MQ_DEFAULT), msg);
    }
*/
    /**
     * 向指定队列发送消息
     *
     * @param queueName 指定队列名称
     * @param msg       消息内容
     */
    public void sendMsgO(String queueName, String msg) {
        jmsMessagingTemplate.convertAndSend(new ActiveMQQueue(queueName), msg);
    }

}
```

**消息的消费者：**      可以看到下面的各个方法上都有一个 @JmsListener注解，时时刻刻监听着对应的队列，一旦对列里面有消息传来，就立马消费

```java
package com.alliance.service.activemq;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.jms.annotation.JmsListener;
import org.springframework.stereotype.Service;

/**
 * @author 19643
 * @date 2018/10/26
 */
@Service
public class MessageConsumerService {
    private static final Logger log = LoggerFactory.getLogger("active");

    @JmsListener(destination = MQQueueType.MQ_DEFAULT)
    public void defaultMq(String text) {
        log.info("【*** " + MQQueueType.MQ_DEFAULT + " 接收消息 ***】" + text);
    }
         //destination是指 该消费者监听的队列名称
    @JmsListener(destination = MQQueueType.MQ_STUDY)  
    public void receiveMessage(String text) {
        log.info("【*** " + MQQueueType.MQ_STUDY + " 接收消息 ***】" + text);
    }
         //destination是指 该消费者监听的队列名称
    @JmsListener(destination = MQQueueType.MQ_GOODS)
    public void receiveGoodsMessage(String text) {
        log.info("【*** " + MQQueueType.MQ_GOODS + " 接收消息 ***】" + text);
    }

}
```

