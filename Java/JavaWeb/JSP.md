## Tomcat的下载

进入Apache官网下载：[http://apache.org](http://apache.org)

点击Projects进入列表找到Tomcat

程序开发通常不会选择最新版本的软件，而是选择退一个版本比较稳定

**注意** Tomcat的环境依赖 Jdk 配置 JAVA_HOME

## JSP三种脚本样式

1. java代码

   ```jsp
   <%
   	java代码
   %>
   ```

2. 输出

   ```jsp
   <%=
       输出变量
   %>
   ```

3. 全局变量

   ```jsp
   <%!
       定义全局变量
   %>
   ```

## JSP指令

page指令的属性

* language：JSP页面使用的脚本语言

* Import：导入类

* pageEncoding：JSP文件自身编码

* contentType：浏览器解析JSP的编码

## JSP内置对象

有9个，不用new即可使用

### out

输出对象

```jsp
<%
	out.print("Hello")
%>
```

### request

请求对象

常见方法

* 根据请求的字段名，返回字段值

  ```java
  String name = request.getParameter("uname");
  //返回是String类型，数值型需要转
  int age = Integer.parseInt(request.getParameter("age"));
  //返回多个值，如checkbox按钮
  String[] hobbies = request.getParameterValues("name");
  ```

* 请求编码

  ```java
  request.setCharacterEncoding("UTF-8");
  ```

* 请求转发

  ```java
  request.getRequestDispatcher("index.jsp").forward(request, response);
  ```

### response

相应对象

常见方法

* 向客户端增加Cookie对象

  ```java
  Cookie cookie = new Cookie(name, value);
  response.addCookie(cookie);
  ```

* 重定向

  ```java
  response.sendRedirect("index.jsp");
  ```

* 响应编码

  ```java
  response.setCharacterEncoding("UTF-8");
  response.setContentType("text/html; charset=UTF-8");
  ```

**注意**：请求转发和重定向的区别

请求转发地址栏不变、数据保留、一次请求

重定向地址栏会变、数据不保留、两次请求

### session

服务端

Cookie：客户端，不是内置对象，由服务端生成，再发给客户端

* 作用：提高访问服务端的效率，但是安全性较差

* 不是内置对象，由javax.servlet.http.Cookie生成

* 方法
  * public Cookie(String name,String value)
  * String getName()
  * String getValue()
  * void setMaxAge(int expiry)：最大有效期（秒）

* 服务端准备Cookie

  response.addCookie(Cookie cookie)

  页面跳转（转发、重定向）

* 客户端获取Cooke

  request.getCookies()

  只能拿到全部的Cookie

  ```java
  //准备Cookie
  String name = request.getParameter("uname")
  Cookie cookie = new Cookie("name",name);
  cookie.setMaxAge(60*60*24);
  
  response.addCookie(cookie);
  response.sendRedirect("index.jsp");
  
  //获取Cookie
  <%! String uname; %>
  <% 
      Cookie[] cookies = request.getCookies();
  	for(Cookie cookie:cookies){
    	if(cookie.getName().equals("name")){
              uname = cookie.getValue();
          }
              out.print(cookie.getName()+"--"+cookie.getValue());
  	}
  %>
  //在输入框中输入
  <form>
  	<inpute type="text" name="uname" value="<%=(uname?"":uname)%>">
  </form>
  ```

* **注意**：Cookie不是内置对象，但是服务端会自动生成一个name=JSESSIONID的cookie，并返回客户端

session机制

客户端第一次请求服务端时，服务端会产生一个session对象（用于保存该客户的信息），并且每个session对象都会有一个唯一的sessionid（用于区分其他session），服务端又会产生一个cookie，并且该cookie的name=jsessionid，value=服务端sessionid的值，然会服务端会在响应客户端的同时将该cookie发送给客户端，这样客户端就有了一个cookie，因此，客户端的cookie就可以和服务端的session一一对应

session方法

* String getId()：获取sessionId
* boolean isNew()：判断是否是新用户
* void invalidate()：使session失效（退出、注销）
* void setAttribute()
* Object getAttribute()
* void setMaxInactiveInterval()：设置最大有效非活动时间
* int getMaxInactiveInterval()：获取最大有效非活动时间

## 四种范围对象

pageContext	JSP页面容器

request	请求对象

session	会话对象

appliation	全局对象

共有的方法

* Object getAttribute(String name)	获取对象
* void setAttribute(String name,Object obj)    创建对象
* k-v对

## EL
```
${requestScope.student}
${域对象.域对象中的属性.属性.属性}
${[''][""]}
```
EL操作符：
* 点操作符 .  使用方便
* 中括号操作符 [] 中间有引号，单双都可以  功能强大，可以特殊字符，获取变量 [name]

EL表达式的隐士对象
* 作用域对象
   pageScope  requestScope  sessionScope  applicationScope
* 参数访问对象
   获取表单 request.getParameter (${param})     request.getParameterValues (${paramValues})    
   


