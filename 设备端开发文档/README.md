Copyright©2016  **云智易**物联云平台（http://www.xlink.cn）

# 物联平台硬件接口文档


## 总概

* 智易硬件SDK提供各种不同CPU架构的C库，方便快捷将各种硬件设备快速安全地接入到云智易物联平台；
* 提供常用模组的例程示例，加快开发者开发周期；
* 提供透传固件，开发者可不用开发设备接入云平台的固件，便可将设备简单快速联网；
* 支持强大的数据端点，可将设备的运行状态、参数和报警信息映射到平台，方便设备的管理和监控，并能快捷将报警内容通过邮件、短信或应用推送给用户；
* 提供完善的硬件通讯协议，很好地支持各种产品的功能实现。

## 硬件SDK开发

### 硬件SDK说明

**主要特点：**

* 本地设备自动探测和扫描联网；
* 内置云端连接服务，无需额外对接开发；
* 支持内、外网网络自动检测和切换；
* 提供云端固件升级和远程配置、诊断和管理服务；
* 支持设备安全认证和数据加密机制；
* 支持微信Air Kiss、SmartLink和easylink一键配置；
* 支持ARM、x86、x64以及MIPS等不同处理器架构；
* 可移植性强，使用操作简单快捷。

详细说明请参考[硬件SDK说明](https://github.com/xlink-corp/xlink-sdk/blob/master/设备端开发文档/1.XlinkSDK规范/1.SDK介绍.md "硬件SDK说明")。

### 硬件SDK及下载地址

硬件SDK提供了常用的wifi模组SDK以及例程，如：汉枫的LPB100、LPT100和LPT200等，其他模组或者芯片型号可联系我司获得。

设备SDK下载地址：[硬件SDK下载](https://github.com/xlink-corp/xlink-sdk/tree/master/设备端开发文档/1.XlinkSDK规范/SDK及固件)。

### 硬件SDK接入流程

接入流程示意图如下图所示：

![](https://raw.githubusercontent.com/xlink-corp/xlink-sdk/master/设备端开发文档/1.XlinkSDK规范/images/硬件接入流程图.jpg)

详情请查看[SDK使用流程](https://github.com/xlink-corp/xlink-sdk/blob/master/设备端开发文档/1.XlinkSDK规范/2.SDK使用流程.md)。

### 硬件SDK接口说明

主要接口如下：

接口名称|接口
----|----
SDK配置|unsigned char XlinkSystemInit(char * product_id, char * product_key, XLINK_USER_CONFIG *config)
SDK主循环|void XlinkSystemLoop(xsdk_time_t c_time, x_int32 timeout_ms)
公网连接|int XlinkSystemTcpLoop(void)

详情请查看[硬件SDK接口](https://github.com/xlink-corp/xlink-sdk/blob/master/设备端开发文档/1.XlinkSDK规范/3.硬件SDK接口文档.md)。

### 数据端点使用说明

数据端点有强大的功能，主要功能：可映射设备运行状态到平台，可通过数据端点设置设备参数或控制设备，设备可通过数据端点将需要存储的数据存储到服务器，设备也可将需要报警的信息通过数据端点推送到用户手上。

主要流程如下图：

![](https://raw.githubusercontent.com/xlink-corp/xlink-sdk/master/设备端开发文档/1.XlinkSDK规范/images/数据端点交互过程.jpg)

数据端点的数据格式、支持的数据类型以及对应接口的使用请参考[数据端点使用](https://github.com/xlink-corp/xlink-sdk/blob/master/设备端开发文档/1.XlinkSDK规范/4.数据端点文档.md)文档。

### 硬件通讯协议规范

通常在数据透传上使用，因为对设备的操作需要一个通讯协议来支持，硬件通讯协调提供作为参考或使用。

详情请查看[硬件通讯协议规范](https://github.com/xlink-corp/xlink-sdk/blob/master/设备端开发文档/2.设备通讯协议规范.md)。

### 常见问题

SDK在开发使用中遇到常见的问题，可参看[常见问题和解答](https://github.com/xlink-corp/xlink-sdk/blob/master/设备端开发文档/3.常见问题和解答.md)文档。


Copyright©2016  **云智易**物联云平台（http://www.xlink.cn）