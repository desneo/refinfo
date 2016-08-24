# 1.Java试题
```java
1) Double xx = (double)(7 / 4);	//xx=1.0
2) int intzero = 0;
   char charzero = '0';
   System.out.println(intzero + charzero);	//48
3) 
```

# 2.Java集合  
## 2.0使用方法
```java
//1)容器.iterator() 要求容器返回一个Iterator。单向，无关类型
//2)next() 下一个元素; hasNext(); it.remove() 将返回的元素删除
Collections.min(Collection)		//最小值
Collections.max(Collection, Comparator);	//自定义比较方法
```


## 2.1 集合转换  
```java
Set-->List：ArrayList<BigDecimal> tempArrayList = new ArrayList<>(ss);  
List-->Set: Set<String> listSet = new HashSet<String>(list);  
Set-->Array: set.toArray(arr);  
Array-->Set: Set<String> set = new HashSet<String>(Arrays.asList(arr));  
List-->Array: list.toArray();  
Array-->List: Arrays.asList(array);  
Map-->Set: Set<String> mapValuesSet = new HashSet<String>(map.values()); 
Map-->List: List<String> mapKeyList = new ArrayList<String>(map.keySet());
String-->Array: char[] chars= xx.toCharArray();
Array-->String: new String(chars)
```

##2.2 List
**过滤list**  
```java
//1--
Iterator<Foo> it = col.iterator();
while( it.hasNext() ) {
  Foo foo = it.next();
  if( !condition(foo) ) it.remove();
}
//2--java8-steam
List<Person> olderThan30 = personList.stream().filter(p -> p.age >= 30).collect(Collectors.toList());
//3--Use CollectionUtils.filter(Collection,Predicate), from Apache Commons.
```
**List排序**
```java
//默认逆排序，List内的Object都必须实现了Comparable接口，否则报错
Collections.sort(arrayList );
Collections.reverse(arrayList );

//自定义排序
//1) 临时声明一个Comparator来实现排序， 	public int compare(Object a, Object b){} 返回值大于0则a在后
////对输出结果进行排序，最新的在最前面--20140613增加，周绍华
List<TaskHead> list = new ArrayList<TaskHead>();
//按离160最近距离排序
Integer[] zz = {100, 150, 200, 0};
List<Integer> xx = Arrays.asList(zz);
System.out.println(xx);
Collections.sort(xx, new Comparator<Integer>()
{
    public int compare(Integer o1, Integer o2)
    {
        return Math.abs(o1 - 160) - Math.abs(o2 - 160);
    }
});
System.out.println(xx);

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


##2.3 MAp  
```
1)负载因子--空表是0，半满状是0.5，默认0.75，每次扩容容量加倍
```
**遍历map**
```java
1) for (Map.Entry<Integer, Integer> entry : map.entrySet()) {}
2) for (Integer key : map.keySet()) { }
3) Iterator<Map.Entry<Integer, Integer>> entries = map.entrySet().iterator(); 
   while (entries.hasNext()) {  
      Map.Entry<Integer, Integer> entry = entries.next();  
   }
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

## 2.4 Set  
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

## 2.5 Queue队列
```
PriorityQueue-优先队列-【可】自定义优先级,实现Comparator接口来改变优先级。当调用peek()、poll和remove()方法时，获取的将是队列中优先级最高的元素！(详见list自定义排序)
```
## 2.6 Stack栈
 
# 3.Java多线程
**synchronized同步锁**

**concurrent--并发工具包**
```java
//以下类都线程安全
ConcurrentHashMap 
LinkedBlockingQueue --线程安全的阻塞(继承BlockingQueue)队列，可以指定容量，也可以不指定，不指定则默认值最大Integer.MAX_VALUE，其中主要用到put和take方法 ，put方法在队列满的时候会阻塞直到有队列成员被消费，take方法在队列空的时候会阻塞，直到有队列成员被放进来。
ConcurrentLinkedQueue -- 非阻塞队列，当queue为空时不阻塞，而是返回null，需要程序自己进行处理
```
# 3.5 Excutor-线程池，启动线程的优选方法
**定义任务类class--实现Runnable接口的run方法**

