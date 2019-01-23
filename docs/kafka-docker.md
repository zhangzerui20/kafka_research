# kafka集群联调环境搭建

## 介绍

本项目学习kafka的方式为：**实践+调试源码**，首先搭建一个kafka的**docker集群**联调环境。

使用linux的同学，可以方便地按照下面文档中的指引，构建一个**host**的集群。

> https://github.com/confluentinc/cp-docker-images/wiki/Getting-Started

考虑到大部分同学，使用的是win或者mac，这两个操作系统不支持host网络模式。这里我又搭建了一个**bridge**网络模式的docker集群。docker中的容器运行在**内网**，我们作为**外网**的client去访问，更加类似于**云上的环境**。后面的实验都使用的是bridge网络模式。

## listener机制

kafka提供了listener的配置项，来区分内网、外网。这里介绍一下listener，listener是kafka中的一个抽象的概念，一个listener是以下三个元素的集合：

1. Host/IP
2. Port
3. Protocol

举个例子来说，对于本实验中的内外网的这种情况，我们需要配置以下两个listener(假设kafka-host为broker的容器名)：

``` shell
INTER://kafka-host:9092
OUTER://localhost:19092
```

这样的话，对于内部的其他broker，通过INTER这个listener来访问；对于docker网络外部的client，通过OUTER来访问。

那么**从broker的角度**来说，一个listener配置表示：

- broker会监听一个配置中的port
- broker使用不同的protocol去解析该port的请求
- 当有client向broker请求metadata的时候，会返回配置中的Host，client会用这个Host去访问数据

## 运行kafka docker集群

进入kafka_container目录，运行一下：

```shell
docker-compose up
```

观察日志，没有异常则启动成功。

成功后可以使用以下几个命令来验证环境是否搭建成功(以win系统为例)：

```
kafka-topics.bat -zookeeper localhost:21810 --create -topic tre --partitions 1 --replication-factor 1 //创建topic

kafka-console-producer.bat --broker-list localhost:29092 --topic tre //生产消息
kafka-console-consumer.bat --bootstrap-server localhost:29092 --topic tre //读取消息
```

如果网络连接异常，可以使用kafka-cat这个工具，这个工具可以模仿client获取metadata的行为，打印出获取到的metadata，可以看到是不是listener没有配置对。



## 附录

一个kafka开发的文章，详细解释了listener

https://rmoff.net/2018/08/02/kafka-listeners-explained/ 

kafka官方的镜像git

https://github.com/confluentinc/cp-docker-images/wiki/Getting-Started