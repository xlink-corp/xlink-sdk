
©2016  **云智易**物联云平台（http://www.xlink.cn）


# XLINK Android SDK集成文档

本文档是面向智能硬件的APP开发者，通过Xlink Android SDK接入云智易物联平台。

## 一.开发前的准备

### 1. SDK功能概述

云智易android sdk 帮助开发者基于android系统上和xlink模块智能设备进行通讯。

### 2.集成准备

* 注册云智易厂商帐号http://app.xlink.cn/login.html
* 新建产品，定义设备的数据端点
* 新建Access Key 生成自己的帐号体系
* 获取到产品ID,Access Key,Access Key Secret

### 3.配置程序
(参考Demo程序AndroidManifest.xml文件)

#### 3.1.在AndroidManifest.xml文件中application 标签下配置添加：

	<!-- XLINK 内网服务 -->
	<service android:name="io.xlink.wifi.sdk.XlinkUdpService" />               
	<!-- XLINK 公网服务 -->
	<service android:name="io.xlink.wifi.sdk.XlinkTcpService" />

#### 3.2. 添加sdk所需要权限

	<!-- 联网权限 -->                  
	<uses-permission android:name="android.permission.INTERNET" />
	<!-- 网络状态 -->                 
	<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
	<!-- wifi状态 -->                  
	<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
	<!-- wifi状态改变 -->
	<uses-permission android:name="android.permission.CHANGE_WIFI_MULTICAST_STATE" />

### 4.混淆打包

如果的项目使用了Proguard混淆打包，为了避免SDK被二次混淆导致无法正常使用SDK，请务必在proguard-project.txt中添加以下代码：

	-libraryjars libs/xlink-wifi-sdk-v*.jar #（集体请查看jar名称）
	-dontwarn io.xlink.wifi.sdk.**
	-keep class io.xlink.wifi.sdk.**{
	         *;
	}

## 二.配置SDK  
(可参考Demo程序MyApp.java文件)

请先把文件夹内 xlink-wifi-sdk-v***.jar 文件拷贝到自己工程libs下。

### 1.初始化SDK
在自定义Application 下的onCreate()函数调用：

	  XlinkAgent.init(applicationContext);

参数会被长期引用，最好使用application的context。

### 2.设置监听器     

请确保使用sdk的功能前最少设置一个通用监听器：

	XlinkAgent.getInstance().addXlinkListener(XlinkNetListener);

该监听器的功能有：(具体回调请参见XlinkNetListener说明)

value


函数：|说明：										
---| ---| ---								
onStart(int code);| 监听启动sdk成功/失败回调 			
onLogin(int code);|监听login登录服务器成功/失败回调
onLocalDisconnect(int code);|监听本地局域网断开回调
onDisconnect(int code);|监听和云端服务器连接状态回调
onRecvPipeData (XDevice device, byte[] data);|	收到设备发送Pipe数据
onRecvPipeSyncData(XDevice device, byte[] data);|收到设备发送同步Pipe数据
onDeviceStateChanged(XDevice xdevice, int state);|设备状态改变
onDataPointUpdate(XDevice xDevice, int key, Object , int channel, int type);|设备数据端点改变

注意事项:

当不需要该监听器时，可以调用
		 
	XlinkAgent.getInstance().removeListener(XlinkNetListener)
进行移除。

### 3.设置设备数据模版

如果设备使用了数据端点，需要根据在后端定义的设备数据端点在SDK进行配置，这样SDK才能解析该设备端点。 

例如下面表格样的数据端点：

索引（key）|变量名|  备注  |数据类型（type）
----|----|----|-----
0|LED|三色灯|单字节
1|KEY|KEY1(按键1)|布尔
2|inr|红外感应距离|单字节
3|Temp|温湿度|短整型
4|RF_IN|无线遥控码|长整型
5|Name|同步名称|字符串
...|...|...|...

转化成json串为：

	[{"key":0,"type":"byte"},{"key":1,"type":"bool"},{"key":2,"type":"byte"},{"key":3,"type":"int16"},{"key":4,"type":"int32"},{"key":5,"type":"string"}]