**newCatchedThreadPool-少用**  
```java
//线程复用，CatchedThreadPool将为每个任务都创建一个线程。然后在它回收旧线程时停止创建新线程
//使用静态的Excutor方法创建！ 当前的main线程继续运行，在Executor中的所有任务完成之后尽快退出。
ExecutorService exec = Executors.newCachedThreadPool();
for(int i=0; i<3; i++){
	exec.execute(new LiftOff());
}
//shutdown()方法的调用可以防止新任务被提交给Executor。
exec.shutdown();
```

**newFixedThreadPool-常用-数量固定，线程复用**
```java
//线程复用，newFixedThreadPool预先一次性执行线程分配，使用有限的线程集来执行所提交的任务-线程池。
//预先一次性执行代价高昂的线程分配，而不必为每个任务都固定的创建线程，节省时间。
//可看出每次同时执行的只有两个线程
ExecutorService exec = Executors.newFixedThreadPool(2);
for(int i=0; i<4; i++){
	exec.execute(new LiftOff());
}
exec.shutdown();
```

**newSingleThreadPool-数量1的newFixedThreadPool**
```java
如果向newSingleThreadPool提交了多个任务，那么这些任务将排队，每个任务都会在下一个任务开始之前结束。
使用场景，如多个线程都需要使用文件系统，可免去同步的操作。
```

## 3.7分支/合并(Fork/Join)框架
**原理**
```java
使用“分而治之”算法。其思路是将算法要处理的数据空间拆成较小的独立块，这是“映射”阶段。一旦块集处理完毕之后，就可以将部分结果收集起来形成最终结果，这是“归约”阶段。
一个例子：计算一个大型整数数组的总和。可以将数组划分为较小的部分，并发线程对这些部分计算部分和。然后部分相加，计算总和。此算法在多核架构上可看到明显的性能提升。
```
**描述**
```
1.fork()	允许执行计划ForkJoinTask一步执行。这允许从现有的ForkJoinTask启动新的ForkJoinTask。
2.join()	允许ForkJoinTask等待另一个ForkJoinTask完成。
RecursiveAction	表示不产生返回值的执行。
RecursiveTask	常用，产生返回值。
```

# 7.Java I/O（SE7）
## 7.1 Path 位置/路径
    注：1）Path可独立存在，只有在读取或写入时才会异常
        2) 去掉./..-->path.normalize() ,快捷方式的真实地址:path.toRealPath()
    Path listing = Paths.get("C:/Users/z00316474/Desktop");
## 7.2 处理目录和目录树
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
## 7.3 Files 操作文件
```java
    Files.createFile(path);
    Files.delete(path);
    Files.copy(source, target [, REPLACE_EXISTING]);    //可设置属性，如覆盖已有文件
    Files.move(source, target);
    Files.exist(path);  //是否存在
    Files.readAttributes(path, "*") //文件属性
```
**读文件**
```java
//java7:
List<String> lines = Files.readAllLines(path, StandardCharsets.UTF_8);
byte[] bytes = Files.readAllBytes(path);  

//java8
Files.lines(path, StandardCharsets.UTF_8).forEach(System.out.println);	//流式打开，内存占用小

//java6--从jar包中读配置文件
//getResourceAsStream入参中如果开头'/',则从classPath根目录(也即jar包根目录)找起；否则以*.class文件所在的目录往下匹配文件
//Class.getClassLoader().getResource(String path)		//path不能以’/'开头时；path是从ClassPath根下获取；
//文件绝对路径(但不可直接读取)：
String xx = TestClass.class.getResource("/config/activejs/xx.js").getPath();
InputStream in = TestClass.class.getResourceAsStream("/config/activejs/xx.js");
BufferedReader br=new BufferedReader(new InputStreamReader(in));  
String s="";  
while(true) { 
	String xx = br.readLine();
	if( null == xx){
		break;
	}
	s += (xx+"\n");
}
System.out.println(s);
```
**写文件**
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
## 7.4 异步I/O
**回调式**
```java
//读取文件最后的100个字符
ByteBuffer buffer = ByteBuffer.allocate(1000);
//异步方式打开文件
AsynchronousFileChannel channel = AsynchronousFileChannel.open(path);
//读文件
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

## 7.5 try-with-resource 资源自动关闭,实现了Closeable接口的类    
    注：1）try后面()中打开的资源会在{}代码执行完成/异常后自动关闭  
        2) 可结合catch、finally使用，在资源关闭后执行
    try (
      java.util.zip.ZipFile zf = new java.util.zip.ZipFile(zipFileName);
      java.io.BufferedWriter writer = java.nio.file.Files.newBufferedWriter(outputFilePath, charset)
    ) {}

## 7.10 文件修改通知 WatchService

## 7.11读取网络文件

# 10.Java时间
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

# 11.Java字符串
**StringBuilder和StringBuffer**
```
一般在单线程的系统环境下优先使用StringBuilder，因为它是非线程安全的。
而在多线程的环境下，比如Web系统，优先使用StringBuffer，因为它当中绝大部分方法都使用了synchronized进行修饰，保证了多线程环境下的线程安全问题。
```

# 15. enum枚举类型
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
//自动将入参匹配到一个函数(按入参个数)，达到同等功能, 典型应用的替换Runnable / Comparator
Collections.sort(list, (o1, o2) -> {
    return Math.abs(o1 - 160) - Math.abs(o2 - 160);
});
//forEach循环
list.forEach((t) -> {
    System.out.println(t);
});
```
**两个冒号**  
```java
//如 objCollection.forEach(someInfrastructure::output);简单来讲，就是构造一个该方法的闭包。比如：
Math::max //等效于(a, b)->Math.max(a, b)
String::startWith  //等效于(s1, s2)->s1.startWith(s2)
s::isEmpty //等效于()->s.isEmpty()
```


