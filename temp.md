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



**threadLocal**  
```java
1、用于线程集的全局变量(当前线程共享)，  private static ThreadLocal<Integer> seqNum = new ThreadLocal<Integer>() 
    seqNum.set(1); seqNum.get(); 
2、单的static是所有线程共享的全局变量

```
