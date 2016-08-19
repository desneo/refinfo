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




**groovy**  
```
//运行groovy脚本
    String[] roots = new String[]{"files/"};    //指定groovy脚本加载目录
    GroovyScriptEngine groovyScriptEngine = new GroovyScriptEngine(roots); //groovy引擎
    Class scriptClass = groovyScriptEngine.loadScriptByName("exp.groovy");  //加載腳本
    Binding binding = newBinding();   //脚本中变量入参
    binding.setVariable("name", "zhousahjkshdkajs");  //设置变量值
    Object output = groovyScriptEngine.run("hello.groovy", binding);
//hello.groovy
return "in param name is ${name}"

//each --> ["Cat", "Dog", "Elephant"].each{yy-> println yy}  注：默认提供一个it变量
      array.add(obj); 
```