通过函数：

	XlinkAgent.setDataTemplate(“设备的产品id”, prodctid_value);

进行设置数据端点（可参考demo程序MyApp.java）

### 注意事项:

* 扫描,操作设备之前需先设置该设备的数据端点；(必须设置，不然解析不出数据端点)
* 如该设备未定义数据端点也需要这样设置： XlinkAgent.setDataTemplate(“该设备的产品id”,“[]”);
* 该设备的产品id 可在企业管理平台查看；
* SDK支持多产品ID；

## 三.登录/注册 厂商自己的用户体系

### 1. 概述

通过厂商自己的用户体系 uid + password换取该用户在云智易平台的唯一appId appAuthkey。获取到 appId 后，能调用：

	XlinkAgent.getInstance().login(appid,authkey);

才能使用云端网络和设备功能。

### 2.使用

* 在企业管理平台/开发者服务/Access key下新建Access key，得到Access Key ID和Access Key Secret；
* 查看 云智易RESTful Service-用户身份集成接口.pdf文件，并参考 Demo程序的注册登录流程(替换HttpAgent.java里面ACCESS_ID SECRET_KEY可直接使用)。

### 3.注意事项

* 如果使用邮箱或者手机号作为uid，其中的邮箱验证，短信验证由厂商自行实现（先判断验证，验证成功后，再调用云智易的接口进行注册）；

* 我们提供重置密码接口，其中的找回密码之前的验证用户 也是由厂商自行实现；

* 该注册之后的uid ,name password 不在云智易 SDK内通用，云智易唯一可识别的是 appID, appAuthey；这里的概念要区分开。

## 四.XlinkAgent类 方法说明

同步错误码(有INT返回值的函数)：

XlinkCode常量|int实际值|说明|使用的函数
---- | ---- | ---- |-----
`SUCCEED`|0|调用方法成功|ALL
`NO_INIT`|-1|未初始化SDK/未调用init(Context mContext)函数|ALL
`NO_HANDSHAKE`|-2|未和设备在局域网内连接成功|connectDevice(),setDeviceAuthorizeCode()，setDataPoint()，sendPipeData()
`ERROR_DEVICE`|-3|错误的设备（有传入设备参数的方法都有该错误码）|connectDevice(),setDeviceAuthorizeCode()，setDataPoint()，sendPipeData(),subscribeDevice()
`NO_CONNECT_SERVER`|-4|未连接服务器/未绑定UDP端口|scanDeviceByProductID函数回调该code是未调用start方法  
`NO_DEVICE`|-6|该设备在sdk未找到（操作设备前，需要调用initDevice(XDevice device)）|connectDevice(),setDeviceAuthorizeCode()，setDataPoint()，sendPipeData(),subscribeDevice()
`ALREADY_EXIST`|-7|任务重复，正在执行中，不允许重复调用；函数有: start  login connectDevice|start(), login(),connectDevice()
`INVALID_PARAM`|-8|参数有误（参数为空/密码长度太长/等等...）|所有带参数的函数
`INVALID_DEVICE_ID`|-9|无效的设备ID|所有带XDevice参数的函数
`NETWORD_UNAVAILABLE`|-10	|网络不可用|所有有联网操作的方法
`INVALID_POINT`|-11|非法的数据端点（1.该设备数据端点索引越界；2.value类型不匹配； 3.未添加正确的数据模版；|setDataPoint()
...	| ... | ... | ...

注意：这里的返回值只是表示调用成功，如果小于0则表示该函数未调用成功，自然也就不会回调监听器里面的函数。


### SDK功能函数：

#### 1.void init(Context mContext)

**1.说明：**

	初始化xlink sdk,使用sdk前，必须调用

**2. 参数**

    mContext ApplicationContext实例  

#### 2.int start()

**1.说明：**

	①.启动内网服务，绑定udp 如第一次使用 需要连接到和wifi设备同一局域网
	②.该链接会自己重连维护
	③.如果确保不使用局域网，该方法可以不调用（可判断是否是移动网络）

**2.返回值:**

	= 0：调用成功
	< 0：调用失败,失败code请观看同步错误码;

