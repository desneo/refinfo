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

//定义函数
def  nonReturnTypeFunc(){  //函数返回值类型可指定类型，如String，如果不return string则报错
     [return] last_line;   //return可选， 最后一行代码的执行结果就是本函数的返回值
}


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

//定义闭包
def sayhello = {
  name ->   //定义入参
    println("name="+name);
}
```

**内置的集合操作**
```
each  //遍历集合，对每一项处理函数
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
	import com.test.SomeScript  	//引入script 
	def s = new SomeScript() 	//实例化脚本
	s.a = 2 	//操作元素
	s.run() 	//运行脚本
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
