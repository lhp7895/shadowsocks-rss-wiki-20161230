# ShadowsocksR 服务端安装教程 #
###说明：###
此教程为单用户版，适合个人用户。如果你是站长，请查看多用户版教程：[多用户版教程](https://github.com/breakwa11/shadowsocks-rss/wiki/Server-Setup(manyuser))

基本库安装 
-----
以下命令均以root用户执行，或sudo方式执行

centos：  

    yum install m2crypto git libsodium

ubuntu/debian：  
 
    apt-get install m2crypto git

如果要使用 salsa20 或 chacha20 或 chacha20-ietf 算法，请安装 [libsodium](https://github.com/jedisct1/libsodium) :

```
apt-get install build-essential
wget https://github.com/jedisct1/libsodium/releases/download/1.0.7/libsodium-1.0.7.tar.gz
tar xf libsodium-1.0.7.tar.gz && cd libsodium-1.0.7
./configure && make -j2 && make install
ldconfig
```
如果曾经安装过旧版本，亦可重复用以上步骤更新到最新版，仅1.0.4或以上版本支持chacha20-ietf

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

####通过配置文件运行####

建立配置文件 `vi /etc/shadowsocks.json`

写入以下内容：
```javascript
{
    "server": "0.0.0.0",
    "server_ipv6": "::",
    "server_port": 8388,
    "local_address": "127.0.0.1",
    "local_port": 1080,
    "password": "mypassword",
    "timeout": 120,
    "method": "aes-256-cfb",
    "protocol": "auth_sha1_compatible",
    "protocol_param": "",
    "obfs": "tls1.0_session_auth_compatible",
    "obfs_param": "",
    "redirect": "",
    "dns_ipv6": false,
    "fast_open": false,
    "workers": 1
}
```

各选项说明：

Name    |    Explanation  | 中文说明
------- | --------------- | ---------------
server |	the address your server listens | 监听地址
server_ipv6 |   the ipv6 address your server listens  | ipv6地址
server_port |	server port                     | 监听端口
local_address|	the address your local listens  | 本地地址
local_port |	local port                      | 本地端口
password |	password used for encryption    | 密码
timeout |	in seconds                      | 超时时间
method |	default: "aes-256-cfb", see Encryption | 加密方式
protocol |      default："origin"     | 协议插件，默认"origin"
protocol_param |      default：""     | 协议插件参数，默认""
obfs   |      default："tls1.0_session_auth_compatible"     | 混淆插件，默认"tls1.0_session_auth_compatible"
obfs_param |      default：""     | 混淆插件参数，默认""
redirect |      default：""     | 重定向参数，默认""
dns_ipv6|     default:false  | 是否优先使用IPv6地址，有IPv6时可开启
fast_open |	use TCP_FASTOPEN, true / false         | 快速打开(仅限linux客户端)
workers	| number of workers, available on Unix/Linux   |线程（仅限linux客户端）

其中protocol有如下取值：

protocol| 说明
-------|----------
"origin"|原版协议
"verify_simple"|带校验的协议
"verify_deflate"|带压缩的协议
"verify_sha1"|带验证抗CCA攻击的协议，可兼容libev的OTA
"auth_simple"|抗重放攻击的协议
"auth_sha1"|带验证抗CCA攻击且抗重放攻击的协议

其中obfs有如下取值：

obfs   | 说明
-------|----------
"plain"|不混淆
"http_simple"|伪装为http协议
"tls_simple"|伪装为tls协议（不建议使用）
"random_head"|发送一个随机包再通讯的协议
"tls1.0_session_auth"|伪装为tls session握手协议，同时能抗重放攻击

各混淆插件的说明请点击这里查看：[混淆插件说明]

注：客户端的protocol和obfs配置必须与服务端的一致。

redirect参数说明：

值为空字符串或一个列表，若为列表示例如  
redirect:["bing.com", "cloudflare.com:443"],  
作用是在连接方的数据不正确的时候，把数据重定向到列表中的其中一个地址和端口（不写端口则视为80），以伪装为目标服务器。

dns_ipv6参数说明：

为true则指定服务器优先使用IPv6地址。仅当服务器能访问IPv6地址时可以用，否则会导致有IPv6地址的网站无法打开。

一般情况下，只需要修改以下五项即可：
```
"server_port":8388,        //端口
"password":"password",     //密码
"protocol":"origin",       //协议插件
"obfs":"http_simple",      //混淆插件
"method":"aes-256-cfb",    //加密方式
```

####多端口配置####
如果要多个用户一起使用的话，请写入以下配置：

```javascript
{
    "server":"0.0.0.0",
    "server_ipv6": "[::]",
    "local_address":"127.0.0.1",
    "local_port":1080,
    "port_password":{
        "80":"password1",
        "443":"password2"
    },
    "timeout":300,
    "method":"aes-256-cfb",
    "protocol": "auth_sha1_compatible",
    "protocol_param": "",
    "obfs": "tls1.0_session_auth_compatible",
    "obfs_param": "",
    "redirect": "",
    "dns_ipv6": false,
    "fast_open": false,
    "workers": 1
}
```
按照格式修改端口和密码：
```
    "port_password":{                  
        "80":"password1",       //端口和密码1
        "443":"password2"       //端口和密码2 
    },         
```

如果要为每个端口配置不同的混淆协议，请写入以下配置：

```javascript
{
    "server":"0.0.0.0",
    "server_ipv6":"::",
    "local_address":"127.0.0.1",
    "local_port":1080,
    "port_password":{
        "8388":{"protocol":"auth_simple", "password":"abcde", "obfs":"http_simple", "obfs_param":""},
        "8389":{"protocol":"origin", "password":"abcde"}
    },
    "timeout":300,
    "method":"aes-256-cfb",
    "protocol": "auth_sha1_compatible",
    "protocol_param": "",
    "obfs": "tls1.0_session_auth_compatible",
    "obfs_param": "",
    "redirect": "",
    "dns_ipv6": false,
    "fast_open": false,
    "workers": 1
}
```
按格式修改端口、密码以及混淆协议。也可以和以前的格式混合使用，如果某个端口不配置混淆协议，则会使用下面的默认"obfs"配置。


####运行子目录内的server.py：####
```
python server.py -c /etc/shadowsocks.json
```

如果要在后台运行：
```
python server.py -c /etc/shadowsocks.json -d start
```
如果要停止/重启：
```
python server.py -c /etc/shadowsocks.json -d stop/restart
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

服务器搭建
--------

建议选择 Ubuntu 14.04 LTS 作为服务器以便使用 [TCP Fast Open]。除非有明确理由，不建议用对新手不友好的 CentOS。

为了更好的性能，VPS 尽量选择 XEN 或 KVM，不要使用 OpenVZ。推荐使用以下 VPS：

- [Digital Ocean] 自带的内核无需自己编译模块即可使用 [hybla] 算法
- [Linode] 功能强大，机房较多

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

如果运行一段时间后，你发现服务器无法连接，同时ssh连上去后，执行  
`netstat -ltnap | grep -c CLOSE_WAIT`  
显示的数值很大（超过50是严重不正常），那么请修改服务器的最大连接数，如果是ubuntu/centos均可修改  
`/etc/security/limits.conf`  
添加两行：  
`*               soft    nofile           32768`  
`*               hard    nofile           131072`  
然后重启机器生效

如果还是出现大量的too many open files错误，可以通过执行以下命令确定占用大量文件数的进程：

    lsof -n |awk '{print $2}'|sort|uniq -c |sort -nr|more


[混淆插件说明]:        https://github.com/breakwa11/shadowsocks-rss/wiki/obfs
[Python]:            https://github.com/breakwa11/shadowsocks-rss/wiki/Python-client
[Linux]:             https://github.com/librehat/shadowsocks-qt5
[Android]:           https://github.com/shadowsocks/shadowsocks-android
[Build Status]:      https://img.shields.io/travis/shadowsocks/shadowsocks/master.svg?style=flat
[Chinese Readme]:    https://github.com/shadowsocks/shadowsocks/wiki/Shadowsocks-%E4%BD%BF%E7%94%A8%E8%AF%B4%E6%98%8E
[配置文件]:     https://github.com/shadowsocks/shadowsocks/wiki/Configuration-via-Config-File
[Coverage Status]:   https://jenkins.shadowvpn.org/result/shadowsocks
[Coverage]:          https://jenkins.shadowvpn.org/job/Shadowsocks/ws/htmlcov/index.html
[Debian sid]:        https://packages.debian.org/unstable/python/shadowsocks
[iOS]:               https://github.com/shadowsocks/shadowsocks-iOS/wiki/Help
[Issue Tracker]:     https://github.com/shadowsocks/shadowsocks/issues?state=open
[TCP Fast Open]:     https://github.com/clowwindy/shadowsocks/wiki/TCP-Fast-Open
[在 Windows 上安装服务端]: https://github.com/shadowsocks/shadowsocks/wiki/Install-Shadowsocks-Server-on-Windows
[Mailing list]:      https://groups.google.com/group/shadowsocks
[OpenWRT]:           https://github.com/shadowsocks/openwrt-shadowsocks
[OS X]:              https://github.com/shadowsocks/shadowsocks-iOS/wiki/Shadowsocks-for-OSX-Help
[PyPI]:              https://pypi.python.org/pypi/shadowsocks
[PyPI version]:      https://img.shields.io/pypi/v/shadowsocks.svg?style=flat
[Travis CI]:         https://travis-ci.org/shadowsocks/shadowsocks
[Troubleshooting]:   https://github.com/shadowsocks/shadowsocks/wiki/Troubleshooting
[Wiki]:              https://github.com/shadowsocks/shadowsocks/wiki
[Windows]:           https://github.com/breakwa11/shadowsocks-csharp
[Digital Ocean]:     https://www.digitalocean.com/?refcode=b1cddd149721
[Linode]:            https://www.linode.com/?r=e7932c8b03f9abc8aab71663b90b689a676402d1
[hybla]:             https://github.com/shadowsocks/shadowsocks/wiki/Optimizing-Shadowsocks
[Bandwagon Host]:    https://bandwagonhost.com/aff.php?pid=19