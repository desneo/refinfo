
# 5. [web.xml详解](http://my.oschina.net/u/1383439/blog/224448)

# 49. Servlet
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

**Servlet下载文件**  
```java
//Servlet提供一个相应类型(Content-Disposition)，并将文件流写入输出流
protected void doGet(HttpServletRequest request, HttpServletResponse response) 
{
final int BYTES = 1024;
int length = 0;
ServletOutputStream outStream = response.getOutputStream();
ServletContext context = getServletConfig().getServletContext();

response.setContentType("text/plain");
String fileToDownload = "resources/files/dtfile.xds";
response.setHeader("Content-Disposition", "attachment; filename=" + fileToDownload.split("/")[2]);

InputStream in = context.getResourceAsStream("/" + fileToDownload);
byte[] bbuf = new byte[BYTES];
while ((in != null) && ((length = in.read(bbuf)) != -1))
{
    outStream.write(bbuf, 0, length);
}
outStream.flush();
outStream.close();
}
```

**上传文件**
```javascript
//参考地址: http://www.jianshu.com/p/46e6e03a0d53
//页面,index.html
<input id="upload" type="button" value="上传" />
<script>
	$("#upload").click(function(){
		debugger;
		var formData = new FormData();
		formData.append('file', $('#file')[0].files[0]);
		$.ajax({
			url : '/servletexample/upload',
			type : 'POST',
			cache : false,
			data : formData,
			processData : false,
			contentType : false
		}).done(function(res) {
			alert("上传成功");
		}).fail(function(res) {
		});
	});
</script>
```
```java
//后台， Part表示一个上传文件，part.write()会将上传的文件写入到临时目录apache-tomcat-8.0.36\work\Catalina\localhost\servletexample
//上传的文件需要自己复制保存到指定位置，临时文件会被清空
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException
{
	Collection<Part> parts = request.getParts();
	System.out.println("parts num:" + parts.size());
	Iterator<Part> temp = parts.iterator();
	while (temp.hasNext())
	{
	    Part part = temp.next();
	    System.out.println(part.getSubmittedFileName());
	    part.write(part.getSubmittedFileName());
	}
}

//web.xml , multipart-config必须配置在上传的servlet中
<servlet>
	<servlet-name>uploadfileServlet</servlet-name>
	<servlet-class>com.servlet.test.UploadFileServlet</servlet-class>
	<!-- 文件上传的配置 -->
	<multipart-config>
		<max-file-size>20848820</max-file-size>
		<max-request-size>418018841</max-request-size>
		<file-size-threshold>1048576</file-size-threshold>
	</multipart-config>
</servlet>
<servlet-mapping>
	<servlet-name>uploadfileServlet</servlet-name>
	<url-pattern>/upload</url-pattern>
</servlet-mapping>
```

**其它**
```java
//forward转发请求到另一个Servlet
RequestDispatcher rd = servletContext.getRequestDispatcher("/NextServlet");
rd.forward(request, response);

//重定向请求到另一个Servlet
httpServletResponse.sendRedirect("/anotherURL");

//Servlet读写
CookieCookie cookie = new Cookie("sessionId","123456789");
cookie.setHttpOnly(true);
cookie.setMaxAge(-30);
response.addCookie(cookie);	//写cookie
Cookie[] cookies = request.getCookies();  //读cookie
```

**session**  
```java
//如果没有则创建session, 需要在向客户端发送任何文档内容之前调用 
HttpSession session = request.getSession();
setAttribute(String name, Object value) //使用指定的名称绑定一个对象到该 session 会话
getAttribute(String name)	//返回在该 session 会话中具有指定名称的对象，如果没有指定名称的对象，则返回 null
removeAttribute(String name)	//从该 session 会话移除指定名称的对象。
getAttributeNames()	//返回 String 对象的枚举，String 对象包含所有绑定到该 session 会话的对象的名称。
long getCreationTime()	//该 session 会话被创建的时间
String getId()	//返回一个包含分配给该 session 会话的唯一标识符的字符串。
long getLastAccessedTime()	//最后一次发送与该 session 会话相关的请求的时间
void invalidate()	//指示该 session 会话无效，并解除绑定到它上面的任何对象。
boolean isNew()	//
int getMaxInactiveInterval()	//返回Servlet容器在客户端访问时保持session会话打开的最大时间间隔，单位分钟。
   //在web.xml中配置session超时时间, 将覆盖 Tomcat 中默认的 30 分钟超时时间。
  <session-config>
    <session-timeout>15</session-timeout>
  </session-config>
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
