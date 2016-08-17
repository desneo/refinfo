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



# 60. Groovy
**基础**  
```groovy
//定义变量
xx = "asdasd"  //全局变量
def str= "i am a person"  //类型不定
String str = "12312"    //若指定类型，则变量类型不可修改

//定义函数
def  nonReturnTypeFunc(){  //函数返回值类型可指定类型，如String，如果不return string则报错
     [return] last_line;   //return可选， 最后一行代码的执行结果就是本函数的返回值
}

//字符串求值
def x = 1
def doubleQuoteWithDollar = "I am $x dolloar" //输出I am 1 dolloar

//List
def aList = [5,'string',true] //List由[]定义，其元素可以是任何对象
        //可以直接通过索引存取，而且不用担心索引越界。如果索引超过当前链表长度，List会自动往该索引添加元素
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
