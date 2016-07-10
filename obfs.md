# 协议和混淆插件说明 #

具体说明参阅[https://github.com/breakwa11/shadowsocks-rss/blob/master/ssr.md](https://github.com/breakwa11/shadowsocks-rss/blob/master/ssr.md)，这里提供一个给新人简易的指导

## 工作原理

C->S方向  
浏览器请求（socks5协议） -> ssr客户端 -> 协议插件（转为指定协议） -> 加密 -> 混淆插件（转为表面上看起来像http/tls）  
-> ssr服务端 -> 混淆插件（分离出加密数据） -> 解密 -> 协议插件（转为原协议） -> 转发目标服务器

其中，协议插件主要用于增加数据完整性校验，增强安全性，包长度混淆等，协议插件主要用于伪装为其它协议

## 客户端

客户端的协议插件暂无配置参数，混淆插件有配置参数，混淆插件列表如下：

`plain`: 不混淆，无参数

`http_simple`: 简易伪装为http get请求，参数为要伪装的域名，如`cloudfront.com`。仅在C#版客户端上支持用逗号分隔多个域名如`a.com,b.net,c.org`，连接时会随机使用其中之一。不填写参数时，会使用此节点配置的服务器地址作为参数。

`http_post`: 与`http_simple`绝大部分相同，区别是使用POST方式发送数据，符合http规范，欺骗性更好，但只有POST请求这种行为容易被统计分析出异常。参数配置与`http_simple`一样

`tls1.2_ticket_auth`: 伪装为tls请求。参数配置与`http_simple`一样

其它插件不推荐使用，在这里忽略

客户端的协议插件，仅建议使用`origin`,`verify_sha1`,`auth_sha1`,`auth_sha1_v2`，解释如下：  
`origin`: 原版协议，为了兼容  
`verify_sha1`: 原版OTA协议，为了兼容  
`auth_sha1`: 高安全性，但有时间校对要求，混淆强度中等  
`auth_sha1_v2`: 较高安全性，无时间校对的要求，适合路由器或树莓派，混淆强度大

如不考虑兼容，可无视前两个

## 服务端

服务端的协议插件，仅`auth_sha1`和`auth_sha1_v2`有协议参数，其值为整数，表示允许的同时在线客户端数量，建议最小值为2。默认值64

服务端的混淆插件，仅`tls1.2_ticket_auth`有混淆参数，其值为整数，表示与客户端之间允许的UTC时间差，单位为秒，为0或不填写（默认）表示无视时间差

其它说明参见客户端部分

