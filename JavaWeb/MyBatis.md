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
   
5. 基础的增删改查（CRUD）

   Mapper文件
   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
      PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
      "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
      <!-- namespace:映射文件类路径名 -->
   <mapper namespace="demo.UserMapper">
      <!-- id是唯一标识符 -->
      <!-- parameterType输入类型，跟#{}类型一致 -->
      <!-- resultType返回类型，自定义实体类是全限定路径 -->
      <select id="selectUserById" parameterType="int" resultType="demo.User">
         select * from user where uid = #{uid}
      </select>
      <!-- MyBatis在语法上只允许一个输入值一个输出值 -->
      <!-- 简单类型输入#{xxx}内可以随意写，但对象时属性名字必须和实体类一一对应，建议都对应 -->
      <insert id="insertUser" parameterType="demo.User">
         insert into user(uname,upass,unname) values(#{uname},#{upass},#{unname})
      </insert>
      <update id="updateUserById" parameterType="demo.User">
         update user set uname = #{uname}, upass = #{upass}, unname = #{unname} where uid = #{uid}
      </update>
      <delete id="deleteUserById" parameterType="int">
         delete from user where uid = #{uid}
      </delete>
      <!-- MyBatis约定，返回值是一个还是多个都一样，按一个处理 -->
      <select id="selectAllUser" resultType="demo.User">
         select * from user
      </select>
   </mapper>
   ```
   测试文件
   ```java
   import java.io.IOException;
   import java.io.Reader;
   import java.util.List;

   import org.apache.ibatis.io.Resources;
   import org.apache.ibatis.session.SqlSession;
   import org.apache.ibatis.session.SqlSessionFactory;
   import org.apache.ibatis.session.SqlSessionFactoryBuilder;

   public class Test {

      //用于连接，方便关闭
      public static SqlSession session = null;

      //加载MyBatis配置文件
      public SqlSession connect() {
         Reader reader;

         try {
            reader = Resources.getResourceAsReader("config.xml");
            SqlSessionFactory sessionFactory = new SqlSessionFactoryBuilder().build(reader);
            session = sessionFactory.openSession();
         } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
         }
         return session;
      }

      //查询全部
      public void selectAllUser() {
         SqlSession session = connect();
         String sql = "demo.UserMapper.selectAllUser";
         List<User> users = session.selectList(sql);
         for(User user : users) {
            System.out.println(user.getUid());
            System.out.println(user.getUname());
            System.out.println(user.getUpass());
            System.out.println(user.getUnname());
         }
         if (session!=null) {
            session.close();
         }
      }

      //通过ID查询
      public void selectUserById() {
         SqlSession session = connect();
         String sql = "demo.UserMapper.selectUserById";
         User user = session.selectOne(sql,1);
         System.out.println(user.getUid());
         System.out.println(user.getUname());
         System.out.println(user.getUpass());
         System.out.println(user.getUnname());
         if (session!=null) {
            session.close();
         }
      }

      //增加
      public void insertUser() {
         SqlSession session = connect();
         String sql = "demo.UserMapper.insertUser";
         User user = new User("fan", "123", "阿达");
         int result = session.insert(sql, user);
         if (result>0) {
            System.out.println("增加"+result+"条数据成功");
         }
         //注意事务方式为JDBC时要手动提交
         session.commit();
         if (session!=null) {
            session.close();
         }
      }

      //修改
      public void updateUserById() {
         SqlSession session = connect();
         String sql = "demo.UserMapper.updateUserById";
         User user = new User(4,"Lin", "456", "黑盒");
         int resu = session.update(sql,user);
         if (resu>0) {
            System.out.println("修改"+resu+"条数据成功");
         }
         session.commit();
         if (session!=null) {
            session.close();
         }
      }

      //通过ID删除
      public void deleteUserById() {
         SqlSession session = connect();
         String sql = "demo.UserMapper.deleteUserById";
         int resu = session.delete(sql,4);
         if (resu>0) {
            System.out.println("删除"+resu+"条数据成功");
         }else {
            System.out.println("删除失败！");
         }
         session.commit();
         if (session!=null) {
            session.close();
         }
      }
   }
   ```

## 官方推荐使用方式
Mapper动态代理的CRUD（MyBatis接口开发）

原则：约定优于配置

具体的实现步骤
1. 基础环境：JDBC包和MyBatis核心包、config配置文件、Mapper文件
2. 与基础方式不同的是，根据约定直接定位SQL
   * 创建接口，接口的全类名和namespace相同，即名字一样，其他约定如下
      * 通常接口文件和Mapper文件在一起，放于mapper包下（注意config中的mapper文件和mapper中的映射路径）
      * 方法名和ID值相同
      * 输入参数和parameterType一致（变量名通常也一致）（没有不写）
      * 返回值和resultType一致，没有是void，注意全部用List
      执行方法，替换SQL语句的位置
      ```java
      UserMapper user1 = session.getMapper(UserMapper.class);
      User user = user1.selectUserById(1);
      ```
      
## 别名设置
1. 单个设置
   在config.xml中configuration和environments标签之间
   ```xml
   <typeAliases>
		<typeAlias type="demo.User" alias="User" />
	</typeAliases>
   ```
   定义别名后不区分大小写
2. 批量设置
   同样位置
   ```xml
   <typeAliases>
		<package name="demo"/>
	</typeAliases>
   ```
   将整个包都别名，别名为类名，同样不区分大小写

## MyBatis整合Log4J日志

加入包，MyBatis中有

开启日志
* 在config中，properties标签下
   ```xml
   <settings>
      <setting name="logImpl" value="LOG4J"/>
   </settings>
   ```
