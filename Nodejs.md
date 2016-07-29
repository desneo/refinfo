# 1.基础
**原理**  
```javascript
1) 代码单线，I/O并行 -->JS执行在单线程，但内部I/O另有另有线程池!
2) 借助libuv实现跨平台。
3) 适用—I/O密集型,cpu密集型可借助C扩展模块提高速度！
4) CommonJS—JS规范
5) 引用模块，var math = require("math");
```
**优势/缺点**  
```
优势:
	1、无需同步，无死锁，无线程上下文切换的开销！
弱点：
	1、无法利用多核CPU
	2、错误引起整个系统退出。
	3、大量计算占用CPU时，无法继续调用异步I/O
```

**对es6的支持情况(浏览器)**
```javascript
npm install –g es-checker
es-checker
//浏览器访问，http://ruanyf.github.io/es-checker/ 进行检测
```
**[升级node](http://www.jb51.net/article/52409.htm)**  
```javascript
node install –g n
n stable	//升级到stable版本
n 6.00	//升级到指定版本
```
**node常用指令**
```javascript
npm -v          #显示版本，检查npm 是否正确安装。
npm install -g express  #全局安装express模块
npm list         #列出已安装模块
npm show express     #显示模块详情
npm update        #升级当前目录下的项目的所有模块
npm update -g express  #升级全局安装的express模块
npm uninstall express  #删除指定的模块
```
 
# 2.文件系统
```javascript
require('fs')
const fs = require('fs');
```
**fs.readdir(path, callback) 读取文件夹**  
```javascript
//对应的同步方法fs.readdirSync(path) , 返回字符串数组文件名列表 (不包含"."和"..")
var fs=require("fs");  
var files = fs.readdirSync("E:\\project\\AS-BES\\checkout");
console.log(files);
```

**fs.readFile(file[, options], callback)  读取文件**  
```javascript
//对应的同步方法,  fs.readFileSync(file[, options])
fs.readFile('/etc/passwd', 'utf8', callback);	//如果指定了option则返回一个字符串，否则返回raw buffer
fs.readFile(fullPath,'utf-8', function (err, data) {
	  fs.appendFileSync('C:\\Users\\z00316474\\Desktop\\fanyi.txt', data);
});
```

**fs.writeFile(file, data[, options], callback) 写入文件**  
```javascript
//对应的同步方法,  fs.writeFileSync(file, data[, options]) , 如果文件已存在，则替换文件（即覆盖）,data可string / buffer
fs.writeFile('message.txt', 'Hello Node.js', (err) => {
  if (err) throw err;
  console.log('It\'s saved!');
});
```

**fs.appendFile(file, data[, options], callback) 追加写入**  
```javascript
//对应的同步方法, fs.appendFileSync(file, data[, options]), 文件不存在则创建，data可string / buffer
	fs.appendFileSync('C:\\Users\\z00316474\\Desktop\\fanyi.txt', data);
```


# 3.模块
**引用模块**  
```
var math = require("math");
```
**模块定义-exports**
```javascript
//node中一个文件就是一个模块，模块中存在一个module对象—代表模块自身，而exports对象是module的属性。将方法挂在exports上即可导出。
//math.js
exports.add = function(){
		var sum = 0;
		var argus = arguments;
		var len = arguments.length;
		var i = 0;
		while(i<len){
			sum += argus[i++];
		}
		return sum;
	}
//test.js
var math = require("./math.js");
console.log(math.add(1, 2, 3));

```
