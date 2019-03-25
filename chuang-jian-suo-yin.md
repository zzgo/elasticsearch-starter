## 创建索引

创建方式一：

```java
//拿到transportClient对象
transportClient.admin().indices().prepareCreate(indexName).get();
```



