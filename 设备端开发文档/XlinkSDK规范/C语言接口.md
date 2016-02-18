# C语言接口
本文主要介绍如何使用Xlink SDK静态库C语言接口。
##1.API接口

###1.1初始化接口

unsigned char XlinkSystemInit(char * product_id, char * product_key, XLINK_USER_CONFIG *config)

功能：系统初始化。

参数：

product_id：产品id，长度为32字节，由云智易云平台产生；

product_key:产品id，长度为32字节，由云智易云平台产生；

config：XLINK_USER_CONFIG请参考“XLINK_USER_CONFIG”结构体说明。

返回值：1 成功；0 失败。

###1.2设置SDK WIFI连接状态

void XlinkSystemSetWifiStatus(unsigned char status);

功能：设置wifi状态值。（只有将wifi 状态设置为1后，SDK才能使用公网）

参数：

status：1表示wifi模块链接上路由器，否则与路由器断开。

返回值：none。

###1.3设置设备默认名称

void XlinkSystemSetDeviceName(char *NameStr);

功能：设置wifi设备名称，最长16个字符。

参数：

NameStr：wifi设备名称。

返回值：none。

###1.4通过公网发送数据给指定APP

x_int32 XlinkSendTcpPipe(unsigned char * data, unsigned int datalen, x_uint32 to_id);

功能：向公网发送TCP数据，此数据将发送到ID号为to_id的设备上。

参数：

data：发送的数据；

Datalen：数据长度（wifi SDK通信默认只能发送小于1000字节）。 

 返回值：发送字节数。

###1.5通过公网发送数据给关注此设备的APP

x_int32 XlinkSendTcpPipe2(unsigned char * data, unsigned int  datalen);

功能：向公网发送数据,此数据将发送到关注了此设备的APP上。

参数：

data：发送的数据；

datalen:数据长度（wifi SDK通信默认只能发送小于1000字节）。

返回值：发送字节数。

###1.6发送数据给本地连接的APP

x_int32 XlinkSendUdpPipe(unsigned char *data, unsigned int  datalen,xlink_addr *addr);

功能：向本地连接了此设备的APP发送数据

参数：

data：发送的数据；

datalen:数据长度（wifi SDK通信默认只能发送小于1000字节）。   

Addr:接收设备的地址，如果需要发送数据给所以连接了设备的app，则addr=NULL。

返回值：发送字节数。

###1.7系统主循环函数

void XlinkSystemLoop(xsdk_time_t c_time, x_int32 timeout_ms);

功能： 系统主循环,需要在任务中循环调用此函数

参数： 

c_time：当前时间；

timeout_ms：超时时间。

返回值：none。

###1.8获取服务器时间

void XlinkGetServerTime(void);

功能： 启动获取时间任务功能，得到时间后会取消此任务。

获得时间通过用户初始化的回调函数输出。如果需要再次获取时间，需要再次调用此函数。

参数：none。

返回值：none。

###1.9获取本地模糊时间

int  XlinkGetSystemTime(XLINK_SYS_TIME *pTime);

功能：获取本地的计时时间。

参数：本地时间结构指针，返回本地时间。 

返回值：none。

###1.10开启扫描发现模式

XLINK_FUNC void XlinkPorcess_UDP_Enable(void);

功能： 此API需要在初始化协议栈后调用，如果没调用此函数，设备将永远无法被手机APP通过本地扫描到。

参数：none。

返回值：none。

###1.11关闭扫描发现模式

XLINK_FUNC void XlinkPorcess_UDP_Disable(void);

功能： 关闭被手机APP扫描发现。

参数：none。

返回值：none。

###1.12复位SDK

XLINK_FUNC void XlinkReSetSDK(void);

功能： 清除清空SDK保存任何数据，重置为出厂设置。

参数：none。

返回值：none。

###1.13查看SDK版本 

char *XlinkSystemVersion(void);

功能：获取本地SDK的版本信息。

参数：none。

返回值：SDK版本信息的指针。

###1.14公网连接

int XlinkSystemTcpLoop(void);

功能：连接公网服务器、维护公网服务器连接和断线重连等。

参数：none。

返回值：none。


##2.结构体说明

###2.1XLINK_USER_CONFIG

结构体定义及说明如下程序清单。

```
typedef struct XLINK_USER_CONFIG {
	//数据端点接收回调函数，此版本不使用，请赋值为NULL
	void (*dataPoint_msg)(unsigned char type, unsigned int index, unsigned int value);
	//接收服务器TCP广播数据的回调函数，默认为NULL
	void (*tcp_pipe2)(unsigned char * Buffer, unsigned int BufferLen, x_uint8 *opt);
	//接收APP通过服务器发送的TCP数据的回调函数
	void (*tcp_pipe)(unsigned char * Buffer, unsigned int BufferLen, x_uint32 AppID, x_uint8 *opt);
	//接收APP通过本地发送的UDP数据的回调函数
	void (*udp_pipe)(unsigned char * Buffer, unsigned int BufferLen,xlink_addr *fromAddr);
	//SDK 内部写配置回调函数，需要将此数据写入flash，需要100个字节空间
	int (*writeConfig)(char *Buffer, unsigned int BufferLen);
	//SDK 内部读本地的配置文件，需要读出flash的配置数据
	int (*readConfig)(char *Buffer, unsigned int BufferLen);
	void (*status)(XLINK_APP_STATUS status);		 //SDK 系统状态信息回调
	void (*upgrade)(XLINK_UPGRADE *data); 		//SDK 收到服务器升级信息回调
	void (*server_time)(XLINK_SYS_TIME *time_p);	 //SDK 收到服务器同步时间信息回调
	xlink_debug_fn DebugPrintf; 	//SDK 内部日志输出回调
	unsigned char wifi_type;	 //WiFi类型
	unsigned short wifisoftVersion; 	//WiFi版本
	unsigned short endpointVersion;	//端点的版本
	unsigned char in_internet; 	//SDK 内部是否使用TCP公网连接
	unsigned char mac[6];		 //设备的MAC地址
    unsigned char pipetype; //硬件协议透传协议选择0:表示普通透传 1:表示透传内容为硬件通讯协议。
    unsigned short devicetype;	 //设备类型
    void (*Xlink_SetDataPoint)(unsigned char *data, int datalen);	//设置数据端点
	void (*Xlink_GetAllDataPoint)(unsigned char *data, int *datalen);	//获取所有数据端点
} XLINK_USER_CONFIG;```


