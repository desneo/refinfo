


# 49.Servlet
```
0、servlet生命周期： tomcat等容器将请求分解成 HttpServletRequest/HttpServletResponse --> 调用servlet.init()
    -->servlet.service() 调用doGet/doPost处理 --> servlet.destroy() 一般只需重写doGet/doPost即可
1、Servlet 是一些遵从Java Servlet API的Java类，这些Java类可以响应web请求。
2、 Servlet必须部署在Java servlet容器才能使用。虽然很多开发者都使用Java Server Pages（JSP）和Java Server   
    Faces（JSF）等Servlet框架，但是这些技术都要在幕后通过Servlet容器把页面编译为Java Servlet。
```

**servlet Filter**  
```java
//1) 对被访问的URL进行预处理，如记录日志、验证等公共逻辑
//2) 过滤器链：若存在多个匹配给定URL模式的个过滤器，它们就会根据web.xml里的配置顺序被调用。
//3) 包含相同URL模式的过滤器（filter）会在Servlet调用前被调用
//4) 过滤器需实现javax.servlet.Filter, init()/destroy()被容器调用。 doFilter()实现处理逻辑，
     调用chain.doFilter(request, response)往下执行
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
    {
        System.out.println(request.getProtocol());
        chain.doFilter(request, response);
    }
//5) WEB-INF/web.xml中配置servlet ， 详见示例1
```

**示例1**  
```
//1) 必须继承HttpServlet，要么继承GenericServlet的普通Servle， 要么HttpServlet 的HTTP Servlet。
//2) 重写doGet() 和 doPost() 方法,如果你向这个servlet发送一个HTTP GET请求，doGet()方法就会被调用。其它方法一般不需重写。
//3) 获取参数: String value1 = req.getParameter("param1");
//4) 为了发送内容给客户端，从HttpServletResponse获取PrintWriter,任何写到这个对象的内容都会被写进outputstream里发到client。
public class FirstServlet extends HttpServlet
{
    private static final long serialVersionUID = 1L;
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException
    {
        response.setContentType("text/html;charset=UTF-8");
        PrintWriter out = response.getWriter();
        try
        {
            // Write some content
            out.println("<html>");
            out.println("<head>");
            out.println("<title>MyFirstServlet</title>");
            out.println("</head>");
            out.println("<body>");
            out.println("<h2>Servlet MyFirstServlet at " + request.getContextPath() + "</h2>");
            out.println("</body>");
            out.println("</html>");
        }
        finally
        {
            out.close();
        }
    }
}

//WEB-INF/web.xml中配置servlet
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" id="WebApp_ID" version="3.1">
   <servlet>
    <servlet-name>MyFirstServlet</servlet-name>
    <servlet-class>com.servlet.test.FirstServlet</servlet-class>
  </servlet>
  <servlet-mapping>
    <servlet-name>MyFirstServlet</servlet-name>
    <url-pattern>/MyFirstServlet</url-pattern>
  </servlet-mapping>
  
  <filter>
	<filter-name>LoggingFilter</filter-name>
    	<filter-class>com.servlet.test.FirstFilter</filter-class>
	</filter>
  <filter-mapping>
	<filter-name>LoggingFilter</filter-name>
	<url-pattern>/*</url-pattern>
  </filter-mapping>
</web-app>
```


# 50. 其它
# 50.1 Tomcat
**配置**  
```
1、端口号: conf/server.xml--> <Connectors -->
2、根目录ROOT，将代码部署在根目录访问时不用加项目名称，即127.0.0.1:8080默认打开的是webapps/ROOT中页面
```

**eclipse中发布项目**  
```
1、tomcat/bin/startup.bat启动可以打开主页127:0.0.1:8080, 
    eclipse中直接添加启动tomcat则不能打开主页，因为要配置tomcat的发布目录
2、window-->showview中添加server-->add a server
   双击server中tomcat-->配置页面-->server Locations选择中间一个(use Tomcat installation),deploy path改成webapps, 超时时间500s
   (如果无法修改，则删除tomcat所有项目，右键clean后打开)
3、新建web项目(Dynamic web project),其中会有src源码目录和资源目录,勾选新建web.xml, tomcat-->add and remove添加即可
4、start server,即可发现项目已部署到 G:\program-my\apache-tomcat-8.0.36\webapps\tt 中, console日志也会有项目发布目录

```
