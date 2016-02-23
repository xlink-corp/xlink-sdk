©2016  **云智易**物联云平台（http://www.xlink.cn）


# Windows SDK集成文档

## 说明

- 该文档适用于使用云智易Windows SDK的开发人员。
- 使用云智易Windows SDK可以将Windows系统作为设备接入到云智易物联网平台，已使用平台提供的相关功能。
- 云智易Windows SDK提供了包括设备（也就是Windows系统）连接云端，发送数据，接收数据，接收平台，接收云端消息以及接收和响应云端控制指令等功能。


## **技术约束**

- Windows SDK使用Windows API编写，通过Windows动态连接库导出函数方式暴露接口。
- Windows SDK支持多种语言调用，如VC，C#，VB，Delphi等。
- 使用Windows SDK必须保证Windows处于能够连接互联网状态。


## **名称解释**

- **ProductID**
  * 产品ID，所有连入云智易的设备，都属于某一个产品，ProductID也就是产品ID，是平台颁发的唯一ID，用来标识这个产品。
- **ProductKey**
  * 产品密钥，每个产品都有一个密钥，该密钥用于产品下的设备激活使用。激活算法SDK已经实现，调用者只需要将该值初始化到SDK中即可。
- **DeviceID**
  * 设备ID，所有的连入云智易的设备，都有一个由云智易颁发的唯一标识ID。这个ID在设备导入到平台时产生。
- **DeviceKey**
  * 设备密钥，设备在激活时得到，设备密钥用于设备连接云端的凭证。需要SDK调用者在本地缓存，以便设备到云端认证并且连接。
- **设备导入**
  * 云智易平台中的设备，采用预添加的方式产生，也就是需要先在云智易平台中添加或导入设备后，该设备才可以使用。
- **设备MAC**
  * MAC：Machine Address Code，设备地址码，所有平台下的设备都有一个MAC属性，设备MAC在产品下唯一。在设备导入平台，以及设备联网激活时使用。MAC地址要求由12位1~9和A-F的字符组成，如FE0011223344BF。
- **设备数据端点**
  * 设备运行状态的描述，一个设备可以有多个数据短点，一个数据端点索引，端点类型，端点值组成。平台可以查看设备的数据端点用于检查设备的运行状态。也可以通过接口更改数据端点，用于改变设备的运行状态。


## **接口**

###### ***注：该文档中所描述的接口都是SDK提供的原始接口，与调用语言无关。针对不同语言的调用Demo，将按需求随SDK一同提供。***

## **接口概览**

1. QueryNewXDevice
2. FreeXDeviceByXID
3. SetXDeviceInfoByXID
4. AddXDeviceDataPointByXID
5. SetXDeviceDataPointByXID
6. StatXDevice
7. StopXDevice
8. SendDevicePipeDataByXID
9. SendDeviceSyncPipeDataByXID
10. SetLogCallback
11. SetXDeviceCallback

## **回调概览**

1. 设备运行状态回调
2. 设备发送数据状态回调
3. 设备接收数据回调
4. 设备数据端点变化回调

## **接口说明**

### **1. QueryNewXDevice**

#### 说明

* 请求一个新的XDevice实例

#### 函数

    int QueryNewXDevice(const char * server_host, const char * product_id, const char * product_key, const  char * mac, const char * device_name);

#### 参数

参数名 | 类型 | 说明
---- | --- | ----
server_host | 字符串 | 设备需要连接云端服务器的地址，ip:port。
product_id | 字符串 | 产品ID
product_key | 字符串 | 产品密钥
mac | 字符串 | 设备MAC
device_name | 字符串 | 设备名称

#### 返回值

值 | 说明
--- | ---
`> 0` | 返回Device在SDK中的ID：XID
`< 0` | 错误

> XID不同于设备id，是SDK给该实例分配的标识ID。以后的控制都通过该ID定位设备。

### **2. FreeXDeviceByXID**

#### 说明

* 释放一个Device实例

#### 函数

    int FreeXDeviceByXID(int xid);

#### 参数

参数 | 类型 | 说明
---- | --- | ----
xid | 整形 | SDK分配的设备实例ID

#### 返回值

值 | 说明
--- | ---
`= 0` | 成功释放实例
`< 0` | 错误

### **3. SetXDeviceInfoByXID**

#### 说明

* 设置一个实例的基本信息。该函数用在调用者已经激活过设备，已经获得了设备的DeviceID和DeviceKey，并且已经保存下来，需要直接让该设备上网前调用。
* 调用该函数前，需要先调用QueryNewXDevice让SDK分配实例对象。得到XID

#### 函数

    SetXDeviceInfoByXID(int xid, int device_id, const char * device_key);

#### 参数

参数 | 类型 | 说明
---- | --- | ----
xid | 整形 | SDK分配的设备实例ID
device_id | 整形 | 云端分配的设备ID
device_key | 整形 | 云端返回的设备密钥

#### 返回值

值 | 说明
--- | ---
`= 0` | 调用成功
`< 0` | 调用失败

### **4. AddXDeviceDataPointByXID**

#### 说明

* 向设备中添加一个数据端点，一般用在设备初始化完成，还没连接上云端之前使用。

