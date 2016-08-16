
# 50. 其它
# 50.1 Tomcat
**配置**  
```
``

**eclipse中发布项目**  
```
1、tomcat/bin/startup.bat启动可以打开主页127:0.0.1:8080, 
    eclipse中直接添加启动tomcat则不能打开主页，因为要配置tomcat的发布目录
2、window-->showview中添加server-->add a server
   双击server中tomcat-->配置页面-->server Locations选择中间一个(use Tomcat installation),deploy path改成webapps,
   修改超时时间500s
   (如果无法修改，则删除tomcat所有项目，右键clean后打开)
3、新建web项目(Dynamic web project),其中会有src源码目录和资源目录
   tomcat --> add and remove添加即可
4、start server,即可发现项目已部署到 G:\program-my\apache-tomcat-8.0.36\webapps\tt 中
  console日志中也会有项目发布目录

```