# 17.Java反射
```
  1) Hibernate中，将select返回的结果封装成对象返回
  2）
```

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

**instanceOf**  
```java
1) p instanceof Student
```


**泛型**
```java
1)泛型必须是对象，不能是简单类型(int float)
2)类型参数可以多个，<T extends SomeClass & interface1 & interface2 & interface3>  //仍保持单继承的规则
3)限制类型，可使用extends， 如<T extends superclass>
4)通配符？表示未知类型， 处理定义List<Object>， 但是传入list<String>时编译报错的情况（因类型擦除）。
5)类型擦除：使用泛型的时候加上的类型参数，会被编译器在编译的时候去掉。这个过程就称为类型擦除。类型擦除的过程，首先是找到用来替换类型参数的具体类。这个具体类一般是Object。如果指定了类型参数的上界的话，则使用这个上界。把代码中的类型参数都替换成具体的类。同时去掉出现的类型声明，即去掉<>的内容。比如: T get()方法声明就变成了Object get()； List<String>就变成了List。 接下来就可能需要生成一些桥接方法（bridge method）。这是由于擦除了类型之后的类可能缺少某些必须的方法
//示例1
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
ObjectFoo<Integer> in = new ObjectFoo<Integer>(123); System.out.println(in.getX());
ObjectFoo<String> str = new ObjectFoo<String>("asdasd"); System.out.println(str.getX());

//示例2
public void inspect(List<Object> list)
{
	for (Object obj : list)
	{
		System.out.println(obj);
	}
	list.add(1); //这个操作在当前方法的上下文是合法的。 
}
public void test()
{
	List<String> strs = new ArrayList<String>();
	inspect(strs); //编译错误, 如果不报错的话会导致往list<String>中添加了一个int 
}
```

**synchronized同步锁**  
```
1)所有对象自动含有单一的锁，当在对上调用其任一synchronized时，对象都被锁住。  
2)当任务要执行被synchronized关键字保护的代码片段的时候，它将首先检查锁是否可用，然后获取锁，执行代码，释放锁。  
3)对象里所有的synchronized方法共享一把锁  
4)在并发时，将域设置成private很重要，否则synchronized关键字就不能防止其他任务直接访问域  
5)用于static方法时，所有对象共享同一把锁  
```
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

**finally**
```
try{}catch(){} finally{//此处代码总会执行}
```

**transient**
```
0)变量将不被序列化，即反序列化后无值。(transient Object[] elementData)。transient只可修饰对象属性(不能func/class/局部变量)
1)serilization--序列化，将对象转成字节(供存储或网络发送)。deserilization-反序列化，将字节重建成对象。(类似js的stringfy和parseJson,两个独立的对象)
2)将需要序列化的类实现Serializable接口就可以
```