**3.对应回调:**

    XlinkNetListener onStart(int code) ，详情请参见XlinkNetListener 说明

#### 3.int login(int appid, String authKey)

**1.说明:**

	a.使用APPID和AuthKey登录到CM服务器。APPID和AuthKey的获取，请查看demo代码
	b.获取到的APPID和AuthKey，由外部APP缓存维护。
	c.该方法不用重复调用，调用一次后，会自己断线重连
	d.只有login成功后，才能使用跟云端有关的服务

**2.参数：**

	String：APPID
	String：AuthKey

**3.返回值：**

	= 0：调用成功
	< 0：调用失败,失败code参见 同步错误码;

**4.对应回调：**

	XlinkNetListener onLogin(int code) ，详情请参见XlinkNetListener 说明

#### 4.boolean initDevice(XDevice device)

**1.说明：**

	a.向SDK中初始化设备节点
	b.APP可以缓存设备节点，在下次程序启动后，可以自行初始化设备节点到SDK，用于跳过Scan步骤。
	c.SDK把设备节点放入内部队列，用于设备的定位和回调时的参数。
	d.初始化设备时SDK不要向设备发送任何消息包，包括handshake。

**2.参数：**

	  XDevice：Device实体对象

**3.返回值：**

	  true:向SDK添加设备成功
	  false:添加设备失败，设备属性错误


#### 5.boolean setDataTemplate(String productID, String product)

**1.说明：**

	添加数据模版,在企业管理台定义的设备数据端点

**2.参数:**

	prodctID  产品id
	product 数据端点描述 是jsonarray格式.

**3.返回值：**

	true  添加成功
	false 添加失败（product 不是json array格式）

#### 6.XDevice JsonToDevice(JSONObject jsonObject)

**1.说明：**

	把jsonObject 序列化成Xdevice对象（除了scan，是获取XDevice实例的唯一接口）

**2.参数：**

    jsonObject ：设备的json对象

**3.返回值**

	①.XDevice : 序列化成功后的Device对象
	②.NULL： jsonObject错误

#### 7.JSONObject   deviceToJson(XDevice device)

**1.说明：**

	如果需要存储设备，通过此接口把device序列话成JSONObject对象，然后存储

**2.参数：**

    device XDevice实例

#### 8.void  stop()

**1.说明：**

	a.停止SDK，释放SDK所有占用的资源,在APP退出,或者不再使用SDK时调用。
	b.该函数会清空initDevice()后设备列表;
	c.清空addXlinkNetListener监听器列表；

#### 9.void addXlinkListener(XlinkNetListener listener)

**1.说明：**

	添加XlinkNetListener 监听器，请确保全局至少有一个监听器。

#### 10.void debug(boolean debug)

**1.说明：**

	设置日志是否打印

#### 11.boolean isConnectedOuterNet()

**1.说明：**

	判断是否连接上xlink服务器

**2.返回值：**

	true/已连接 false/未连接

#### 12.boolean isConnectedLocal()

	1.说明：判断sdk 是否启动本地内网服务
	2.返回值：true/启用 false/未启用

### 下面是操作设备的函数

#### 1.int scanDeviceByProductID(String productId,ScanDeviceListener listener)

**1.说明：**

	通过productid扫描内网内所有对应的设备;需开启wifi 并连接到设备所在的wifi网络

**2.参数：**

      String：productid  设备的产品id
      ScanDeviceListener listener 监听器

**3.返回值：**

     = 0：调用成功
     < 0：调用失败,失败code参见 同步错误码;

**4.对应回调：**

   	ScanDeviceListener onGotDeviceByScan(XDevice device)


#### 2. int connectDevice(XDevice device, String auth, ConnectDeviceListener connectListener)

**1.说明：**

	调用该函数用于连接设备，确定设备处于内网还是外网;

**2.参数：**

    XDevice：Device实体兑现
    auth ：设备授权码
    connectListener： 监听器

**3.返回值：**

    =0：调用成功，
    < 0：调用失败,失败code参见 同步错误码;

