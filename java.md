#2.Java集合  
##2.1 Set  
**HashSet 常用，可null**
```
	源码中使用HashMap实现（只使用Key部分功能）  
	HashSet<BigDecimal> ss = new HashSet<>();  
	ss.add(new BigDecimal(123));  
```
**LinkedHashSet 按插入顺序迭代**  
```
	节点上维护着双重列表，即可知道插入顺序  
```
**TreeSet 按指定方式排序** 
```
	可用Comparator指定排序方式，  
```
##2.2 MAp  
**HashMap 常用**
```
	HashMap<String , Double> map = new HashMap<>(); 
	map.put("语文" , 80.0);
```
**HashTable 线程安全**  
**LinkedHashMap按插入顺序**  
**TreeMap 可自定义排序**  
**Properties *.properties文件**  
```
	Properties prop = new Properties(); 
	FileInputStream fis = new FileInputStream("prop.properties");
	prop.load(fis); 
 ``` 
##2.3 List
**ArrayList 常用，查询快，增删慢，非线程安全（底层数组）**  
```
//内部Object[]实现
1) 容量不足时每次扩容1/2，会触发一次数组复制动作
```
**LinkedList 查询慢，增删快，非线程安全（底层链表）**  
```java
//链表实现，Node<E>
Node(Node<E> prev, E element, Node<E> next) {
    this.item = element;
    this.next = next;
    this.prev = prev;
}
```
**Vector 线程安全，效率低（实现同ArrayList,底层数组,方法加synchronized）**  


##2.4 集合转换  
    Set-->List：ArrayList<BigDecimal> tempArrayList = new ArrayList<>(ss);  
    List-->Set: Set<String> listSet = new HashSet<String>(list);  
    Set-->Array: set.toArray(arr);  
    Array-->Set: Set<String> set = new HashSet<String>(Arrays.asList(arr));  
    List-->Array: list.toArray();  
    Array-->List: Arrays.asList(array);  
    Map-->Set:  
        Set<String> mapValuesSet = new HashSet<String>(map.values()); 
        List<String> mapKeyList = new ArrayList<String>(map.keySet()); 

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


##3.10 文件修改通知 WatchService

# 4.Java时间
```
Date	日期
SimpleDateFormat	日期格式
GreogrianCalendar	日历
TimeUnit.SECONDS.sleep(10);	/封装，增加可读性
Thread.sleep(10);	//毫秒数

```
**时区**  
```java
//GMT,自1970.1.1 00:00:00开始的毫秒数
System.currentTimeMillis();
//获取当前的时区(jvm默认使用操作系统时区)
System.out.println(TimeZone.getDefault());
System.out.println(new Date());
//设置项目时区
TimeZone.setDefault(TimeZone.getTimeZone("GMT"));
System.out.println(TimeZone.getDefault());
//所有Date都是GMT时间，在格式化输出时会根据当前时区进行增减
System.out.println(new Date());
System.out.println(new Date(System.currentTimeMillis() + 28800000));
```
**时间<-->字符串**  
```
//时间转字符串
SimpleDateFormat dateFormat = new SimpleDateFormat(
        "yyyy-MM-dd HH:mm:ss");
String dateTime = dateFormat.format(new Date());
System.out.println(new Date());
System.out.println(new Date().getTime());

//字符串转时间，减去当前时区，再转GMT
String dateStr = dateTime;
//dateFormat.setTimeZone(TimeZone.getTimeZone("GMT"));
Date dateTmp = dateFormat.parse(dateStr);
System.out.println(dateTmp);
System.out.println(dateTmp.getTime());
```

**String/Date<-->TimeStamp**
```
//1) TimeStamp在util.Date上增加了毫微秒的时间访问控制，getNanos（）， 格式 2016-07-28 12:51:56.919
//2) 是为了与数据库中的Timestamp数据类型进行匹配
System.out.println(new Timestamp(new Date().getTime()));	//Date-->TimeStamp
System.out.println(new Date(new Timestamp(System.currentTimeMillis()).getTime()));	//TimeStamp-->date
//string --> TimeStamp, 先转成Date
```

**Calendar日历**  
```
//计算一年中的第几个星期是几号
SimpleDateFormat df=new SimpleDateFormat("yyyy-MM-dd"); 
Calendar cal=Calendar.getInstance(); 
cal.set(Calendar.YEAR, 2006); 
cal.set(Calendar.WEEK_OF_YEAR, 1); 
cal.set(Calendar.DAY_OF_WEEK, Calendar.MONDAY); 
System.out.println(df.format(cal.getTime()));
```

