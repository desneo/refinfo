
#3.Java I/O（SE7）
##3.1 Path 位置/路径
    注：1）Path可独立存在，只有在读取或写入时才会异常
        2) 去掉./..-->path.normalize() ,快捷方式的真实地址:path.toRealPath()
    Path listing = Paths.get("C:/Users/z00316474/Desktop");
##3.2 处理目录和目录树
**目录中查找文件**
```java
try (DirectoryStream<Path> stream = Files.newDirectoryStream(path,"*.doc"))
{
	for (Path path : stream)
	{
		System.out.println(path.getFileName());
	}
}
```
**遍历目录树-重写visitFile即可**
```java
    public static void main(String[] args) throws IOException
    {
        Path path = Paths.get("F:/project/C10/20160704-");
        Files.walkFileTree(path, new findPropertyVisitor());
    }
    private static class findPropertyVisitor extends SimpleFileVisitor<Path>{
        @Override
        public FileVisitResult visitFile(Path file, BasicFileAttributes attributes){
            if(file.toString().endsWith(".properties")){
                System.out.println(file.getFileName());
            }
            return FileVisitResult.CONTINUE;
        }
    }
```
##3.3 Files 操作文件
```java
    Files.createFile(path);
    Files.delete(path);
    Files.copy(source, target [, REPLACE_EXISTING]);    //可设置属性，如覆盖已有文件
    Files.move(source, target);
    Files.exist(path);  //是否存在
    Files.readAttributes(path, "*") //文件属性
```
**读取文件**
```java
java7:
List<String> lines = Files.readAllLines(path, StandardCharsets.UTF_8);
byte[] bytes = Files.readAllBytes(path);  
java8: Files.lines(path, StandardCharsets.UTF_8).forEach(System.out.println);	//流式打开，内存占用小  
```
**写入文件**
```java
try(BufferedWriter writer = Files.newBufferedWriter(path, StandardCharsets.UTF_8, StandardOpenOption.WRITE)){
    writer.write("Hello World!");
}
```
**文件寻址定位**
```java
//读取文件最后的100个字符
ByteBuffer buffer = ByteBuffer.allocate(100);
FileChannel channel = FileChannel.open(path, StandardOpenOption.READ);
channel.read(buffer, channel.size()-100);
```
##3.4 异步I/O
**回调式**
```java
//读取文件最后的100个字符
ByteBuffer buffer = ByteBuffer.allocate(1000);
//异步方式打开文件
AsynchronousFileChannel channel = AsynchronousFileChannel.open(path);
//读取文件
channel.read(buffer, 0, buffer,
        new CompletionHandler<Integer, ByteBuffer>()
        {

            @Override
            //读取完成时回调方法
            public void completed(Integer paramV, ByteBuffer paramA)
            {
                
            }

            @Override
            public void failed(Throwable paramThrowable,
                    ByteBuffer paramA)
            {
            }
        });
```

## 3.5 try-with-resource 资源自动关闭,实现了Closeable接口的类    
    注：1）try后面()中打开的资源会在{}代码执行完成/异常后自动关闭  
        2) 可结合catch、finally使用，在资源关闭后执行
    try (
      java.util.zip.ZipFile zf = new java.util.zip.ZipFile(zipFileName);
      java.io.BufferedWriter writer = java.nio.file.Files.newBufferedWriter(outputFilePath, charset)
    ) {}

##3.10 文件修改通知 WatchService


# 10.Java字符串
**StringBuilder和StringBuffer**
```
一般在单线程的系统环境下优先使用StringBuilder，因为它是非线程安全的。
而在多线程的环境下，比如Web系统，优先使用StringBuffer，因为它当中绝大部分方法都使用了synchronized进行修饰，保证了多线程环境下的线程安全问题。
```

# 16.Lambdas-匿名函数
**[示例](http://blog.csdn.net/ioriogami/article/details/12782141)**
```java
//三部分组成：参数列表，箭头（->），以及一个表达式或语句块。
public int add(int x, int y) { return x + y; }	//转换如下
(int x, int y) -> x + y;	
或 (x, y) -> x + y; //返回两数之和,java编译器自动根据上下文判断
或 (x, y) -> { return x + y; } //显式指明返回值
c -> { return c.size(); }	//只有一个参数自动推导
```
**lambdas的用处**
```java
//自动将入参匹配到一个函数(入参个数)，达到同等功能效果
//典型的替换Runnable / Comparator
Thread oldSchool = new Thread( new Runnable () {
	@Override
	public void run() {
	    System.out.println("This is from an anonymous class.");
	}
	} );
//转换后
Thread gaoDuanDaQiShangDangCi = new Thread( () -> {
	System.out.println("This is from an anonymous method (lambda exp).");} )
```

# 17.Java反射-reflect

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

**远程debug**
```
下一个断点: F8  单步： F6	  进入： F5  
查看变量：　debug视图--》 Variables --> 变量名--》voProperties --> properties -->table即可  

--》打开远程debug： debug图标 --> debug configuration -->  Remote java Application --》 配置地址端口--》 勾选"Allow termination of remote VM"
--》 查看debug远程端口：  /home/business/opt/container/bin/catalina.sh -->  搜索 Xdebug --》（常用:8090）
-->打开debug视图 --》 右上角 open persperctive --> debug
```
    
# maven
**安装**
```
注：下载后解压即可(先安装jdk), 升级下载最新包，修改M2_HOME值即可
1.在“系统变量”中增加一个变量，名 M2_HOME , 值 H:\program\apache-maven-3.2.3 （Maven的安装路径）。 
2.在“ 系统变量”Path中末尾加 %M2_HOME%\bin;	

配置完成，以下两条指令可执行成功：
echo %M2_HOME%		//变量是否指向了正确的安装目录
mvn  -v			//能否正确找到mvn的执行脚本
```

**IDE中配置maven**
```
window-->preference-->搜索maven-->Installations-->add
```

**maven原理**
```
//传递性依赖
   例子：项目有一个Spring-aop:4.1.1.RELEASE的依赖，而实际上Spring-aop也有自己的依赖（maven仓库中的pom.xml），maven自动解析依赖获得依赖的包。
//依赖冲突的处理
   如果项目A有这样的依赖关系：A->B->C->X(1.0)、A->D->X(2.0), 这样依赖路径上有两个版本的X。原则如下：
   1.路径最近者优先。如上1.0的路径长度是3，2.0的长度是2，则2.0的X会被使用。
   2.路径长度相同时，第一声明者优先。即在pom.xml中使用先声明的那个。

查看依赖信息
   mvn dependency:tree 优先 --> 解析成依赖树，可以看出某个依赖是从哪个路径引入的。
   mvn dependency:list 	--> 解析并显示依赖列表。 列出所有依赖的文件。
```
