# python版客户端使用教程 #

基本库安装 
-----
以下命令均以root用户执行，或sudo方式执行

centos: 
    
    yum install m2crypto git

ubuntu/debian:
    
    apt-get install m2crypto git

windows:

[在Windows上安装服务端/客户端]


获取源代码
-----
`git clone -b manyuser https://github.com/breakwa11/shadowsocks.git`

执行完毕后会在当前目录新建一个shadowsocks目录。

进入子目录：

    cd shadowsocks/shadowsocks

####快捷运行####
```
python local.py -s server_ip -p 443 -k password -m aes-256-cfb -o http_simple

#说明：-p 端口 -k 密码  -m 加密方式 -o 混淆插件
```
如果要后台运行：

    python local.py -s server_ip -p 443 -k password -m aes-256-cfb -d start

如果要停止/重启：

    python local.py -d stop/restart

查看日志：
 
    tail -f /var/log/shadowsocks.log

用 -h 查看所有参数


####通过配置文件运行####

建立配置文件 `vi /etc/shadowsocks.json`

写入以下内容：
```javascript
{
    "server":"0.0.0.0",
    "server_ipv6": "::",
    "server_port":8388,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"mypassword",
    "timeout":300,
    "method":"aes-256-cfb",
    "obfs":"http_simple",
    "fast_open": false,
    "workers": 1
}
```


一般情况下，只需要修改以下四项即可：
```
"server":"0.0.0.0",        //服务器地址
"server_port":8388,        //端口
"password":"password",     //密码
"method":"aes-256-cfb",    //加密方式
```

运行:

    python local.py -c /etc/shadowsocks.json

后台运行：

    python local.py -c /etc/shadowsocks.json -d start

如果要停止/重启：

    python local.py -d stop/restart

查看日志：
 
    tail -f /var/log/shadowsocks.log


设置代理：
-------

默认地址：127.0.0.1   默认端口: 1080 

注：python版客户端只支持socks代理。



[在Windows上安装服务端/客户端]:   https://github.com/breakwa11/shadowsocks-rss/wiki/Server-Setup-on-Windows

# Python client setup guide

Basic setup
--
If not mentioned, the following steps are run by the `root` user.

CentOS:

`yum install m2crypto git`

Ubuntu/Debian:
`apt-get install m2crypto git`

Windows:
[Server-Setup-on-Windows](https://github.com/breakwa11/shadowsocks-rss/wiki/Server-Setup-on-Windows)

Obtain source code
--
`git clone -b manyuser https://github.com/breakwa11/shadowsocks.git`

Enter subdirectory `shadowsocks/shadowsocks`

## Running via command line
```
python local.py -s <server_ip> \
                -p <port> \
                -k <keyphrase> \
                -m <encryption> \
                -o <obfus> \
                -l <local_port>
```

Replace `<variable>` with appropriate values.

If require daemonization, append `-d start` on the above command. To stop or restart, execute
`python local.py -d stop # or restart`

Check logs:
```
tail -f /var/log/shadowsocks.log
```

`-h` shows the documentation.

## Running via configuration file

Create a configuration file at `/etc/shadowsocks.json`

Write the configuration:
```javascript
{
    "server":"0.0.0.0",
    "server_ipv6": "::",
    "server_port": <port>,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"<password>",
    "timeout":300,
    "method":"<encryption>",
    "obfs":"<obfs>",
    "fast_open": false,
    "workers": 1
}
```

Replace `<variables>` with appropriate values.

Then execute the following commands:

```
python local.py -c /etc/shadowsocks.json
```

You may combine with `-d start/restart/stop` options to initialize/restart/stop
the daemon.

Proxy setup
--

Default address: `127.0.0.1`
Default port: 1080

Note: Python client only supports SOCKS proxy.

Install on Windows server: [https://github.com/breakwa11/shadowsocks-rss/wiki/Server-Setup-on-Windows](https://github.com/breakwa11/shadowsocks-rss/wiki/Server-Setup-on-Windows)
