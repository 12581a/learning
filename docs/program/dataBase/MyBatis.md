# Mybatis

## Mybatis简介

MyBatis是一款优秀的持久层框架，它支持自定义SQL、存储过程以及高级映射。MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作。MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。

springboot与mybatis整合：

UserMapper文件

```java
@Mapper
public interface UserMapper {
    @Select("select * from user where id=#{id}")
    User getUserById(Integer id);

    @Delete("delete from user where id=#{id}")
    int deleteById(Integer id);
}
```

然后直接在Controller中调用就行了。

```java
@RestController
public class UserController {
    @Autowired
    private UserMapper userMapper;

    @GetMapping("user/{id}")
    public User getUserById(@PathVariable("id") Integer id){
        User user = userMapper.getUserById(id);
        return user;
    }
}
```

application配置文件

```properties
spring:
  datasource:
    username: root
    password: 123456
    url: jdbc:mysql://localhost:3306/test?serverTimezone=UTC
    type: com.alibaba.druid.pool.DruidDataSource
mybatis:
  config-location: classpath:mybatis/mybatis-config.xml
  mapper-locations: classpath:mybatis/mapper/*.xml
```

## #{}和${}的区别是什么？

 `#{}`表示一个占位符 ，如` select * from user where id=#{id} `, Mybatis 会将 sql 中的`#{}`替换为`?`号

 `${}`是 Properties 文件中的变量占位符，它可以用于标签属性值和 sql 内部，属于静态文本替换，如在配置数据源时`<property name="password" value="${jdbc.password}"/>`

## 查询

### 一对一查询

文章表：id | title | content | aid

作者表：id | name | age

```xml
<resultMap id="ArticleMap" type="com.Article">
  <id property="id" column="id" />
  <result property="title" column="title"/>
  <result property="content" column="content"/>
  <result property="author.id" column="author_id"/>
  <result property="author.name" column="author_name"/>
  <result property="author.age" column="author_age"/>
</resultMap>

<resultMap id="ArticleMap" type="com.Article">
  <id property="id" column="id" />
  <result property="title" column="title"/>
  <result property="content" column="content"/>
  <association property="author" javaType="com.Author" columnPrefix="author_">
  	  <id property="id" column="id" />
  	  <result property="name" column="name"/>
      <result property="age" column="age"/>
  </association>
</resultMap>

<select id="getAticle" resultMap="ArticleMap">
	select a.*,au.id as author_id,au.name as author_name,au.age as author_age
    from article as a,author as au where a.aid = au.id
</select>
```

### 一对多查询

**3.Insert, Update, Delete 元素的属性** 

| 属性            | 描述                                                         |
| :-------------- | :----------------------------------------------------------- |
| `id`            | 在命名空间中唯一的标识符，可以被用来引用这条语句。           |
| `parameterType` | 将会传入这条语句的参数的类全限定名或别名。这个属性是可选的，因为 MyBatis 可以通过类型处理器（TypeHandler）推断出具体传入语句的参数，默认值为未设置（unset）。 |
| `resultType`    | 结果的类型。通常 MyBatis 可以推断出来，但是为了更加准确，写上也不会有什么问题。MyBatis 允许将任何简单类型用作主键的类型，包括字符串。如果生成列不止一个，则可以使用包含期望属性的 Object 或 Map。 |

 解决列名不匹配的方式:

```xml
<select id="selectUsers" resultType="User">
  select user_id as "id",user_name as "userName",hashed_password as "hashedPassword" from 	some_table where id = #{id}
</select>
```

上面的例子没有一个需要显式配置 `ResultMap`，这就是 `ResultMap` 的优秀之处——你完全可以不用显式地配置它们。 那显式使用外部的 `resultMap` 会怎样:

```xml 
<resultMap id="userResultMap" type="User">
  <id property="id" column="user_id" />
  <result property="username" column="user_name"/>
  <result property="password" column="hashed_password"/>
</resultMap>
```

 然后在引用它的语句中设置 `resultMap` 属性就行了（注意我们去掉了 `resultType` 属性）。比如: 

```xml
<select id="selectUsers" resultMap="userResultMap">
  select user_id, user_name, hashed_password
  from some_table
  where id = #{id}
</select>
```

## 动态SQL

**if关键字**

```xml
<select id = "getBookByPage">
	select * from t_book
	<if test = "start != null and size != null">
		limit #{start},#{size}
	</if>
<select>
```

**Choose关键字**

```xml
<select id = "getBookByAuName">
	select * from t_book
    where 1 = 1 
    <choose>
        <when test = "name != null">
        	and b_name = #{name}
        </when>
        <when test = "author != null">
        	and author = #{author}
        </when>
        <otherwise>
        </otherwise>
	</choose>
<select>    
```






































