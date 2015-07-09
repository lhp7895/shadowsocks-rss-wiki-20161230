# ShadowsocksR 服务端安装教程 #

以下命令均以root用户执行，或sudo方式执行

### 基本库安装 ###
centos：  
`yum install python-setuptools && easy_install pip`  
`yum install m2crypto git`

ubuntu/debian：  
`apt-get install python-pip`  
`apt-get install m2crypto git`

### 安装cymysql ###
pip install cymysql

### 获取源代码 ###
单用户版（个人用户使用这个）  
`git clone –b master https://github.com/breakwa11/shadowsocks.git`  

多用户版（个人用户或ss站长使用这个）  
`git clone –b manyuser https://github.com/breakwa11/shadowsocks.git`

执行完毕后此目录会新建一个shadowsocks目录

### 更新源代码 ###
如果代码有更新可用本命令更新代码

进入shadowsocks目录  
`cd shadowsocks`  
执行  
`git pull`

### 服务端配置 ###
shadowsocks目录内，文件Config.py：  
MYSQL\_HOST = 'mdss.mengsky.net' #**前端mysql域名/IP**  
MYSQL\_PORT = 3306    #**mysql端口**  
MYSQL\_USER = 'ss'    #**mysql用户名(建议不要用Root账户)**  
MYSQL\_PASS = 'ss'    #**mysql密码**  
MYSQL\_DB = 'shadowsocks'    #**数据库名**  

文件config.json：  
"method":"aes-256-cfb",    #**修改成您要的加密方式的名称**

### 服务端运行与停止 ###
**以下仅限manyuser分支**

shadowsocks目录内  

单用户需要再进入shadowsocks二级子目录再执行，多用户在父目录执行  
`python server.py`

这时可查看有运行情况，检查有没有错误。如果服务端没有错误，而连接不上，需要检查iptables或firewall(centos7)的防火墙配置

增加脚本可执行权限  
`chmod +x *.sh`

后台运行  
`./run.sh`

后台运行时查看运行情况  
`./tail.sh`

停止运行  
`./stop.sh`

其它异常  
如果你的服务端python版本在2.6以下，那么必须更新python到2.6.x或2.7.x版本