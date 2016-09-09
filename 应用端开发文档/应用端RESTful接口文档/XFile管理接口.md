
# XFile管理接口

	XFile管理接口是用于管理用户企业资源存储与下载，当前全部资源为公共读写

# **接口概览**
1. [XFile上传](#upload)
2. [XFile下载](#download)
3. [获取xfile文档列表(未实现)](#get_xfile_list)


# **接口详情**

### **<a name="upload">1. XFile上传</a>**

	XFile文件上传接口用于企业或者用户上传资源, 文件被存与文件存储运营商服务器, 目前有阿里云oss或者腾讯云cos,返回文件上传记录信息, 企业管理员以及用户可调用。


**Request**

URL

	POST /v2/xfile/upload?content=xxxx

| 字段 | 是否必须 | 描述 |
| --- | --- | --- |
| content | 是 | 请求参数,文件类型type和公共读public_read的base64,如base64({"type":"jpg","public_read":true,"file_name":"文件名称"}) |
| type|是| 文件类型,比如jpg,apk|
| public_read | 否 | 公共读,true or false|
| file_name | 否 | 文件名称|

Header

	Access-Token:"调用凭证"

Content

	使用POST发送文件方式发送

**Response**

Header

	HTTP/1.1 200 OK

Content

	{
		"id":"xfile记录id",
    	"download_url":"XFile资源地址"
	}


### **<a name="download">XFile下载(待定接口)</a>**

	XFile文件下载接口用于企业管理员或用户下载资源, 通过API Server以HTTP 302方式向文件存储运营商下载资源, 目前有阿里云oss或者腾讯云cos,返回下载文件流, 企业管理员以及用户可调用


**Request**

URL

	GET /v2/xfile/download?sign=xxxxxx

| 字段 | 是否必须 | 描述 |
| --- | --- | --- |
| sign | 是 | 初步定为上传时记录id |

Header

	Access-Token:"调用凭证"

Content

	无

**Response**

Header

	HTTP/1.1 200 OK

Content

	{
    	"url":"云服务器商对应的资源地址"
	}

### **<a name="get_xfile_list">获取xfile文档列表(未实现)</a>**

	获取上传文件的列表

**Request**

URL

	POST /v2/xfile_list

Header

	Access-Token:"调用凭证"

Content

	{
	    "offset": "请求列表的偏移量",
	    "limit": "请求数量",
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

#### Response ####

*Header*

	HTTP/1.1 200 OK

*Content*

	{
		"count":"",
		"list":[
			{
			    "id" : "文件ID",
				"name":"文件名",
			    "md5" : "文件MD5",
			    "size" : 5839,
			    "ower" : "文件上传者",
			    "public_read" : true,
			    "create_time" : "创建时间",
			    "xlink_url" : "云智易链接地址",
			    "real_url" : "文件真实存放的url"
			}
		]
	}
	

字段 | 是否必须 | 描述
---- | ---- | ----
id | 是 | 文件ID
name | 是 | 文件名
md5 | 是 | 文件的MD5值
xlink_url | 是 | 云智易链接地址
real_url | 是 | 文件真实存放的url
size | 是 | 文件大小
ower | 是 | 文件上传者
ower_type | 是 | 见[文件上传者类型](#owner_type)
public_read | 是 | 是否公共读
create_time | 是 | 文件上传时间,例 2016-09-09T03:55:05.537Z


### **<a name="owner_type">文件上传者类型</a>**

枚举值 | 说明
---- | ----
1 | 用户拥有
2 | 管理员拥有
3 | 设备拥有