#2.Java集合  
##2.1 Set  
###2.1.1 HashSet 常用，可null   
    源码中使用HashMap实现（只使用Key部分功能）  
    HashSet<BigDecimal> ss = new HashSet<>();  
    ss.add(new BigDecimal(123));  
###2.1.2 LinkedHashSet 按插入顺序迭代  
    节点上维护着双重列表，即可知道插入顺序  
###2.1.3 TreeSet 按指定方式排序  
    可用Comparator指定排序方式，  
##2.2 MAp  
###2.2.1 HashMap 常用  
    HashMap<String , Double> map = new HashMap<>(); 
    map.put("语文" , 80.0);
###2.2.2 HashTable 线程安全  
###2.2.3 LinkedHashMap按插入顺序  
###2.2.4 TreeMap 可自定义排序  
###2.2.5 Properties *.properties文件  
    Properties prop = new Properties(); 
    FileInputStream fis = new FileInputStream("prop.properties");
    prop.load(fis); 
    
##2.3 List
###2.3.1 ArrayList 常用，查询快，增删慢，非线程安全（底层数组）  
###2.3.2 LinkedList 查询慢，增删快，非线程安全（底层链表）  
###2.3.3 Vector 线程安全，效率低，（底层数组）  

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
**遍历目录树**
```java

```
##3.3 File 操作文件
```java
    Files.createFile(path);
    Files.delete(path);
    Files.copy(source, target [, REPLACE_EXISTING]);    //可设置属性，如覆盖已有文件
    Files.move(source, target);
    Files.exist(path);  //是否存在
    Files.readAttributes(path, "*") //文件属性
```





#20.Java其它  
##20.1try-with-resource 资源自动关闭,实现了Closeable接口的类    
    注：1）try后面()中打开的资源会在{}代码执行完成/异常后自动关闭  
        2) 可结合catch、finally使用，在资源关闭后执行
    try (
      java.util.zip.ZipFile zf = new java.util.zip.ZipFile(zipFileName);
      java.io.BufferedWriter writer = java.nio.file.Files.newBufferedWriter(outputFilePath, charset)
    ) {}

#eclipse快捷键  
    Alt+Shift+B 打开面包屑视图，展示当天文件的路径（重要）  
    Ctrl+Shift+G  展示调用当前方法的所有类（鼠标定位到这个方法） 
    Ctrl+T / F4  当前接口的所有实现类    
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
