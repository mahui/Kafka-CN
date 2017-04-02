# 介绍 Introduction

> based on http://kafka.apache.org/intro

## Apache Kafka™ 是一个消息流平台。这是什么意思？

我们认为流平台有以下三个关键功能：

1. 它允许你发布／订阅记录流。在这方面它与消息队列或者企业消息系统类似。
2. 它允许你以容错方式存储记录流。
3. 它允许你处理消息流。

Kafka 能做什么？

它被用于两大类应用：

1. 构建系统或应用间可以可靠获取数据的实时数据流管道。
2. 构建转换或反应数据流的实时流应用程序。

为了理解 Kafka 是如何做到以上事情的，让我们自下而上的深入了解探索 Kafka 的功能。

首先了解一些概念：

- Kafka 可以单个服务运行，也可以多个服务集群运行。
- Kafka 以 topic 对消息流进行分类存储。
- 每一条记录包含一个 key，一个 value，和一个时间戳（timestamp）。

Kafka 有四个核心 API：

- [Producer API](http://kafka.apache.org/documentation.html#producerapi) 允许应用程序发布消息流到一个或者多个 Kafka topic。
- [Consumer API](http://kafka.apache.org/documentation.html#consumerapi) 允许应用程序订阅一个或者多个 topic，并且可以处理发布到这些 topic 上的消息流。
- [Streams API](http://kafka.apache.org/documentation/streams) 允许应用程序充当流处理器，从一个或者多个 topic 中消费一个输入流（input stream），并且产生一个输出流（output stream）到一个或者多个 topic，有效的将输入流（input stream）转换为输出流（output stream）。
- [Connector API](http://kafka.apache.org/documentation.html#connect) 允许构建一个可重复使用的 producer 或者 consumer ，可以将已有的应用或数据系统连接到 Kafka topic。比如，一个关系型数据库的 connector 可以捕获表的每一次更改。

![](http://kafka.apache.org/0102/images/kafka-apis.png)

Kafka 中客户端和服务端之间的通讯用的基于简单的，高性能的，语言无关的 [TCP 协议](https://kafka.apache.org/protocol.html)。 该协议版本化，并保持与旧版本协议的兼容。我们推荐 Java 客户端，当然也有[很多语言](https://cwiki.apache.org/confluence/display/KAFKA/Clients)的客户端可用。

## Topics 和 Logs

让我们首先深入了解下 Kafka 提供的消息流核心抽象 —— 主题（topic）。

主题是发布过的记录的分类或是 feed 名称。Kafka 的主题总是多订阅者的；也就是说，一个主题可以有 0 个，1 个或者多个消费者 (consumer) 可以订阅写进相应主题的数据。

对于每一个主题，Kafka 集群维护如下的一个分区日志：

![](http://kafka.apache.org/0102/images/log_anatomy.png)

每一个分区是一个有序的，不可变记录序列，连续的提交到一个结构化的提交日志中。分区中的每一条记录都会分配一个称为 offset 的 id 序列号，可以独一无二的分辨出分区中的每一条记录。

Kafka 集群根据配置的保留期保留所有发布的记录，无论它们是否被消费。例如，如果保留期被配置为两天，那么，在记录被发布后的两天内是可以被消费的，过了两天时间，则会被丢弃，释放空间。Kafka 的性能是对于数据大小连续增长方面是有效的，所以长时间存储数据不是问题。

![](http://kafka.apache.org/0102/images/log_consumer.png)

事实上，每一个消费者基础保留的唯一元数据是消费者在 log 中的 offset 或者 position 。offset 被消费者控制：通常，消费者会在读取记录时线性的提高其 offset，但实际上，由于位置是由消费者控制的，所以它可以以任何顺序消费记录。比如，一个消费者可以重置到一个旧的 offset 去处理过去的数据，或者跳过最近的记录，消费最新的数据。

这些功能的组合意味着 Kafka 的消费者非常轻量级 —— 它可以加入或者离去，而不对集群或者其它消费者造成很大影响。比如，你可以用我们的命令行工具去浏览任意主题中的内容而不用去改变任何被已有消费者消费过的记录。
