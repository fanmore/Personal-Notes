## 静态资源访问
由于设置了拦截，所有请求都会被SpringMVC处理，那么同样，js等静态资源也无法正常访问，所以需要配置静态资源访问不拦截

在springmvc.xml中配置
```xml
<!--  前端控制器，静态资源不拦截  -->
<mvc:resources mapping="/js/**" location="/js/"/>
<mvc:resources mapping="/images/**" location="/images/"/>
<mvc:resources mapping="/css/**" location="/css/"/>
```
注意如果还不生效需要重启一下
