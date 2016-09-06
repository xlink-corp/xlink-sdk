# Home管理接口文档

	Home解决用户在设备使用过程的家庭场景需求，具体有Home家庭的创建管理、家庭消息盒子等内容。

# 接口概览

## 1.[Home管理](#home_manager)
**1.1 [创建Home](#add_home)**

**1.2 [修改Home](#update_home)**

**1.3 [删除Home](#delete_home)**

**1.4 [邀请Home的成员](#add_home_member)**

**1.5 [用户获取接收到Home邀请的记录列表](#get_invitee_list)**

**1.6 [用户获取发出的邀请记录列表](#get_inviter_list)**

**1.7 [用户接受邀请](#home_member_invite_accept)**

**1.8 [用户拒绝邀请](#home_member_invite_deny)**

**1.9 [删除Home的成员](#delete_home_member)**

**1.10 [修改Home的成员](#update_home_member)**

**1.11 [用户获取Home列表](#get_home_list)**

**1.12 [设备设置归属于Home](#set_device_home_id)**

**1.13 [将设备从home中移除](#delete_device_home_id)**

**1.14 [获取Home中的设备列表](#get_home_device_list)**

**1.15 [修改用户和设备的权限](#update_device_authority)**

## 2.[Home Inbox管理](#inbox_manager)

**2.1 [Home成员发送消息到Inbox](#home_inbox_add)**

**2.2 [Home成员查看Inbox消息列表](#home_inbox_list)**

**2.3 [Home成员删除Inbox消息](#home_inbox_delete)**
 
### 3.[附录](#appendix)


# 接口详情

## <a name="home_manager">1. Home管理


### <a name ="add_home">1.1 创建Home</a>

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
				"expire_time":"到期的时间",			
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

### <a name ="update_home">1.2 修改Home</a>

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
				"expire_time":"到期的时间",			
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

### <a name ="delete_home">1.3 删除Home</a>

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

### <a name ="add_home_member">1.4 邀请Home的成员</a>

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

	{
		"invite_id" : "邀请ID"
	}

字段 | 是否必须 | 描述
---- | ---- | ----
invite_id | 是 | 邀请ID

### <a name="get_invitee_list">1.5 用户获取接收到Home邀请的记录列表</a>

	用户通过本接口可以查看已接收到的Home邀请的所有记录列表。

**Request**

URL

	GET /v2/home/invitee_list?user_id={user_id}

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
	            "_id": "标识ID",
	            "home_id": "Home ID",
	            "invitee": "被邀请者用户ID",
	            "inviter": "邀请者用户ID",
				"role":"被邀请者的角色类型",
				"authority":"被邀请者对设备的控制权限",
				"expire_time":"被邀请者的到期时间"
	            "status": "邀请状态，等待接受，已接受，已拒绝，已失效，具体枚举值见附录",
	            "end_time": "邀请记录有效截止时间",
	            "create_time": "创建时间"
	        }
	    ]
	}


字段 | 是否必须 | 描述
---- | ---- | ---- 
id | 是 | 邀请记录ID
home_id | 是 | 家庭ID
invitee | 是 | 被邀请这用户ID
inviter | 是 | 邀请者用户ID
status | 是 | 邀请状态，等待接受，已接受，已拒绝，已失效，具体枚举值见[附录](#invite_status)
end_time | 是 | 邀请记录有效截止时间， 例：2014-10-09T08:15:40.843Z
create_time | 是 | 创建时间，例：2014-10-09T08:15:40.843Z
role | 是 | home成员的角色类型,见[home成员类型](#home_member_role_type)
authority | 否 | 对设备的控制权限，**R可读，W可写，RW可读可写；默认为RW**；
expire_time | 否 | home成员的到期时间，例：2014-10-09T08:15:40.843Z


### <a name="get_inviter_list">1.6 用户获取发出的邀请记录列表</a>

**Request**

URL

	GET /v2/home/inviter_list?user_id={user_id}

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
	            "_id": "标识ID",
	            "home_id": "Home ID",
	            "invitee": "被邀请者用户ID",
	            "inviter": "邀请者用户ID",
	            "status": "邀请状态，等待接受，已接受，已拒绝，已失效，具体枚举值见附录",
	            "end_time": "邀请记录有效截止时间",
	            "create_time": "创建时间"
	        }
	    ]
	}


字段 | 是否必须 | 描述
---- | ---- | ---- 
id | 是 | 邀请记录ID
home_id | 是 | 家庭ID
invitee | 是 | 被邀请这用户ID
inviter | 是 | 邀请者用户ID
status | 是 | 邀请状态，等待接受，已接受，已拒绝，已失效，具体枚举值见[附录](#invite_status)
end_time | 是 | 邀请记录有效截止时间， 例：2014-10-09T08:15:40.843Z
create_time | 是 | 创建时间，例：2014-10-09T08:15:40.843Z


### <a name ="home_member_invite_accept">1.7 用户接受邀请</a>

	接收到home管理员发送的邀请后，用户响应邀请。

**Request**

URL

	POST /v2/home/{home_id}/user_accept

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

### <a name ="home_member_invite_deny">1.8 用户拒绝邀请</a>

	接收到home管理员发送的邀请后，用户响应改邀请。

**Request**

URL

	POST /v2/home/{home_id}/user_deny

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

### <a name ="delete_home_member">1.9 删除Home的成员</a>

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

### <a name ="update_home_member">1.10 修改Home的成员</a>

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
role | 是 | home成员的角色类型,见[home成员类型](#home_member_role_type)
expire_time | 否 | home成员的到期时间，例：2014-10-09T08:15:40.843Z

**Response**

Header

	HTTP/1.1 200 OK

Content

	无

### <a name ="get_home_list">1.11 用户获取Home列表</a>

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
	                    "expire_time": "到期的时间"
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

### <a name ="set_device_home_id">1.12 设置设备归属于Home</a>

	home的管理员把设备加入到home中。

**Request**

URL

	POST /v2/home/{home_id}/device_add

Header

	Content-Type:"application/json"
	Access-Token:"调用凭证"

Content

	{
	    "device_id": "设备ID",
		"authority":"权限类型",
		"sub_role":"用户和设备的订阅关系"
	}

字段|是否必须|描述
---- | ---- | ----
device_id | 是 | 设备ID
authority | 是 | 对设备的控制权限，**R可读，W可写，RW可读可写**
sub_role | 是 | 用户和设备的订阅关系；0：管理员；1：普通用户

**Response**

Header

	HTTP/1.1 200 OK

Content

	无

### <a name ="delete_device_home_id">1.13 将设备从home中移除</a>

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

### <a name ="get_home_device_list">1.14 获取Home中的设备列表</a>

	home的成员可以获取home的设备列表

**Request**

URL

	GET /v2/home/{home_id}/devices

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
	            "id": "设备ID",
	            "mac": "设备MAC地址",
	            "is_active": "是否激活",
	            "active_date": "激活时间",
	            "is_online": "是否在线",
	            "last_login": "最近登录时间",
	            "mcu_mod": "MCU型号",
	            "mcu_version": "MCU版本号",
	            "firmware_mod": "固件型号",
	            "firmware_version": "固件版本号",
	            "product_id": "所属的产品ID",
	            "access_key": "设备访问码"
	        }
	    ]
	}

### <a name ="update_device_authority">1.15 修改用户和设备的权限</a>

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


## <a name="inbox_manager">2. Inbox管理</a>
	
	Home下的消息盒子系统，Home下所有成员共享所有消息，每个成员只可以删除自己所发的消息，管理员或以上权限成员拥有最高权限。

### <a name ="home_inbox_add">2.1 Home成员发送消息到Inbox</a>

**Request**

URL

	/v2/home/{home_id}/inbox/message_add

Header

	Content-Type:"application/json"
	Access-Token:"调用凭证"
	
Content

	{
		"type":"消息类型，见附录",
	    "title": "消息标题",
	    "content": "消息内容"
	}


字段 | 是否必须 | 描述
---- | ---- | ---- 
type | 是 | 消息类型，见[附录](#inbox_message_type) 
title| 是 | 消息标题
content | 是 | 消息内容 


**Response**

Header

	HTTP/1.1 200 OK

Content

	{
	    "id": "消息ID",
	    "home_id": "家庭ID",
	    "type": "消息类型,见附录",
	    "creator": "消息创建者ID",
	    "creator_type": "消息创建者类型，设备、用户",
		"create_time": "创建时间",
	    "title": "消息标题",
	    "content": "消息内容"
	}


字段 | 是否必须 | 描述
---- | ---- | ---- 
id | 是 | 消息ID
home_id | 是 | 家庭ID
type | 是 | 消息类型，见[附录](#inbox_message_type)
creator | 是 | 创建者ID
creator_type | 是 | 消息创建者类型，设备、用户,见[附录](#creator_type)
create_time | 是 | 创建时间，例：2014-10-09T08:15:40.843Z
title | 是 | 消息标题
content | 是 | 消息内容

### <a name="home_inbox_list">2.2 Home成员查看Inbox消息列表</a>


**Request**

URL

	POST /v2/home/{home_id}/inbox/messages

Header

	Content-Type:"application/json"
	Access-Token:"调用凭证"

Content

	{
	    "offset": "请求的偏移量",
	    "limit": "请求的数量上限",
	    "query": {
	        "filed1": {
	            "$in": [
	                "字段值",
	                "字段值"
	            ]
	        },
	        "filed2": {
	            "$lt": "字段值"
	        }
	    },
	    "order": {
	        "filed1": "desc",
	        "filed2": "asc"
	    }
	}


**Response**

Header

	HTTP/1.1 200 OK

Content

	{
	    "count": "总数量",
	    "list": [
	        {
	            "id": "消息ID",
	            "home_id": "家庭ID",
	            "type": "消息类型,见附录",
	            "creator": "消息创建者ID",
	            "creator_type": "消息创建者类型，设备、用户",
	            "create_time": "创建时间",
	            "title": "消息标题",
	            "content": "消息内容"
	        }
	    ]
	}

字段 | 是否必须 | 描述
---- | ---- | ---- 
id | 是 | 消息ID
home_id | 是 | 家庭ID
type | 是 | 消息类型，见[附录](#inbox_message_type)
creator | 是 | 创建者ID
creator_type | 是 | 消息创建者类型，设备、用户,见[附录](#creator_type)
create_time | 是 | 创建时间，例：2014-10-09T08:15:40.843Z
title | 是 | 消息标题
content | 是 | 消息内容


### <a name="home_inbox_delete">2.3 Home成员删除Inbox消息</a>

	Home成员只能删除本人发送的Inbox消息，管理员或以上可以删除任意Inbox消息

**Request**

URL

	DELETE /v2/home/{home_id}/inbox/message/{message_id}

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



## <a name="appendix">附录</a> ##

### <a name="home_member_role_type">home成员类型</a>

角色类型 | 枚举值
----	|----
超级管理员	| 1
管理员	| 2
普通用户 | 3
访客     | 4


### <a name="invite_status">邀请状态</a>

邀请状态 | 枚举值
---- | ---- 
等待接受 | 0 
已接受 | 1 
已拒绝 | 2 
已失效 | 3


### <a name="inbox_message_type">消息类型</a>

消息类型 | 枚举值
---- | ---- 
1 | 普通消息

### <a name="creator_type">消息创建者类型</a>

消息创建者类型 | 枚举值
---- | ---- 
用户 | 1 
设备 | 2