**4.返回结果：**

    ConnectDeviceListener  onConnectDevice(XDevice xDeivce, int ret)

#### 3.int setDeviceAuthorizeCode(XDevice device, String oldAuthorize, String newAuthorize, SetDeviceAuthorizeListener listener)

**1.说明：**

	修改设备密码，该方法是根据connectDevice所返回的设备网络环境 来发送数据。

**2.参数：**

	XDevice：Device实体对象
	String：Old device auth code
	String：New device auth code
	listener：监听器

**3.返回值：**

	>0：调用成功，返回消息ID
	< 0:  app本地错误;详情参见同步错误码;

**4. 结果回调：**

	onSetDeviceAuthorizeCode 跟setLocalDeviceAuthorizeCode方法一样


#### 4.int setDataPoint(XDevice xdevice, int key, Object value,SetDataPointListener listener)

**1.说明：**

	设置设备的数据端点

**2.参数：**

	DeviceObject：Device实体对象
	key：端点索引
	value：端点值

**3.返回值：**

	>0：调用成功，返回消息ID
	< 0:  app本地错误;详情参见同步错误码;

**4.结果回调：**

	SetDataPointListener onSetDataPoint();

#### 5.int subscribeDevice(XDevice device, String authCode,SubscribeDeviceListener listener)

**1.说明：**

	a.订阅设备(必须有公网环境)(如果在公网环境下使用
	b.公网环境调用 XlinkAgent#connectDevice()会自动调用该函数;
	c.设置设备密码后，订阅关系会清空

**2.参数：**

	DeviceObject：Device实体对象
	authCode：设备授权码
	listener： 监听器

**3.返回值:**

	= 0：调用成功；
	< 0:  app本地错误;详情参见同步错误码;

**4.结果回调：**

	onSubscribeDevice()

#### 6.int sendPipeData(XDevice device, byte[] data, SendPipeListener listener)

**1.说明：**

	向设备发送pipe数据包.

**2.参数：**

	DeviceObject：Device实体对象
	byte[]：Pipe数据

**3.返回值：**

	>0：调用成功，返回消息ID
	< 0:  app本地错误;详情参见同步错误码;

**4.结果回调：**

	onSendPipeData

## 五.XlinkNetListener 回调说明：

### 1. onStart(int code)

	1.说明：调用XlinkAgent.start()的回调
	2.返回值 code:

XlinkCode常量|int实际值|说明
---- | ---- | ----
`SUCCEED`|0|成功
`LOCAL_CONNECT_ERROR`	|-1	|绑定端口失败
...|...|...

### 2. onLogin(int code)  

	1.说明：调用XlinkAgent.login的回调(如果已经login 服务器成功，是不会再回调该函数)
	2.返回值 code:

XlinkCode 常量|int实际值|说明
---- | ---- | ----
`SUCCEED`|0|登录服务器成功
`CLOUD_CONNECT_ERROR`|-1|连接公网服务器失败（解析域名失败/无网络连接/网络响应超时）
`CLOUD_CONNECT_NO_NETWORK`|-2|无物理网络连接
`TIMEOUT`|-100|登录服务器超时（原因：手机网络不稳定)
`SERVER_CODE_INVALID_PARAM`|1|参数错误（sdk内部错误）
`SERVER_CODE_INVALID_KEY`|2|app key不正确
`SERVER_CODE_UNAVAILABLE_ID`|3|非法的 appid
`SERVER_CODE_SERVER_ERROR`|4|服务器内部错误
... | ...	| ...

### 3.  onDisconnect(int code)

**1.说明：**

	对应于login，当app从xlink服务掉线时，会回调该方法
	云端连接会自己断线重连(网络异常，心跳异常才会，其他异常需要处理)，不用重复调用Login()方法

**2.返回值 code:**

XlinkCode 常量|int实际值|说明
---- | ---- | ----
`CLOUD_STATE_DISCONNECT`|-1|网络问题导致和服务器连接中端(不需要处理，会自动重连)
`CLOUD_KEEPALIVE_ERROR`|-2|和服务器心跳异常，导致从服务器掉线(不需要处理，会自动重连)
`CLOUD_SERVICE_KILL`|-3|XlinkTcpServrce服务被异常杀死（如360等安全软件）.（需要重新调用login函数）
`CLOUD_USER_EXTRUSION`|-4|该app id在其他地方登录(提示用户，帐号被挤)


