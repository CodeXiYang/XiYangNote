# MyBatis进行CRUD操作

## 1. namespace

1. 将上面案例中的UserMapper接口改名为 UserDao； 

2. 将UserMapper.xml中的namespace改为为UserDao的路径 .
3. 再次测试

**结论:**配置文件中namespace中的名称为对应Mapper接口或者Dao接口的完整包名,必须一致！

## 2. select

1.select标签是mybatis中最常用的标签之一

2.select语句有很多属性可以详细配置每一条SQL语句

- `id` 
  - 命名空间中唯一的标识符
  - 接口中的方法名与映射文件中的SQL语句ID 一一对应
- parameterType
  - 传入SQL语句的参数类型 。【万能的Map，可以多尝试使用】
- resultType
  - SQL语句返回值类型。【完整的类名或者别名】

**【案例】**：根据id查询用户

在UserMapper中添加对应方法

```java
public interface UserMapper { 
    //查询全部用户 
    List<User> selectUser(); 
    //根据id查询用户 
    User selectUserById(int id); 
}
```

 在UserMapper.xml中添加Select语句

```xml
<select id="selectUserById" resultType="com.kuang.pojo.User"> 
    select * from user where id = #{id} 
</select>
```

 测试类中测试

```java
@Test public void tsetSelectUserById() { 
    SqlSession session = MybatisUtils.getSession(); 
    //获取SqlSession连接 
    UserMapper mapper = session.getMapper(UserMapper.class); 
    User user = mapper.selectUserById(1); 
    System.out.println(user); 
    session.close(); 
}
```

**【案例】**: 根据 密码 和 名字 查询用户

<u>思路一：直接在方法中传递参数</u>

1. 在接口方法的参数前加 @Param属性

2. Sql语句编写的时候，直接取@Param中设置的值即可，不需要单独设置参数类型

   ```java
   //通过密码和名字查询用户 
   User selectUserByNP(@Param("username") String username,@Param("pwd") String pwd); 
   /* 
   <select id="selectUserByNP" resultType="com.kuang.pojo.User"> 
   	select * from user where name = #{username} and pwd = #{pwd} 
   </select> 
   */
   ```

<u>思路二：使用万能的Map</u>

1.  在接口方法中，参数直接传递Map； 

   ```java
   User selectUserByNP2(Map<String,Object> map);
   ```

2.  编写sql语句的时候，需要传递参数类型，参数类型为map

   ```xml
   <select id="selectUserByNP2" parameterType="map" resultType="com.kuang.pojo.User"> 
       select * from user where name = #{username} and pwd = #{pwd} 
   </select>
   ```

3.  在使用方法的时候，Map的 key 为 sql中取的值即可，没有顺序要求！

   ```java
   Map<String, Object> map = new HashMap<String, Object>(); 
   map.put("username","小明"); 
   map.put("pwd","123456"); 
   User user = mapper.selectUserByNP2(map);
   ```



**总结：**如果参数过多，我们可以考虑直接使用Map实现，如果参数比较少，直接传递参数即可

## 3. insert

*我们一般使用insert标签进行插入操作，它的配置和select标签差不多！*

**【案例】**：给数据库增加一个用户

1. 在UserMapper接口中添加对应的方法

   ```java
   //添加一个用户 
   int addUser(User user);
   ```

2. 在UserMapper.xml中添加insert语句

   ```xml
   <insert id="addUser" parameterType="com.kuang.pojo.User"> 
       insert into user (id,name,pwd) values (#{id},#{name},#{pwd}) 
   </insert>
   ```

3. 测试

   ```java
   @Test public void testAddUser() { 
       SqlSession session = MybatisUtils.getSession(); 
       UserMapper mapper = session.getMapper(UserMapper.class); 
       User user = new User(5,"王五","zxcvbn"); 
       int i = mapper.addUser(user); 
       System.out.println(i); 
       session.commit(); //提交事务,重点!不写的话不会提交到数据库 
       session.close(); 
   }
   ```

**注意点**：[增、删、改] / [写] 操作需要提交事务！



## 4. update

*我们一般使用update标签进行更新操作，它的配置和select标签差不多！*

**【案例】**：修改用户的信息

1. 同理，编写接口方法

   ```java
   //修改一个用户 
   int updateUser(User user);
   ```

2. 编写对应的配置文件SQL

   ```xml
   <update id="updateUser" parameterType="com.kuang.pojo.User"> 
       update user set name=#{name},pwd=#{pwd} where id = #{id} 
   </update>
   ```

3. 测试

   ```java
   @Test public void testUpdateUser() { 
       SqlSession session = MybatisUtils.getSession(); 
       UserMapper mapper = session.getMapper(UserMapper.class); 
       User user = mapper.selectUserById(1); 
       user.setPwd("asdfgh"); 
       int i = mapper.updateUser(user); 
       System.out.println(i); 
       session.commit(); //提交事务,重点!不写的话不会提交到数据库 
       session.close(); 
   }
   ```

   



## 6. delete

*我们一般使用delete标签进行删除操作，它的配置和select标签差不多！*

**【案例】**：根据id删除一个用户

1.  同理，编写接口方法

   ```java
   //根据id删除用户 
   int deleteUser(int id);
   ```

2.  编写对应的配置文件SQL

   ```xml
   <delete id="deleteUser" parameterType="int"> 
       delete from user where id = #{id} 
   </delete>
   ```

3. 测试

   ```java
   @Test public void testDeleteUser() { 
       SqlSession session = MybatisUtils.getSession(); 
       UserMapper mapper = session.getMapper(UserMapper.class); 
       int i = mapper.deleteUser(5); 
       System.out.println(i); 
       session.commit(); //提交事务,重点!不写的话不会提交到数据库 
       session.close(); 
   }
   ```

## 7. CRUD操作注意事项

1. 所有的增删改操作都需要提交事务！
2. 接口所有的普通参数，尽量都写上@Param参数，尤其是多个参数时，必须写上！
3. 有时候根据业务的需求，可以考虑使用map传递参数！
4. 为了规范操作，在SQL的配置文件中，我们尽量将Parameter参数和resultType都写上！



## 思考

*模糊查询like语句该怎么写?*

1. 第1种：在Java代码中添加sql通配符。

   ```xml
   string wildcardname = “%smi%”; 
   list<name> names = mapper.selectlike(wildcardname); 
   <select id=”selectlike”> 
       select * from foo where bar like #{value} 
   </select>
   ```

2. 第2种：在sql语句中拼接通配符，会引起sql注入

   ```xml
   string wildcardname = “smi”; 
   list<name> names = mapper.selectlike(wildcardname); 
   <select id=”selectlike”> 
       select * from foo where bar like "%"#{value}"%" 
   </select>
   ```

   