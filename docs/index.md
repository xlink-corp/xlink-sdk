## 云智易物联平台Open API


### 概览 (Overview)


云智易物联网平台是一个面向企业和开发者的 全栈 [IoT平台](https://www.xlink.cn/products/iot-platform/) (IoT Platform)，支持设备快速接入、数据管理、边缘计算、可视化分析和智能应用搭建。
它广泛应用于 智慧园区、工业 IoT、智能制造、智慧能源、智慧城市等场景，帮助企业快速构建安全、可靠、可扩展的物联网应用。

#### 核心功能 (Key Features)

🌐 多协议接入：支持 MQTT、CoAP、HTTP、Modbus、OPC UA、BACnet 等工业与楼宇协议

⚡ 高并发与可扩展性：云原生架构，支持百万级设备并发连接

📊 数据管理与可视化：内置 时序数据库、规则引擎、数据大屏

🧠 AI 与边缘计算：支持 AI 模型推理、视频 AI 分析、边缘节点协同

🛠️ 零代码 / 低代码搭建：通过可视化拖拽快速构建业务应用

🔒 安全与合规：支持 TLS/SSL、OAuth2.0、RBAC、租户隔离

云智易物联网平台的开放接口分为设备接入、APP开发、平台对接三个主要部分。


### 一、设备接入

使用云智易提供的嵌入式SDK，可以方便地将设备接入到物联网云平台，实现远程控制、远程管理、数据采集、OTA等相关的功能。

#### 设备接入指引

1. 开发者可以通过标准的MQTT协议快速接入云智易物联网平台；
2. 对于暂时没有设备，想体验接入的开发者，可以通过[mqtt client](https://mqtt-client.app/)在线工具，实现快速接入并体验云智易物联网平台的相关功能。
3. 除了MQTT，还支持CoAP、HTTP，TCP/UPP，GB28181、LoRAWAN等众多协议，详细信息欢迎访问[云智易物联网平台](https://www.xlink.cn/)官方网站了解。


### 二、平台Open API对接

云智易平台具备平台级的对接能力，提供了近200个Open API的RESTful接口。企业可以按需将云智易提供的物联网平台功能，集成到自有的业务系统中。

#### 平台对接指引

1. 开发者登录云智易[企业管理台](https://x.xlink.cn)，注册一个企业帐号；
2. 通过平台的API网关，创建应用，并选择所需要的接口授权。
3. 参考[物联平台管理接口文档](https://github.com/xlink-corp/xlink-sdk/tree/master/物联平台管理接口文档)，按需集成相关的接口功能。


### 三、了解更多解决方案
作为可大规模、全球化商用的专业[物联网平台](https://www.xlink.cn/)，云智易还提供了更多物联网相关的智能解决方案：
1. [智慧园区综合管理系统](https://www.xlink.cn/solutions/smart-park/)
2. [智慧园区整体解决方案](https://www.xlink.cn/solutions/smart-park/)
3. [FMCS厂务管理系统](https://www.xlink.cn/solutions/factory-control-system/)