### 4. onLocalDisconnect(int code);

**1.说明：**

	本地服务断开

**2.断开原因 code:**

XlinkCode 常量|int实际值|说明
---- | ---- | ----
`LOCAL_THREAD_ERROR`|-1|无物理网络
`LOCAL_SERVICE_KILL`|-2|XlinkUdpServrce服务被异常杀死（如360等安全软件),需要重新调用start函数。
...|...|...

### 5. onRecvPipeData(XDevice device, byte[] data)

**1.说明：**

	收到局域网内设备推送的pipe数据 （跟设备直连，返回的数据）

**2.返回值 :**

	device：  该设备的 pipe数据
	data：byte数据

### 6. onRecvPipeSyncData(XDevice device, byte[] data)

**1.说明：**

	收到服务器推送的同步pipe数据

**2.返回值 :**

	device：  该设备的 pipe数据
	data：byte数据

### 7. onDataPointUpdate(XDevice xDevice, int key,Object value, int channel,int type);

**1.说明：**

	设备数据节点发生改变，会回调此方法

**2.返回值 :**

	device：  设备实例
	key：      端点索引
	value     端点值
	channel： 通道，区分是在局域网直连设备的更新端点，还是云端更新端点、channel ==XlinkCode.CHANGED_UPDATAPOINT_LOCAL  为本地局域网通道channel ==XlinkCode.CHANGED_UPDATAPOINT_CLOUD  为yun'd云端网络通道

### 8.  type: value的类型，


XlinkCode 常量|具体int值|说明
---- | ---- | ----
`POINT_TYPE_BOOLEAN`|1|布尔值
`POINT_TYPE_BYTE`|2|byte单字节
`POINT_TYPE_SHORT`|3|int16 (short)
`POINT_TYPE_INT`|4|int32 (int)
`POINT_TYPE_STRING`|5|string


### 9.onDeviceStateChanged(XDevice xdevice, int state);

**1.说明：**

	设备当连接成功/掉线，如果设备有状态变化会回调该方法.
	每当设备掉线，会自动调用connectDevice 一次。

**2.返回值 :**

	device：  设备实例
	state: 状态 :

XlinkCode 常量|	int实际值|	说明
-----|-----|-----
`DEVICE_CHANGED_CONNECTING`|	-1|	设备重新连接中
`DEVICE_CHANGED_OFFLINE`|	-2|	设备掉线
`DEVICE_CHANGED_CONNECT_SUCCEED`|	-3|	设备重新连接成功
...|	...|	...

## 六.XLINK 设备的操作

### 1.设备和用户的关系:
#### ①.内网模式:

	内网通讯不经过云智易服务器，在无互联网环境下也能使用设备;
	在这种模式下，用户和设备唯一凭证是设备密码
	内网控制之前，必须先调用XlinkAgent connectDevice()函数，否则进行设备操作，会同步返回 NO_HANDSHAKE -2的错误码

#### ②.公网模式:

	设备激活后，设备有属性 deviceID,该属性是每个设备在云智易后台唯一不重复的标识
	在公网环境，调用XlinkAgent connectDevice()函数连接设备成功后，SDK会自动把当前appId 跟 deviceID 进行订阅绑定;
	当有了订阅关系，公网环境下就无需调用XlinkAgent connectDevice()函数，可直接进行设备的操作。

#### ③.设备密码:

	每个设备都有一个设备密码，设备需要设置密码后才能使用,该密码是设备的唯一凭证;
	可调用 XlinkAgent setDeviceAuthorizeCode() 进行修改设备密码（也是通过该方法设置初始密码）
	修改设备密码成功后，服务器会清空以前所有的订阅关系，这时需要重新订阅设备才能使用公网进行控制设备

