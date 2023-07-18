# 1、Redis的命令

`Set key value EX longTime` 设置过期时间

`ttl key`查询指定可以的过期时间，如果是负数，表明已经过期了。

<img src="/Users/jiang/Desktop/Workspace/笔记文档/Kafka.assets/image-20230717151839607.png" alt="image-20230717151839607" style="zoom:50%;" />

当我们在执行Redis事务时，需要注意一点，它仅会阻止其他客户端对Redis服务器状态的改变，但会同时允许服务器从其他客户端接收相应的命令。