©2016  **云智易**物联云平台（http://www.xlink.cn）

# 设备端REST接口开发文档

	云智易平台为设备端提供设备激活、设备获取数据等操作的REST接口，通过调用REST接口，设备无需进行连接云端即可完成简单的操作。



接入设备端REST接口流程：

* 【设备激活】:可通过云端激活、也可通过REST接口激活
* 请求【设备登录】REST接口获得具有设备身份的【调用凭证】（Access-Token）
* 通过具有【调用凭证】（Access-Tokon）进行数据的操作

**注：【调用凭证】（Access-Token）有效时间为2个小时，若设备端需要维持【调用凭证】一直有效，需2个小时内通过REST接口[刷新凭证](#refresh_token)换取新的【调用凭证】。**


## 接口概览

1. [设备激活](#device_active)
2. [设备认证](#device_auth)
3. [刷新凭证](#refresh_token)
4. [获取数据端点列表](#datapoint_list)
5. [错误码说明](#error_code)


## 接口详情

### <a name="device_active">1. 设备激活</a>

**Request**

URL

	POST /v2/device_active
	
Header

	Content-Type:application/json

Content

	{
	    "mac":"MAC地址",
	    "mcu_mod":"MCU型号",
	    "mcu_version":"MCU版本号",
	    "firmware_mod":"固件型号",
	    "firmware_version":"固件版本号",
	    "active_code":"激活码"
	}

字段	| 是否必须 | 描述
---- | ---- | ----
mac | 是 | MAC地址
mcu_mod | 是 | MCU型号
mcu_version | 是| MCU版本号
firmware_mod | 是 | 固件型号
firmware_version | 是 | 固件版本号
active_code | 是 | 激活码

**Response**

Header

	HTTP/1.1 200 OK	

Content

	{
	    "device_id":"设备ID",
	    "authorize_code":"认证码"
	}

### <a name="device_auth">2. 设备认证</a>

**Request**
URL

	POST /v2/device_login

Header

	Content-Type:application/json

Content

	{
	    "product_id":"产品ID",
		"mac":"MAC地址",
	    "authorize_code":"认证码"
	}

**Reponse**

Header

	HTTP/1.1 200 OK	

Content

	{
	    "access_token": "调用凭证", 
	    "refresh_token": "刷新凭证",
		"expire_in":"有效期（秒）", 
	    "device_id": "设备ID"
	}
	

### <a name="refresh_token">3. 刷新凭证</a>


**Request**

URL

	POST /v2/device_refresh

Header

	Content-Type:application/json
	Access-Token:"调用凭证"

Content

	{
	    "refresh_token": "刷新凭证" 
	}

**Response**

Header

	HTTP/1.1 200 OK	

Content

	 {
        "access_token":"新的调用凭证",
        "refresh_token":"新的刷新凭证",
        "expire_in":"有效期"
    }

### <a name="datapoint_list">4. 获取数据端点列表</a>

本接口详情见[文档](https://github.com/xlink-corp/xlink-sdk/blob/master/%E7%89%A9%E8%81%94%E5%B9%B3%E5%8F%B0%E7%AE%A1%E7%90%86%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3/%E4%BA%A7%E5%93%81%E4%B8%8E%E8%AE%BE%E5%A4%87%E7%AE%A1%E7%90%86%E6%8E%A5%E5%8F%A3.md#listProductDataPoint)




### <a name="error_code">5. 错误码说明***</a>

详情请见[文档](https://github.com/xlink-corp/xlink-sdk/blob/master/%E5%BA%94%E7%94%A8%E7%AB%AF%E5%BC%80%E5%8F%91%E6%96%87%E6%A1%A3/%E5%BA%94%E7%94%A8%E7%AB%AFRESTful%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3/%E9%94%99%E8%AF%AF%E7%A0%81%E8%AF%B4%E6%98%8E.md)

©2016  **云智易**物联云平台（http://www.xlink.cn）