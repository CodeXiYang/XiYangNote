# JWT(json web token)



## 登录存session



## 有状态,无状态



## 单点登录(sso)



## jwt是什么

java web token;用于生成token令牌的

jwt是加密之后的json数据



## jwt字符串

通过jwt构成的json字符串会是`header.body.key(钥匙)这样的形式`;header表示整个信息的header部分,body表示整个信息的主体部分,key表示整个信息的钥匙.后端会根据这个钥匙进行解析匹配,看通过这个钥匙解析出来的内容和之前的内容是否相同

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ
```



## 分析jwt字符串都构成

JWT是由三段信息构成的，将这三段信息文本用.链接一起就构成了Jwt字符串;第一部分称头部(header),第二部分叫载荷(plyload),第三部分是签证(signature)





## jjwt

**jjwt** ( java jwt ) ,用于生成jwt字符串的工具类 [github地址](https://github.com/jwtk/jjwt)



jjwt的基本使用

[code-01] 

[code-01-text]

创建maven项目,导入如下依赖

```xml
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-api</artifactId>
    <version>0.11.2</version>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-impl</artifactId>
    <version>0.11.2</version>
    <scope>runtime</scope>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-jackson</artifactId> <!-- or jjwt-gson if Gson is preferred -->
    <version>0.11.2</version>
    <scope>runtime</scope>
</dependency>
```

原始使用:

```java
    @Test
    public void jjwtUtilTest(){
        //----------生成jwt字符串
        Map<String, Object> map = new HashMap<String,Object>();
        map.put("id","1");
        map.put("name","admin");
        Key key = Keys.secretKeyFor(SignatureAlgorithm.HS256); //锁(加密token和解密token的关键,必须是同一把锁)
        String token = Jwts.builder() //建造一个token
            .setClaims(map) //设置主体
            .setExpiration(new Date(System.currentTimeMillis()+60*60*1000)) //设置过期时间,设置过期时间1h
            .signWith(key) //生成签证
            .compact(); //建造出来
        System.out.println(token);
        //eyJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiYWRtaW4iLCJpZCI6IjEiLCJleHAiOjE1OTczMjM1NDd9.u36WAyhA3u4Ffkx22LcrlFzzb2lXEwHFmRQ5Ns5-Rag

        //解析jwt字符串
        Jws<Claims> claimsJws = Jwts.parserBuilder() //建造一个解析器
                .setSigningKey(key) //开锁
                .build() //建造出来
                .parseClaimsJws(token);//将jwt解析出来
        //通过解析出来的东西拿到自己想要的东西
        Claims body = claimsJws.getBody();
        System.out.println(body);
        //可以通过具体的key得到指定的值
        Object name = body.get("name");
        Object id = body.get("id");
        System.out.println(name);
        System.out.println(id);
    }
    /*
    * 执行结果:
    *   eyJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiYWRtaW4iLCJpZCI6IjEiLCJleHAiOjE1OTczMjQyNjN9.5czR-fO7gqQjTV-yixIO5vqecK9Tb1g8qQuvHgrO_dw
    *   {name=admin, id=1, exp=1597324263}
    *   admin
    *   1
    * 注意点:
    *   1. 加密解密必须是同一个key
    *   2. 如果时间太短,解密前如果都过期了,也是无法解密的
    * */
```

将加密和解析过程封装成两个方法

```java
static Key key;
static {
    key = Keys.secretKeyFor(SignatureAlgorithm.HS256);
}
//加密
public static String createToken(Map<String,Object> body,Integer mill){
    return Jwts.builder()
        .setClaims(body)
        .setExpiration(new Date(System.currentTimeMillis()+mill*60*1000))
        .signWith(key)
        .compact();
}
//解析
public static Claims parseToken(String token){
    return Jwts.parserBuilder()
        .setSigningKey(key)
        .build()
        .parseClaimsJws(token)
        .getBody();
}
```

idea基于maven在其他项目中想要使用这个工具的过程





