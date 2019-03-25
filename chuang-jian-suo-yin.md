## 创建索引

创建方式一：

```java
//拿到transportClient对象
transportClient.admin().indices().prepareCreate(indexName).get();
```

创建方式二：

```java
//拿到transportClient对象
/**1.创建索引映射*/
transportClient.admin().indices()
.prepareCreate(indexName).setSettings("{\"index\":{\"analysis\":{\"analyzer\":{\"ik_pinyin_analyzer\":{\"tokenizer\":\"ik_max_word\",\"filter\":[\"my_pinyin\",\"word_delimiter\"]},\"ik_dituhui\":{\"tokenizer\":\"ik_max_word\",\"filter\":[\"stemmer\"]}},\"filter\":{\"my_pinyin\":{\"type\":\"pinyin\",\"first_letter\":\"prefix\",\"padding_char\":\" \"}}}}}", XContentType.JSON).get();

//创建mapping
PutMappingRequest mapping = Requests.putMappingRequest(indexName).type("1")
.source("{\""+ 1 +"\": {\"properties\": "
    + "{\"combine\": "
    + "{\"type\": \"text\","
    + "\"fields\": {"
    + "\"" + "pinyin" +"\": {"
    + "\"type\": \"text\","
    + "\"analyzer\": \"ik_pinyin_analyzer\""
    + "},"
    + "\"combine\": {"
    + "\"type\": \"text\","
    + "\"analyzer\": \"ik_dituhui\""
    + "}}}}}}", XContentType.JSON);
transportClient.admin().indices().putMapping(mapping).actionGet();
```

注意：这里是使用ik分词器和pinyin的。以后进行补充，也可以百度。

