定义base64为**URL safe base64，且不带padding**，具体格式如下：

`ssr://base64(host:port:protocol:method:obfs:base64pass/?obfsparam=base64param&remarks=base64remarks&group=base64group&udpport=0&uot=0)`

其中，base64pass及之前以':'分隔的，不可省略，而'/?'及其后面的内容，可按需要写上。  
字符串使用UTF8编码，编码后必须以urlsafebase64编码，包括密码、混淆参数、备注、group

udpport参数及uot目前仅C#客户端支持

示例：

```
服务器IP： 127.0.0.1
端口： 1234
密码： aaabbb
加密： aes-128-cfb
协议： auth_aes128_md5
混淆： tls1.2_ticket_auth
混淆参数： breakwa11.moe
备注： 测试中文
```

生成的带备注结果：  
`ssr://MTI3LjAuMC4xOjEyMzQ6YXV0aF9hZXMxMjhfbWQ1OmFlcy0xMjgtY2ZiOnRsczEuMl90aWNrZXRfYXV0aDpZV0ZoWW1KaS8_b2Jmc3BhcmFtPVluSmxZV3QzWVRFeExtMXZaUSZyZW1hcmtzPTVyV0w2Sy1WNUxpdDVwYUg`

生成的不带备注的标准结果（结果唯一）：  
`ssr://MTI3LjAuMC4xOjEyMzQ6YXV0aF9hZXMxMjhfbWQ1OmFlcy0xMjgtY2ZiOnRsczEuMl90aWNrZXRfYXV0aDpZV0ZoWW1KaS8_b2Jmc3BhcmFtPVluSmxZV3QzWVRFeExtMXZaUQ`

如果你生成的不带备注的结果结果与上面的不一致，那么请检查实现代码，否则可能导致部分环境下识别错误。