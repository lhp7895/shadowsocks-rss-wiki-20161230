# ShadowsocksR 服务端安装教程(多用户版) #

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
`git clone -b manyuser https://github.com/breakwa11/shadowsocks.git`

执行完毕后此目录会新建一个shadowsocks目录，其中根目录的是多用户版（即数据库版），子目录中的是单用户版。

### 服务端配置 ###
shadowsocks目录内，文件Config.py： 
MYSQL\_HOST = 'mdss.mengsky.net' #**前端mysql域名/IP** 
MYSQL\_PORT = 3306    #**mysql端口** 
MYSQL\_USER = 'ss'    #**mysql用户名(建议不要用Root账户)** 
MYSQL\_PASS = 'ss'    #**mysql密码** 
MYSQL\_DB = 'shadowsocks'    #**数据库名** 

文件config.json： 
"method":"aes-256-cfb",    #**修改成您要的加密方式的名称**

数据库字段说明:
   ```
  id                 //用户id
  email              //用户邮箱
  pass               //用户密码
  passwd             //ss密码
  t                  //最后使用的时间
  u                  //已上传流量
  d                  //已下载流量
  transfer_enable    //可用流量
  port               //ss端口
  switch             //保留字段
  enable             //启用或禁用ss帐号（1启用，0禁用）
  type               //保留字段
  last_get_gift_time    //保留字段
  last_rest_pass_time   //保留字段
  ```

### 服务端运行与停止 ###

shadowsocks目录内 

多用户在父目录执行 
`python server.py`

这时可查看有运行情况，检查有没有错误。如果服务端没有错误，而连接不上，需要检查iptables或firewall(centos7)的防火墙配置

增加脚本可执行权限 
`chmod +x *.sh`

后台运行（ssh窗口关闭后也继续运行） 
`./run.sh`

后台运行时查看运行情况 
`./tail.sh`

停止运行 
`./stop.sh`

### 更新源代码 ###
如果代码有更新可用本命令更新代码

进入shadowsocks目录 
`cd shadowsocks` 
执行 
`git pull` 
成功后重启ss服务

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
`lsof -n |awk '{print $2}'|sort|uniq -c |sort -nr|more`
