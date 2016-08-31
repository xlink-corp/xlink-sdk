# Home管理接口文档

# 接口概览

1. [创建Home](#add_home)
2. [修改Home](#update_home)
3. [删除Home](#delete_home)
4. [邀请Home的成员](#add_home_member)
5. [删除Home的成员](#delete_home_member)
6. [修改Home的成员](#update_home_member)
7. [用户接受邀请](#home_member_invite_accept)
8. [用户拒绝邀请](#home_member_invite_deny)
9. [用户获取Home列表](#get_home_list)
10. [设备设置归属于Home](#set_device_home_id)
11. [将设备从home中移除](#delete_device_home_id)
12. [获取Home中的设备列表](#get_home_device_list)
13. [修改用户和设备的权限](#update_device_authority)
14. [附录](#appendix)

# 接口详情

## <a name ="add_home">1. 创建Home</a>

**Request**

URL

	POST /v2/home

Header

	Content-Type:"application/json"
	Access-Token:"调用凭证"

Content

	{
		"name":"home的名称",
	}

**Response**

Header

	HTTP/1.1 200 OK

Content

	{
	    "id":"home的Id",
		"name":"home的名称",
		"user_list":[
			{
				"user_id":"用户Id",				
				"role":"角色类型",	
				"expire_time":"到期是时间",			
			}
		],
		"creator":"创建者Id",
		"create_time":"创建时间",
		"version":"数据的版本号"
	}

字段 | 是否必须 | 描述
---- | ---- | ----
id |是 | home的Id
name | 是 | home的名称
user_list | 是 | 属于home的成员列表
user_list.user_id | 是 | 用户Id
user_list.role | 是 | home成员的角色类型,见[home成员类型](#home_member_role_type)
user_list.expire_time | 是 | home成员的到期时间，例：2014-10-09T08:15:40.843Z
creator | 是 | home的创建者Id
create_time |是 | 创建时间，例：2014-10-09T08:15:40.843Z
version |是 | 数据的版本号

## <a name ="update_home">2. 修改Home</a>

	只有超级管理员和管理员才能修改home。

**Request**

URL

	PUT /v2/home/{home_id}

Header

	Content-Type:"application/json"
	Access-Token:"调用凭证"

Content

	{
		"name":"home的名称",
	}

**Response**

Header

	HTTP/1.1 200 OK

Content

	{
	    "id":"home的Id",
		"name":"home的名称",
		"user_list":[
			{
				"user_id":"用户Id",				
				"role":"角色类型",	
				"expire_time":"到期是时间",			
			}
		],
		"creator":"创建者Id",
		"create_time":"创建时间",
		"update_time":"最近一次的修改时间",
		"version":"数据的版本号"
	}

字段 | 是否必须 | 描述
---- | ---- | ----
id |是 | home的Id
name | 是	| home的名称
user_list | 是 | 属于home的成员列表
user_list.user_id | 是 | 用户Id
user_list.expire_time | 是 | home成员的到期时间，例：2014-10-09T08:15:40.843Z
user_list.role | 是 | home成员的角色类型,见[home成员类型](#home_member_role_type)
creator | 是 | home的创建者Id
update_time |是 | 最近一次的修改时间，例：2014-10-09T08:15:40.843Z
create_time |是 | 创建时间，例：2014-10-09T08:15:40.843Z
version |是 | 数据的版本号

## <a name ="delete_home">3. 删除Home</a>

	只有创建者才能删除Home

**Request**

URL

	DELETE /v2/home/{home_id}

Header

	Content-Type:"application/json"
	Access-Token:"调用凭证"

Content

	无

**Response**

Header

	HTTP/1.1 200 OK

Content

	无

## <a name ="add_home_member">4. 邀请Home的成员</a>

	只有超级管理员和管理员才能添加成员。

**Request**

URL

	POST /v2/home/{home_id}/user_invite

Header

	Content-Type:"application/json"
	Access-Token:"调用凭证"

Content

	{
		"account":"用户注册的账号",
		"role":"home成员的角色类型",
		"authority":"对设备的控制权限",
		"expire_time":"home成员的到期时间"
	}

字段 | 是否必须 | 描述
---- | ---- | ----
account |是 | 用户注册的账号
role | 是 | home成员的角色类型,见[home成员类型](#home_member_role_type)
authority | 否 | 对设备的控制权限，**R可读，W可写，RW可读可写；默认为RW**；
expire_time | 否 | home成员的到期时间，例：2014-10-09T08:15:40.843Z

**Response**

Header

	HTTP/1.1 200 OK

Content

	无

## <a name ="delete_home_member">5. 删除Home的成员</a>

	超级管理员能删除管理员,超级管理员和管理员能删除其他成员。除了创建者外，任何成员能够自行退出Home。

**Request**

URL

	DELETE /v2/home/{home_id}/user/{user_id}

Header

	Content-Type:"application/json"
	Access-Token:"调用凭证"

Content

	无

**Response**

Header

	HTTP/1.1 200 OK

Content

	无

## <a name ="update_home_member">6. 修改Home的成员</a>

	超级管理员能修改管理员,超级管理员和管理员才能修改其他成员。

**Request**

URL

	PUT /v2/home/{home_id}/user/{user_id}

Header

	Content-Type:"application/json"
	Access-Token:"调用凭证"

Content

	{
		"role":"home成员的角色类型",
		"expire_time":"home成员的到期时间"
	}

字段 | 是否必须 | 描述
---- | ---- | ----
account |是 | 用户注册的账号
role | 是 | home成员的角色类型,见[home成员类型](#home_member_role_type)
authority | 否 | 对设备的控制权限，**R可读，W可写，RW可读可写；默认为RW**；
expire_time | 否 | home成员的到期时间，例：2014-10-09T08:15:40.843Z

**Response**

Header

	HTTP/1.1 200 OK

Content

	无

## <a name ="home_member_invite_accept">7. 用户接受邀请</a>

	接收到home管理员发送的邀请后，用户响应邀请。

**Request**

URL

	PSOT /v2/home/{home_id}/user_accept

Header

	Content-Type:"application/json"
	Access-Token:"调用凭证"

Content

	{
		"invite_id" : "邀请ID"
	}

字段 | 是否必须 | 描述
---- | ---- | ----
invite_id | 是 | 邀请ID

**Response**

Header

	HTTP/1.1 200 OK

Content

	无

## <a name ="home_member_invite_deny">8. 用户拒绝邀请</a>

	接收到home管理员发送的邀请后，用户响应改邀请。

**Request**

URL

	PSOT /v2/home/{home_id}/user_deny

Header

	Content-Type:"application/json"
	Access-Token:"调用凭证"

Content

    {
        "invite_id" : "邀请ID",
        "reason":"用户拒绝分享的原因"
    }


字段 | 是否必须 | 描述
---- | ---- | ----
invite_code | 是 | 邀请ID
reason | 否 | 用户拒绝分享的原因

**Response**

Header

	HTTP/1.1 200 OK

Content

	无

## <a name ="get_home_list">9. 用户获取Home列表</a>

	获取home列表。

**Request**

URL

	GET /v2/homes?user_id={user_id}

Header

	Content-Type:"application/json"
	Access-Token:"调用凭证"

Content

	无

**Response**

Header

	HTTP/1.1 200 OK

Content

    {
	    "count": "总数量",
	    "list": [
	        {
	            "id": "home的Id",
	            "name": "home的名称",
	            "user_list": [
	                {
	                    "user_id": "用户Id",
	                    "role": "角色类型",
	                    "expire_time": "到期是时间"
	                }
	            ],
	            "creator": "创建者Id",
	            "create_time": "创建时间",
	            "update_time": "最近一次的修改时间",
	            "version": "数据的版本号"
	        }
	    ]
	}


字段 | 是否必须 | 描述
---- | ---- | ----
id |是 | home的Id
name | 是	| home的名称
user_list | 是 | 属于home的成员列表
user_list.user_id | 是 | 用户Id
user_list.expire_time | 是 | home成员的到期时间，例：2014-10-09T08:15:40.843Z
user_list.role | 是 | home成员的角色类型,见[home成员类型](#home_member_role_type)
creator | 是 | home的创建者Id
update_time |是 | 最近一次的修改时间，例：2014-10-09T08:15:40.843Z
create_time |是 | 创建时间，例：2014-10-09T08:15:40.843Z
version |是 | 数据的版本号

## <a name ="set_device_home_id">10. 设置设备归属于Home</a>

	home的管理员把设备加入到home中。

**Request**

URL

	POST /v2/home/{home_id}/device_add

Header

	Content-Type:"application/json"
	Access-Token:"调用凭证"

Content

	{
	    "device_id": "设备ID"
	}

**Response**

Header

	HTTP/1.1 200 OK

Content

	无

## <a name ="delete_device_home_id">11. 将设备从home中移除</a>

	home的管理员把设备从home中移除。

**Request**

URL

	DELETE /v2/home/{home_id}/device/{device_id}

Header

	Content-Type:"application/json"
	Access-Token:"调用凭证"

Content

	无

**Response**

Header

	HTTP/1.1 200 OK

Content

	无

## <a name ="get_home_device_list">12. 获取Home中的设备列表</a>

	home的成员可以获取home的设备列表

见标准文档接口[查询设备列表](#https://github.com/xlink-corp/xlink-sdk/blob/master/%E7%89%A9%E8%81%94%E5%B9%B3%E5%8F%B0%E7%AE%A1%E7%90%86%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3/%E4%BA%A7%E5%93%81%E4%B8%8E%E8%AE%BE%E5%A4%87%E7%AE%A1%E7%90%86%E6%8E%A5%E5%8F%A3.md#listDevice)。


## <a name ="update_device_authority">13. 修改用户和设备的权限</a>

	管理员及以上才能修改其他home成员对设备的操作权限。

**Request**

URL

	PUT /v2/home/{home_id}/device_permission

Header

	Content-Type:"application/json"
	Access-Token:"调用凭证"

Content

	{
		"user_id":"home成员的用户Id",
		"device_id":"设备ID",
		"authority":"权限类型"
	}

字段|是否必须|描述
---- | ---- | ----
user_id | 是 | home成员的用户Id
authority | 是 | 对设备的控制权限，**R可读，W可写，RW可读可写**；

**Response**

Header

	HTTP/1.1 200 OK

Content

	无


## <a name="appendix">附录</a> ##

## <a name="home_member_role_type">home成员类型</a>

角色类型 | 枚举值
----	|----
超级管理员	| 1
管理员	| 2
普通用户 | 3
访客     | 4