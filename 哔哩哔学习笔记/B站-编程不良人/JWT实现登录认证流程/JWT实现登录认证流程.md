# JWT实现登录认证流程

![image-20201209110308575](assets/image-20201209110308575.png)

## 1. 什么是JWT

> *摘自[官网](https://jwt.io/introduction/):* JSON Web Token (JWT) is an open standard ([RFC 7519](https://tools.ietf.org/html/rfc7519)) that defines a compact and self-contained way for securely transmitting information between parties as a JSON object. This information can be verified and trusted because it is digitally signed. JWTs can be signed using a secret (with the **HMAC** algorithm) or a public/private key pair using **RSA** or **ECDSA**.
>
> *翻译:* JSON Web令牌（JWT）是一个开放标准（RFC 7519），它定义了一种紧凑而独立的方法，用于在各方之间安全地将信息作为JSON对象传输。由于此信息是经过数字签名的，因此可以被验证和信任。可以使用秘密（使用HMAC算法）或使用RSA或ECDSA的公钥/私钥对对JWT进行签名

通俗解释: JWT简称<u>JSON Web Token</u>，也就是通过JSON形式作为Web应用中的令牌，用于在各方之间安全地将信息作为JSON对象传输。在数据传输过程中还可以完成数据加密、签名等相关处理。

## 2. JWT能做什么

### 2.1 授权

这是使用JWT的最常见方案。一旦用户登录，每个后续请求将包括JwT，从而允许用户访问该令牌允许的路由，服务和资源。单点登录是当今广泛使用JWT的一项功能，因为它的开销很小并且可以在不同的域中轻松使用。**前后端系统之间**

### 2.2 信息交换

JSON Web Token是在各方之间安全地传输信息的好方法。因为可以对JWT进行签名（例如，使用公钥/私钥对），所以您可以确保发件人是他们所说的人。此外，由于签名是使用标头和有效负载计算的，因此您还可以验证内容是否遭到篡改。**后端与后端系统之间**



## 3. 为什么是JWT

### 3.1 传统的Session认证

#### 3.1.1 认证方式

我们知道，http协议本身是一种无状态的协议，而这就意味着如果用户向我们的应用提供了用户名和密码来进行用户认证，那么下一次请求时，用户还要再一次进行用户认证才行，因为根据http协议，我们并不能知道是哪个用户发出的请求，所以为了让我们的应用能识别是哪个用户发出的请求，我们只能在服务器存储一份用户登录的信息，这份登录信息会在响应时传递给浏览器，告诉其保存为cookie，以便下次请求时发送给我们的应用，这样我们的应用就能识别请求来自哪个用户了，这就是传统的基于sessioni认证。

#### 3.1.2 认证流程

流程图如下: 

![image-20201209111346927](assets/image-20201209111346927.png)

session认证代码:

```java
@RestController
@RequestMapping("/test")
public class TestController {
    @GetMapping("/test")
    public String test(String userName, HttpServletRequest httpServletRequest){
        httpServletRequest.getSession().setAttribute("username",userName);
        return "login ok~";
    }
}
```

cookie信息:

![image-20201209150210189](assets/image-20201209150210189.png)

#### 3.1.3 暴露的问题

1. 每个用户经过我们的应用认证之后，我们的应用都要在服务端做一次记录，以方便用户下次请求的鉴别，通常而言session都是保存在内存中，而随着认证用户的增多，服务端的开销会明显增大

2. 用户认证之后，服务端做认证记录，如果认证的记录被保存在内存中的话，这意味着用户下次请求还必须要请求在这台服务器上，这样才能拿到授权的资源，这样在分布式的应用上，相应的限制了负载均衡器的能力。这也意味着限制了应用的扩展力。

3. 因为是基于cookie来进行用户识别的，cookie如果被截获，用户就会很容易受到跨站请求伪造的攻击。

4. 在前后端分离系统中就更加痛苦：如下图所示:

   ![image-20201209114816465](assets/image-20201209114816465.png)

   也就是说前后端分离在应用解耦后增加了部署的复杂性。通常用户一次请求就要转发多次。如果用session每次携带sessionid到服务器，服务器还要查询用户信息。同时如果用户很多。这些信息存储在服务器内存中，给服务器增加负担。还有就是CSRF（跨站伪造请求攻击）攻击，session是基于cookie进行用户识别的，cookie如果被截获，用户就会很容易受到跨站请求伪造的攻击。还有就是sessionid就是一个特征值，表达的信息不够丰富。不容易扩展。而且如果你后端应用是多节点部署。那么就需要实现session共享机制。
   不方便集群应用。

### 3.2 基于JWT认证

![image-20201209115226881](assets/image-20201209115226881.png)

#### 3.2.1 jwt认证流程

1. 首先，前端通过web表单将自己的用户名和密码发送到后端的接口。这一过程一般是一个HTTP POST请求。建议的方式是通过SSL加密的传输（https协议），从而避免敏感信息被嗅探。
2. 后端核对用户名和密码成功后，将用户的id等其他信息作为JWT Payload（负载），将其与头部分别进行Base64编码拼接后签名，形成一个JWT.形成的JWT就是一个形同111.zzz，xxx的字符串
3. 后端将JWT字符串作为登录成功的返回结果返回给前端。前端可以将返回的结果保存在localStorage或sessionStorage上，退出登录时前端删除保存的JWT即可。
4. 前端在每次请求时将JWT放入HTTP Header中的Authorization位。（解决XSS和XSRF问题）
5. 后端检查是否存在，如存在验证JWT的有效性。例如，检查签名是否正确；检查Token是否过期；检查Token的接收方是否是自己（可选）。
6. 验证通过后后端使用JWT中包含的用户信息进行其他逻辑操作，返回相应结果。

#### 3.2.2 jwt优势

1. 简洁（Compact）：可以通过URL，POST参数或者在HTTP header发送，因为数据量小，传输速度也很快
2. 自包含（Self-contained）：负载中包含了所有用户所需要的信息，避免了多次查询数据库
3. 因为Token是以JSON加密的形式保存在客户端的，所以JWT是跨语言的，原则上任何web形式都支持。
4. 不需要在服务端保存会话信息，特别适用于分布式微服务。



## 4. JWT的结构是什么

### 4.1 令牌组成

1. 标头(Header)
2. 有效载荷(Payload)
3. 签名(Signature)

因此，JWT通常如下所示：

- xxxxx.yyyyy.zzzzz 

  `Header.Payload.Signature`

### 4.2 Header

标头通常由两部分组成：令牌的类型（即JWT）和所使用的签名算法，例如HMAC SHA256或RSA.它会使用Base64编码组成JWT结构的第一部分。

注意：Base64是一种编码，也就是说，它是可以被翻译回原来的样子来的。它并不是一种加密过程。

```json
{
    "alg":"HS256", //加密/签名算法
    "typ":"JWT" //类型,目前只有JWT类型
}
```



### 4.3 Payload

令牌的第二部分是有效负载，其中包含声明。声明是有关实体（通常是用户）和其他数据的声明。同样的，它会使用Base64编码组成JWT结构的第二部分

注意: 官方声明,不要在这里放用户的敏感信息,比如用户密码

```json
{
    "sub":"1234567890",
    "name":"John Doe",
    "admin":ture
}
```

### 4.4 Signature

前面两部分都是使用Base64进行编码的，即前端可以解开知道里面的信息.Signature需要使用编码后的header和payload以及我们提供的一个密钥(随机盐)，然后使用header中指定的签名算法（HS256）进行签名。签名的作用是保证JWT没有被篡改过

![image-20201209151928290](assets/image-20201209151928290.png)

如：
`HMACSHA256（base64UrlEncode（header）+"."+ base64UrlEncode（payload），secret）；`

**签名目的:**

- 最后一步签名的过程，实际上是对头部以及负载内容进行签名，防止内容被窜改。如果有人对头部以及负载的内容解码之后进行修改，再进行编码，最后加上之前的签名组合形成新的JWT的话，那么服务器端会判断出新的头部和负载形成的签名和JWT附带上的签名是不一样的。如果要对新的头部和负载进行签名，在不知道服务器加密时用的密钥的话，得出来的签名也是不一样的。

**信息安全问题:**

- 在这里大家一定会问一个问题：Base64是一种编码，是可逆的，那么我的信息不就被暴露了吗？
- 是的。所以，<u>在JWT中，不应该在负载里面加入任何敏感的数据</u>。在上面的例子中，我们传输的是用户的User ID，这个值实际上不是什么敏 感内容，一般情况下被知道也是安全的。但是像密码这样的内容就不能被放在JWT中了。如果将用户的密码放在了JWT中，那么怀有恶意的第三方通过Base64解码就能很快地知道你的密码了。因此JWT适合用于向Web应用传递一些非敏感信息。JWT还经常用于设计用户认证和授权系 统，甚至实现web应用的单点登录。

### 4.5 图示jwt组成

jwt在没有经过base64编码的时候的组成如下图所示:

![image-20201209152638300](assets/image-20201209152638300.png)

上图这些信息通过base64编码后放在一起就是下面这种形式

![image-20201209152929162](assets/image-20201209152929162.png)

编码后拼接成字符串的形式有如下优点:

1. 输出是三个由点分隔的Base64-URL字符串，可以在HTML和HTTP环境中轻松传递这些字符串，与基于XML的标准（例如SAML）相比，它更紧凑。
2. 简洁（Compact）
	可以通过URL，POST参数或者在HTTP header发送，因为数据量小，传输速度快
3. 自包含（Self-contained）: Payload自己包含业务中所需要的非敏感数据,例如用户名
	负载中包含了所有用户所需要的信息，避免了多次查询数据库



## 5. 普通Java环境中使用JWT 🚩

引入依赖

```xml
<!--jwt-->
<dependency>
    <groupId>com.auth0</groupId>
    <artifactId>java-jwt</artifactId>
    <version>3.4.0</version>
</dependency>
```

### 5.1 生成token令牌

```java
    //生成令牌
    @Test
    public void creatToken(){
        Map<String, Object> map = new HashMap<>();
        Calendar instance = Calendar.getInstance();
        instance.add(Calendar.SECOND,200); //200秒
        //生成token
        String token = JWT.create()
                .withHeader(map) //jwt的header部分
                .withClaim("username","张三") //jwt的payload部分
                .withClaim("userId",1)
                .withClaim("sex","男")
                .withClaim("age",22)
                .withExpiresAt(instance.getTime()) // 设置token的过期时间
                .sign(Algorithm.HMAC256("TOKEN!$#%@WRRET"));//jwt的sin部分,解析令牌会使用到这里面自定义的字符串

        //打印token
        System.out.println(token);
        /*打印结果
        * eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzZXgiOiLnlLciLCJleHAiOjE2MDc1MDE3NDYsInVzZXJJZCI6MSwiYWdlIjoyMiwidXNlcm5hbWUiOiLlvKDkuIkifQ.70hZbckEs-aDrIxD_mFNT1d0nei0XDOqM1gAJAppbxY
         * */
    }
```



### 5.2 解析token令牌

```java
//解析令牌
    @Test
    public void analysisToken(){
        String token = "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzZXgiOiLnlLciLCJleHAiOjE2MDc1MDE3NDYsInVzZXJJZCI6MSwiYWdlIjoyMiwidXNlcm5hbWUiOiLlvKDkuIkifQ.70hZbckEs-aDrIxD_mFNT1d0nei0XDOqM1gAJAppbxY\n";
        JWTVerifier jwtVerifier = JWT.require(Algorithm.HMAC256("TOKEN!$#%@WRRET")).build(); //构建验签对象(会验证算法和签名两个部分)
        DecodedJWT decodedJWT = jwtVerifier.verify(token); //解析token信息
        //打印token中的信息
        System.out.println("用户名:"+decodedJWT.getClaim("username").asString());
        System.out.println("用户id:"+decodedJWT.getClaim("userId").asInt());
        System.out.println("用户性别:"+decodedJWT.getClaim("sex").asString());
        System.out.println("用户年龄:"+decodedJWT.getClaim("age").asInt());
        System.out.println("token过期时间:"+decodedJWT.getExpiresAt());
        /**
         * 用户名:张三
         * 用户id:1
         * 用户性别:男
         * 用户年龄:22
         * token过期时间:Wed Dec 09 16:15:46 CST 2020
         */
    }
```

### 5.3 token解析常见异常

- SignatureVerificationException：签名不一致异常
- TokenExpiredException：令牌过期异常
- AlgorithmMismatchException：算法不匹配异常
- InvalidClaimException：失效的payload异常







## 6. 封装工具类-JWTUtils

为简化后续使用jwt的开发,将jwt封装一个工具类,该工具类中所包含的方法需要包含三个功能

- 根据传入的信息封装生成token的方法
- 验证token
- 获取token中的payload信息

```java
/**
 * @program: jwtdemo
 * @description: JWTUtils工具类
 * @author: XiYang
 * @create: 2020-12-09 16:59
 **/
public class JWTUtils {
    //自定义token签名
    private static String tokenCode = "TOKEN!$#%@WRRET";

    /**
     * 生成token
     * @param map 传入一个map,map存放外面传入的信息
     * @return 返回token令牌
     */
    public static String getToken(Map<String,String> map){
        JWTCreator.Builder builder = JWT.create(); //构建jwt对象
        map.forEach((k,v)->{ //将传入的对象信息添加到JWT的payload中
            builder.withClaim(k,v);
        });
        Calendar instance = Calendar.getInstance();//构建jwt的过期时间为7天
        instance.add(Calendar.SECOND,7);
        builder.withExpiresAt(instance.getTime());//设置token的过期时间
        String token = builder.sign(Algorithm.HMAC256(tokenCode));//设置token的sign部分的算法和签名
        return token;
    }

    /**
     * 验证传入令牌的(算法和签名)
     * @param token
     */
    public static void verify(String token){
        //如果验签有问题,会直接爆出异常
        JWT.require(Algorithm.HMAC256(tokenCode)).build().verify(token);//验证传入的token令牌的正确性
    }

    /**
     * 获取token中的payload里面包含的信息
     * @param token 传递token令牌
     * @return payload包含的信息
     */
    public static DecodedJWT getTokenPayload(String token){
        DecodedJWT verify = JWT.require(Algorithm.HMAC256(tokenCode)).build().verify(token);
        return verify;
    }
}
```

工具类测试:

```java
    @Test
    public void JwtUtilsTest(){
        JWTUtils jwtUtils = new JWTUtils();
        Map<String, String> user = new HashMap<>();
        user.put("UserSex","男");
        user.put("UserAge","18");
        user.put("userName","张三");
        //生成token
        String token = JWTUtils.getToken(user);
        //验证token
        JWTUtils.verify(token);
        //获取token中payload的内容
        DecodedJWT tokenPayload = JWTUtils.getTokenPayload(token);
        System.out.println(tokenPayload.getClaim("UserSex").asString());
        System.out.println(tokenPayload.getClaim("UserAge").asString());
        System.out.println(tokenPayload.getClaim("userName").asString());
        /**
         * 男
         * 18
         * 张三
         */
    }
```



## 7. SpringBoot整合JWT实现登录流程

搭建SpringBoot+MyBatis+JWT环境

pom.xml文件

```xml
        <!--sringboot启动器-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>
        <!--springbootweb启动器-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!--jwt-->
        <dependency>
            <groupId>com.auth0</groupId>
            <artifactId>java-jwt</artifactId>
            <version>3.4.0</version>
        </dependency>
        <!--mybatis-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.2.7</version>
        </dependency>
        <!--lombok-->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
        <!--druid连接池-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.1.10</version>
        </dependency>
        <!--引入mysql-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
```

application.yml文件编写

```yml

```





## 参考

> 视频地址: https://www.bilibili.com/video/BV1i54y1m7cP
>
> 我是编程不良人， JWT前后端分离系统的认证方案、单点登录系统的认证方案！ JWT整合springboot完成认证、以及认证优化~ 基于客户端的存储认证标记的解决方案！



