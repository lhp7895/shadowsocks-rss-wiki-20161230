# ShadowsocksR 多用户版安装教程 #
注:多用户版需配合 [ss-panel] 等前端使用。


以下命令均以root用户执行，或sudo方式执行

### 基本库安装 ###
centos： 
```
yum install python-setuptools && easy_install pip
yum install m2crypto git
```
ubuntu/debian： 
```
apt-get install python-pip
apt-get install m2crypto git
```
### 安装cymysql ###

    pip install cymysql

### 获取源代码 ###
`git clone -b manyuser https://github.com/breakwa11/shadowsocks.git`

执行完毕后此目录会新建一个shadowsocks目录，其中根目录的是多用户版（即数据库版），子目录中的是单用户版。

根目录即 ./shadowsocks

子目录即 ./shadowsocks/shadowsocks 


### 服务端配置 ###
shadowsocks目录内，文件Config.py或apiconfig.py： 
```
TRANSFER_MUL = 1.0  //流量系数，设置为2.0的话用1M算为2M

MYSQL_HOST = 'localhost'  //前端mysql域名/IP
MYSQL_PORT = 3306         //mysql端口
MYSQL_USER = 'ss'         //mysql用户名(建议不要用Root账户)
MYSQL_PASS = 'ss'         //mysql密码
MYSQL_DB = 'shadowsocks'  //数据库名
```
如果是新版本，把apiconfig.py复制为userapiconfig.py后，对apiconfig.py里以上内容进行相应修改

文件config.json复制一份到user-config.json，然后编辑： 
```
"method":"aes-256-cfb",                   //修改成您要的加密方式的名称
"protocol": "auth_sha1_compatible",       //修改成您要的协议插件名称
"obfs": "tls1.0_session_auth_compatible", //修改成您要的混淆插件名称
```

数据库字段说明:
```
  id                 //用户id
  email              //用户邮箱
  pass               //用户密码
  passwd             //ss密码
  t                  //最后使用的时间
  u                  //已上传流量
  d                  //已下载流量
  transfer_enable    //可用流量（总量）
  port               //ss端口
  switch             //保留字段
  enable             //启用或禁用ss帐号（1启用，0禁用）
  type               //保留字段
  last_get_gift_time    //保留字段
  last_rest_pass_time   //保留字段
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

后台运行（ssh窗口关闭后也继续运行） 

`./run.sh`

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

如果运行一段时间后，你发现服务器无法连接，同时ssh连上去后，发现进程不存在，那么可能是达到了系统的最大连接数 

如果是ubuntu/centos均可修改`/etc/sysctl.conf`

找到`fs.file-max`这一行，修改其值为1024000，并保存退出。然后执行`sysctl -p`使其生效

打开文件`/etc/security/limits.conf`

添加两行： 
```
*               soft    nofile           512000
*               hard    nofile          1024000
```
保存后，重启操作系统生效

针对ubuntu系统，你还需要额外的在运行前使用ulimit命令设置最大文件数，可使用附带的运行脚本。  
如果使用supervisor进程守护，需要修改文件`/etc/default/supervisor`，添加一行：  
`ulimit -n 512000`  
再启动你的服务

[ss-panel]:            https://github.com/orvice/ss-panel