# 应用端RESTful接口文档

* 云智易提供了RESTful API接口，APP开发者可通过本系列接口完成对云智易用户相关的接入。


## **本系列文档目录**


### [获取调用凭证](#https://github.com/xlink-corp/xlink-sdk/blob/master/%E5%BA%94%E7%94%A8%E7%AB%AF%E5%BC%80%E5%8F%91%E6%96%87%E6%A1%A3/%E5%BA%94%E7%94%A8%E7%AB%AFRESTful%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3/%E8%8E%B7%E5%8F%96%E6%8E%A5%E5%8F%A3%E8%B0%83%E7%94%A8%E5%87%AD%E6%8D%AE.md)

### [用户身份接口](#https://github.com/xlink-corp/xlink-sdk/blob/master/%E5%BA%94%E7%94%A8%E7%AB%AF%E5%BC%80%E5%8F%91%E6%96%87%E6%A1%A3/%E5%BA%94%E7%94%A8%E7%AB%AFRESTful%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3/%E7%94%A8%E6%88%B7%E8%BA%AB%E4%BB%BD%E6%8E%A5%E5%8F%A3.md)

### [设备功能接口](#https://github.com/xlink-corp/xlink-sdk/blob/master/%E5%BA%94%E7%94%A8%E7%AB%AF%E5%BC%80%E5%8F%91%E6%96%87%E6%A1%A3/%E5%BA%94%E7%94%A8%E7%AB%AFRESTful%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3/%E8%AE%BE%E5%A4%87%E5%8A%9F%E8%83%BD%E6%8E%A5%E5%8F%A3.md)

### [设备分享接口](#https://github.com/xlink-corp/xlink-sdk/blob/master/%E5%BA%94%E7%94%A8%E7%AB%AF%E5%BC%80%E5%8F%91%E6%96%87%E6%A1%A3/%E5%BA%94%E7%94%A8%E7%AB%AFRESTful%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3/%E8%AE%BE%E5%A4%87%E5%88%86%E4%BA%AB%E6%8E%A5%E5%8F%A3.md)

### [用户消息接口](#https://github.com/xlink-corp/xlink-sdk/blob/master/%E5%BA%94%E7%94%A8%E7%AB%AF%E5%BC%80%E5%8F%91%E6%96%87%E6%A1%A3/%E5%BA%94%E7%94%A8%E7%AB%AFRESTful%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3/%E7%94%A8%E6%88%B7%E6%B6%88%E6%81%AF%E6%8E%A5%E5%8F%A3.md)



## **关于RESTfule接口说明**


### **HTTP请求头部扩展字段：**

* 调用云智易平台RESTful接口需要满足特定的HTTP扩展字段。

字段 | 说明
---- | ----
Access-Token | 应用前端访问RESTful接口的调用凭证，通过用户[登陆认证](https://xlink.gitbooks.io/sdk-app/content/app_user_restful/yonghu_shen_fen_jie_kou.html)获取 


## **RESTful接口返回错误码**

* 应用端开发者调用RESTful接口后根据HTTP响应的状态码来确定是否请求成功。

请看[错误码说明](https://xlink.gitbooks.io/sdk-app/content/app_user_restful/cuowuma_shuo_ming.html)。


