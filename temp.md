# maven
**基础问题** 
```
1、pom.xml总是在项目的根目录。
2、约定优于配置：
		源码目录为 src/main/java
		编译输出目录为 target/classes/
		打包方式默认为jar(如果不指定packaging标签的话)
		包输出目录为target
3、maven中通过groupId、artifactId、version定位到一个唯一jar、pom、car（packaging标签，默认jar）。
``` 

**pom.xml解析**  
```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>	//当前pom模型的版本，3.0必须是4.0.0
  <groupId>com.huawei</groupId>		//必须，项目属于哪个组，一般值项目关联的组织/公司
  <artifactId>ttt</artifactId>		//必须，项目在组中唯一的id
  <version>0.0.1-SNAPSHOT</version>	//必须，项目当前版本(snapchat-快照，开发中非稳定版本)
  <packaging>war</packaging>	//可选，打包方式，默认jar
  <name>Maven hello project</name>	//可选，对用户更友好的项目名称
</project>
```

**maven指令**
```
//test前会自动compile，package前会自动test，install前会自动package
mvn clean compile   //编译
mvn clean test
mvn clean package   //打包（成jar后war）
mvn clean install   //将工程打出的包安装到本地仓库
```
