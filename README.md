## 云智易物联平台开发说明


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

[云智易物联网平台](https://www.xlink.cn/)的开放接口分为设备接入、APP开发、平台对接三个主要部分。


### 一、设备接入

使用云智易提供的嵌入式SDK，可以方便地将设备接入到物联网云平台，实现远程控制、远程管理、数据采集、OTA等相关的功能。

#### 设备接入指引

1. 开发者可以通过标准的MQTT协议快速接入云智易物联网平台；
2. 对于暂时没有设备，想体验接入的开发者，可以通过[mqtt client](https://mqtt-client.app/)在线工具，实现快速接入并体验云智易物联网平台的相关功能。
3. 除了MQTT，还支持CoAP、HTTP，TCP/UPP，GB28181、LoRAWAN等众多协议，详细信息欢迎访问云智易[物联网平台](https://www.xlink.cn/)官方网站了解。

### 二、APP开发

通过云智易提供的APP SDK，开发者可以快速实现对物联设备的远程访问、设备控制和管理。
云智易平台的APP SDK分为种类型的接口：

* Android和iOS原生SDK，用于实现新设备的扫描、添加、访问和控制，及云端的连接和通讯等功能。
* RESTFul API，提供了如[用户注册](https://github.com/xlink-corp/xlink-sdk/blob/master/%E5%BA%94%E7%94%A8%E7%AB%AF%E5%BC%80%E5%8F%91%E6%96%87%E6%A1%A3/%E5%BA%94%E7%94%A8%E7%AB%AFRESTful%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3/%E7%94%A8%E6%88%B7%E8%BA%AB%E4%BB%BD%E6%8E%A5%E5%8F%A3.md#email_register)、[认证](https://github.com/xlink-corp/xlink-sdk/blob/master/%E5%BA%94%E7%94%A8%E7%AB%AF%E5%BC%80%E5%8F%91%E6%96%87%E6%A1%A3/%E5%BA%94%E7%94%A8%E7%AB%AFRESTful%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3/%E7%94%A8%E6%88%B7%E8%BA%AB%E4%BB%BD%E6%8E%A5%E5%8F%A3.md#user_auth)，以及[设备分享](https://github.com/xlink-corp/xlink-sdk/blob/master/%E5%BA%94%E7%94%A8%E7%AB%AF%E5%BC%80%E5%8F%91%E6%96%87%E6%A1%A3/%E5%BA%94%E7%94%A8%E7%AB%AFRESTful%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3/%E8%AE%BE%E5%A4%87%E5%88%86%E4%BA%AB%E6%8E%A5%E5%8F%A3.md)等功能接口。


#### APP接入指引

1. 开发者登录云智易[企业管理台](https://admin.xlink.cn)，注册一个企业帐号；
2. 在管理台中创建一个产品；
3. 参考[APP iOS SDK接口文档](https://github.com/xlink-corp/xlink-sdk/blob/master/%E5%BA%94%E7%94%A8%E7%AB%AF%E5%BC%80%E5%8F%91%E6%96%87%E6%A1%A3/APP%20iOS%20SDK%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3.md)以及[APP Android SDK接口文档](https://github.com/xlink-corp/xlink-sdk/blob/master/%E5%BA%94%E7%94%A8%E7%AB%AF%E5%BC%80%E5%8F%91%E6%96%87%E6%A1%A3/APP%20Android%20SDK%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3.md)开发对应的APP程序。
4. 参考[微信智能硬件接入指南](https://github.com/xlink-corp/xlink-sdk/blob/master/%E5%BA%94%E7%94%A8%E7%AB%AF%E5%BC%80%E5%8F%91%E6%96%87%E6%A1%A3/%E5%BE%AE%E4%BF%A1%E6%99%BA%E8%83%BD%E7%A1%AC%E4%BB%B6%E6%8E%A5%E5%85%A5%E6%8C%87%E5%8D%97.md)，开发者可以让硬件方便的接入微信硬件平台。


### 三、平台对接

云智易平台具备平台级的对接能力，提供了近200个[Xlink Open API](https://xlink-corp.github.io/xlink-OpenAPI/) RESTful接口。企业可以按需将云智易提供的物联网平台功能，如[设备管理](https://github.com/xlink-corp/xlink-sdk/blob/master/%E7%89%A9%E8%81%94%E5%B9%B3%E5%8F%B0%E7%AE%A1%E7%90%86%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3/%E4%BA%A7%E5%93%81%E4%B8%8E%E8%AE%BE%E5%A4%87%E7%AE%A1%E7%90%86%E6%8E%A5%E5%8F%A3.md)、[用户管理](https://github.com/xlink-corp/xlink-sdk/blob/master/%E7%89%A9%E8%81%94%E5%B9%B3%E5%8F%B0%E7%AE%A1%E7%90%86%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3/%E7%94%A8%E6%88%B7%E7%AE%A1%E7%90%86%E6%8E%A5%E5%8F%A3.md)、[设备控制](https://github.com/xlink-corp/xlink-sdk/blob/master/%E7%89%A9%E8%81%94%E5%B9%B3%E5%8F%B0%E7%AE%A1%E7%90%86%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3/%E8%AE%BE%E5%A4%87%E6%8E%A7%E5%88%B6%E6%8E%A5%E5%8F%A3.md)、[数据收集](https://github.com/xlink-corp/xlink-sdk/blob/master/%E7%89%A9%E8%81%94%E5%B9%B3%E5%8F%B0%E7%AE%A1%E7%90%86%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3/%E6%95%B0%E6%8D%AE%E7%BB%9F%E8%AE%A1%E5%88%86%E6%9E%90%E6%8E%A5%E5%8F%A3.md)，集成到自有的业务系统中。

#### 平台对接指引

1. 开发者登录云智易[企业管理台](https://x.xlink.cn)，注册一个企业帐号；
2. 通过平台的授权管理，创建一个AccessID和Key，通过平台[授权接口](https://github.com/xlink-corp/xlink-sdk/blob/master/%E7%89%A9%E8%81%94%E5%B9%B3%E5%8F%B0%E7%AE%A1%E7%90%86%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3/%E6%8E%88%E6%9D%83%E7%AE%A1%E7%90%86.md#auth)，获取到访问云智易平台能力的Token。
3. 参考[物联平台管理接口文档](https://github.com/xlink-corp/xlink-sdk/tree/master/物联平台管理接口文档)，按需集成相关的接口功能。


作为可大规模、全球化商用的专业[物联网平台](https://www.xlink.cn/)，云智易还提供了更多物联网相关的智能场景应用，详细方案请访问云智易官方网站。

