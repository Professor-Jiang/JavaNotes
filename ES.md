# ES

## 一、基础操作

## 1、什么是elasticsearch？

ES是一个开源的高扩展的**分布式全文搜索引擎**，是整个Elastic Stack技术栈的核心。他可以近乎实时的存储、检索数据，本身扩展性很好，可以扩展到上百台服务器，处理PB级别的数据。

![img](https://cdn.nlark.com/yuque/0/2023/png/32673675/1692027927167-1e0eb413-d169-420f-8f14-07324fc2043b.png)



![img](https://cdn.nlark.com/yuque/0/2023/png/32673675/1692159772834-2bb7cb40-c94c-41ea-a3a1-970adfd3a6af.png)

![img](https://cdn.nlark.com/yuque/0/2023/png/32673675/1692159810478-1dec604a-ca16-41fa-9a51-970e79072cd2.png)

## 2、创建索引

![img](https://cdn.nlark.com/yuque/0/2023/png/32673675/1692159908452-25d61636-bdde-4d88-b447-dcb7525e1168.png)

![img](https://cdn.nlark.com/yuque/0/2023/png/32673675/1692159983821-b63b2890-5c1a-4e0c-9af7-b00d9d6706a2.png)

![img](https://cdn.nlark.com/yuque/0/2023/png/32673675/1692160029677-33365af3-7d00-4dc2-8e4d-568382b9c934.png)

![img](https://cdn.nlark.com/yuque/0/2023/png/32673675/1692545789421-bf8648ff-f80f-40b7-b31e-01a48eb0db8a.png)

创建索引只能使用put请求，不能使用post。如果PUT完，再次PUT，则提示已经存在。说明PUT是满足幂等性的。post为什么不能使用，因为它不具有幂等性。

## 3、如何查看所有的索引信息？

使用`http://127.0.0.1:9200/_cat/indices?v`

![image-20230821225221227](ES.assets/image-20230821225221227.png)

## 4、删除索引

![image-20230821225644641](ES.assets/image-20230821225644641.png)

![image-20230821225710790](ES.assets/image-20230821225710790.png)

## 5、创建文档

![image-20230821224758622](ES.assets/image-20230821224758622.png)

索引创建好之后，接下来创建文档，创建文档可以使用post请求。

![image-20230820234255388](ES.assets/image-20230820234255388.png)

多次提交可以发现返回的id不同，说明post请求是不具有幂等性的。那么如果实现幂等性如何操作呢？-->指定id

## 5、创建文档时指定id

![image-20230820234522395](ES.assets/image-20230820234522395.png)

![image-20230821230600910](ES.assets/image-20230821230600910.png)

## 6、查询文档数据

### 6.1 根据id查询

![image-20230821231022350](ES.assets/image-20230821231022350.png)

### 6.2 查询所有数据

![image-20230821231055820](ES.assets/image-20230821231055820.png)

## 7、更新文档数据

### 7.1 全量数据更新

![image-20230821231203330](ES.assets/image-20230821231203330.png)



![image-20230821231408158](ES.assets/image-20230821231408158.png)

### 7.2 局部数据更新

![image-20230821231421547](ES.assets/image-20230821231421547.png)

## 8、删除文档数据

![image-20230821231532091](ES.assets/image-20230821231532091.png)

## 9、按照条件查询

![image-20230821231636205](ES.assets/image-20230821231636205.png)将条件输入到请求题当中，注意还是GET请求。

![image-20230821231810651](ES.assets/image-20230821231810651.png)

全量查询

![image-20230821231909429](ES.assets/image-20230821231909429.png)

## 10、分页查询

通过size自动将数据划分为若干页，之后通过from，指定需要查询的页的开始位置。

![image-20230821232051115](ES.assets/image-20230821232051115.png)

通过"_source"指定想查看的字段

![image-20230821232551864](ES.assets/image-20230821232551864.png)

## 11、数据排序

![image-20230821232712521](ES.assets/image-20230821232712521.png)

## 12、条件查询

需要满足多个条件如何查询

![image-20230821232931584](ES.assets/image-20230821232931584.png)

![image-20230821233036545](ES.assets/image-20230821233036545.png)

满足或如何查询？

![image-20230821233139523](ES.assets/image-20230821233139523.png)

如果再加个">"条件呢？

![image-20230821233304391](ES.assets/image-20230821233304391.png)

## 13、ES的倒排索引

![image-20230821233523996](ES.assets/image-20230821233523996.png)

看这个例子，为什么可以根据“小华”查询到数据，因为小华被分词了，先根据”小“查询，又根据“华”查询。![image-20230821233613725](ES.assets/image-20230821233613725.png)

如果想精准匹配，需要使用`match_phrase`![image-20230821233826375](ES.assets/image-20230821233826375.png)

## 14、对查询内容进行高亮显示

指定需要高亮哪些字段。

![image-20230821234047547](ES.assets/image-20230821234047547.png)

## 15、聚合操作

![image-20230822224440696](ES.assets/image-20230822224440696.png)

## 16、映射关系

![image-20230822224847978](ES.assets/image-20230822224847978.png)

```json
"type" : "text" 表示可以分词；
"type" : "keyword" 表示可以不可以分词；
"index" :  true 表示建立索引；

```

![image-20230822225105320](ES.assets/image-20230822225105320.png)

![image-20230822225136401](ES.assets/image-20230822225136401.png)

如果index为true，默认是不展示的，如果是false，那么会展现出来。

接下来往user索引中添加一条数据，如下图。

![image-20230822225331179](ES.assets/image-20230822225331179.png)

接下来做查询：

![image-20230822225522025](ES.assets/image-20230822225522025.png)

![image-20230822225632294](ES.assets/image-20230822225632294.png)

由上面的图，可以看出keyword的作用。

![image-20230822225713042](ES.assets/image-20230822225713042.png)

![image-20230822225758290](ES.assets/image-20230822225758290.png)

![image-20230822225826559](ES.assets/image-20230822225826559.png)

现在index的作用也体现出来了。

# 二、Java访问ES

Elasticsearch软件是由Java语言开发的，所以支持通过Java API的方式对elasticsearch服务进行访问。

1、创建一个maven项目，然后添加es的依赖。





# 三、ES集群

单台Elasticsearch服务器提供服务，负载能力有限，超过阈值，服务器性能就会大大降低甚至不可用，所以生产环境中，ES以集群方式存在。

一个ES集群有一个唯一的名字标识，这个名字默认是`elasticsearch`，节点可以通过指定ES集群名称来选择加入这个集群。

集群中包含很多服务器，一台服务器上可能有多个节点。一般来讲为了高可用，一台服务器对应一个节点。节点作为集群的一部分，它存储数据，参与集群的索引和搜索功能。

## 3.1 windows环境下集群部署

![image-20230831231640941](ES.assets/image-20230831231640941.png)

集群在config下配置。

![image-20230831231729990](ES.assets/image-20230831231729990.png)

![image-20230831231805001](ES.assets/image-20230831231805001.png)

### （1）去掉注释，设置集群名称。设置节点名称。

![image-20230831231931248](ES.assets/image-20230831231931248.png)

### （2） 设置主机名称、设置端口号。

![image-20230831232016417](ES.assets/image-20230831232016417.png)

### （3）设置通信端口，设置当前节点可以为master节点，可以为数据节点。

![image-20230831232311862](ES.assets/image-20230831232311862.png)

![image-20230831232251904](ES.assets/image-20230831232251904.png)

### （4）设置允许跨域

![image-20230831232408552](ES.assets/image-20230831232408552.png)

此时一个节点设置好了，启动。启动完毕后，通过postman查看状态：

![image-20230831232608504](ES.assets/image-20230831232608504.png)

再添加两个节点。直接复制elasticsearch文件夹，然后分别修改他们的配置文件。

集群名称必须一致。节点名称分别为node-1002，node-1003。端口分别为1002，1003。通信端口分别为9302，9303。

![image-20230831232940727](ES.assets/image-20230831232940727.png)

**另外，需要额外添加查找模块配置。**

![image-20230831233231406](ES.assets/image-20230831233231406.png)

![image-20230831233456075](ES.assets/image-20230831233456075.png)

![image-20230831233559286](ES.assets/image-20230831233559286.png)

## 3.2 Linux环境下集群部署

### （1）单点一些小配置

![image-20230831233830426](ES.assets/image-20230831233830426.png)

![image-20230831234125342](ES.assets/image-20230831234125342.png)

![image-20230831234152065](ES.assets/image-20230831234152065.png)

### （2）对于集群，首先要安装es，然后修改配置。同样也需要对各个节点按照单点配置一下。

![image-20230831234802959](ES.assets/image-20230831234802959.png)

![image-20230831234900517](ES.assets/image-20230831234900517.png)

上面的配置端口需要修改一下，如果你是在一台机器上部署了多个节点。，可以通过如下访问方式来查看节点数量。

![image-20230831235512934](ES.assets/image-20230831235512934.png)

# 四、Elasticsearch进阶

**1 索引（index）：**

**2 文档（document）：**一个文档是一个可被索引的基础信息单元，也就是一条数据。**文档以json的形式存在**，在一个index里面，你可以存储任意多的文档。

**3 字段（field）：**相当于数据表的字段。

**4 映射（mapping）：**mapping是处理数据的方式和规则方面做一些限制，如：某个字段的数据类型、默认值、分析器、是否被索引等等。这些都是映射里面可以设置的。

**5 分片（shards）：**

![image-20230901000350898](ES.assets/image-20230901000350898.png)

![image-20230901000403251](ES.assets/image-20230901000403251.png)

**5 副本（Replicas）：**

![image-20230901000518070](ES.assets/image-20230901000518070.png)

**6 分配（Allocation）**

![image-20230901000629402](ES.assets/image-20230901000629402.png)

## 4.1 系统架构

![image-20230911230602047](ES.assets/image-20230911230602047.png)

P0、P1、P2是数据的分片，R0、R1、R2是副本。

## 4.2 单节点集群

我们在一个集群（仅包含一个节点）中创建名为users的索引，同时，我们设置分片数量为3，副本数为1。

```json
{
  "settings":{
			"number_of_shards" : 3,
			"number_of_replicas" : 1
  }
}
```

![image-20230911231253110](ES.assets/image-20230911231253110.png)

然后可以使用get的请求方式，查看是否设置成功。

现在由于我们的集群只有一个节点，所以三个分片都在这一个节点中。如何查看呢？google有对应的插件支持。

![image-20230911231519088](ES.assets/image-20230911231519088.png)

我们输入对应的端口号，然后点击连接。

![image-20230911231702886](ES.assets/image-20230911231702886.png)

绿色的是分片，灰色的那个是副本【实际上是通过边框是否加粗来看的，边框加粗的是分片，未加粗的是副本】，此时副本的状态不正常，处于”未分配“状态。因为副本不能和与原来数据在一个节点上，这样做没有意义。

![image-20230911231606984](ES.assets/image-20230911231606984.png)

虽然是yellow状态，但是当前我们的集群是正常运行的，但存在硬件故障时丢失数据的风险。

![image-20230911232115512](ES.assets/image-20230911232115512.png)

## 4.3 多节点集群

此时，我们再启动一个节点，修改配置，加入集群。此时我们再去观察下节点的状态。

![image-20230911232433116](ES.assets/image-20230911232433116.png)

可以观察到，现在1001节点是三个分片，1002节点作为1001节点分片的副本。此时集群节点是green状态。

![image-20230911232544201](ES.assets/image-20230911232544201.png)

现在考虑一个问题，当我们的数据量非常大时，需要水平扩容，我们在集群中增加一个节点。当增加一个节点时，es集群应当自动均衡负载，重新对分片、副本进行分配。现在再查看下。

![image-20230911232913554](ES.assets/image-20230911232913554.png)

1001、1003节点上各自有一个节点被分配到节点1003上面。现在每个节点都拥有2个分片，而不是之前的3个。这表示每个节点的硬件资源将被更少的分片所共享，每个分片的性能将会得到提升。

![image-20230911233231596](ES.assets/image-20230911233231596.png)

为了增加集群的吞吐量，我们对已经建立好的es集群修改其副本数量。

![image-20230911233310056](ES.assets/image-20230911233310056.png)

可以看到，现在有三个主分片，每个分片都拥有两个副本。

![image-20230911233428979](ES.assets/image-20230911233428979.png)

现在假设有一个节点1001（它是原来的主节点，根据星号来判断的）挂掉了，那么会发生什么？

![image-20230911233640974](ES.assets/image-20230911233640974.png)

现在1002成为了主节点。1001节点数据全部丢失。集群仍然能够正常对外提供服务。

假设现在1001节点恢复了，那么会发生什么？

![image-20230911233907222](ES.assets/image-20230911233907222.png)

可以发现，主分片没有重新做选举。

## 4.4 如何往集群中存储数据

现在需要考虑数据的存储与查询。假设我们有一条数据需要存储，那么应该往哪存储？

`路由计算：hash（id）%主分片数量 = 主分片索引`

存储完毕后，节点副本同步数据，现在去哪里查询呢？用户可以访问任何一个节点获取数据，这个节点称之为协调节点，节点会自动去寻找。

![image-20230913234700379](ES.assets/image-20230913234700379.png)

也可通过设置参数，当P0保存完数据之后，就立即反馈的。

![image-20230913234935376](ES.assets/image-20230913234935376.png)

![image-20230913235031431](ES.assets/image-20230913235031431.png)

## 4.5 查询数据的流程

![image-20230913235433563](ES.assets/image-20230913235433563.png)

## 4.6 更新数据的流程

![image-20230913235741517](ES.assets/image-20230913235741517.png)

## 4.7 分片原理

![image-20230913235856439](ES.assets/image-20230913235856439.png)

## 4.8 倒排索引

![image-20230914000002048](ES.assets/image-20230914000002048.png)

分词器，

如果设置keyword不能分词，text可以分词，此外还有一些其他的配置。

词条：索引中最小存储和查询单元。

词典：字典，词条的集合，B+树，HashMap

倒排表



![image-20230914000512063](ES.assets/image-20230914000512063.png)

![image-20230914000542530](ES.assets/image-20230914000542530.png)