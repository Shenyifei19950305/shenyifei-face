这边是自己整理的 具体可以看讲师的笔记

消息队列

生产者 消费者 中间的消息中间件 只涉及消息的发送和接受 实现系统轻松的解耦

属于异步分布式系统

 

为什么不用activemq 开源 但是数据完整度没有rabbitmq好 kafaka又适合大数据的高吞吐量 一般的项目没必要用到kafaka

 

Rabbitmq基于AMQO协议

适合跨平台的网络交互数据格式

 

这边省略rabbitmq的安装、

有关虚拟机 交换机 队列的创建都在rabbitmq的管理页面进行创建

 

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml1948\wps1.jpg) 

 

Server就是rabbitmq内部

所![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml1948\wps2.jpg)

 

用的比较多的是前面五种

第一种 就是普通的单点消息收发 第二种其实可以理解为多个服务器一起领取消息 处理消息阻塞超时的问题 

第三种是广播模式 fanout 可以理解群发 邮件群发就是采用这种模式 这里涉及了交换机的概念

第四种订阅发布问题 direct 加入了路由关键字 就有点类似消息加了权限 根据关键字让不同的消息消费者收到不同的内容

第五种种是在第四种的模式上加入可通配符 

 

 

代码可以看教师 笔记 这边就不贴了，因为后期开发用的是springboot重点说下 就可以了

步骤就是 先创建一个工厂模式 这边工厂模式需要重点说下 因为是rabbitmq是重量级的

所以希望只被加载一次 所以 需要私有静态  不能只看笔记（笔记没有把冗余部分封装成ulti类） 要去看代码

 

第二种方式 一般消息消费都是采用轮询的 就是平均分配 也就是所谓确认自动机制开启

，但是会出现问题 一旦其中一个消费服务器宕机了 数据就会丢失

所以 一般要关闭自动确认 要手动确认消息 可以设置通道每次消费多少条消息 避免丢失

 

一般都用springboot开发比较简单明了

MQ的应用场景

异步处理

传统的没有用消息中间件一般

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml1948\wps3.jpg) 

用了消息中间件

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml1948\wps4.jpg) 

 

应用解耦

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml1948\wps5.jpg) 

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml1948\wps6.jpg) 

 

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml1948\wps7.jpg) 

 

最后就是镜像集群 有点类似哨兵机制

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml1948\wps8.jpg) 

主从复制完后采用镜像同步 保证了支持主动切换