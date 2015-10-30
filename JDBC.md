# JDBC

## JDBC的步骤
* 在安装相应的驱动程序后，现在是时候建立使用JDBC的数据库连接。
* 导入JDBC包： 添加import语句到Java程序导入所需的类在Java代码中。
* 注册JDBC驱动程序：这一步会导致JVM加载所需的驱动程序实现到内存中，因此它可以实现JDBC请求。
* 数据库URL制定：这是创建格式正确的地址指向到要连接的数据库。
* 创建连接对象：最后，代码调用DriverManager对象的getConnection()方法来建立实际的数据库连接。

## 链接Postgre Sql
    import java.sql.Connection;
    import java.sql.DriverManager;
    import java.sql.SQLException;

    public class ConnUtil
    {
        public static Connection getConn()
        {
            Connection conn = null;
            try
            {
                Class.forName("org.postgresql.Driver");
                String url = "jdbc:postgresql://192.168.2.150:5432/postgres";
                try
                {
                    conn = DriverManager.getConnection(url, "postgres", "postgres");
                }
                catch (SQLException e)
                {
                    e.printStackTrace();
                }
            }
            catch (ClassNotFoundException e)
            {
                e.printStackTrace();
            }

            return conn;
        }

    }

## Statement
一旦获得一个连接，我们可以与数据库进行交互。在JDBC Statement 和 PreparedStatement </br>
接口定义的方法和属性，使可以发送SQL或PL/SQL命令和从数据库接收数据。</br>
它们还定义方法，帮助Java和数据库使用SQL数据类型之间转换数据的差异。</br>

下表提供了每个接口的用途概要，了解决定使用哪个接口？</br>
* Statement--使用通用访问数据库。当在运行时使用静态SQL语句。 Statement接口不能接受的参数。
* PreparedStatement--当计划多次使用SQL语句。 那么可以PreparedStatement接口接收在运行时输入参数。


http://www.yiibai.com/jdbc/jdbc-insert-records.html

## 执行

* boolean execute(String SQL) : 返回一个布尔值true，如果ResultSet对象可以被检索，否则返回false。使用这个方法来执行SQL DDL语句，或当需要使用真正的动态SQL。

* int executeUpdate(String SQL) : 返回受影响的SQL语句执行的行数。使用此方法来执行，而希望得到一些受影响的行的SQL语句 - 例如，INSERT，UPDATE或DELETE语句。
* ResultSet executeQuery(String SQL) : 返回ResultSet对象。当希望得到一个结果集使用此方法，就像使用一个SELECT语句。

## Select之后的结果

     Statement stmt = conn.createStatement();
     ResultSet rs = stmt.executeQuery( "SELECT * FROM MyTable" );
     while ( rs.next() ) {
         int numColumns = rs.getMetaData().getColumnCount();
         for ( int i = 1 ; i <= numColumns ; i++ ) {
            // 与大部分Java API中下标的使用方法不同，字段的下标从1开始
            // 当然，还有其他很多的方式（ResultSet.getXXX()）获取数据
            System.out.println( "COLUMN " + i + " = " + rs.getObject(i) );
         }
     }
     rs.close();
     stmt.close();
     
## java类型和数据库类型的对应表

<table border="1" cellpadding="2" cellspacing="0" width="50%">
<caption>從SQL到Java類型對映的JDBC規範</caption>
<tr bgcolor="#EFEFEF">
<th width="50%">SQL類型</th>
<th width="50%">Java類型</th>
</tr>
<tr>
<td>CHAR</td>
<td>java.lang.String</td>
</tr>
<tr>
<td>VARCHAR</td>
<td>java.lang.String</td>
</tr>
<tr>
<td>LONGVARCHAR</td>
<td>java.lang.String</td>
</tr>
<tr>
<td>NUMERIC</td>
<td>java.math.BigDecimal</td>
</tr>
<tr>
<td>DECIMAL</td>
<td>java.math.BigDecimal</td>
</tr>
<tr>
<td>BIT</td>
<td>boolean</td>
</tr>
<tr>
<td>TINYINT</td>
<td>byte</td>
</tr>
<tr>
<td>SMALLINT</td>
<td>short</td>
</tr>
<tr>
<td>INTEGER</td>
<td>int</td>
</tr>
<tr>
<td>BIGINT</td>
<td>long</td>
</tr>
<tr>
<td>REAL</td>
<td>float</td>
</tr>
<tr>
<td>FLOAT</td>
<td>double</td>
</tr>
<tr>
<td>DOUBLE</td>
<td>double</td>
</tr>
<tr>
<td>BINARY</td>
<td>byte[]</td>
</tr>
<tr>
<td>VARBINARY</td>
<td>byte[]</td>
</tr>
<tr>
<td>LONGVARBINARY</td>
<td>byte[]</td>
</tr>
<tr>
<td>DATE</td>
<td>java.sql.Date</td>
</tr>
<tr>
<td>TIME</td>
<td>java.sql.Time</td>
</tr>
<tr>
<td>TIMESTAMP</td>
<td>java.sql.Timestamp</td>
</tr>
<tr>
<td>BLOB</td>
<td>java.sql.Blob</td>
</tr>
<tr>
<td>CLOB</td>
<td>java.sql.Clob</td>
</tr>
<tr>
<td>Array</td>
<td>java.sql.Array</td>
</tr>
<tr>
<td>REF</td>
<td>java.sql.Ref</td>
</tr>
<tr>
<td>Struct</td>
<td>java.sql.Struct</td>
</tr>
</table>

## PreparedStatement

    PreparedStatement ps = null;
     ResultSet rs = null;
     try {
     ps = conn.prepareStatement( "SELECT i.*, j.* FROM Omega i, Zappa j
          WHERE i = ? AND j = ?" );
     // 使用问号作为参数的标示
     
     // 进行参数设置
     // 与大部分Java API中下标的使用方法不同，字段的下标从1开始，1代表第一个问号
     // 当然，还有其他很多针对不同类型的类似的PreparedStatement.setXXX()方法
     ps.setString(1, "Poor Yorick");
     ps.setInt(2, 8008);
     
     // 结果集
     rs = ps.executeQuery();
     while ( rs.next() ) {
         int numColumns = rs.getMetaData().getColumnCount();
         for ( int i = 1 ; i <= numColumns ; i++ ) {
            // 与大部分Java API中下标的使用方法不同，字段的下标从1开始
            // 当然，还有其他很多的方式（ResultSet.getXXX()）获取数据
            System.out.println( "COLUMN " + i + " = " + rs.getObject(i) );
         }
     
     }
     catch (SQLException e) {
      // 异常处理
     }
     finally { // 使用finally进行资源释放
      try {
       rs.close();
       ps.close();
      } catch( SQLException e){} // 异常处理：忽略close()时的错误
     }
