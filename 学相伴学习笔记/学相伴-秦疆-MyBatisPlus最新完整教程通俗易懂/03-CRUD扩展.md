# CRUD扩展

## 1. 插入操作

```java
// 测试插入 
@Test public void testInsert(){ 
    User user = new User(); 
    user.setName("狂神说Java"); 
    user.setAge(3); 
    user.setEmail("24736743@qq.com"); 
    int result = userMapper.insert(user); // 帮我们自动生成id 
    System.out.println(result); // 受影响的行数 
    System.out.println(user); // 发现，id会自动回填 
}
```

**ps: 数据库插入的id的默认值为：全局的唯一id**

### 1.1 主键生成策略

默认 ID_WORKER 全局唯一id

分布式系统唯一id生成：https://www.cnblogs.com/haoxinyue/p/5208136.html

**雪花算法：**

snowflflake是Twitter开源的分布式ID生成算法，结果是一个long型的ID。其核心思想是：使用41bit作为毫秒数，10bit作为机器的ID（5个bit是数据中心，5个bit的机器ID），12bit作为毫秒内的流水号（意味着每个节点在每毫秒可以产生 4096 个 ID），最后还有一个符号位，永远是0。可以保证几乎全球唯一！

**主键自增**

我们需要配置主键自增：

1. 实体类字段上 @TableId(type = IdType.AUTO) 
2. 数据库字段一定要是自增！
3. 再次测试插入即可！

**其余的源码解释**

```java
public enum IdType { 
    AUTO(0), // 数据库id自增 
    NONE(1), // 未设置主键 
    INPUT(2), // 手动输入 
    ID_WORKER(3), // 默认的全局唯一id 
    UUID(4), // 全局唯一id 
    uuid ID_WORKER_STR(5); //ID_WORKER 字符串表示法 
}
```



## 2. 更新操作

```java
// 测试更新
@Test public void testUpdate(){ 
    User user = new User(); // 通过条件自动拼接动态sql 
    user.setId(6L); 
    user.setName("关注公众号：狂神说"); 
    user.setAge(18); 
    // 注意：updateById 但是参数是一个 对象！ 
    int i = userMapper.updateById(user); 
    System.out.println(i); 
}
```

所有的sql都是自动帮你动态配置的！

### 2.1 自动填充

创建时间、修改时间！这些个操作一遍都是自动化完成的，我们不希望手动更新！

阿里巴巴开发手册：所有的数据库表：gmt_create、gmt_modifified几乎所有的表都要配置上！而且需要自动化！

**方式一:数据库级别（工作中不允许你修改数据库）**

1. 在表中新增字段 create_time, update_time

2. 再次测试插入方法，我们需要先把实体类同步！

   ```java
   private Date createTime; 
   private Date updateTime;
   ```

3. 再次更新查看结果即可

**方式二：代码级别**

1. 删除数据库的默认值、更新操作！

2. 实体类字段属性上需要增加注解

   ```java
   // 字段添加填充内容 
   @TableField(fill = FieldFill.INSERT) 
   private Date createTime; 
   @TableField(fill = FieldFill.INSERT_UPDATE) 
   private Date updateTime;
   ```

3. 编写处理器来处理这个注解即可！

   ```java
   @Slf4j 
   @Component // 一定不要忘记把处理器加到IOC容器中！ 
   public class MyMetaObjectHandler implements MetaObjectHandler { 
       // 插入时的填充策略 
       @Override 
       public void insertFill(MetaObject metaObject) { 
           log.info("start insert fill....."); 
           // setFieldValByName(String fieldName, Object fieldVal, MetaObject metaObjectthis.setFieldValByName("createTime",new Date(),metaObject); this.setFieldValByName("updateTime",new Date(),metaObject); }// 更新时的填充策略 @Override public void updateFill(MetaObject metaObject) { log.info("start update fill....."); this.setFieldValByName("updateTime",new Date(),metaObject); } }
   ```

4. 测试插入

5. 测试更新、观察时间即可！

### 2.2 乐观锁

在面试过程中，我们经常会被问道乐观锁，悲观锁！这个其实非常简单！

**乐观锁** : 故名思意十分乐观，它总是认为不会出现问题，无论干什么不去上锁！如果出现了问题，再次更新值测试

