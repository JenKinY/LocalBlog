JDBC连接数据库：

```java
import java.sql.*;
public class Conn {
    Connection con;
    public Connection getConnection() {
        try {
            Class.forName("com.mysql.jdbc.Driver");  System.out.println("数据库驱动加载成功");
        } catch(ClassNotFoundException e){
            e.printStackTrace();
        }
        try {
            con = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/mysql?characterEncoding=UTF-8","root","youzc");
            System.out.println("数据库连接成功");
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return con;
    }
    public static void main(String[] args) {
        Conn c = new Conn();
        c.getConnection();

    }
}

```