**threadLocal**  
```java
1、用于线程集的全局变量(当前线程共享)，  private static ThreadLocal<Integer> seqNum = new ThreadLocal<Integer>() 
    seqNum.set(1); seqNum.get(); 
2、单的static是所有线程共享的全局变量
```

# 21.Stream java8
```java
//1) Stream对集合功能的增强。类似Iterator，可顺序/并行对原Stream进行过滤汇总 ，数据源可无限。
//3) 创建Stream(Collection已实现接口)-->转换Stream，不影响源,返回新的Stream对象，可多次-->对Stream进行操作，得到想要的结果
//4) Steam操作（Intermediate型，后可多个）: 
      map (mapToInt, flatMap 等)、 filter、 distinct、 sorted、 peek、 limit、 skip、 parallel、 sequential、 unordered
      limit--取前多少个; skip--扔掉前n个
//5) Steam操作(Terminal型，只能一个，流的最后操作，会启动流的遍历)
      forEach、 forEachOrdered、 toArray、 reduce、 collect、 min、 max、 count、 anyMatch、 allMatch、 noneMatch、 findFirst、 findAny、 iterator
      sorted--排序，与数组排序优势在于可先进行，filter/distinct等操作后再排序，减少数量，缩短执行时间
//6）list.parallelStream() ,创建并行的stream
//70 stream转成其他结构： 
  stream.collect(Collectors.toList());  
  Set set1 = stream.collect(Collectors.toSet());
list.stream().filter(num->{return num>0;}).collect(Collectors.toList());   //过滤list中大于0值，转成list返回(lambda返回true/false)
list.stream().distinct().count()  //distinct, 去重，需重写equals(和hashCode)方法 ， 统计个数
List<String> output = wordList.stream().map(String::toUpperCase).collect(Collectors.toList()); //map/faltmap,所有单词转大写，一一映射
```

#50.Java其它  
## 50.0 java配置
```
1)安装，jdk和jre需安装到不同目录（F:\program\java8\jdk1.8.0_102\） //java -version有值则安装成功
2)环境变量-->系统变量-->新建JAVA_HOME，值F:\program\java8\jdk1.8.0_102(jdk的安装路径)	//windows环境变量key不区分大小写
	  -->系统变量-->Path中添加 %JAVA_HOME%\bin;%JAVA_HOME%\jre\bin;
	  -->系统变量-->CLASSPATH（无则新建）中添加 .;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar;
	  -->若成功，则javac有输出值
3) 注：若javac还是无法执行，检查bin目录下是否有javac.exe,否则重装
```
## 50.1 java查看源码  
```
    手动: 下载*.tar.gz的jdk包--src.zip(源码，*.exe中可能无此文件)-->欲附加源码的jar，右键，properties-->External File.. ,src.zip  
    IDE自动下载(maven): window-->preference-->maven-->勾选，download  sources / javaDoc  
```

# 50.2 重写equals方法
```java
//同时必须重写hashCode方法，以维护相等的对象具有相等的hash码（不重写无效）
@Override
public boolean equals(Object obj)
{
    if (this == obj)
    {
        return true;
    }
    Student xx = (Student)obj; //类型转换
    return this.firstName.equals(xx.firstName);
}
@Override
public int hashCode()
{
    return this.getFirstName().hashCode();
}
```


## 50.15 Java字符编码 
```java
1)java程序内部字符集使用unicode表示的（2字节），但unicode只定义了表示，没有定义存储字符时的表示方法。
2)内置部分字符集： StandardCharsets.UTF_8
3)只有当从外部引入byte[]或向外部输出byte[]时才需要指定编码。如socket、file操作等！  
//编码转换,字符省略时默认'utf-8'  
String ss = "周123"; 
System.out.println(new String(ss.getBytes("UTF-8"), StandardCharsets.UTF_8));
//当前运行时的字符集
Charset.defaultCharset().displayName();
//是否支持字符集  
Charset.isSupported("gbk");
//当前支持的所有字符集
Set<String> charsetNames = Charset.availableCharsets().keySet();
```

