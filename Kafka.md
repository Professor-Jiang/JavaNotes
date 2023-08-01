# 一、Kafka定义

![image-20230723214958616](/Users/jiang/Desktop/Workspace/笔记文档/Kafka.assets/image-20230723214958616.png)

**Kafka传统定义**：Kafka是一个**分布式**的基于**发布/订阅模式**的**消息队列**，主要用于大数据实时处理领域。

**发布/订阅**：消息的发布者不会将消息直接发送给订阅者，而是将发布的消息分为不同的类别，订阅者值接受感兴趣的消息。

**Kafka最新定义**：KafKa是一个开源的分布式**事件流平台**（Event Streaming Platform），被广泛应用于高性能数据管道、流分析、数据集成和关键任务应用。

目前企业中比较常见的消息队列产品主要有Kafka、ActiveMQ、RabbitMQ、RocketMQ等。在大数据场景主要采用Kafka作为消息队列。在JavaEE开发中主要采用AcitveMQ、RabbitMQ、RocketMQ。传统的消息队列的主要应用场景包括：**缓存/削峰、解耦、异步通信**。

![image-20230717195641159](/Users/jiang/Desktop/Workspace/笔记文档/Kafka.assets/image-20230717195641159.png)

![image-20230717195252393](/Users/jiang/Desktop/Workspace/笔记文档/Kafka.assets/image-20230717195252393.png)

![image-20230717195522616](/Users/jiang/Library/Application Support/typora-user-images/image-20230717195522616.png)

## 1.1 消息队列的两种模式

**1）点对点模式**

**2）发布/订阅模式。 **（基本上都使用发布/订阅模式）

![image-20230723220302963](/Users/jiang/Desktop/Workspace/笔记文档/Kafka.assets/image-20230723220302963.png)

## 1.2 KafKa结构

![image-20230723221121062](/Users/jiang/Desktop/Workspace/笔记文档/Kafka.assets/image-20230723221121062.png)

# 二、KafKa安装

```shell
tar -zxvf <tar包名> -C <目录名>
# -z：压缩和解压缩 ".tar.gz" 格式；
```

![image-20230723222227180](/Users/jiang/Desktop/Workspace/笔记文档/Kafka.assets/image-20230723222227180.png)

![image-20230723222422037](/Users/jiang/Desktop/Workspace/笔记文档/Kafka.assets/image-20230723222422037.png)

![image-20230723222508820](/Users/jiang/Desktop/Workspace/笔记文档/Kafka.assets/image-20230723222508820.png)

![image-20230723222332001](/Users/jiang/Desktop/Workspace/笔记文档/Kafka.assets/image-20230723222332001.png)

kafka安装，官网下载包，tar命令解压即可。

# 三、KafKa使用

![image-20230723230930590](/Users/jiang/Desktop/Workspace/笔记文档/Kafka.assets/image-20230723230930590.png)

创建名为`first`的主题，分区数量为`1`，分区副本为`3	`。

![image-20230723231357014](/Users/jiang/Desktop/Workspace/笔记文档/Kafka.assets/image-20230723231357014.png)

查看主题的详情信息，发现当前的分区为0，有三个分区副本，分别在三台机器上`2，0，1`。Leader为`2`，说明Hadoop104机器上的分区是工作分区，当前机器和Hadoop上的分区都是作为follower的，是不工作的。【只有leader的分区是工作的】

![image-20230723231459250](/Users/jiang/Desktop/Workspace/笔记文档/Kafka.assets/image-20230723231459250.png)

![image-20230723231641119](/Users/jiang/Desktop/Workspace/笔记文档/Kafka.assets/image-20230723231641119.png)

接下来，修改分区数量为`3`，【注意：分区数量只能增加，不能减少】。

![image-20230723232101985](/Users/jiang/Desktop/Workspace/笔记文档/Kafka.assets/image-20230723232101985.png)

创建生产者，向topic中传递数据。创建消费者，查看数据是否被传递到topic中。

![image-20230723232500930](/Users/jiang/Desktop/Workspace/笔记文档/Kafka.assets/image-20230723232500930.png)

![image-20230723232727602](/Users/jiang/Desktop/Workspace/笔记文档/Kafka.assets/image-20230723232727602.png)

发现读不到数据，因为需要先订阅后发布。那么现在再生产一条消息。

![image-20230723232835633](/Users/jiang/Desktop/Workspace/笔记文档/Kafka.assets/image-20230723232835633.png)

![image-20230723232902418](/Users/jiang/Desktop/Workspace/笔记文档/Kafka.assets/image-20230723232902418.png)

可以发现，现在可以收到消息了。那么之前的消息能不能获取到呢。加上一个参数即可。`--from-beginning`，现在历史消息也可以读取到了。

![image-20230723233047620](/Users/jiang/Desktop/Workspace/笔记文档/Kafka.assets/image-20230723233047620.png)

# 四、KafKa生产者原理

![image-20230723234326611](/Users/jiang/Desktop/Workspace/笔记文档/Kafka.assets/image-20230723234326611.png)



测试一下 version2.0
复现pull代码的错误

