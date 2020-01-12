## 原生servlet的获取
```java
@RequestMapping("/testServlet")
public String getServlet(HttpServletRequest request, HttpServletResponse response){
    System.out.println(request);
    System.out.println(response);
    HttpSession session = request.getSession();
    System.out.println(session);
    return null;
}
```
如果没有servlet的提示，是因为没有导入servlet包，tomcat是有的，所以项目能正常运行，但是maven中没有，需要导入依赖，且只能在编译测试阶段起作用，不能在运行时生效
```xml
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>3.0.1</version>
    <scope>provided</scope>
</dependency>
```

## SpringMVC的方式
原生获取方式高度与jar包耦合，不加入依赖都报错，需要使用SpringMVC的model

开启jsp中EL表达式的支持`isELIgnored="false"`
```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" isELIgnored="false" %>
```
