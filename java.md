

#eclipse快捷键  
Alt+Shift+B 打开面包屑视图，展示当天文件的路径（重要）  
Ctrl+Shift+G  展示调用当前方法的所有类（鼠标定位到这个方法） 
F4  当前接口的所有实现类    
Alt+Shift+R  统一修改参数名字/类变量、 方法变量等  
Ctrl+E/Ctrl+F6 展示当前已打开的所有文件  
Alt+Shift+->  范围选取  
Ctrl+Shift+P  跳到对应的大括号处  
Alt+Shift+M   抽取子方法（先选中代码块)  
Ctrl+Shift+Y/X  选中字符转大小写  
Ctrl+K/Ctrl+Shift+K 向下/向上查找  
Ctrl+L  跳转指定行  
Ctrl+T 搜索class  
Ctrl+R 搜索java文件  
Ctrl+D  删除当前行  
Ctrl+o  当前文件的属性和方法  
Ctrl+H  搜索  
eclispe各种视图 -->Window-->Showview-->Other  




#远程debug  
下一个断点: F8  单步： F6	  进入： F5  
查看变量：　debug视图--》 Variables --> 变量名--》voProperties --> properties -->table即可  

--》打开远程debug： debug图标 --> debug configuration -->  Remote java Application --》 配置地址端口--》 勾选"Allow termination of remote VM"
--》 查看debug远程端口：  /home/business/opt/container/bin/catalina.sh -->  搜索 Xdebug --》（常用:8090）
-->打开debug视图 --》 右上角 open persperctive --> debug
