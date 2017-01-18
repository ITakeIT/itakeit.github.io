---
layout: post
title: Get API
categories: [Elasticsearch, Java]
description: Elasticsearch 在实际中的作用是越来越强了
keywords: Elasticsearch,Java,API
---

## Get API
GET API 可以从 Elasticsearch 获取指定 `id` 的 JSON 文档.下面的实例时,获取一个JSON文档: `index` 为 `twitter`, `type` 为 `tweet`, `id` 为 `1` :

```java
GetResponse response = client.prepareGet("twitter", "tweet", "1").get();
```

## 操作线程(Operation Threading)
GET API 允许设置线程模型.你在同一个节点上执行 API 时,会运行不同的线程模型.

可以选择不同的线程执行 API. `operationThreaded ` 默认设置为 `true`,就是 默认执行的是不同的线程.下面演示的是设置为 `false` 在相同的线程执行:

```java
GetResponse response = client.prepareGet("twitter", "tweet", "1")
        .setOperationThreaded(false)
        .get();
```

## 参考链接:
1. [GET API](https://www.elastic.co/guide/en/elasticsearch/client/java-api/5.1/java-docs-get.html)