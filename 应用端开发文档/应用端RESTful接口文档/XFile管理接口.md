
# XFile管理接口

	XFile管理接口是用于管理用户企业资源存储与下载，当前全部资源为公共读写

# **接口概览**
1. [XFile上传](#upload)
2. [XFile下载](#download)


# **接口详情**

### **<a name="upload">1. XFile上传</a>**

	XFile文件上传接口用于企业或者用户上传资源, 文件被存与文件存储运营商服务器, 目前有阿里云oss或者腾讯云cos,返回文件上传记录信息, 企业管理员以及用户可调用。


**Request**

URL

	POST /v2/xfile/upload?content=xxxx

| 字段 | 是否必须 | 描述 |
| --- | --- | --- |
| content | 是 | 请求参数,文件类型type和公共读public_read的base64,如base64({"type":"jpg","public_read":true}) |
| type|是| 文件类型,比如jpg,apk|
| public_read | 否 | 公共读,true or false|

Header

	Access-Token:"调用凭证"

Content

	文件二进制数据

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