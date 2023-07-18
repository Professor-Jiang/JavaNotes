## 一、Promethues简介

## 二、Prometheus数据模型

采集的监控数据均以指标（metric）的形式保存在内置的**时间序列数据库**（TSDB）当中。每一条时间序列由指标名称（metric name）以及一组标签（键值对）唯一标识。其中指标名称反应被监控样本的含义（例如：http_requests_total标识系统的http请求数量）,指标名称由ASCII字符、数字、下划线以及冒号组成。其中以 `__` 作为前缀的标签，是系统保留的关键字，只能在系统内部使用。

### 2.1样本

在时间序列中的每一个点称为一个样本，样本由以下三部分组成：

- 指标：指标名称和描述当前样本特征的labelsets；
- 时间戳：毫秒级时间戳；
- 样本值：float64浮点型数据表示当前样本的值。  【可以看出是Go开发的】

### 2.2表达方式

```xml
<metric name>{<label name>=<label value>, ...}
```

指标名称，标签集合。

例如：指标名称为`api_http_requests_total`，标签为`method="POST"`和`handler="/messages"`的时间序列可以表示为：

```xml
api_http_requests_total{method="POST", handler="/messages"}
```

### 2.3指标类型

#### 2.3.1 Counter（计数器）

单调递增数据指标。例如：使用counter类型的指标来表示服务的请求数、已完成的任务数、错误发生的次数等。counter主要有两个方法：

```go
Inc()  //counter值加1
Add(float64) //counter值加上指定的值，指定的值若<0,会panic
```

Counter类型数据可以让用户了解事件产生的速率的变化。

```go
rate(http_requests_total[5m]) //通过rate()函数获取HTTP请求量的增长率
topK(10, http_requests_total) //查询当前系统中，访问量前10的HTTP地址
```

#### 2.3.2 Gauge（仪表盘）

任意变化数据指标，即可增可减。Gauge通常用于温度、内存使用率、当前并发请求数量这种指标数据。

```Go
dalta(cpu_temp_celsius{host="zeus"}[2h])  //利用dalta()函数获取CPU温度在两小时内的差异
// predict_linear()基于简单线性回归的方式，对样本数据的变化趋势做出预测。例如，基于 2 小时的样本数据，来预测主机可用磁盘空间在 4 个小时之后的剩余情况：
predict_linear(node_filesystem_free{job="node"}[2h], 4 * 3600) < 0
```

#### 2.3.3 Histogram（直方图）

看一个例子。

以系统API调用的平均响应时间为例：如果大多数API请求都维持在100ms的响应时间范围内，而个别请求的响应时间需要5s。如何统计？

一种方式是按照请求延迟的范围进行分组。例如，统计延迟在0～10ms之间的请求数有多少，而10～20ms之间的请求数又有多少。通过这种方式可以快速分析系统慢的原因。

Histogram和Summary都是为了解决上述问题，通过Histogram和Summary类型的监控指标，我们可以快速了解监控样本的分布情况。

Histogram在一段时间范围内对数据进行采样（通常是请求持续时间或响应大小等），并将其计入可配置的存储桶（bucket）中，后续可通过指定区间筛选样本，也可统计样本总数，最后一般将数据展示为直方图。

Histogram 类型的样本会提供三种指标（假设指标名称为 `<basename>`）：

- 样本的值分布在 bucket 中的数量，命名为 `<basename>_bucket{le="<上边界>"}`。解释的更通俗易懂一点，这个值表示指标值小于等于上边界的所有样本数量。

