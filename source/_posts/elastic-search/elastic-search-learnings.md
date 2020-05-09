---
title: Elastic Search 搜索
date: 2020-05-09 19:36:44
tags:
  - Elastic Search
categories:
  - 学习笔记
---

## 如何搜索
这里我们会尽量用java作为例子来解释如何使用ElasticSearch。[ES HighLevel REST Client](https://www.elastic.co/guide/en/elasticsearch/client/java-rest/current/java-rest-high-search.html)
### BoolQuery 的用法与特性

BoolQuery 作为一个主要的Query结构一般用于整个Query的根。Query的结果会根据score默认排序

```java
searchSourceBuilder.query(QueryBuilders.boolQueryBuilder()); 
searchRequest.source(searchSourceBuilder);
```

BoolQuery 拥有4个不同的Query类型。
```java
// 必须匹配，匹配会增加score。MatchQuery 这里表示"名字"必须包含“大卫”或者“David”其中一个, 多个匹配会增加其结果的score。
boolQueryBuilder.must(QueryBuilders.matchQuery("名字", "大卫 David")) 
// 每一个query类型都可以添加多个子Query，其关系是“AND”。并且可以是BoolQuery
boolQueryBuilder.must(QueryBuilders.boolQuery()) 
// 必须匹配，匹配不影响score。TermsQuery 这里表示"id"必须等于“123”或者“321”
boolQueryBuilder.filter(QueryBuilders.termsQuery("id", "123"， "321"))
// 非必须匹配，匹配增加score。
boolQueryBuilder.should()
// 设定最少匹配“should”的数量，如果没有规定“must”/“filter”默认值是1，有的话默认值是0
boolQueryBuilder.minimalShouldMatch(1) 
// 必须不包含，不影响score。
boolQueryBuilder.must_not()
```
<!--more-->

### 排序

默认的排序是按照score来进行的。我们也可以自己添加用来排序的键。

```java
// 按日期的排序，默认是正序
searchSourceBuilder.sort(SortBuilders.fieldSort("日期"))
// 按score排序，默认是倒序。当多个sort被提供的时候，按其先后顺序优先排列
searchSourceBuilder.sort(SortBuilders.scoreSort())
```

当搜索请求中包含sort时，搜索结果中会包含sortValues。例如以上sort的结果中，每一个hit会有如下Array

```java
result.getHits().getHits()[0].getSortValues();
// ["2020-01-01", 2.2]
```

### 翻页（Pagination）
ES 有3种主要的翻页方式。“from” + “size”， scroll， search_after  
Pagination of results can be done by using the from and size but the cost becomes prohibitive when the deep pagination is reached. The index.max_result_window which defaults to 10,000 is a safeguard, search requests take heap memory and time proportional to from + size. The Scroll api is recommended for efficient deep scrolling but scroll contexts are costly and it is not recommended to use it for real time user requests. The search_after parameter circumvents this problem by providing a live cursor. The idea is to use the results from the previous page to help the retrieval of the next page.

from+size 就是在request中定义从第几个结果开始，然后一共请求多少个结果来进行翻页，需要client持续计算下个from的值。
```java
// 第一页
searchSourceBuilder.size(100)
...
// 第二页
searchSourceBuilder.from(100)
searchSourceBuilder.size(100)
```

scroll 的工作原理是当一个搜索请求发来的时候，ES会完成搜索并且保留一个结果的副本，并返回一个scroll_id, client可以用这个scroll_id 继续发送请求去取得下一页内容。一般不用于实时搜索。常用于大量数据的提取，例如re-index整个数据库。
```java
// 开启scroll模式，设定保存副本时间为5分钟
searchRequest.scroll("5m");
SearchResponse searchResponse = es.search(searchRequest);
String scrollId = searchResponse.getScrollId();
// 使用上个request的scorllId去获取下一页
SearchScrollRequest scrollRequest = new SearchScrollRequest(scrollId); 
scrollRequest.scroll("5m");
es.scroll(scrollRequest);
```

search_after 的工作原理是将上个请求结果中最后一项的sortValues包含在下个请求的search_after参数中。并且两个搜索请求需有相同的排序方式和Query。轻量级，快速有效  

```java
// 当使用search_after是需要确保至少有一个unique的sort key。
searchSourceBuilder.sort(SortBuilders.fieldSort("uuid"))
...
Object[] sortValues = result.getHits().getHits().last().getSortValues();
// 这个searchRequest 必需要与前一个searchRequest有相同的sort和query才能得到期待的结果
searchRequest.searchAfter(sortValues);
```

-----
* A useful [playground tool](https://www.katacoda.com/courses/elasticsearch/playground)
* Getting Started by [index sample data](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/getting-started-index.html) 