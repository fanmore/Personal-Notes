## 什么是MyBatis
MyBatis可以简化JDBC操作，实现数据的持久化

## 配置
下载：[MyBatis](http://www.mybatis.org/mybatis-3/zh/getting-started.html)

在入门的第一行可进入Github下载

简单项目加入核心包即可

## 使用步骤
1. 导入数据库连接包、MyBatis核心包并导入类路径
2. 准备表对应的实体类，创建set、get及构造有参、无参方法
3. 创建XxxMapper.xml映射文件
   模板（可写操作这张表的所有SQL语句）
   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
     PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
     "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="映射全限定路径名">
     <!-- id是唯一标识符 -->
     <!-- parameterType输入类型，跟#{}类型一致 -->
     <!-- resultType返回类型，自定义实体类是全限定路径 -->
     <select id="selectBlog" parameterType="int" resultType="Blog">
       select * from Blog where id = #{id}
     </select>
   </mapper>
   ```
   **注意**：模板文件和配置文件都在下载的MyBatis文件中的PDF文件中有

4. MyBatis配置
   在src下创建config.xml
   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
    PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
      <properties resource="db.properties"></properties>
      <environments default="development">
         <environment id="development">
            <transactionManager type="JDBC" />
            <dataSource type="POOLED">
               <!-- 配置数据库信息 -->
               <property name="driver" value="${driver}" />
               <property name="url" value="${url}" />
               <property name="username" value="${username}" />
               <property name="password" value="${password}" />
            </dataSource>
         </environment>
      </environments>
      <mappers>
         <!-- 加载映射文件，映射Mapper文件路径名，注意/ -->
         <mapper resource="demo/UserMapper.xml" />
      </mappers>
   </configuration>
   ```
   db.properties
   ```xml
   driver = com.mysql.cj.jdbc.Driver
   url = jdbc:mysql://localhost:3306/blog?serverTimezone=UTC
   username = root
   password = fanjun
   ```
   
5. 测试
   ```java
   import java.io.IOException;
   import java.io.Reader;

   import org.apache.ibatis.io.Resources;
   import org.apache.ibatis.session.SqlSession;
   import org.apache.ibatis.session.SqlSessionFactory;
   import org.apache.ibatis.session.SqlSessionFactoryBuilder;

   public class Test {

      public static void main(String[] args) {
         //加载MyBatis配置文件
         try {
            Reader reader = Resources.getResourceAsReader("config.xml");
            SqlSessionFactory sessionFactory = new SqlSessionFactoryBuilder().build(reader);
            SqlSession session = sessionFactory.openSession();
            //操作数据库
            //sql：namespace.id
            String sql = "demo.UserMapper.selectUserById";
            //虽然selectOne返回Object类型，但在Mapper中申明了返回类型
            User user = session.selectOne(sql,1);
            System.out.println(user.getUid());
            System.out.println(user.getUname());
            System.out.println(user.getUpass());
            System.out.println(user.getUnname());
         } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
         }
      }
   }

   ```