# 17.Java反射
```
  1) Hibernate中，将select返回的结果封装成对象返回
  2）
```
# 19. Java关键字
**extends和implements**  
```
  extends继承普通class或abstract(抽象)类（java单继承）
  implements多继承能力，实现interface(接口)。 注： abstract implements interface  
```
**interface**
```
  1)实现多重继承, public interface Tinterface 
  2)方法都是public(可不写，默认)，只需定义返回值和名字，不能有实现
  3)属性默认是public static final(可不写，默认)
```
**abstract**
```
  1)修饰class，可无抽象方法。public abstract class AbstractList<E>
  2)修饰方法，public abstract void sleep(); //子类中必须实现
```
**final**
```
  值不可改变
  1)final int i=100 , i值不能改变
  2)final File f=new File("c:\\test.txt"); //f不能重新赋值，但f.xx可以
```
**static**
```
  不需new对象即可调用到 静态方法/变量
  1)static方法-->  public static void print()
  2)static变量--> 静态变量为所有对象共享，内存中只一个副本，当且仅当类第一次加载时被初始化一次
  3)static代码块--> 可任何位置，形成静态代码块优化性能，类初次加载时会按照顺序执行static代码块，且只执行一次
     private static Date startDate,endDate;
     static{
        startDate = Date.valueOf("1946");
        endDate = Date.valueOf("1964");
     }
```
**this/super**
```
1)this 指当前对象自己 (可用于返回对象自己)  
2)super指代父类。a)super() 调用父类中的初始化方法  b)super.ss()	 调用父类中方法/属性
```

**泛型**
```java
1)泛型必须是对象，不能是简单类型(int float)
2)类型参数可以多个，<T extends SomeClass & interface1 & interface2 & interface3>  //仍保持单继承的规则
3)限制类型，可使用extends， 如<T extends superclass>
4)参数类型可通配符类型，Class<?> classType = Class.forName("java.lang.String")
public class ObjectFoo<T>
{
    private T x;
    
    public ObjectFoo(T x)
    {
        this.x = x;
    }
    
    public T getX()
    {
        return x;
    }
    
    public void setX(T x)
    {
        this.x = x;
    }
}
ObjectFoo<Integer> in = new ObjectFoo<Integer>(123);
System.out.println(in.getX());

ObjectFoo<String> str = new ObjectFoo<String>("asdasd");
System.out.println(str.getX());
```

**synchronized同步锁**  
	1)所有对象自动含有单一的锁，当在对上调用其任一synchronized时，对象都被锁住。  
	2)当任务要执行被synchronized关键字保护的代码片段的时候，它将首先检查锁是否可用，然后获取锁，执行代码，释放锁。  
	3)对象里所有的synchronized方法共享一把锁  
	4)在并发时，将域设置成private很重要，否则synchronized关键字就不能防止其他任务直接访问域  
	5)用于static方法时，所有对象共享同一把锁  
```java
//用于代码块--优先
public void println(String x) {
    synchronized (this) {
        print(x);
        newLine();
    }
//用于方法
public synchronized boolean add(E e) {}
```

**transient**
```
0)变量将不被序列化，即反序列化后无值。(transient Object[] elementData)。transient只可修饰对象属性(不能func/class/局部变量)
1)serilization--序列化，将对象转成字节(供存储或网络发送)。deserilization-反序列化，将字节重建成对象。(类似js的stringfy和parseJson,两个独立的对象)
2)将需要序列化的类实现Serializable接口就可以
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
## 20.3 try-with-resource 资源自动关闭,实现了Closeable接口的类    
    注：1）try后面()中打开的资源会在{}代码执行完成/异常后自动关闭  
        2) 可结合catch、finally使用，在资源关闭后执行
    try (
      java.util.zip.ZipFile zf = new java.util.zip.ZipFile(zipFileName);
      java.io.BufferedWriter writer = java.nio.file.Files.newBufferedWriter(outputFilePath, charset)
    ) {}
## 20.5 解析html jsoup  
## 20.6 java初始化
```java
1) java数组： int[] a = { 5, 4, 2, 4, 9, 1 };
2) class中的int默认值0，对象默认值null
3) 初始化顺序，先静态对象，再非静态对象
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
