# jdbc



### 连接jdbc

```java
public class  ConUtil{
    public static ResourceBundle rb=ResourceBundle.getBundle("jdbc");
    public static String className;
    public static String url;
    public static String name;
    public static String pwd;
    
        static {
        className = rb.getString("dirver");
        url = rb.getString("url");
        userName = rb.getString("userName");
        userPwd = rb.getString("userPwd");
        try {
              //加载mysql驱动
            Class.forName(className);
        } catch (ClassNotFoundException var1) {
            var1.printStackTrace();
        }
    }
    
    public  Connection GetCon(){
        //得到连接对象
        Connection con=null;
        try {
            con = DriverManager.getConnection(url,name,pwd);
            //con = DriverManager.getConnection("jdbc:mysql://localhost:3306/database?characterEncoding=utf-8","root","root");
        } catch (SQLException e) {
            
            e.printStackTrace();
        }
        return con;
    }

    //关闭数据库
    public static void GetClose(ResultSet rs, PreparedStatement pre, Connection con){
        //判断三个对象是否正在使用
        if(rs!=null){
            try {
                rs.close();
            } catch (SQLException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }
        if(pre!=null){
            try {
                pre.close();
            } catch (SQLException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }
        if(con!=null){
            try {
                con.close();
            } catch (SQLException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }
    }
}
```

### jdbc.properties

```properties
userName=root
userPwd=root
url=jdbc:mysql://localhost:3306/mysql?useUnicode=true&characterEncoding=UTF-8
driver=com.mysql.jdbc.Driver
```

### dbcp.properties

```properties
userName=root
userPwd=root
url=jdbc:mysql://localhost:3306/student?characterEncoding=UTF-8
driver=com.mysql.jdbc.Driver
maxActive=100
maxIdle=90
minIdle=20
maxWait=1000
```