#### ③.设备的分享:

	调用deviceTojson把需要分享的设备序列号成功json字符串，然后把该json字符串分享（分享方式有二维码，等等）给被分享人，
	在被分享人中调用jsonToDevice把该字符串反序列化成XDevice实体类。

### 2.设备的获取:

#### ①.配置设备上网

	配置设备上网是和wifi厂商有关，不同的wifi芯片对应的配置程序和SDK不同，一般是由WIFI厂商提供
	这个不属于XLINK SDK的一部分。需要APP开发者找到对应的   WIFI配置SDK，自行将功配置功能集成到APP中。
	比如HF的Smartlink，庆科的Easylink等


#### ②.当用户第一次使用SDK时，通过扫描获取

	XlinkAgent scanDeviceByProductId(String productId,   ScanDeviceListener listener) 获取设备实例

#### ③.如果有设备实例序列化对象，通过该接口获取：

	XlinkAgent  JsonToDevice(JSONObject jsonObject) 把jsonObject 反序列化成设备实例

### 3.设备的存储:  

#### ①.SDK不提供存储设备,SDK有提供了设备序列化接口:

    XlinkAgent  deviceToJson(XDevice device)
	可以获取该设备的jsonObject对象，而后可转换为String字符串进行存储(云端/本地)
	Demo程序的存储方式是数据库本地存储；

#### ②.如果需要云端同步设备，可使用云智易数据服务集成接口进行云端同步设备

	(详情请查阅文档 云智易RESTful Service(数据服务集成接口))
	注意：在app运行周期过程中，设备的属性可能会被更改（如 deviceID, 设备版本号等信息）;
	当退出程序之前，需要重新调用deviceToJson 接口，获取最新的设备json对象;
	然后匹配下之前的设备json对象，如果有修改，则需要把最新的设备json对象替换。            

### 4.设备的操作:

#### ①. 扫描到设备后可通过isInit() 属性判断设备是否初始化过，如果未初始化设备需要设置初始授权码才能使用设备

(1).流程如下：      

* Scan Device
	* isInit()==true
		* 连接设备需要提示，让用户输入之前设置过的设备密码
		* 连接设备成功
		* 控制设备
	* isInit()==false
		* 设置设备初始密码(设置成功后，自行存储)
		* 连接设备
		* 连接设备成功
		* 控制设备

注：参数里oldAuthorize可随便填入4位以上字符串

(2). 序列化接口不提供存储设备密码，设备密码存储由第三方自行决定，可参考Demo;

	如果设备安全等级不高，上述步骤可由APP自动实现；
	（Demo就是这种方式：扫描设备成功后，判断isInit==false则设置内置的设备密码“8888”，然后连接设备都是传入8888进行连接）

#### ②. 如果使用 XlinkAgent  JsonToDevice(JSONObject jsonObject) 方法获取的设备实例需要调用：

	XlinkAgent  initDevice(XDevice device) 
	向SDK添加设备；否则调用和设备相关的接口会返回错误码:NO_DEVICE

#### ③. 如果需要扩展设备属性,可参考Demo程序

	io.xlink.wifi.js.bean/Device.java

### 5.设备的控制:

#### ①. 数据透传:

	XlinkAgent sendPipeData(XDevice device, byte[] data, SendPipeListener listener) ;

#### ②. 数据端点控制：

	XlinkAgent setDataPoint(XDevice xdevice, int key, Object value,SetDataPointListener listener)

### 6.设备的推送数据:

#### ①. 数据透传:

	XlinkNetListener  onRecvPipeData(XDevice xdevice, byte[] data)
	该接口回调出设备对当前app发送的数据。
	XlinkNetListener onRecvPipeSyncData(XDevice xdevice, byte[] data)
	该接口回调出设备对所以在线订阅过xdevice的app发送的数据

注意：有可能回调出来的XDevice 对象只有 deviceId一个属性,是因为在sdk内部未找到当前XDevice实例,只能把带deviceId一个属性的设备回调出来

#### ②. 数据端点:

	XlinkNetListener  onDataPointUpdate(XDevice xdevice,int key,Object value,int hannel,int type)
	如果设备定义了数据端点，当设备数据端点有变化时，会通过该函数回调出设备当前的数据端点

