# ShadowsocksR 服务端安装教程 #
###说明：###
此教程为单用户版，适合个人用户。如果你是站长，请查看多用户版教程：[多用户版教程](https://github.com/breakwa11/shadowsocks-rss/wiki/Server-Setup(manyuser-with-mysql))

基本库安装 
-----
以下命令均以root用户执行，或sudo方式执行

centos：  

    yum install git

ubuntu/debian：  
 
    apt-get install git

获取源代码
-----
`git clone -b manyuser https://github.com/breakwa11/shadowsocks.git`

执行完毕后此目录会新建一个shadowsocks目录，其中根目录的是多用户版（即数据库版，个人用户请忽略这个），子目录中的是单用户版(即shadowsocks/shadowsocks)。

根目录即 ./shadowsocks

子目录即 ./shadowsocks/shadowsocks

服务端配置
-----
进入子目录：
```
cd shadowsocks/shadowsocks
```

####快速运行####
```
python server.py -p 443 -k password -m aes-256-cfb -o http_simple

#说明：-p 端口 -k 密码  -m 加密方式 -P 协议插件 -o 混淆插件
```
如果要后台运行：
```
python server.py -p 443 -k password -m aes-256-cfb -o http_simple -d start
```
如果要停止/重启：
```
python server.py -d stop/restart
```
查看日志：
```
tail -f /var/log/shadowsocks.log
```

用 -h 查看所有参数

####修改配置文件####

建立配置文件，如果你的ss目录是`/root/shadowsocks`  
通过执行`cp config.json user-config.json`快速创建一个

修改`user-config.json`中的`server_port`，`password`等字段，具体可参见：  
https://github.com/breakwa11/shadowsocks-rss/wiki/config.json


####运行子目录内的server.py：####
```
python server.py
```

如果要在后台运行：
```
python server.py -d start
```
如果要停止/重启：
```
python server.py -d stop/restart
```
查看日志：
```
tail -f /var/log/shadowsocks.log
```
### 更新源代码 ###
如果代码有更新可用本命令更新代码

进入shadowsocks目录  
`cd shadowsocks`  
执行  
`git pull`  
成功后重启ss服务


客户端
------
注：以下客户端只有windows客户端和python版客户端可以使用ssr新特性，其他原版客户端只能以兼容的方式连接ssr服务器（目前ssr仍兼容大部分旧版客户端）。

* [Windows] / [OS X]
* [Linux]
* [Android] / [iOS]
* [OpenWRT]

在你本地的 PC 或手机上使用图形客户端。具体使用参见它们的使用说明。

也可以直接使用 [Python] 版客户端（命令行）。

### 其它异常 ###
如果你的服务端python版本在2.6以下，那么必须更新python到2.6.x或2.7.x版本

其它参见 https://github.com/breakwa11/shadowsocks-rss/wiki/ulimit


[Python]:            https://github.com/breakwa11/shadowsocks-rss/wiki/Python-client
[Linux]:             https://github.com/librehat/shadowsocks-qt5
[Android]:           https://github.com/shadowsocks/shadowsocks-android
[Debian sid]:        https://packages.debian.org/unstable/python/shadowsocks
[iOS]:               https://github.com/shadowsocks/shadowsocks-iOS/wiki/Help
[OpenWRT]:           https://github.com/shadowsocks/openwrt-shadowsocks
[OS X]:              https://github.com/shadowsocks/shadowsocks-iOS/wiki/Shadowsocks-for-OSX-Help
[Windows]:           https://github.com/breakwa11/shadowsocks-csharp