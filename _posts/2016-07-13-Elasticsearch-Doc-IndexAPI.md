<!-- ---
layout: post
title: Index API
categories: [Elasticsearch, Java]
description: Elasticsearch 在实际中的作用是越来越强了
keywords: Elasticsearch,Java,API
--- -->

[TOC]

# Index API(索引API)
索引 API 允许你创建自定义的 JSON 文档存储到Elasticsearch.

## 一.创建 JSON 文档
创建 JSON 文档的方式有多中:

- 用 `byte[]` 或者 `String` 手动创建
- 用 `Map` 创建, `Map` 会自动转化为等价的 JSON 
- 用第三方库序列化自定义的类对象,比如 `Jackson` 库
- 用内置的工具类方法 `XContentFactory.jsonBuilder()` 创建  

在内部, 每种方式都要转化为 `byte[]` . 因此,如果已经是 `byte[]` 格式,就可以直接使用. `jsonBulider` 直接构造为 `byte[]`,所以 `jsonBulider` 是效率最高的方式.

### 1.手动创建
手动创建没有任何的难度,只是要注意 [Date Fromat](https://www.elastic.co/guide/en/elasticsearch/reference/5.x/mapping-date-format.html) `yyyy-MM-dd`  

```java
String json = "{" +
        "\"user\":\"kimchy\"," +
        "\"postDate\":\"2013-01-30\"," +
        "\"message\":\"trying out Elasticsearch\"" +
    "}";
```

### 2.使用 Map
Map 是 `key:value` 的数据集合. 它呈现为 JSON 的结构:

```java
Map<String, Object> json = new HashMap<String, Object>();
json.put("user","kimchy");
json.put("postDate",new Date());
json.put("message","trying out Elasticsearch");
```

### 3.用 Jackson 序列化自定义对象
你可以使用 [Jackson](http://wiki.fasterxml.com/JacksonHome) 将自定义对象序列化为 JSON.使用时,在 Maven 中添加依赖.然后就可以使用 `ObjectMapper` 去序列化自定义对象了:

```java
import com.fasterxml.jackson.databind.*;

// instance a json mapper
ObjectMapper mapper = new ObjectMapper(); // create once, reuse

// generate json
byte[] json = mapper.writeValueAsBytes(yourbeaninstance);
```

### 4.用 Elasticsearch 内置的类
Elasticsearch 提供了内置的类 `XContentFactory` ,帮助你去创建 JSON 文档.

```Java
import static org.elasticsearch.common.xcontent.XContentFactory.*;

XContentBuilder builder = jsonBuilder()
    .startObject()
        .field("user", "kimchy")
        .field("postDate", new Date())
        .field("message", "trying out Elasticsearch")
    .endObject()
```

## 参考链接
1. [Index API](https://www.elastic.co/guide/en/elasticsearch/client/java-api/5.1/java-docs-index.html#java-docs-index-generate)