**悲观锁**：故名思意十分悲观，它总是认为总是出现问题，无论干什么都会上锁！再去操作！

我们这里主要讲解 乐观锁机制！

乐观锁实现方式：

- 取出记录时，获取当前 version
- 更新时，带上这个version
- 执行更新时， set version = newVersion where version = oldVersion
- 如果version不对，就更新失败

```sql
乐观锁：1、先查询，获得版本号 version = 1 
-- A 
update user set name = "kuangshen", version = version + 1 where id = 2 and version = 1 
-- B 线程抢先完成，这个时候 version = 2，会导致 A 修改失败！ 
update user set name = "kuangshen", version = version + 1 where id = 2 and version = 1
```

**测试一下MP的乐观锁插件**

1. 给数据库中增加version字段！

2. 我们实体类加对应的字段

   ```java
   @Version //乐观锁Version注解 
   private Integer version;
   ```

3. 注册组件

   ```java
   // 扫描我们的 mapper 文件夹 
   @MapperScan("com.kuang.mapper") 
   @EnableTransactionManagement 
   @Configuration // 配置类 
   public class MyBatisPlusConfig { 
       // 注册乐观锁插件
       @Bean public OptimisticLockerInterceptor optimisticLockerInterceptor() { 
           return new OptimisticLockerInterceptor();
       } 
   }
   ```

4. 测试一下！

   ```java
   // 测试乐观锁成功！ 
   @Test public void testOptimisticLocker(){ 
       // 1、查询用户信息 
       User user = userMapper.selectById(1L); 
       // 2、修改用户信息 
       user.setName("kuangshen"); 
       user.setEmail("24736743@qq.com"); 
       // 3、执行更新操作 
       userMapper.updateById(user); 
   }
   // 测试乐观锁失败！多线程下
   @Test public void testOptimisticLocker2(){ 
       // 线程 1 
       User user = userMapper.selectById(1L); 
       user.setName("kuangshen111"); 
       user.setEmail("24736743@qq.com"); 
       // 模拟另外一个线程执行了插队操作 
       User user2 = userMapper.selectById(1L); 
       user2.setName("kuangshen222"); 
       user2.setEmail("24736743@qq.com"); 
       userMapper.updateById(user2); // 自旋锁来多次尝试提交！ 
       userMapper.updateById(user); // 如果没有乐观锁就会覆盖插队线程的值！ 
   }
   ```

   



## 3. 查询操作

```java
// 测试查询 
@Test public void testSelectById(){ 
    User user = userMapper.selectById(1L); 
    System.out.println(user); 
}
// 测试批量查询！
@Test public void testSelectByBatchId(){ 
    List<User> users = userMapper.selectBatchIds(Arrays.asList(1, 2, 3)); 
    users.forEach(System.out::println); 
}
// 按条件查询之一使用map操作 
@Test public void testSelectByBatchIds(){ 
    HashMap<String, Object> map = new HashMap<>(); 
    // 自定义要查询 
    map.put("name","狂神说Java"); 
    map.put("age",3);
    List<User> users = userMapper.selectByMap(map); 
    users.forEach(System.out::println); 
}
```



### 3.1 分页操作

*分页在网站使用的十分之多！*

1、原始的 limit 进行分页

2、pageHelper 第三方插件

3、MP 其实也内置了分页插件！

**如何使用**

1. 配置拦截器组件即可

   ```java
   // 分页插件 
   @Bean public PaginationInterceptor paginationInterceptor() { 
       return new PaginationInterceptor(); 
   }
   ```

2. 直接使用Page对象即可！

   ```java
   // 测试分页查询 
   @Test public void testPage(){ 
       // 参数一：当前页 
       // 参数二：页面大小 
       // 使用了分页插件之后，所有的分页操作也变得简单的！ 
       Page<User> page = new Page<>(2,5); 
       userMapper.selectPage(page,null); 
       page.getRecords().forEach(System.out::println); 
       System.out.println(page.getTotal()); 
   }
   ```

   

## 4. 删除操作

