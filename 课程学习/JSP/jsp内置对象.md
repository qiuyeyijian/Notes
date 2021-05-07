## JSP内置对象

 内置对象是在JSP页面中无需创建就可以直接使用的变量。在jsp中一共有9个这样的对象，他们分别是：

* out	(JspWriter)
* config   (ServeletConfig)
* page    (当前JSP的真身类型)
* **pageContext**    (PageContext)
* exception    (Throwable)
* **request**    (HttpServletRequest)
* **response**    (HttpServletResponse)
* **application**    (ServletContext)
* **Session**    (HttpSession)



### jsp 四个域对象



* pageContext 范围， 当前页面内有效
* request 范围， 当前的请求内有效
* session 范围，当前会话内有效
* application 范围，当前这次服务器生命周期内有效



域对象的共同特点是管理域中的属性，他们都有相同的方法：

* void setAttribute(String name, Object value);

* Object getAttribute(String name, object value);

* void removeAttribute(String name, Object value);

  

 