### 1.XDevice暴露属性：

boolean isInit()

	说明返回该设备是否第一次使用(第一次使用原密码，可直接设置新密码，使用设备必须先设置设备密码)
boolean isHandshake()

	说明返回该设备是否可用户局域网直连
boolean isValidId()

	说明返回该设备是否激活
int getDevcieConnectStates()

	说明返回该设备当前连接状态XlinCode.DEVICE_STATE_OFFLIEN等
String getProductId

	说明：返回该设备产品ID
String getAddress

	说明：返回该设备在局域网的ip地址
String  getMacAddress

	说明：返回该设备mac地址,唯一的设备标识量

其他不必要属性请见XDevice API文档


## 七.监听器返回Code说明

###返回码都在XlinkCode类下  

#### 1.ConnectDeviceListener onConnectDevice(XDevice xDevice, int result);

	连接设备监听器
	retsult有:

XlinkCode 常量|	int返回值|	说明
-----|-----|-----
`DEVICE_STATE_OUTER_LINK`|	1|	连接设备成功，设备处于公网环境
`DEVICE_STATE_LOCAL_LINK`|	0|	连接设备成功，设备处于局域网环境
`CONNECT_DEVICE_TIMEOUT`|	200	|连接设备超时，设备假在线/手机网络差/设备网络差
`CONNECT_DEVICE_SERVER_ERROR`|	104|	app内部错误
`CONNECT_DEVICE_INVALID_KEY`|	102	|设备认证失败，密码错误
`CONNECT_DEVICE_OFFLINE`|	110	|设备不在线
`CONNECT_DEVICE_NO_ACTIVATE`|	109	|设备未激活
`CONNECT_DEVICE_OFFLINE_NO_LOGIN`|	111|	设备局域网不在线，且当前app只有局域网环境，无法云端连接设备
...|	...	|...


#### 2.ScanDeviceListener onGotDeviceByScan(XDevice device);

	扫描设备监听器
	device：扫描到的设备对象;

#### 3.SendPipeListener onSendLocalPipeData(XDevice device, int code,messageId);

	发送pipe数据监听器
	code有:

XlinkCode 常量|	INT返回值|	说明
-----|-----|-----
`SUCCEED`	|0|	成功（除了SUCCEED，其他的都是发送失败）
`TIMEOUT`|	-100|	发送pipe数据超时
`SERVER_CODE_INVALID_PARAM`|	1	|公网环境下：参数错误。内网环境下：会话ID已经失效，需要重新连接设备，一般出现在设备重启过。
`SERVER_CODE_UNAVAILABLE_ID`|	3|	设备id错误（设备未激活）
`SERVER_CODE_SERVER_ERROR`|	4|	服务器内部错误
`SERVER_CODE_UNAUTHORIZED`|	5|	设备未授权（无订阅关系，需要调用函数connectDevice/subscribeDevice 重新在服务器建立订阅关系）
`SERVER_DEVICE_OFFLIEN`|	10|	设备不在线
...|	...|	...


messageId  : 跟调用sendPipe接口返回的 msgId 一一对应


#### 4.SetDataPointListener onSetDataPoint(XDevice xdevice, int code，int messageId);

	设置设备数据端点
	返回的code 跟SendPipeListener 一样
	messageId：跟调用sendPipe接口返回的 msgId 一一对应

#### 5.SetDeviceAuthorizeListener  onSetLocalDeviceAuthorizeCode(XDevice device, int code,messageId);

	设置设备密码
	返回的code 跟SendPipeListener 一样，新增底下错误码；

新增
XlinkCode|	INT值|	说明
----|----|----
`SERVER_CODE_INVALID_KEY`	|2|	设置密码失败，认证设备失败（oldAuth错误）
messageId  : 跟调用sendPipe接口返回的 msgId 一一对应


#### 6.SubscribeDeviceListener onSubscribeDevice(XDevice device, int code);

	订阅设备
	返回的code 跟SendPipeListener  一样



完!

©2016 云智易物联云平台（http://www.xlink.cn）
