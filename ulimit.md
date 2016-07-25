# 修改系统连接数

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