```json
// 在总共2次请求当中。http 请求响应时间 <=0.005 秒 的请求次数为0 
io_namespace_http_requests_latency_seconds_histogram_bucket{path="/",method="GET",code="200",le="0.005",} 0.0
// 在总共2次请求当中。http 请求响应时间 <=0.01 秒 的请求次数为0 
io_namespace_http_requests_latency_seconds_histogram_bucket{path="/",method="GET",code="200",le="0.01",} 0.0
// 在总共2次请求当中。http 请求响应时间 <=0.025 秒 的请求次数为0
io_namespace_http_requests_latency_seconds_histogram_bucket{path="/",method="GET",code="200",le="0.025",} 0.0 io_namespace_http_requests_latency_seconds_histogram_bucket{path="/",method="GET",code="200",le="0.05",} 0.0 io_namespace_http_requests_latency_seconds_histogram_bucket{path="/",method="GET",code="200",le="0.075",} 0.0 io_namespace_http_requests_latency_seconds_histogram_bucket{path="/",method="GET",code="200",le="0.1",} 0.0 io_namespace_http_requests_latency_seconds_histogram_bucket{path="/",method="GET",code="200",le="0.25",} 0.0 io_namespace_http_requests_latency_seconds_histogram_bucket{path="/",method="GET",code="200",le="0.5",} 0.0 io_namespace_http_requests_latency_seconds_histogram_bucket{path="/",method="GET",code="200",le="0.75",} 0.0 io_namespace_http_requests_latency_seconds_histogram_bucket{path="/",method="GET",code="200",le="1.0",} 0.0 io_namespace_http_requests_latency_seconds_histogram_bucket{path="/",method="GET",code="200",le="2.5",} 0.0 io_namespace_http_requests_latency_seconds_histogram_bucket{path="/",method="GET",code="200",le="5.0",} 0.0 io_namespace_http_requests_latency_seconds_histogram_bucket{path="/",method="GET",code="200",le="7.5",} 2.0
// 在总共2次请求当中。http 请求响应时间 <=10 秒 的请求次数为 2
io_namespace_http_requests_latency_seconds_histogram_bucket{path="/",method="GET",code="200",le="10.0",} 2.0 io_namespace_http_requests_latency_seconds_histogram_bucket{path="/",method="GET",code="200",le="+Inf",} 2.0
```

- 所有样本值的大小总和，命名为`<basename>_sum`

```json
// 实际含义： 发生的2次 http 请求总的响应时间为 13.107670803000001 秒
io_namespace_http_requests_latency_seconds_histogram_sum{path="/",method="GET",code="200",} 13.107670803000001
```

- 样本总数，命名为 `<basename>_count`。值和 `<basename>_bucket{le="+Inf"}` 相同。

```json
// 实际含义： 当前一共发生了 2 次 http 请求
io_namespace_http_requests_latency_seconds_histogram_count{path="/",method="GET",code="200",} 2.0
```

可以通过 `histogram_quantile() 函数`来计算 Histogram 类型样本的`分位数`。假设样本的 9 分位数（quantile=0.9）的值为 x，即表示小于 x 的采样值的数量占总体采样值的 90%。

#### 2.3.4 Summary（摘要）

与Histogram类似，表示一段时间内的数据采样结果，但是他直接存储了分位数（通过客户端计算，然后展示出来），而不是通过区间计算。

Summary类型样本也提供三种指标：

样本值的分位数分布情况，命名为`<basename>{quantile="<φ>"}`。

```json
// 含义：这 12 次 http 请求中有 50% 的请求响应时间是 3.052404983s  
io_namespace_http_requests_latency_seconds_summary{path="/",method="GET",code="200",quantile="0.5",} 3.052404983
// 含义：这 12 次 http 请求中有 90% 的请求响应时间是 8.003261666s 
io_namespace_http_requests_latency_seconds_summary{path="/",method="GET",code="200",quantile="0.9",} 8.003261666
```

所有样本值的大小总和，命名为 `<basename>_sum`。

```json
// 含义：这12次 http 请求的总响应时间为 51.029495508s 
io_namespace_http_requests_latency_seconds_summary_sum{path="/",method="GET",code="200",} 51.029495508
```

样本总数，命名为 `<basename>_count`。

```json
// 含义：当前一共发生了 12 次 http 请求
io_namespace_http_requests_latency_seconds_summary_count{path="/",method="GET",code="200",} 12.0
```

Histogram和Summary的异同：

- 他们都包含了`<basename>_sum`和`<basename>_count` 指标。
- Histogram 需要通过 `<basename>_bucket` 来计算分位数，而 Summary 则直接存储了分位数的值。