1. 根据 id 删除记录

   ```java
   // 测试删除 
   @Test public void testDeleteById(){ 
       userMapper.deleteById(1240620674645544965L); 
   }
   // 通过id批量删除 
   @Test public void testDeleteBatchId(){ 
       userMapper.deleteBatchIds(Arrays.asList(1240620674645544961L,124062067464554496 2L)); 
   }
   // 通过map删除 
   @Test public void testDeleteMap(){ 
       HashMap<String, Object> map = new HashMap<>(); 
       map.put("name","狂神说Java"); 
       userMapper.deleteByMap(map);
   }
   ```

   我们在工组中会遇到一些问题：逻辑删除！

### 4.1 逻辑删除

*物理删除 ：从数据库中直接移除* 

*逻辑删除 ：再数据库中没有被移除，而是通过一个变量来让他失效！ deleted = 0 => deleted = 1*

管理员可以查看被删除的记录！防止数据的丢失，类似于回收站！

测试一下：

1. 在数据表中增加一个 deleted 字段

2. 实体类中增加属性

   ```java
   @TableLogic //逻辑删除 
   private Integer deleted;
   ```

3. 配置！

   ```java
   // 逻辑删除组件！
   @Bean public ISqlInjector sqlInjector() { 
       return new LogicSqlInjector(); 
   }
   ```

   ```properties
   # 配置逻辑删除 
   mybatis-plus.global-config.db-config.logic-delete-value=1 
   mybatis-plus.global-config.db-config.logic-not-delete-value=0
   ```

4. 测试一下删除！

   记录依旧在数据库，但是值确已经变化了！

## 5. 性能分析插件

*我们在平时的开发中，会遇到一些慢sql。测试！ druid,,,,,*

作用：性能分析拦截器，用于输出每条 SQL 语句及其执行时间

MP也提供性能分析插件，如果超过这个时间就停止运行！

1. 导入插件

   ```java
   /** * SQL执行效率插件 */
   @Bean @Profile({"dev","test"})// 设置 dev test 环境开启，保证我们的效率 
   public PerformanceInterceptor performanceInterceptor() { 
       PerformanceInterceptor performanceInterceptor = new PerformanceInterceptor(); 
       performanceInterceptor.setMaxTime(100); 
       // ms设置sql执行的最大时间，如果超过了则不 执行 
       performanceInterceptor.setFormat(true); 
       // 是否格式化代码 
       return performanceInterceptor; 
   }
   ```

   记住，要在SpringBoot中配置环境为dev或者 test 环境！

2. 测试使用！

   ```java
   @Test void contextLoads() { 
       // 参数是一个 Wrapper ，条件构造器，这里我们先不用 null 
       // 查询全部用户 
       List<User> users = userMapper.selectList(null); 
       users.forEach(System.out::println); 
   }
   ```

   使用性能分析插件，可以帮助我们提高效率！

## 6. 条件构造器

*十分重要：Wrapper*

我们写一些复杂的sql就可以使用它来替代！

1. 测试一，记住查看输出的SQL进行分析

   ```java
   @Test void contextLoads() { 
       // 查询name不为空的用户，并且邮箱不为空的用户，年龄大于等于12 
       QueryWrapper<User> wrapper = new QueryWrapper<>(); 
       wrapper .isNotNull("name") .isNotNull("email") .ge("age",12); 
       userMapper.selectList(wrapper).forEach(System.out::println); 
       // 和我们刚才学习 的map对比一下 
   }
   ```

2. 测试二，记住查看输出的SQL进行分析

   ```java
   @Test void test2(){ 
       // 查询名字狂神说 
       QueryWrapper<User> wrapper = new QueryWrapper<>(); 
       wrapper.eq("name","狂神说"); 
       User user = userMapper.selectOne(wrapper); // 查询一个数据，出现多个结果使用List 或者 Map 
       System.out.println(user); 
   }
   ```

3. 测试三，记住查看输出的SQL进行分析

   ```java
   @Test void test3(){ 
       // 查询年龄在 20 ~ 30 岁之间的用户 
       QueryWrapper<User> wrapper = new QueryWrapper<>(); 
       wrapper.between("age",20,30); // 区间 
       Integer count = userMapper.selectCount(wrapper); // 查询结果数 
       System.out.println(count); 
   }
   ```

