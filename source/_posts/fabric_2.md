---
title: Hyperledger Fabric 集群配置
date: 2019-12-27 16:10:31
tags: ["Fabric"]
categories: 区块链
---

Apache Kafka是一个分布式发布 - 订阅消息系统和一个强大的队列，可以处理大量的数据，并使您能够将消息从一个端点传递到另一个端点。 Kafka适合离线和在线消息消费。 Kafka消息保留在磁盘上，并在群集内复制以防止数据丢失。 Kafka构建在ZooKeeper同步服务之上。 它与Apache Storm和Spark非常好地集成，用于实时流式数据分析。[参考文档](http://kafka.apache.org/intro)
<!-- more -->

# Docker Compose 配置文件

## Zookeeper
```yaml
version '2'


```
