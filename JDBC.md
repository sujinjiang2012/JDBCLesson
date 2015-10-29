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

## Select

* boolean execute(String SQL) : 返回一个布尔值true，如果ResultSet对象可以被检索，否则返回false。使用这个方法来执行SQL DDL语句，或当需要使用真正的动态SQL。

* int executeUpdate(String SQL) : 返回受影响的SQL语句执行的行数。使用此方法来执行，而希望得到一些受影响的行的SQL语句 - 例如，INSERT，UPDATE或DELETE语句。
* ResultSet executeQuery(String SQL) : 返回ResultSet对象。当希望得到一个结果集使用此方法，就像使用一个SELECT语句。
