# 2.Java集合  
## 2.0使用方法
```
//1)容器.iterator() 要求容器返回一个Iterator。单向，无关类型
//2)next() 下一个元素; hasNext(); it.remove() 将返回的元素删除
Collections.min(Collection)		//最小值
Collections.max(Collection, Comparator);	//自定义比较方法
```
## 2.1 Set  
```
定义equals()方法（int、string已自带）以确保对象唯一性。无序。
```
**HashSet 常用，可null**
```
//1) 源码中使用HashMap实现（只使用Key部分功能）  
HashSet<BigDecimal> ss = new HashSet<>();  
ss.add(new BigDecimal(123));  
```
**LinkedHashSet 按插入顺序迭代**  
```
1)兼具速度与顺序，节点上维护着双重列表，即可知道插入顺序  
2)元素必须定义hashCode()方法来确保唯一性(或默认)
```
**TreeSet 按指定方式排序** 
```
1)按插入顺序保存，将元素存储在红-黑树数据结构中。
2)自定义排序方式，实现Comparable接口，详见见List中排序
```
##2.2 MAp  
```
1)负载因子--空表是0，半满状是0.5，默认0.75，每次扩容容量加倍
```
**HashMap 常用，可null**
```
HashMap<String , Double> map = new HashMap<>(); 
map.put("语文" , 80.0);
```
**HashTable 线程安全,继承Dictionary,key/value不可为null，**  
**LinkedHashMap按插入顺序**  
```
遍历次序按照插入顺序进行，比HashMap慢一点，但迭代访问更快，因其内部是使用数组实现。
```
**TreeMap 可自定义排序** 
```
基于红黑树的实现，次序由Comparator决定。详见List中自定义顺序例子
```
**Properties *.properties文件**  
```
	Properties prop = new Properties(); 
	FileInputStream fis = new FileInputStream("prop.properties");
	prop.load(fis); 
 ``` 
##2.3 List
**List排序**
```
//默认逆排序，List内的Object都必须实现了Comparable接口，否则报错
Collections.sort(arrayList );
Collections.reverse(arrayList );

//自定义排序
//1) 临时声明一个Comparator来实现排序， 	public int compare(Object a, Object b){} 返回值大于0则a在后
////对输出结果进行排序，最新的在最前面--20140613增加，周绍华
List<TaskHead> list = new ArrayList<TaskHead>();
Collections.sort(list, new Comparator<TaskHead>(){
	public int compare (TaskHead t1, TaskHead t2){
		return t2.getId().compareTo(t1.getId());
	}
});

//2)自定义class实现Comparable接口
class Employee implements Comparable<Employee>
{
    private int id;
    
    //返回值>0,则this被排在后面,arg0放前面; <0则this在前面。
    public int compareTo(Employee other)
    {
        if (id < other.id)
            return -1;
        if (id > other.id)
            return 1;
        return 0;
    }
}

```

**ArrayList 常用，查询快，增删慢，非线程安全（底层数组）**  
```
//内部Object[]实现
1) 容量不足时每次扩容1/2，会触发一次数组复制动作
```
**LinkedList 查询慢，增删快，非线程安全（底层链表）**  
//可当作堆栈、队列和双向队列使用， addFirst() 、getLast() 、 removeFirst()
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
```
Set-->List：ArrayList<BigDecimal> tempArrayList = new ArrayList<>(ss);  
List-->Set: Set<String> listSet = new HashSet<String>(list);  
Set-->Array: set.toArray(arr);  
Array-->Set: Set<String> set = new HashSet<String>(Arrays.asList(arr));  
List-->Array: list.toArray();  
Array-->List: Arrays.asList(array);  
Map-->Set:  
  Set<String> mapValuesSet = new HashSet<String>(map.values()); 
  List<String> mapKeyList = new ArrayList<String>(map.keySet());
```

## 2.5 Queue队列
```
PriorityQueue-优先队列-【可】自定义优先级,实现Comparator接口来改变优先级。当调用peek()、poll和remove()方法时，获取的将是队列中优先级最高的元素！(详见list自定义排序)
```
## 2.6 Stack栈
 

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
1)属性/func参数--值不可改变
  a)final int i=100 , i值不能改变
  b)final File f=new File("c:\\test.txt"); //f不能重新赋值，但f.xx可以
2)方法--子类不得覆盖重写该方法，确保在继承中使方法行为保持不变
3)class--表明不打算继承该类，而且也不允许别人继承。 fianl class Art {}
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
