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
```

##2.2 List
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
