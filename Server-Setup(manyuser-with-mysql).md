# ShadowsocksR 多用户版安装教程 #
注:多用户版需配合 [ss-panel] 等前端使用。


以下命令均以root用户执行，或sudo方式执行

### 基本库安装 ###
centos： 
```
yum install python-setuptools && easy_install pip
yum install git
```
ubuntu/debian： 
```
apt-get install python-pip
apt-get install git
```
### 安装cymysql ###

    pip install cymysql

### 获取源代码 ###
`git clone -b manyuser https://github.com/breakwa11/shadowsocks.git`

执行完毕后此目录会新建一个shadowsocks目录，其中根目录的是多用户版（即数据库版），子目录中的是单用户版。

根目录即 ./shadowsocks

子目录即 ./shadowsocks/shadowsocks 


### 服务端配置 ###
shadowsocks目录内，把apiconfig.py复制为userapiconfig.py后，对userapiconfig.py里以上内容进行相应修改： 
```
API_INTERFACE = 'sspanelv2' //修改接口类型
```
根据你的数据库类型，需正确选择使用sspanelv2, sspanelv3, sspanelv3ssr之一

然后把mysql.json复制为usermysql.json，并修改里面的内容：
```
{
    "host": "127.0.0.1",
    "port": 3306,
    "user": "ss",
    "password": "pass",
    "db": "shadowsocks",
    "node_id": 1,
    "transfer_mul": 1.0,
    "ssl_enable": 0,
    "ssl_ca": "",
    "ssl_cert": "",
    "ssl_key": ""
}
```
以上包括（按次序）：数据库服务器地址，端口，数据库登陆用户名，密码，数据库表，节点ID（sspanelv3支持），流量比率，开启mysql的SSL连接等等

要注意**sspanelv3必须正确填写node_id才能正常使用**，并且在填写该ID前，必须在面板上已经添加好该节点，以确定节点ID后，再在此处填写。

文件config.json复制一份到user-config.json，然后编辑： 
```
"method":"aes-256-cfb",                   //修改成您要的加密方式的名称
"protocol": "auth_sha1_compatible",       //修改成您要的协议插件名称
"obfs": "tls1.0_session_auth_compatible", //修改成您要的混淆插件名称
```

### 服务端运行与停止 ###

进入根目录：
    
    cd shadowsocks
 
运行：
    
    python server.py

这时可查看有运行情况，检查有没有错误。如果服务端没有错误，而连接不上，需要检查iptables或firewall(centos7)的防火墙配置

#### 通过脚本运行 ####

增加脚本可执行权限 

`chmod +x *.sh`

后台运行（无log，ssh窗口关闭后也继续运行） 

`./run.sh`

后台运行（输出log，ssh窗口关闭后也继续运行） 

`./logrun.sh`

后台运行时查看运行情况 

`./tail.sh`

停止运行 

`./stop.sh`

注：通过脚本运行默认日志会保存在根目录的ssserver.log，可手动查看。

### 更新源代码 ###
如果代码有更新可用本命令更新代码

进入shadowsocks目录 

`cd shadowsocks` 

执行 

`git pull` 

成功后重启ss服务

### 其它异常 ###
如果你的服务端python版本在2.6以下，那么必须更新python到2.6.x或2.7.x版本

其它参见 https://github.com/breakwa11/shadowsocks-rss/wiki/ulimit

[ss-panel]:            https://github.com/orvice/ss-panel