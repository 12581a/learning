# ElasticSearch

**倒排索引**

创建索引

> PUT： http://localhost:9200/_doc

查看索引

> GET： http://localhost:9200/_doc

查看所有索引

> GET： http://127.0.0.1:9200/_cat/indices?v 

创建文档

> POST： http://localhost:9200/_doc/1001

全部修改文档

> PUT： http://localhost:9200/_doc/1001

可以部分修改文档

> POST： http://localhost:9200/_update/1001

可以根据条件删除文档

查询索引下所有的文档

> GET：http://localhost:9200/_doc/_search

匹配查询

```json
{
 "query": {
	"match": {
		"title": "手机"
	}
  }
}
```

字段匹配查询

```json
{
 "query": {
 	"term": {
		"title": {
			"value": "apple"
			}
		}
	}
}
```

高级查询

```json
/**
	"match_phrase":完全匹配
*/
{
	"query":{
		"match":{
			"title":"华为手机"
		}
	},
    "highlight": {
		"fields": {
			"title": {}
		}
	}
	"_source":["title"],
	"sort":{
		"price":{
			"order":"asc"
		}
	}
}
```

组合查询

```json
{
	"query":{
		"bool":{
			"should":[
				{
					"match":{
						"title": "OPPO"
					}
				},
				{
					"match":{
						"title": "华为"
					}
				}
			],
			"filter":{
				"range":{
					"price":{
						"gt":5000
					}
				}
			}
		}
		
	}
}
```









































