# mybatis入门

## 1.环境

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <!--父工程-->
    <groupId>org.example</groupId>
    <artifactId>mybatis</artifactId>
    <version>1.0-SNAPSHOT</version>

    <!--指定JDK的编译版本-->
    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>


    <!-- 导入依赖-->
    <dependencies>
        <!--mysql驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.49</version>
        </dependency>
        <!--mybatis-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.3</version>
        </dependency>
        <!--Junit-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
    </dependencies>



    <!--解决静态资源过滤问题-->
    <build>
        <resources>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>false</filtering>
            </resource>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>false</filtering>
            </resource>
        </resources>
    </build>
</project>
```



## 2.配置

在resources文件夹下创建mybatis-config.xml配置文件

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
<!--    环境-->
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://127.0.0.1:3306/test?useSSL=false&amp;serverTimezone=GMT%2B8&amp;useUnicode=true&amp;characterEncoding=utf8"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <!--每一个mapper.xml都需要在mybatis核心控制文件中注册-->
        <mapper resource="com/zuxia/mapper/UserMapper.xml"/>
    </mappers>

</configuration>
```



## 3.用法

### 1.获取SqlSessionFactory工程类

```
    private static SqlSessionFactory sqlSessionFactory;
    /**
     * 从 XML 中构建 SqlSessionFactory
     */
    static{
        try {
            String resource="mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
             sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

/**
 *从 SqlSessionFactory 中获取 SqlSession
 */
    public static SqlSession getSqlSession(){
        return sqlSessionFactory.openSession();
    }
```

### 2.创建Mapper.xml文件实现接口中方法

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace绑定一个对应的dao接口-->
<mapper namespace="com.zuxia.mapper.IUserMapper">
    <!--resultType返回类型泛型  id接口方法名 parameterType参数类型-->
    <select id="queryAll" resultType="com.zuxia.pojo.Userpojo">
        select * from orders
    </select>

    <insert id="saveUser" parameterType="com.zuxia.pojo.Userpojo" >
        insert into user values(#{id},#{username},#{sex});
    </insert>

    <delete id="delUser" parameterType="int" >
        delete
        from user
        where id=#{id};
    </delete>

    <update id="updUser" parameterType="com.zuxia.pojo.Userpojo">
        update user
        set username=#{username},sex=#{sex}
        where id=#{id}
    </update>
</mapper>
```

### 3.测试 测试类文件夹尽量与实例文件夹结构相对应

```
 @Test
    public  void queryTest(){

        //获得SqlSession对象
        SqlSession session = MybatisUtil.getSqlSession();
        /**
         *  方法一
         */
        //通过反射机制得到Mapper接口
        IUserMapper iUserMapper = session.getMapper(IUserMapper.class);
        //得到结果集
        List<Userpojo> userpojoList1 =iUserMapper.queryAll();
        for (Userpojo user:userpojoList1){
            System.out.println(user);
        }

        /**
         *方法二
         */
        List<Userpojo> userpojoList2 = session.selectList("com.zuxia.mapper.IUserMapper.queryAll");
        for (Userpojo user:userpojoList2){
            System.out.println(user);
        }
        //session不是线程安全的，确保每次都能够执行关闭
        session.close();
    }

```

### 4.注意事项

#### 1.增删改需要事务提交

```
 sqlsession.commit();
```



#### 2.sqlsession不是线程安全的，确保每次都能够执行关闭

```
session.close();
```

## Mybatis插入数据同时获取主键ID

>用Mybatis插入主任务的时候，同时要把子任务插入到数据库中
>
>主任务[insert](https://so.csdn.net/so/search?q=insert&spm=1001.2101.3001.7020)之后，通过主任务对象的getID，就可以获取主任务ID
>
>因为Mybatis将数据写入内存（事务）或者磁盘（不是事务）时，会将主键ID的值返回到对象的ID中。

```xml
需要在Mybatis的插入语句前加入这一句话

<selectKey resultType="java.lang.Long" keyProperty="ID" order="AFTER" >
  SELECT LAST_INSERT_ID()
</selectKey>
```



```
当设置了useGeneratedKeys="true" keyProperty="id"后，它会在你插入数据库的同时，将这个对象的id值改为最新的那个id，然后只需要取出就可以了


```

