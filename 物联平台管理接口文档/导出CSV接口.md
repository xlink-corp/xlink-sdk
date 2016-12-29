©2016  **云智易**物联云平台（http://www.xlink.cn）

# 导出CSV接口

	本文档包括各种导出CSV任务接口, 包括设备列表/用户列表/经销商列表等等。

# **接口预览**

1. [创建导出CSV任务](#addExport)
2. [获取单个导出任务](#getExport)
3. [获取导出任务授权链接](#authLinkUrl)
4. [导出任务列表](#listExport)

# **接口详情**

### **<a name="addExport">1.创建导出CSV任务</a>**

	企业管理员增加单个导出任务

**Request**

URL

	POST /v2/export_task

Header

	Content-Type:application/json
	Access-Token:"调用凭证"

Content

	{
		"name":"导出任务名称",
		"describe":"导出任务描述",
		"type":"导出任务类型",
		"params":{
			"filter": [
		        "字段A",
		        "字段B"
		    ],
		 	"query": {
		        "filed1": {
		            "$like": "字段值"
		        },
		        "filed3": {
		            "$lt": "字段值"
		        }
		    },
		    "order": {
		        "filed1": "desc",
		        "filed2": "asc"
		    }
		},
		"extend":{
			"拓展参数"
		}
	}

字段|是否必须|描述
---- | ---- | ----
name | 是 | 导出任务名称
describe | 是 | 导出任务描述
type | 是 | 导出任务类型, 见[附录](#export_task_type)
params | 是 | 导出任务参数, 用于查询需要导出数据; 备注:导出聚合设备列表时, 请参照***产品与设备管理接口***中的***设备聚合接口***整个请求条件
extend | 否 | 额外条件, 见[附录](#export_task_extend)

**Response**

Header

	HTTP/1.1 200 OK

Content

	{
		"id":"导出任务id"
	}
	
### **<a name="getExport">2.获取单个导出任务</a>**

	企业管理员获取单个导出任务详情

**Request**

URL

	GET /v2/export_task/{export_task_id}

Header

	Content-Type:application/json
	Access-Token:"调用凭证"

Content

	无


**Response**

Header

	HTTP/1.1 200 OK

Content

	{
        "id": "导出任务id",
        "creator_id": "创建任务成员id",
        "creator_name": "创建任务成员名称",
        "name": "导出任务名称",
        "describe": "导出任务描述",
        "type": "导出任务类型。",
        "status": "导出任务状态",
        "params": "导出任务参数",
        "size": "导出任务生成csv大小",
        "link_url": "链接地址, 未授权",
        "create_time": "创建时间",
        "result_desc": "结果描述",
		"total":"总上传的对象数,用于计算进度",
		"finished":"完成上传的对象数,用于计算进度",
		"begin_time":"任务开始执行时间",
		"end_time":"任务结束执行时间",
		"extend":{
			"app_id":"平台appid, 仅在维保有效",
			"product_id":"产品id, 目前仅用于设备列表"
		}
    }

字段|是否必须|描述
---- | ---- | ----
id | 是 | 导出任务id
creator_id | 是 | 创建任务成员id
creator_name | 是 | 创建任务成员名称
name | 是 | 导出任务名称
describe | 是 | 导出任务描述
type | 是 | 导出任务类型,见[附录](#export_task_type)
status | 是 | 导出任务状态, 见[附录](#export_task_status)
params | 是 | 导出任务参数
size | 是 | 导出任务生成csv大小
link_url | 是 | 链接地址, 未授权
create_time | 是 | 创建时间,例：2014-10-09T08:15:40.843Z
result_desc | 是 | 结果描述
total | 是 | 总上传的对象数,用于计算进度
finished | 是 | 完成上传的对象数,用于计算进度
begin_time | 是 | 任务开始执行时间
end_time | 是 | 任务结束执行时间
extend.app_id | 是 | 平台appid, 目前仅在维保有效
extend.product_id | 是 | 平台product_id, 目前仅在设备列表有效


### **<a name="authLinkUrl">3.获取导出任务授权链接</a>**

	企业管理员获取导出任务结果csv下载地址

**Request**

URL

	GET /v2/export_task/{export_task_id}/auth_link

Header

	Content-Type:application/json
	Access-Token:"调用凭证"

Content

	无


**Response**

Header

	HTTP/1.1 200 OK

Content

	{
	    "link_url":"授权过的下载链接"
	}

字段|是否必须|描述
---- | ---- | ----
link_url | 是 | 授权过的下载链接, 具有时效性


### **<a name="listExport">4.导出任务列表</a>**

	企业管理员查看企业下所有的导出列表。

**Request**

URL

	POST /v2/export_tasks

Header

	Content-Type:application/json
	Access-Token:"调用凭证"

Content

	{
	    "offset": "请求列表的偏移量",
	    "limit": "请求数量",
	    "filter": [
	        "字段A",
	        "字段B"
	    ],
	    "query": {
	        "filed1": {
	            "$like": "字段值"
	        },
	        "filed3": {
	            "$lt": "字段值"
	        }
	    },
	    "order": {
	        "filed1": "desc",
	        "filed2": "asc"
	    }
	}

字段 | 是否必须 | 描述
---- | ---- | ----
offset | 否 | 从某个偏移量开始请求，默认为0
limit | 否 | 请求的条目数量，默认为10
order | 否 | 可以指定通过设备默认的某个字段排序，desc降序，asc升序
filter |否 | 字段过滤，集合类型，可以指定返回结果列表的字段，可以<br>包含扩展属性字段
query | 否 | 查询条件，可以根据不同字段加上不同的比较指令来查询，查<br>询条件字段包含设备所有默认字段，不支持扩展属性字段，支<br>持比较指令包含如下:<br>$in：包含于该列表任意一个值<br>$lt：小于该字段值<br>$lte：小于或等于字段值<br>$gt：大于该字段值<br>$gte：大于或等于该字段值<br>$like：模糊匹配该字段值

**Response**

Header

	HTTP/1.1 200 OK

Content

	{
	    "count": "总数量",
	    "list": [
	       {
		        "id": "导出任务id",
		        "creator_id": "创建任务成员id",
		        "creator_name": "创建任务成员名称",
		        "name": "导出任务名称",
		        "describe": "导出任务描述",
		        "type": "导出任务类型。",
		        "status": "导出任务状态",
		        "params": "导出任务参数",
		        "size": "导出任务生成csv大小",
		        "link_url": "链接地址, 未授权",
		        "create_time": "创建时间",
		        "result_desc": "结果描述",
				"total":"总上传的对象数,用于计算进度",
				"finished":"完成上传的对象数,用于计算进度",
				"begin_time":"任务开始执行时间",
				"end_time":"任务结束执行时间"
		    }
	    ]
	}

字段|是否必须|描述
---- | ---- | ----
id | 是 | 导出任务id
creator_id | 是 | 创建任务成员id
creator_name | 是 | 创建任务成员名称
name | 是 | 导出任务名称
describe | 是 | 导出任务描述
type | 是 | 导出任务类型,见[附录](#export_task_type)
status | 是 | 导出任务状态, 见[附录](#export_task_status)
params | 是 | 导出任务参数
size | 是 | 导出任务生成csv大小
link_url | 是 | 链接地址, 未授权
create_time | 是 | 创建时间,例：2014-10-09T08:15:40.843Z
result_desc | 是 | 结果描述
total | 是 | 总上传的对象数,用于计算进度
finished | 是 | 完成上传的对象数,用于计算进度
begin_time | 是 | 任务开始执行时间
end_time | 是 | 任务结束执行时间


# 附录

### **<a name="export_task_type">1.导出任务类型</a>**

| 名称 | 值 | 说明 |
| --- | --- | --- |
| Device | 1 | 设备列表 |
| User | 2 | 用户列表 |
| Alert | 3 | 告警信息列表 |
| Heavybuyer | 4 | 大客户列表 |
| Dealer | 5 | 经销商列表 |
| Warranty | 6 | 维保列表 |
| DeviceSessionLog | 7 | 设备上下线 |
| WechatAuthDevice | 8 | 微信设备列表 |
| DeviceAggregate | 9 | 设备聚合列表 |
| BroadcastTaskList | 10 | 推送历史列表 |
| DeviceQrcodeList | 11 | 设备二维码 | 
| SalesmanList | 12 | 业务员列表 | 
| SalesRecordList | 13 | 业务员销售列表 |


### **<a name="export_task_status">2.导出任务状态</a>**

| 名称 | 值 | 说明 |
| --- | --- | --- |
| ToBeExported | 1 | 待导出 |
| Exporting | 2 | 导出中 |
| Exported | 3 | 导出完成 |
| Invalid | 4 | 无效导出 |
| Expired | 5 | 过期 |


### **<a name="export_task_extend">3.导出任务拓展属性</a>**


* Device设备列表拓展属性:

	```
	{
		"extend":{
			"product_id":"产品ID"
		}
	}
	```

* User用户列表拓展属性:
	
	
	```
	{
		"extend":{
		}
	}
	```

* Alert告警信息列表拓展属性:

	```
	{
		"extend":{
		}
	}
	```

* Heavybuyer大客户列表拓展属性:

	```
	{
		"extend":{
		}
	}
	```

* Dealer经销商列表拓展属性:

	```
	{
		"extend":{
		}
	}
	```

* Warranty维保列表拓展属性:

	```
	{
		"extend":{
			"app_id":"目前用于维保列表, 用于查找创建访问node服务的token"
		}
	}
	```	

* DeviceSessionLog设备上下线拓展属性:

	```
	{
		"extend":{
		}
	}
	```

* WechatAuthDevice微信设备列表拓展属性:

	```
	{
		"extend":{
		}
	}
	```

* DeviceAggregate设备聚合列表拓展属性:
	
	```
	{
		"extend":{
			"product_id":"产品ID"
		}
	}
	```

* BroadcastTaskList推送历史列表拓展属性:

	

* DeviceQrcode设备二维码拓展属性:

	```
	{
		"extend":{
			"custom_field": [
		        "id",
		        "mac",
		        "pid",
		        "sn"
		    ],
		    "custom_property": {
		        "{key}": "{value}"
		    },
			"format":{
				"prefix":"前缀字符串",
				"suffix":"后缀字符串",
				"encode":"对二维码数据进行编码，不包括前缀后缀"
			},
			"product_id":"产品ID"
		}
	}
	```

* SalesmanList业务员列表拓展属性:

	```
	{
		"extend":{
			"app_id":"目前用于业务员列表, 用于查找创建访问node服务的token"
		}
	}
	```	

* SalesRecordList业务员销售列表:

	```
	{
		"extend":{
			"app_id":"目前用于业务员销售列表, 用于查找创建访问node服务的token"
		}
	}
	```