# 1.单例模式-Singleton
**描述**  
```
有些对象只需要一个，如:配置文件、线程池、缓存、日志对象等。实例过多导致结果不一致或是资源占用。
    1.将构造方法私有化，不允许外部直接创建对象，private
    2.创建类的唯一实例,使用private static修 
    3.提供一个获取唯一实例的方法，使用public static修饰
```
**饿汉模式-加载时慢-获取快-线程安全**  
```java
public class Singleton1
{
    // 1.将构造方法私有化，不允许外部直接创建对象
    private Singleton1(){}
	//2.创建类的唯一实例,使用private static修饰
    private static Singleton1 instance = new Singleton1();
        //3.提供一个获取唯一实例的方法，使用public static修饰
    public static Singleton1 getInstance(){
        return instance;
    }
}
```
**懒汉模式-非线程安全-加载快-第一次获取慢**
```java
private static Singleton2 instance ;
//3.提供一个获取唯一实例的方法，使用public static修饰
public static Singleton2 getInstance(){
	if(null == instance){
		instance = new Singleton2();
	}
	return instance;
}
```
