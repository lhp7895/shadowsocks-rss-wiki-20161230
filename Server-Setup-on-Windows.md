#在Windows上安装服务端/客户端(python版)#

* 安装[Python]

       ![](https://cloud.githubusercontent.com/assets/493124/5639371/0b91b9fa-9650-11e4-9782-44526d25f2fa.png)

* 安装 [OpenSSL for Windows]
* 安装 [git] 或 [手动下载zip源码包]

获取源代码
-----
`git clone -b manyuser https://github.com/breakwa11/shadowsocks.git`

执行完毕后会在当前目录新建一个shadowsocks目录。

进入子目录：

    cd shadowsocks/shadowsocks

####快捷运行####
```
python server.py -p 443 -k password -m aes-256-cfb

#说明：-p 端口 -k 密码  -m 加密方式
```

####通过配置文件运行####

建立配置文件 `shadowsocks.json`

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
    "fast_open": false,
    "workers": 1
}
```
一般情况下，只需要修改以下三项即可：
```
"server_port":8388,        //端口
"password":"password",     //密码
"method":"aes-256-cfb",    //加密方式
```

运行（请自行替换shadowsocks.json文件路径）:

    python server.py -c shadowsocks.json

或直接修改根目录下的 config.json 文件，然后运行：

    python server.py


客户端(python版)
-----

```
python local.py -s server_ip -p 443 -k password -m aes-256-cfb

#说明：-s 服务器地址 -p 端口 -k 密码  -m 加密方式
```

####通过配置文件运行####

建立配置文件 `shadowsocks.json`

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

运行（请自行替换shadowsocks.json文件路径）:

    python local.py -c shadowsocks.json

或直接修改根目录下的 config.json 文件，然后运行：

    python local.py










[手动下载zip源码包]:            https://github.com/breakwa11/shadowsocks
[git]:                        http://git-scm.com/download/
[python]:                     https://www.python.org/downloads/windows/
[OpenSSL for Windows]:        https://slproweb.com/products/Win32OpenSSL.html