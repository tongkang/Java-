### Mybatis

#### 1、使用Mybatis

1. 引入Mybatis的依赖

2. 构建SqlSessionFactory

   ```java
   String resource = "org/mybatis/example/mybatis-config.xml";
   InputStream inputStream = Resources.getResourceAsStream(resource);
   SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
   ```

3. 创建config.xml文件

   ```java
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
     PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
     "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
     <environments default="development">
       <environment id="development">
         <transactionManager type="JDBC"/>
         <dataSource type="POOLED">
           <property name="driver" value="${driver}"/>
           <property name="url" value="${url}"/>
           <property name="username" value="${username}"/>
           <property name="password" value="${password}"/>
         </dataSource>
       </environment>
     </environments>
     <mappers>
       <mapper class="com.github.hcsp.sql.Sql$UserMapper"/> 
     </mappers>
   </configuration>
   
   修改对应的property
   ```

4. 在接口上写上注释就可以进行查询了

   ```java
       interface UserMapper{
           @Select("select * from user")
           List<User> getUsers();
       }
   
   main函数中
    UserMapper mapper = session.getMapper(UserMapper.class);    /*获取方法反射*/          System.out.println(mapper.getUsers());   /*调用执行方法*/
   ```

   

#### 2、使用xml来写sql

1. 引入在config.xml中的mappers中`<mapper resource="db/mybatis/UserMapper.xml"/>`

2. 创建UserMapper.xml文件

3. ```java
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="com.hcsp.UserMapper">  /*命名空间名可以随便写*/
       <select id="selectUser" resultType="User">
       select * from User   /*可以写入很长的拼接sql*/
     </select>
   </mapper>
   ```

4. 在main中使用就可以调用了

   ```java
   try (SqlSession session = sqlSessionFactory.openSession()) {
               System.out.println(session.selectList("com.hcsp.UserMapper.selectUser"));
   
    }
   ```



#### 3、传入可变参数的sql语句

1. 首先在User实体类中生产get和set方法

2. 把UserMapper.xml中的resultMap换成自己的实体类全路径`com.github.hcsp.sql.Sql$User`。因为User在同一文件下，所以使用`Sql$`来拼接，一般会是分开文件放的

3. resultMap是可以使用别名来替换的，按照顺序放在config.xml文件中去

   ```java
   <typeAliases>
           <typeAlias alias="User" type="com.github.hcsp.sql.Sql$User" />
    </typeAliases>
   ```

4. selectList是可以传递参数的

   ```java
   System.out.println(session.selectList("com.hcsp.UserMapper.selectUser",1));
   ```

#### 4、参数的#{}和${}的区别

1. #{}是解析接收的阐述

2. ${}是直接拼接到参数上的

总的来说，使用#{}是来防止注入的。

````java
-- 查出所有的数据
select * from User where name = '' or 1=1 --' 
````



#### 5、传递的数据可以使用Map来传递

可以替换实体类User，一般用于传递临时变量

```java
HashMap<String, Object> user = new HashMap<>();
user.put("name","wangwu");
```



#### 6、动态SQL

1. foreach的使用

   ```java
   <select id="selectIds" resultType="User">
           SELECT *
           FROM User P
           WHERE ID in
           <foreach item="item" index="index" collection="ids"
                    open="(" separator="," close=")">
               #{item}
           </foreach>
   </select>
   
   -- 用map传入的时候
   	user.put("ids", Arrays.asList(1,2,3));
   ```

   

#### 7、association的使用

  当返回的数据中存在多个表（User，Goods，Order）的字段时候

```java
<select id="getInnerJoinOrders" resultMap="Order">  --Order接收的是resultMap中的数据
        select orders.id as order_id,
            user.NAME as user_name,
            goods.NAME as goods_name,
            orders.GOODS_PRICE as goods_price,
            orders.GOODS_PRICE * orders.GOODS_NUM as total_price
        from "ORDER" orders
        inner join "USER" user on user.ID = orders.USER_ID
        inner join "GOODS" goods on goods.ID = orders.GOODS_ID
        where goods.NAME is not null and user.NAME is not null
    </select>

    <resultMap id="Order" type="com.github.hcsp.mybatis.entity.Order">
        <result property="id" column="order_id"/>
        <result property="totalPrice" column="total_price"/>
        -- 下面是拼接其他表中的字段，其中column对应上面查询as后面的别名
        <association property="user" javaType="com.github.hcsp.mybatis.entity.User">
            <result property="name" column="user_name"/>
        </association>
        <association property="goods" javaType="com.github.hcsp.mybatis.entity.Goods">
            <result property="name" column="goods_name"/>
            <result property="price" column="goods_price"/>
        </association>
    </resultMap>
```



