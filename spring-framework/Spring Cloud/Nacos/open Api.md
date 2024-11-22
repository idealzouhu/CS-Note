

```bash
curl -X POST "http://127.0.0.1:8848/nacos/v1/cs/configs?dataId=user-service.properties&group=DEFAULT_GROUP&content=useLocalCache=true&content=cacheSize=100"

curl -X GET "http://127.0.0.1:8848/nacos/v1/cs/configs?dataId=user-service.properties&group=DEFAULT_GROUP&namespaceId=public"
```





```bash
  curl -d "dataId=nacos.example" ^
  -d "group=DEFAULT_GROUP" ^
  -d "namespaceId=public" ^
  -d "content=contentTest" ^
  -X POST "http://127.0.0.1:8848/nacos/v2/cs/config"
  
  
  curl -X GET 'http://127.0.0.1:8848/nacos/v2/cs/config?dataId=nacos.example&group=DEFAULT_GROUP&namespaceId=public'
```







nacos server 2.1.2  暂时只支持 open API v2