# 60. Groovy
**基础**  
```groovy
== 相当于java中的equals

//安全引用操作符, 如果xx==null， 则后面不执行
println xx?.yy;  

//字符串求值， 或${x}
def x = 1
def doubleQuoteWithDollar = "I am $x dolloar" //输出I am 1 dolloar

//定义变量
xx = "asdasd"  //全局变量
def str= "i am a person"  //类型不定
String str = "12312"    //若指定类型，则变量类型不可修改

//对象
def xx= {}; xx.name="asdasd"

//定义函数
def  nonReturnTypeFunc(){  //函数返回值类型可指定类型，如String，如果不return string则报错
     [return] last_line;   //return可选， 最后一行代码的执行结果就是本函数的返回值
}


//List
def aList = [5,'string',true] //List由[]定义，其元素可以是任何对象
        //可以直接通过索引存取，而且不用担心索引越界。如果索引超过当前链表长度，List会自动往该索引添加元素
array.add(obj); 
def xx = []; xx +="name"
println(aList[0..1])  //取前2个值
println(aList[-1])  //倒数第一个值
aList.size  ===>结果是101

//Map
def aMap = ['key1':'value1','key2':true]  //Map由[:]定义，注意其中的冒号。冒号左边是key，右边是Value。
println aMap['keyName']   //取值，这种表达方法更传统一点
aMap.anotherkey = "i am map"  //为map添加新元素
def key1="wowo"
def aConfusedMap=[key1:"who am i?"] //aConfuseMap中的key1到底是"key1"还是变量key1的值“wowo”？显然，答案是字符串"key1"。
                              //如果要是"wowo"的话，def aConfusedMap=[(key1):"who am i?"]
```

**闭包**
```
//Groovy中，当函数的最后一个参数是闭包的话，可以省略圆括号。
targetFile.eachLine { //流式操作每一行
    String line ->  println line
}

//定义闭包
def sayhello = {
  name ->   //定义入参
    println("name="+name);
}
```

**内置的集合操作**
```
each  //["Cat", "Dog", "Elephant"].each{yy-> println yy}(默认提供一个it变量)	遍历集合，对每一项处理函数
collect //手机每一项处理后的返回值，类似java8 lamdba中map
inject  //集合分组
findAll //找到所有匹配元素
max   //集合中最大值
min
```

**正则表达式**  
```groovy
~     创建匹配模式
=~    创建一个匹配器
==~   计算是否匹配

```

**操作xml**  
```
//创建xml
import groovy.xml.*
def st = new StringWriter()
MarkupBuilder mb  = new MarkupBuilder(st);
mb.feed{
    entry(id:"1234567"){
        title{
         show(name:"1234","fdfsa")
        }
         link:"readf"
    }
}
print st

//解析xml
//XmlParser 支持xml文档的GPath表达式，支持findAll、find的查找方式
//XmlSlurper  类似XmlParser，懒加载方式
//DOMCategory 用一些语法支持DOM的底层解析
def xmlSource = new File('xmllocation')
def slurper= new XmlSlurper().parse(xmlSource)
println  result.person*.city    //result.person我们会得到多个节点，这时候使用列表操作符*.可以对多个节点收集信息并返回为一个集合
println  result.person*.@name   //@--获取对应属性的值

def result = xml.parse(new File("C:/Users/xiaonanzhi/Person.xml"))
result.person.find{it->
 if(it.@name == "xiao5"){
 println it.link.@rel
 }
}

```

