

#远程debug  
下一个断点: F8  单步： F6	  进入： F5  
查看变量：　debug视图--》 Variables --> 变量名--》voProperties --> properties -->table即可  

--》打开远程debug： debug图标 --> debug configuration -->  Remote java Application --》 配置地址端口--》 勾选"Allow termination of remote VM"
--》 查看debug远程端口：  /home/business/opt/container/bin/catalina.sh -->  搜索 Xdebug --》（常用:8090）
-->打开debug视图 --》 右上角 open persperctive --> debug
