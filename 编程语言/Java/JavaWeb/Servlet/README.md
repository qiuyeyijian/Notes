## Servlet 笔记

## 概念

Servlet：运行在服务端的小程序。本质是一个接口，定义了Java类被浏览器访问到（Tomcat识别）的规则。

将来我们自定义一个类，实现Servlet接口，复写方法。

### 创建一个Servlet项目

`启动IDEA -> New Project -> Java Enterprise`

1. 创建一个JavaEE项目
2. 定义一个类，实现Servlet接口
3. 实现接口中的抽象方法
4. 配置Servlet，在Web.xml中配置

```xml
<servlet>
    <servlet-name>demo01</servlet-name>
    <servlet-class>com.qiuyeyijian.Demo01.Main</servlet-class>
</servlet>

<servlet-mapping>
    <servlet-name>demo01</servlet-name>
    <url-pattern>/demo01</url-pattern>
</servlet-mapping>
```





### 执行原理

1. 当服务器接受到客户端浏览器请求后，会解析请求URL路径，获取访问的Servlet的资源路径
2. 查找web.xml文件，是否有对应的`<url-pattern>`标签体内容
3. tomcat会将字节码文件加载进内存，并且创建其对象（反射）
4. 调用其方法



## Servlet体系结构

### Servlet的体系结构

Servlet -> GenericServlet -> HttpServlet

接口			抽象类						抽象类

GenericServlet：将Servlet接口中其他的方法做了默认空实现，只将service()方法作为抽象。将来定义Servlet类时，可以继承GenericServlet，实现service()方法即可

HttpServlet：对http协议的一种封装，简化操作
  		1. 定义类继承HttpServlet
  		2. 复写doGet/doPost方法

### Servlet相关配置

 一个Servlet可以定义多个访问路径 ： @WebServlet({"/d4","/dd4","/ddd4"})
路径定义规则：

>    			1. /xxx：路径匹配
>   			2. /xxx/xxx:多层路径，目录结构
>  			3. *.do：扩展名匹配

```java
@WebServlet("/demo01")
public class ServletDemo01 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("doGet...");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("doPost...");
    }
}
```





## Filter过滤器

接下来实现一个过滤器，请求和响应的字符全部转为`UTF-8`编码

### 创建一个过滤器

```java
@WebFilter("/*")
public class CharacterEncodingFilter extends HttpFilter {
    @Override
    public void init() throws ServletException {
        System.out.println("过滤器初始化");
    }

    @Override
    protected void doFilter(HttpServletRequest req, HttpServletResponse res, FilterChain chain) throws IOException, ServletException {
        req.setCharacterEncoding("utf-8");
        res.setCharacterEncoding("utf-8");
        res.setContentType("text/html;charset=UTF-8");

        System.out.println("过滤执行前");
        // 过滤器操作完成，放行请求和响应
        chain.doFilter(req, res);
        System.out.println("过滤执行后");

    }

    @Override
    public void destroy() {
        System.out.println("过滤器销毁");
    }
}

```





## Listener监听器

























### 允许跨域请求

```java
/* 允许跨域的主机地址 */
response.setHeader("Access-Control-Allow-Origin", "*");  
/* 允许跨域的请求方法GET, POST, HEAD 等 */
response.setHeader("Access-Control-Allow-Methods", "*");  
/* 重新预检验跨域的缓存时间 (s) */
response.setHeader("Access-Control-Max-Age", "3600");  
/* 允许跨域的请求头 */
response.setHeader("Access-Control-Allow-Headers", "*");  
/* 是否携带cookie */
response.setHeader("Access-Control-Allow-Credentials", "true");  
```

