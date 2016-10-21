定义base64为**URL safe base64，且不带padding**，具体格式如下：

`ssr://base64(host:port:protocol:method:obfs:base64pass/?obfsparam=base64param&remarks=base64remarks&group=base64group&udpport=0&uot=0)`

其中，base64pass及之前以':'分隔的，不可省略，而'/?'及其后面的内容，可按需要写上

udpport参数及uot目前仅C#客户端支持