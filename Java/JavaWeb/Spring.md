## 环境下载

[Maven下载地址](https://maven.springframework.org/release/org/springframework/spring/)

[Maven库](https://mvnrepository.com)

## Eclipse 的 spring 插件

[下载地址](https://spring.io/tools3/sts/all)

不用解压，在 Eclipse 中的 Help 中打开 intall 添加即可

**注意** : 也可以直接在网址中下载集成的 Eclipse 文件，打开即可使用

## 基础项目

开发 spring 至少需要 5+1 个jar

* aop（开发AOP特性需要的 jar）
* beans（处理 Bean 的 jar）
* context（处理 spring 上下文的 jar）
* core（核心 jar）
* expression（表达式 jar）
* commons-logging（日志，第三方，在 Maven 库中搜索下载）

加入类路径，创建配置文件 applicationContext.xml (用 spring bean config... 创建可以有头文件)

## IOC

优势：传统的项目中每用一个对象需要 new 一次，耦合性太高，new 的位置太分散，不便于维护。可以优化就是简单工厂模式，统一 new 对象。但是自己写的工厂将工序增加，同时问题不彻底，而 SpringIOC 就是一个超级工厂，任何类型都可以放，同时将获取对象的方式变成直接从容器中拿，而不是自己创建。因此 IOC 叫控制反转，IOC 在一次大会上为了方便理解 IOC 将名字改为 DI（依赖注入）。DI 就是从属性的角度理解，将值注入给属性，将属性注入给对象。所以 IOC 和 DI 是一个东西，只是从不同的角度理解。

总结：IOC / DI ，就是无论什么对象，都可以直接从 SpringIOC 容器中获取，而不是自己创建。

IOC 分2步，第一步给 SpringIOC 中存放对象并赋值，第二步拿。

## DI

IOC 容器赋值时，简单类型（8个基本 + String）赋值用 value，如果是对象类型，用 ref ，ref = "引用对象的 id 值" ，因此实现了对象与对象之间的依赖关系。

**注意** ：依赖注入底层是使用反射实现的。

注入方式：set 方式、构造器方式、P命名空间

set方法：
```xml
<!-- id 唯一标识符 class 指定类型，下面这句等同于 User user = new User(),加载时自动 new 一个对象 -->
<bean id="user" class="demo01.User">
	<!-- 通过 set 方法给对象赋值 -->
	<property name="name" value="李四"></property>
	<property name="age" value="21"></property>
</bean>
<!-- IOC 容器赋值时，简单类型（8个基本 + String）赋值用 value，如果是对象类型，用 ref ，ref = "引用对象的 id 值" -->
```
构造器方法：
```xml
<bean id="user" class="demo01.User">
	<!-- 通过 构造器 方法给对象赋值  -->
	<constructor-arg value="wangwu" ></constructor-arg>
	<constructor-arg value="23"></constructor-arg>
	<!-- 同样，简单类型（8个基本 + String）赋值用 value，如果是对象类型，用 ref ，ref = "引用对象的 id 值"  -->
	<!-- 不指定需要和构造器顺序一致，可以指定 name index type 可指定一个也可以多个 -->
</bean>
```

P命名空间：
需要加入 P 的支持
```xml
xmlns:p="http://www.springframework.org/schema/p"
<!-- 同样需要注意 ref ， 注意留空格 -->
```

**注意** ：三种方式在注入时都依赖无参构造

注入各种数据类型

* list
   ```xml
   <bean id="" class="">
	<property name="">
		<list>
			<value></value>
		</list>
	</property>
   </bean>
   ```
* 其余大同小异

**注意** ： value 可以用 <value> 这样不用引号
	
## 注解实现 IOC

1. @Component("name")
2. 扫描器
   ```xml
   <context:component-scan base-package="demo01"></context:component-scan>
	<!-- 多个包用 , 隔开 -->
   ```

   




