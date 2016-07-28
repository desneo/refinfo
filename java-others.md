# 18.Java异常
```java
//java7
try {     
} catch (FileNotFoundException | ParseException | ConfigurationException e) {
	System.err.println("Config file '" + fileName + "' is missing or malformed");   
} catch (IOException iox) {     
	System.err.println("Error while processing file '" + fileName + "'");   
	throw e;	//抛出异常
	throw new NullpointException();
} 
```
**自定义异常**
```
//最重要的是异常名字
class SimpleException extends Exception{}
```

# 19. enum枚举类型
```java
	Color xx = Color.RED;
	switch (xx)
	{
	    case RED:
	        System.out.println(xx.getValue());
	        break;
	    default:
	        break;
	}
public class EnumTest
{
    public static enum Color
    {
       //enum可给定值
        RED("AA"), YELLOW("ff");
        private String name;
        private Color(String namein)
        {
            this.name = namein;
        }
        public String getValue()
        {
            return name;
        }
    }
}
```

#20.Java其它  
## 20.0 java配置
```
1)安装，jdk和jre需安装到不同目录（F:\program\java8\jdk1.8.0_102\） //java -version有值则安装成功
2)环境变量-->系统变量-->新建JAVA_HOME，值F:\program\java8\jdk1.8.0_102(jdk的安装路径)	//windows环境变量key不区分大小写
	  -->系统变量-->Path中添加 %JAVA_HOME%\bin;%JAVA_HOME%\jre\bin;
	  -->系统变量-->CLASSPATH（无则新建）中添加 .;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar;
	  -->若成功，则javac有输出值
3) 注：若javac还是无法执行，检查bin目录下是否有javac.exe,否则重装
```
## 20.1 java查看源码  
```
    手动: 下载*.tar.gz的jdk包--src.zip(源码，*.exe中可能无此文件)-->欲附加源码的jar，右键，properties-->External File.. ,src.zip  
    IDE自动下载(maven): window-->preference-->maven-->勾选，download  sources / javaDoc  
```

## 20.5 解析html jsoup  

```
## 20.15 Java字符编码
```java
1)java程序内部字符集使用unicode表示的（2字节），但unicode只定义了表示，没有定义存储字符时的表示方法。
2)只有当从外部引入byte[]或向外部输出byte[]时才需要指定编码。如socket、file操作等！  
//编码转换,字符省略时默认'utf-8'  
String ss = "周123"; 
System.out.println(new String(ss.getBytes("UTF-8"), "UTF-8"));
//当前运行时的字符集
Charset.defaultCharset().displayName();
//是否支持字符集  
Charset.isSupported("gbk");
//当前支持的所有字符集
Set<String> charsetNames = Charset.availableCharsets().keySet();
```


#eclipse
**快捷键** 
```
Alt+Shift+B 打开面包屑视图，展示当天文件的路径（重要）  
Ctrl+Shift+G  展示调用当前方法的所有类（鼠标定位到这个方法） 
Ctrl+T / F4  当前接口的所有实现类
Ctrl+Shift+O 删除unuse的包
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
``` 
**配置** 
```
1)[关闭变量名后自动补全类型字符](http://www.itnose.net/detail/6143864.html)  
2)Ctrl+S时自动格式化代码，删除unuse的包:  
	windows->preference->java/Editor/Save action->勾选Format source Code, Organize import  
3)背景颜色  
	代码区: Window->Preferences->General->Editors->Text Editors->Backgroud colors->85/123/205  
	package区域：修改windows主题，桌面->右键，个性化->窗口颜色->窗口->85/123/205  
	关键字颜色：[方案2](http://blog.csdn.net/etjnety/article/details/7846703/)  
```



#远程debug  
    下一个断点: F8  单步： F6	  进入： F5  
    查看变量：　debug视图--》 Variables --> 变量名--》voProperties --> properties -->table即可  

    --》打开远程debug： debug图标 --> debug configuration -->  Remote java Application --》 配置地址端口--》 勾选"Allow termination of remote VM"
    --》 查看debug远程端口：  /home/business/opt/container/bin/catalina.sh -->  搜索 Xdebug --》（常用:8090）
    -->打开debug视图 --》 右上角 open persperctive --> debug
