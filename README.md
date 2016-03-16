# 云智易物联平台开发说明


## 总概

* 云智易物联平台开发分为设备接入、APP开发、平台对接三个主要部分。

## 设备接入

### 说明

* 使用云智易提供的硬件SDK，可以方便的将硬件设备接入到云智易物联平台中。
* 接入平台的硬件，可以实现远程控制、远程管理等相关的功能。

### 接入综述

1. 开发者可以通过[SDK介绍](https://github.com/xlink-corp/xlink-sdk/blob/master/设备端开发文档/1.XlinkSDK规范/1.SDK介绍.md)了解SDK的基本概念。
2. 通过[接入流程](https://github.com/xlink-corp/xlink-sdk/blob/master/设备端开发文档/1.XlinkSDK规范/2.接入流程.md)了解硬件接入的具体流程。
3. 开发者可以通过[C语言接口](https://github.com/xlink-corp/xlink-sdk/blob/master/设备端开发文档/1.XlinkSDK规范/3.C语言接口.md)完成对具体硬件的功能开发。

## APP开发

### 说明

* 通过云智易提供的APP SDK，开发者可以实现对接入云智易平台的硬件设备的发现、控制、管理等相关功能。
* 云智易现在提供了[iOS部分的SDK](https://github.com/xlink-corp/xlink-sdk/blob/master/%E5%BA%94%E7%94%A8%E7%AB%AF%E5%BC%80%E5%8F%91%E6%96%87%E6%A1%A3/APP%20iOS%20SDK%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3.md)以及[Android部分的SDK](https://github.com/xlink-corp/xlink-sdk/blob/master/%E5%BA%94%E7%94%A8%E7%AB%AF%E5%BC%80%E5%8F%91%E6%96%87%E6%A1%A3/APP%20Android%20SDK%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3.md)。
* 云智易也提供了对于[微信硬件平台](http://iot.weixin.qq.com)的支持，通过[微信智能硬件接入指南](https://github.com/xlink-corp/xlink-sdk/blob/master/%E5%BA%94%E7%94%A8%E7%AB%AF%E5%BC%80%E5%8F%91%E6%96%87%E6%A1%A3/%E5%BE%AE%E4%BF%A1%E6%99%BA%E8%83%BD%E7%A1%AC%E4%BB%B6%E6%8E%A5%E5%85%A5%E6%8C%87%E5%8D%97.md)，开发者可以让硬件方便的接入微信硬件平台。

### 接入综述

1. 云智易平台APP开发集成分为两大部分，一部分是原生SDK集成，主要是用来进行设备扫描、发现控制以及APP连接云端，实现云端通讯功能。一部分是RESTFul API部分，提供用来进行[用户注册](https://github.com/xlink-corp/xlink-sdk/blob/master/%E5%BA%94%E7%94%A8%E7%AB%AF%E5%BC%80%E5%8F%91%E6%96%87%E6%A1%A3/%E5%BA%94%E7%94%A8%E7%AB%AFRESTful%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3/%E7%94%A8%E6%88%B7%E8%BA%AB%E4%BB%BD%E6%8E%A5%E5%8F%A3.md#email_register)、[认证](https://github.com/xlink-corp/xlink-sdk/blob/master/%E5%BA%94%E7%94%A8%E7%AB%AF%E5%BC%80%E5%8F%91%E6%96%87%E6%A1%A3/%E5%BA%94%E7%94%A8%E7%AB%AFRESTful%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3/%E7%94%A8%E6%88%B7%E8%BA%AB%E4%BB%BD%E6%8E%A5%E5%8F%A3.md#user_auth)、以及[设备分享](https://github.com/xlink-corp/xlink-sdk/blob/master/%E5%BA%94%E7%94%A8%E7%AB%AF%E5%BC%80%E5%8F%91%E6%96%87%E6%A1%A3/%E5%BA%94%E7%94%A8%E7%AB%AFRESTful%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3/%E8%AE%BE%E5%A4%87%E5%88%86%E4%BA%AB%E6%8E%A5%E5%8F%A3.md)等功能的实现。

### 基本接入流程

1. 开发者登录[云智易企业管理台](https://admin.xlink.cn)，注册一个企业帐号。
2. 在管理台中创建产品。
3. 通过硬件开发相关资料开发硬件。
4. 参考[APP iOS SDK接口文档](https://github.com/xlink-corp/xlink-sdk/blob/master/%E5%BA%94%E7%94%A8%E7%AB%AF%E5%BC%80%E5%8F%91%E6%96%87%E6%A1%A3/APP%20iOS%20SDK%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3.md)以及[APP Android SDK接口文档](https://github.com/xlink-corp/xlink-sdk/blob/master/%E5%BA%94%E7%94%A8%E7%AB%AF%E5%BC%80%E5%8F%91%E6%96%87%E6%A1%A3/APP%20Android%20SDK%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3.md)开发对应的APP程序。

## 平台对接

### 说明

* 除了对端到端的设备连接和开发支持，云智易平台提供了平台级的对接能力。厂商完全可以按照自己的需求，将云智易提供的物联网平台功能，如[设备管理](https://github.com/xlink-corp/xlink-sdk/blob/master/%E7%89%A9%E8%81%94%E5%B9%B3%E5%8F%B0%E7%AE%A1%E7%90%86%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3/%E4%BA%A7%E5%93%81%E4%B8%8E%E8%AE%BE%E5%A4%87%E7%AE%A1%E7%90%86%E6%8E%A5%E5%8F%A3.md)、[用户管理](https://github.com/xlink-corp/xlink-sdk/blob/master/%E7%89%A9%E8%81%94%E5%B9%B3%E5%8F%B0%E7%AE%A1%E7%90%86%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3/%E7%94%A8%E6%88%B7%E7%AE%A1%E7%90%86%E6%8E%A5%E5%8F%A3.md)、[设备控制](https://github.com/xlink-corp/xlink-sdk/blob/master/%E7%89%A9%E8%81%94%E5%B9%B3%E5%8F%B0%E7%AE%A1%E7%90%86%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3/%E8%AE%BE%E5%A4%87%E6%8E%A7%E5%88%B6%E6%8E%A5%E5%8F%A3.md)、[数据收集](https://github.com/xlink-corp/xlink-sdk/blob/master/%E7%89%A9%E8%81%94%E5%B9%B3%E5%8F%B0%E7%AE%A1%E7%90%86%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3/%E6%95%B0%E6%8D%AE%E7%BB%9F%E8%AE%A1%E5%88%86%E6%9E%90%E6%8E%A5%E5%8F%A3.md)，集成到自有的平台系统中。
* 云智易平台的对接能力通过标准的RESTful接口提供。

### 接入综述

1. 开发者登录[云智易企业管理台](https://admin.xlink.cn)，注册、登录一个企业帐号。
2. 通过平台的授权管理，创建一个AccessID和Key，通过平台[授权接口](https://github.com/xlink-corp/xlink-sdk/blob/master/%E7%89%A9%E8%81%94%E5%B9%B3%E5%8F%B0%E7%AE%A1%E7%90%86%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3/%E6%8E%88%E6%9D%83%E7%AE%A1%E7%90%86.md#auth)，获取到访问云智易平台能力的token。
3. 在通过平台对应的接口，完成各种平台对接动作。

