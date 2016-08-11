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
//1) Stream是元素的集合，类似Iterator，可顺序/并行对原Stream进行过滤汇总
//2) 实现Stream接口 
//3) 创建Stream(Collection已实现接口)-->转换Stream，不影响源,返回新的Stream对象，可多次-->对Stream进行操作，得到想要的结果
//4） Steam的归并操作，allMatch， anyMatch, nonMatch, max/min等
list.stream().filter(num->{return num>0;}).collect(Collectors.toList());   //示例，过滤list中大于0值，转成list返回
list.stream().distinct().count()  //distinct, 去重，需重写equals(和hashCode)方法 ， 统计个数


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