#### 函数

    int AddXDeviceDataPointByXID(int xid, int index, int type, const void * defaut_value);

#### 参数

参数 | 类型 | 说明
---- | --- | ----
xid | 整形 | 实例ID
index | 整形 | 端点索引
type | 整形 | 端点类型
default_value | 可变类型 | 初始化值

#### 返回值

值 | 说明
--- | ---
`= 0` | 成功
`< 0` | 失败

### **5. SetXDeviceDataPointByXID**

#### 说明

* 设置数据端点值，如果设备已经连接云端，数据短点值的变化会通知到云端，云端可以通过对应接口完成指定动作。如触发告警、通知等。

#### 函数

    int SetXDeviceDataPointValueByXID(int xid, int index, int type, const void * value, int len);

#### 参数

参数 | 类型 | 说明
---- | --- | ----
xid | 整形 | 实例ID
index | 整形 | 端点索引
type | 整形 | 端点类型
value | 可变类型 | 值，不同的类型，传入的类型不同
len | 整形 | 值的长度

#### 返回值

值 | 说明
--- | ---
`= 0` | 成功
`< 0` | 失败

### **6. StatXDevice**

#### 说明

* 启动一个设备，让设备连接到云端。
* 若设备还未激活，设备会自动完成激活后连接的动作。
* 设备的激活状态，连接状态将通过设置的状态回调（SetXDeviceCallback ）进行通知。

#### 函数

    int StartXDevice(int xid);

#### 参数

参数 | 类型 | 说明
---- | --- | ----
xid | 整形 | 实例ID

#### 返回值

值 | 说明
--- | ---
`= 0` | 成功
`< 0` | 失败

### **7. StopXDevice**

#### 说明

* 让设备从云端断开

#### 函数

    int StopXDevice(int xid);

#### 参数

参数 | 类型 | 说明
---- | --- | ----
xid | 整形 | 实例ID

#### 返回值

值 | 说明
--- | ---
`= 0` | 成功
`< 0` | 失败

### **8. SendDevicePipeDataByXID**

#### 说明

* 向指定的APP发送透传消息

#### 函数

    int	SendDevicePipeDataByXID(int xid, int app_id, const char * value, int len);

#### 参数

参数 | 类型 | 说明
---- | --- | ----
xid | 整形 | 设备实例ID
app_id | 整形 | 需要发送给的接收者ID
value | 二进制数据 | 发送的数据
len | 整形 | 发送的数据长度

#### 返回值

值 | 说明
--- | ---
`= 0` | 成功
`< 0` | 失败

### **9. SendDeviceSyncPipeDataByXID**

#### 说明

* 向所有订阅过该设备的APP广播透传消息

#### 函数

    SendDeviceSyncPipeDataByXID(int xid, const char * value, int len);

#### 参数

参数 | 类型 | 说明
---- | --- | ----
xid | 整形 | 设备实例ID
value | 二进制数据 | 发送的数据
len | 整形 | 发送的数据长度

#### 返回值

值 | 说明
--- | ---
`= 0` | 成功
`< 0` | 失败

### **10. SetLogCallback**

#### 说明

* 设置日志回调接口，可以让外围程序看到SDK运行日志。

#### 函数

    void SetLogCallback(void * lpLogCallback);

#### 参数

参数 | 类型 | 说明
---- | --- | ----
lpLogCallback | 函数指针 | 回调函数。回调函数类型定义见后面说明。

#### 返回值

值 | 说明
--- | ---
na | na

### **11. SetXDeviceCallback**

#### 说明

* 设置SDK回调，SDK的设备动作和操作都是异步完成的，所以必须设置回调才可以知道设备动作状态，以及操作的最终结果。

#### 函数

    void SetXDeviceCallback(void * lpDeviceCallback, void * lpDeviceSendCallback, void * lpDeviceRecvCallback, void * lpDeviceDataPointCallback);

#### 参数

参数 | 类型 | 说明
---- | --- | ----
- lpDeviceCallback | 函数指针 | 设备运行状态回调
- lpDeviceSendCallback | 函数指针 | 设备发送数据状态回调
- lpDeviceRecvCallback | 函数指针 | 设备接收数据回调
- lpDeviceDataPointCallback | 函数指针 | 设备数据端点变化回调

#### 返回值

值 | 说明
--- | ---
na | na


## **回调说明**

### **1. 设备运行状态回调**

#### 说明

* 设备的联网状态，激活状态等基本状态通过该回调通知

#### 定义

    typedef void (CALLBACK * OnXDeviceCallbackPtr)(int nDeviceEvent, int xid, void * param0, void * param1);

#### 参数

参数 | 类型 | 说明
---- | --- | ----
nDeviceEvent | 整形 | 事件类型，具体类型见附录
xid | 整形 | 实例ID
param0 | 可变类型 | 参数，不同的事件，值不同
param1 | 可变类型 | 参数，不同的事件，值不同

### **2. 设备发送数据状态回调**

#### 说明

* 设备可以通过接口发送数据给指定的APP或者直接发送数据给云平台，发送数据是个异步的过程，结果通过该回调通知

