### URL

POST  http://218.245.102.212:18088/v2/xlink/aiagent/web_crawl/search

### Header

```json
{
  "Authorization" : "Bearer xk-j2342f656568Ytpu_1478BAF*BB"
}
```



### Body

```json
{
    "question":"物联网的前景",
    "search_engine_type":"baidu",
    "offset":0,
    "count":30,
    "crawl_conc":5,
    "use_proxy":true
}
```

search_engine_type 可选值： baidu, bing, google

crawl_conc 抓取页面的并发数

offset 返回结果的偏移量

count 返回结果的条目数

use_proxy 是否采用代理

