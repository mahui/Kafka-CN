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

Kafka 中客户端和服务端之间的通讯用的基于简单的，高性能的，语言无关的 [TCP 协议](https://kafka.apache.org/protocol.html)。 该协议版本化，并保持与旧版本协议的兼容。我们推荐 Java 客户端，当然也有[很多语言](https://cwiki.apache.org/confluence/display/KAFKA/Clients)的客户端可用。

## Topics 和 Logs