#### 定义

    typedef void (CALLBACK * OnXDeviceSendCallbackPtr)(int nSendEvent, int xid, void * param0, void * param1);

#### 参数

参数 | 类型 | 说明
---- | --- | ----
nSendEvent | 整形 | 发送事件，值见附录
xid | 整形 | 实例ID
param0：参数 | 可变类型 | 不同的事件，值不同
param1：参数 | 可变类型 | 不同的事件，值不同

### **3. 设备接收数据回调**

#### 说明

* 当设备收到云端发送下来的数据或指令，通过该回调通知外部，用于做对应处理。

#### 定义

    typedef void (CALLBACK * OnXDeviceRecvCallbackPtr)(int nRecvEvent, int xid, void * param0, void * param1, void * param2);

#### 参数

参数 | 类型 | 说明
---- | --- | ----
nRecvEvent | 整形 | 接收事件，值见附录
xid | 整形 | 实例ID
param0：参数 | 可变类型 | 不同的事件，值不同
param1：参数 | 可变类型 | 不同的事件，值不同

### **4. 设备数据端点变化回调**

#### 说明

* 当设备的数据端点发生变化，无论是本地或者云端引起的变化，都会通过该接口进行通知。

#### 定义

    typedef void (CALLBACK * OnXDeviceDataPointCallbackPtr)(int xid, int index, int type, void * data);

#### 参数

参数 | 类型 | 说明
---- | --- | ----
xid | 整形 | 实例ID
index | 整形 | 索引
type | 整形 | 类型
data | 可变类型 | 值，不同的类型，返回的值不同

## **SDK通用调用流程**

### **1. 加载和使用**

1. 通过企业管理台获取设备ProductID和ProductKey
2. 加载SDK库
3. 调用SetXDeviceCallback设置各种回调
4. 调用QueryNewXDevice获取新实例
    - 若设备已经激活，并且本地缓存了DeviceID和DeviceKey，调用SetXDeviceInfoByXID设置基本属性
5. 调用AddXDeviceDataPointByXID初始化设备数据端点，需要和管理台中，产品的数据端点设置对应。
6. 调用StatXDevice启动设备
7. 通过各种回调监听设备状态，完成外围程序处理。

### **2. 释放和卸载**

1. 调用StopXDevice停止设备
2. 调用FreeXDeviceByXID释放实例
3. 释放SDK库。

## **附录**

### **1. 通用错误码**

值 | 说明
--- | ---
0 | 没有错误
-1 | 未知错误
-100 | 数据端点已经存在
-101 | 数据端点不存在
-404 | 设备不存在

### **2. 数据端点类型**

定义 | 枚举值 | 说明
------- | --- | ---
`DATAPOINT_TYPE_BOOL` | 0 | 布尔
`DATAPOINT_TYPE_BYTE` | 1 | 单字节
`DATAPOINT_TYPE_INT16` | 2 | 短整形
`DATAPOINT_TYPE_INT32` | 3 | 长整形

### **3. 设备运行状态回调**

定义 | 枚举值 | 说明
---- | ----- | ---
`E_DEVICE_INIT` | 1 | 设备初始化完毕
`E_DEVICE_CONNECTING` | 2 | 设备正在连接云端服务器
`E_DEVICE_ACTIVATE` | 3 | 设备激活成功。`param0:(int)device_id, param1:(const char *)device_key`
`E_DEVICE_CONNECT` | 4 | 设备连接云端完成
`E_DEVICE_DISCONNECT` | 5 | 设备从云端断开。`param0:(int)reason, param1:(int)more reason`
`E_APP_CONNECTED` | 10 | 有APP从内网连接上这个设备了；`param0:(int)session_id`
`E_APP_DISCONNECT` | 11 | 内网APP与这个设备的连接断开；`param0:(int)session_id`

### **4. 设备发送数据状态回调**

定义 | 枚举值 | 说明
---- | ----- | ---
`E_DEVICE_SEND_PIPE` | 100 | 设备发送透传指令结果。`param0：(int)messgeid; param1:(int)result;`
`E_DEVICE_SEND_PIPE_SYNC` | 101 | 设备发送透传SYNC。`param0：(int)messgeid; param1:(int)result;`
`E_DEVICE_SEND_LOCAL_PIPE` | 102 | 设备发送本地透传指令结果，`param0：(int)messgeid; param1:(int)result;`

### **5. 设备接收数据回调**

定义 | 枚举值 | 说明
---- | ----- | ---
`E_DEVICE_RECV_PIPE` | 200 | 设备接收透传指令。`param0:(int)app_id;param1:(int)data len;param2:(const char *)data;`
`E_DEVICE_RECV_PIPE_SYNC` | 201 | 预留
`E_DEVICE_RECV_PIPE_LOCAL` | 202 | 设备接收本地APP发送的透传指令。`param0:(int)session_id;param1:(int)data len;param2:(const char *)data;`
`E_DEVICE_RECV_ACCESS_KEY` | 203 | 设备收到APP设置的ACCESS_KEY。`param0:(int)session_id;param1:(int)access_key;`


©2016  **云智易**物联云平台（http://www.xlink.cn）
