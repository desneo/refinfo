**js读取网络文件**  
```javascript
const https = require('https');

var req = https.request("https://nodejs.org/dist/v4.4.4/node-v4.4.4-x64.msi", (res) => {
  console.log('statusCode: ', res.statusCode);
  console.log('headers: ', res.headers);

  res.on('data', (d) => {
    process.stdout.write(d);
  });
});
req.end();

req.on('error', (e) => {
  console.error(e);
});
```

**Stream**
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
**重写equals方法**
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

**lambda**  
```java
//两个冒号， objCollection.forEach(someInfrastructure::output);
//注： 简单来讲，就是构造一个该方法的闭包。比如：
Math::max //等效于(a, b)->Math.max(a, b)
String::startWith  //等效于(s1, s2)->s1.startWith(s2)
s::isEmpty //等效于()->s.isEmpty()
```

**泛型**
```java
//1、通配符？表示未知类型， 处理定义List<Object>， 但是传入list<String>时编译报错的情况（因类型擦除）。
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
//2、类型擦除：使用泛型的时候加上的类型参数，会被编译器在编译的时候去掉。这个过程就称为类型擦除。类型擦除的过程，首先是找到用来替换类型参数的具体类。这个具体类一般是Object。如果指定了类型参数的上界的话，则使用这个上界。把代码中的类型参数都替换成具体的类。同时去掉出现的类型声明，即去掉<>的内容。比如: T get()方法声明就变成了Object get()； List<String>就变成了List。 接下来就可能需要生成一些桥接方法（bridge method）。这是由于擦除了类型之后的类可能缺少某些必须的方法
```
