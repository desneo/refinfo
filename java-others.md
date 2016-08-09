



# 10.Java字符串
**StringBuilder和StringBuffer**
```
一般在单线程的系统环境下优先使用StringBuilder，因为它是非线程安全的。
而在多线程的环境下，比如Web系统，优先使用StringBuffer，因为它当中绝大部分方法都使用了synchronized进行修饰，保证了多线程环境下的线程安全问题。
```



# 17.Java反射-reflect