4. 测试四，记住查看输出的SQL进行分析

   ```java
   // 模糊查询 
   @Test void test4(){ 
       // 查询年龄在 20 ~ 30 岁之间的用户 
       QueryWrapper<User> wrapper = new QueryWrapper<>(); 
       // 左和右 t% 
       wrapper 
           .notLike("name","e") 
           .likeRight("email","t"); 
       List<Map<String, Object>> maps = userMapper.selectMaps(wrapper); 
       maps.forEach(System.out::println); 
   }
   ```

5. 测试五

   ```java
   // 模糊查询 
   @Test void test5(){ 
       QueryWrapper<User> wrapper = new QueryWrapper<>(); // id 在子查询中查出来 
       wrapper.inSql("id","select id from user where id<3");
       List<Object> objects = userMapper.selectObjs(wrapper); 
       objects.forEach(System.out::println); 
   }
   ```

6. 测试六

   ```java
   //测试六 
   @Test void test6(){ 
       QueryWrapper<User> wrapper = new QueryWrapper<>(); 
       // 通过id进行排序 
       wrapper.orderByAsc("id"); 
       List<User> users = userMapper.selectList(wrapper); 
       users.forEach(System.out::println); 
   }
   ```

   

## 7. 代码自动生成器

*dao、pojo、service、controller都给我自己去编写完成！*

AutoGenerator 是 MyBatis-Plus 的代码生成器，通过 AutoGenerator 可以快速生成 Entity、Mapper、Mapper XML、Service、Controller 等各个模块的代码，极大的提升了开发效率。

```java
// 代码自动生成器 
public class KuangCode {
    public static void main(String[] args) { 
        // 需要构建一个 代码自动生成器 对象 
        AutoGenerator mpg = new AutoGenerator(); 
        // 配置策略 
        // 1、全局配置 
        GlobalConfig gc = new GlobalConfig(); 
        String projectPath = System.getProperty("user.dir"); 
        gc.setOutputDir(projectPath+"/src/main/java"); 
        gc.setAuthor("狂神说"); 
        gc.setOpen(false); 
        gc.setFileOverride(false); 
        // 是否覆盖 
        gc.setServiceName("%sService"); 
        // 去Service的I前缀 
        gc.setIdType(IdType.ID_WORKER); 
        gc.setDateType(DateType.ONLY_DATE); 
        gc.setSwagger2(true); 
        mpg.setGlobalConfig(gc); 
        //2、设置数据源 
        DataSourceConfig dsc = new DataSourceConfig(); 
        dsc.setUrl("jdbc:mysql://localhost:3306/kuang_community? useSSL=false&useUnicode=true&characterEncoding=utf-8&serverTimezone=GMT%2B8"); 
        dsc.setDriverName("com.mysql.cj.jdbc.Driver"); 
        dsc.setUsername("root"); 
        dsc.setPassword("123456"); 
        dsc.setDbType(DbType.MYSQL); 
        mpg.setDataSource(dsc); 
        //3、包的配置 
        PackageConfig pc = new PackageConfig(); 
        pc.setModuleName("blog"); 
        pc.setParent("com.kuang"); 
        pc.setEntity("entity"); 
        pc.setMapper("mapper"); 
        pc.setService("service"); 
        pc.setController("controller"); 
        mpg.setPackageInfo(pc); 
        //4、策略配置 
        StrategyConfig strategy = new StrategyConfig(); 
        strategy.setInclude("blog_tags","course","links","sys_settings","user_record"," user_say"); // 设置要映射的表名 
        strategy.setNaming(NamingStrategy.underline_to_camel); 
        strategy.setColumnNaming(NamingStrategy.underline_to_camel); 
        strategy.setEntityLombokModel(true); // 自动lombok； 
        strategy.setLogicDeleteFieldName("deleted"); // 自动填充配置 
        TableFill gmtCreate = new TableFill("gmt_create", FieldFill.INSERT); 
        TableFill gmtModified = new TableFill("gmt_modified", FieldFill.INSERT_UPDATE); 
        ArrayList<TableFill> tableFills = new ArrayList<>(); tableFills.add(gmtCreate); 
        tableFills.add(gmtModified); 
        strategy.setTableFillList(tableFills); // 乐观锁 
        strategy.setVersionFieldName("version");
        strategy.setRestControllerStyle(true);
        strategy.setControllerMappingHyphenStyle(true); // localhost:8080/hello_id_2 mpg.setStrategy(strategy); 
        mpg.execute(); //执行 
    } 
}
```

测试结果