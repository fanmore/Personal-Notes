## JDBC API

* DriverManager   管理不同的驱动
* Connection    连接数据库
* Statement（PreparedStatement）   增删改查
* CallableStatement   调用数据库中的存储过程/存储函数
* ResultSet   返回结果集

## JDBC访问数据库的具体步骤

* 导入驱动，加载具体的驱动类
* 与数据库建立连接
* 发送SQL，执行
* 处理结果集（查询）

## 示例（MySql）

连接数据库的优化

```java
    import java.sql.Connection;
    import java.sql.DriverManager;
    import java.sql.PreparedStatement;
    import java.sql.ResultSet;
    import java.sql.SQLException;
    import java.sql.Statement;


    public class DbUtil {

      public static Connection connection = null;
      public static PreparedStatement preparedStatement = null;
      public static ResultSet resultset = null;

      public PreparedStatement conn(String sql,Object[] params) {
        String DRIVER = "com.mysql.cj.jdbc.Driver";
        String URL = "jdbc:mysql://localhost:3306/dbname?useSSL=false&serverTimezone=UTC";
        String USER = "root";
        String PASS = "root";

        try {
          Class.forName(DRIVER);
          connection = DriverManager.getConnection(URL,USER,PASS);
          preparedStatement = connection.prepareStatement(sql);
          if (params!=null) {
            for (int i = 0; i < params.length; i++) {
              preparedStatement.setObject(i+1, params[i]);
            }
          }
        } catch (ClassNotFoundException e) {
          // TODO Auto-generated catch block
          e.printStackTrace();
        } catch (SQLException e) {
          // TODO Auto-generated catch block
          e.printStackTrace();
        }
        return preparedStatement;
      }

      //关闭连接
      public static void closeAll(ResultSet resultset,Statement pstmt,Connection connection) {
        try {
          if (resultset!=null) resultset.close();
          if (pstmt!=null) pstmt.close();
          if (connection!=null) connection.close();
        } catch (Exception e) {
          // TODO: handle exception
          e.printStackTrace();
        }
      }

      //通用增删改
      public boolean Update(String sql,Object[] params) {
        PreparedStatement pre = conn(sql, params);
        int count;
        try {
          count = pre.executeUpdate();
          if (count > 0) {
            return true;
          }
        } catch (SQLException e) {
          // TODO Auto-generated catch block
          e.printStackTrace();
        }finally {
          closeAll(null, preparedStatement, connection);
        }
        return false;
      }

      //通用查询
      public ResultSet Query(String sql,Object[] params) {
        PreparedStatement pre = conn(sql, params);
        try {
          resultset = pre.executeQuery();
        } catch (SQLException e) {
          // TODO Auto-generated catch block
          e.printStackTrace();
        }
        return resultset;
        //各连接没有关，使用后记得关闭
      }
    }
```

使用示例
```java
    import java.sql.ResultSet;
    import java.sql.SQLException;
    import java.util.ArrayList;
    import java.util.List;

    import com.blog.entity.Article;
    import com.blog.util.DbUtil;

    public class DoArticle {
      //增加文章
      public boolean addArticle(Article article) {
        DbUtil dbUtil = new DbUtil();
        String sql = "insert into article values(?,?)";
        Object[] params = {article.getTitle(),article.getContent()};
        return dbUtil.Update(sql, params);
      }

      //修改文章
      public boolean updateArticle(Article article) {
        DbUtil dbUtil = new DbUtil();
        String sql = "update article set title=?,content=? where id=?";
        Object[] params = {article.getTitle(),article.getContent(),article.getId()};
        return dbUtil.Update(sql, params);
      }

      //删除文章
      public boolean deleteArticle(Article article) {
        DbUtil dbUtil = new DbUtil();
        String sql = "delete from article where id = ?";
        Object[] params = {article.getId()};
        return dbUtil.Update(sql, params);
      }

      //查询全部文章
      @SuppressWarnings("static-access")
      public List<Article> allArticles() {
        List<Article> articles = new ArrayList<>();
        DbUtil dbUtil = new DbUtil();
        String sql = "select * from article";
        ResultSet rs = dbUtil.Query(sql, null);
        try {
          while(rs.next()) {
            String title = rs.getString("title");
            String content = rs.getString("content");
            Article arti = new Article(title,content);
            articles.add(arti);
          }
        } catch (SQLException e) {
          // TODO Auto-generated catch block
          e.printStackTrace();
        }finally {
          if (rs!=null)
            try {
              rs.close();
            } catch (SQLException e) {
              // TODO Auto-generated catch block
              e.printStackTrace();
            }
          dbUtil.closeAll(dbUtil.resultset, dbUtil.preparedStatement, dbUtil.connection);
        }
        return articles;
      }
    }
```

## 文本类型

* 对于文本类型，在预处理时可使用setCharacterStream处理

存入
```java
    String sql = "insert into tablename values(?,?)";
    PrepareStatement pstmt = connection.prepareStatement(sql);
    pstmt.setInt(1,1);
    File file = new File("E:\\a.txt");
    InputStream in = new FileInputStream(File);
    Reader reader = new InputStreamReader(in,"utf-8");
    pstmt.setCharacterStream(2,reader,(int)file.length());
    int count = pstmt.executeUpdate();
```
读取
```java
    if(rs.next()){
        Reader reader = rs.getCharacterStream("");
        Write write = new FileWriter("位置");
        
        char[] chs = new char[100];
        int len = -1;
        while((len = reader.read(chs)) !=-1){
            writer.write(chs,0,len);
        }
        writer.close();
        reader.close();
    }
```
