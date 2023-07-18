# Kafka

**Kafka传统定义**：Kafka是一个分布式的基于发布、订阅模式的消息队列，主要用于大数据实时处理领域。

**发布/订阅**：消息的发布者不会将消息直接发送给订阅者，而是将发布的消息分为不同的类别，订阅者值接受感兴趣的消息。

**Kafka最新定义**：KafKa是一个开源的分布式事件流平台（Event Streaming Platform），被广泛应用于高性能数据管道、流分析、数据集成和关键任务应用。

目前企业中比较常见的消息队列产品主要有Kafka、ActiveMQ、RabbitMQ、RocketMQ等。在大数据场景主要采用Kafka作为消息队列。在JavaEE开发中主要采用AcitveMQ、RabbitMQ、RocketMQ。传统的消息队列的主要应用场景包括：**缓存/削峰、解耦、异步通信**。

![image-20230717195641159](/Users/jiang/Library/Application Support/typora-user-images/image-20230717195641159.png)

![image-20230717195252393](/Users/jiang/Library/Application Support/typora-user-images/image-20230717195252393.png)

![image-20230717195522616](/Users/jiang/Library/Application Support/typora-user-images/image-20230717195522616.png)

**消息队列的两种模式：**

**1）点对点模式**

**2）发布/订阅模式。 **（基本上都使用发布/订阅模式）

![image-20230717195858987](/Users/jiang/Library/Application Support/typora-user-images/image-20230717195858987.png)

