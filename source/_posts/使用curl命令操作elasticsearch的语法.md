---
title: 使用curl命令操作elasticsearch的语法
date: 2018-03-11 22:26:24
tags:
---

## Insert:
#### Syntax 1(with docId):
> curl -XPUT 'http://esHost:9200/indexName/typeName/docId' -d 'JSONData'

```
curl -XPUT 'http://localhost:9200/admins/admin/123' -d '{
  "name" : "Arun Thundyill Saseendran",
  "email" : "200900",      
  "phone" : "1297497", 
  "password": "8-0854200",
  "type":"super"
}'
```
result:

> {"_index":"admins_4","_type":"admin","_id":"1","_version":1,"created":true}

#### Syntax 2(without docId):
> curl -X POST -H "Content-Type: application/json" -H "Cache-Control: no-cache" -d 'JSONData' 'http://esHost:9200/indexName/typeName/'


## Search:
There are two method for searching:
### URI
#### Syntax:
> curl -XGET 'http://esHost:9200/indexName/typeName/_search?q={filter}'

### Request Body
> curl -XGET 'http://esHost:9200/indexName/typeName/_search' -d '{
>  "query": {
>    "term":{filed_name:search_value}
>  }
> }'
```
curl -XGET 'localhost:9200/pc_web/command/_search?pretty' -d '{
  "query": {
    "filtered": {
      "filter": {
        "and":{ 
          "filters": [
            {
              "terms": {"client_id":["_yXgPHq2XkEZ"]}
            },
            {
              "terms":{"state": ["init"]}
            }
          ]
        }
      }
    }
  },
  "size":65535
}'
```

创建存储数据：
curl -XPOST -H "Content-Type: application/json" -H "Cache-Control: no-cache" -d '{
  "parent": "1",
  "client_id": "-yXgPHq2XkEZ",
  "task_type": "normal_scan",
  "params": {},
  "state": "init",
  "creation_time":1465373344,
  "progress": 0,
  "sent_count": 0,
  "debug_message": "no error" 
} 
' 'http://localhost:9200/pc_web/command/'

删除指定的INDEX（database）的数据：
> curl -XDELETE 'localhost:9200/index/type/document'

删除制定的index：
> curl -XDELETE http://localhost:9200/index

删除制定的type的数据：
> curl -XDELETE http://localhost:9200/index/type
