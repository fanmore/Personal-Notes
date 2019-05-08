## 什么是MyBatis
MyBatis可以简化JDBC操作，实现数据的持久化

## 配置
下载：[MyBatis](http://www.mybatis.org/mybatis-3/zh/getting-started.html)

在入门的第一行可进入Github下载

简单项目加入核心包即可

## 使用步骤
1. 导入数据库连接包、MyBatis核心包
2. 准备表对应的实体类
3. 创建XxxMapper.xml映射文件
   模板
   ```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
	PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
	"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="实体类全限定路径名">
	<!-- id是唯一标识符 -->
	<!-- parameterType输入类型，跟#{}类型一致 -->
	<!-- resultType返回类型，自定义实体类是全限定路径 -->
	<select id="selectBlog" parameterType="int" resultType="Blog">
		select * from Blog where id = #{id}
	</select>
</mapper>
   ```