**[文件I/O操作](http://docs.groovy-lang.org/latest/html/groovy-jdk/java/io/File.html)**  
```groovy
//主要使用闭包操作
def targetFile = new File(文件名) //创建file对象
//读文件
targetFile.getBytes()  //文件内容一次性读出，返回类型为byte[]
List	readLines()   //行读取
targetFile.eachLine { //流式操作每一行
    String line ->  println line
}

//写文件
targetFile.append(Object text, String charset)    //尾部追加，不存在则创建
targetFile.write(String text)   
targetFile.write(String text, String charset) 
````

**java<-->groovy互相调用**  
```groovy
//groovy<--java
	import java.sql.Date;
	Date xx = new Date();

//java<--groovy
//Test.java
	Test test = new Test();
	String[] roots = new String[]{"files/"};	//指定groovy脚本加载目录
	GroovyScriptEngine groovyScriptEngine = new GroovyScriptEngine(roots); //groovy引擎
	Class scriptClass = groovyScriptEngine.loadScriptByName("exp.groovy");	//加載腳本
	GroovyObject scriptInstance = (GroovyObject)scriptClass.newInstance();	//實例化腳本
	//調用方法，傳入參數
	Test ret = (Test)scriptInstance.invokeMethod("helloWithParam",new Object[]{test,100});
	System.out.println(ret.getAge());
//exp.groovy
	def helloWithParam(person, age){
		person.age = age;
		return person;
	}

//groovy<--groovy
    String[] roots = new String[]{"files/"};    //指定groovy脚本加载目录
    GroovyScriptEngine groovyScriptEngine = new GroovyScriptEngine(roots); //groovy引擎
    Class scriptClass = groovyScriptEngine.loadScriptByName("exp.groovy");  //加載腳本
    Binding binding = newBinding();   //脚本中变量入参
    binding.setVariable("name", "zhousahjkshdkajs");  //设置变量值
    Object output = groovyScriptEngine.run("hello.groovy", binding);
//hello.groovy
	return "in param name is ${name}"
```


**包**  
```
//默认导入
java.io.*
java.lang.*
java.math.BigDecimal
java.math.BigInteger
java.net.*
java.util.*
groovy.lang.*
groovy.util.*

//引入jar包
*.groovy中加入 import java.math.* 即可！
```

**配置**  
```
1、下载apache-groovy-sdk-2.4.7.zip --> 环境变量：GROOVY_HOME=G:\program-my\groovy-2.4.7 ， path添加：%GROOVY_HOME%/bin; 
2、测试 groovy -version 
        groovyconsole.bat //grrovy控制台，运行Ctrl+R (view中可去掉 show script in output)
3、groovyc hello.groovy //编译[可免] ;  groovy hello.groovy
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
**插件**  
```
1) 在线安装：eclipse-->help-->marketpalce
   离线：插件jar包放在eclipse下plugin目录
2) 打开文件文件： Open Explorer (市场中名字eclispe explorer)
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
4) 设置tab键为4个空格
	window->preference->General->Editors->Text Editors,选中右侧的 insert space for tabs;
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
1.“系统变量”中增加变量 M2_HOME , 值 H:\program\apache-maven-3.2.3 （Maven的安装路径）。 
2.“ 系统变量”Path中末尾加 %M2_HOME%\bin;	
//测试安装成功
echo %M2_HOME%		//变量是否指向了正确的安装目录
mvn  -v			//能否正确找到mvn的执行脚本

//IDE中配置maven
window-->preference-->搜索maven-->Installations-->add
```

**基础问题** 
```
1、pom.xml总是在项目的根目录。
2、约定优于配置：
		源码目录为 src/main/java
		编译输出目录为 target/classes/
		打包方式默认为jar(如果不指定packaging标签的话)
		包输出目录为target
3、maven中通过groupId、artifactId、version定位到一个唯一jar、pom、car。
5、maven指令
	//test前会自动compile，package前会自动test，install前会自动package
	mvn clean compile   //编译
	mvn clean test
	mvn clean package   //打包（成jar后war）
	mvn clean install   //将工程打出的包安装到本地仓库

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
  
  <dependencies>  
	<dependency>  
	    <groupId>org.springframework</groupId>  //必选
	    <artifactId>spring-core</artifactId>  //必选
	    <version>${springframework.version}</version	//必选  
	    <type>jar</type>   //可选，依赖类型，默认jar
	    //可选，依赖范围，默认compile-编译/测试/运行都需要；test-只测试需要；provided-编译测试；runtime-运行
	    <scope>compile</scope>  
	</dependency>  
  </dependencies>  
</project>
```


**其它**  
```
10、创建maven project：new project-->maven project --> 勾选"Create a simple project" 不使用骨架
11、[创建webapp项目](http://jingyan.baidu.com/article/9f63fb91a7d2a5c8400f0e20.html)：
	先创建一个普通的maven project（package选war），src/main下面会生成webapp目录--> 工程 properties-->project facets -->java选中1.8
	-->Danamic web module取消，apply， 再勾选apply-->生成webContent目录-->将其下的MEATA_INF和WEB_INF复制到src/main/webapp下
	-->删掉webcontent-->
	修改发布规则-->工程 右键 properties --> Deployment Assembly -->测试目录不需发布可以去掉
					   --> 指定web路径-->add-->folder-->选中webapp目录
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
