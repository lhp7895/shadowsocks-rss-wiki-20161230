# ShadowsocksR 多用户版安装教程 #

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

### 获取源代码 ###
`git clone -b manyuser https://github.com/breakwa11/shadowsocks.git`

执行完毕后此目录会新建一个shadowsocks目录，其中根目录的是多用户版（即数据库版），子目录中的是单用户版。

根目录即 ./shadowsocks

子目录即 ./shadowsocks/shadowsocks 


### 服务端配置 ###
shadowsocks目录内，把apiconfig.py复制为userapiconfig.py后，对userapiconfig.py里以上内容进行相应修改： 
```
API_INTERFACE = 'mudbjson' //修改接口类型
```

接着，通过使用脚本`mujson_mgr.py`添加端口及相应的加密、协议、混淆等配置，具体方法通过执行以下命令该脚本的提示：  
`python mujson_mgr.py`

### 服务端运行与停止 ###

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