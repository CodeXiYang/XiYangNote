# Filter过滤器

## 1. Filter作用

Filter：过滤器 ，用来过滤网站的数据；

- 处理中文乱码
- 登录验证….

![image-20201219155633649](assets/image-20201219155633649.png)

## 2. Filter开发步骤

1. 导包

2. 编写过滤器

   - 导包不要错

     实现Filter接口，重写对应的方法即可

     ```java
     public class CharacterEncodingFilter implements Filter { 
         //初始化：web服务器启动，就以及初始化了，随时等待过滤对象出现！
         public void init(FilterConfig filterConfig) throws ServletException {
             System.out.println("CharacterEncodingFilter初始化"); 
         }
         //Chain : 链 
         /*
         1. 过滤中的所有代码，在过滤特定请求的时候都会执行
         2. 必须要让过滤器继续同行
         chain.doFilter(request,response);
         */ 
         public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
             request.setCharacterEncoding("utf-8"); 
             response.setCharacterEncoding("utf-8"); 
             response.setContentType("text/html;charset=UTF-8"); 
             System.out.println("CharacterEncodingFilter执行前....");
             chain.doFilter(request,response);//让我们的请求继续走，如果不 写，程序到这里就被拦截停止！ 
             System.out.println("CharacterEncodingFilter执行后...."); }
         //销毁：web服务器关闭的时候，过滤会销毁 
         public void destroy() { 
             System.out.println("CharacterEncodingFilter销毁");
         } 
     }
     ```

3. 在web.xml中配置 Filter

   ```xml
   <filter> 
       <filter-name>CharacterEncodingFilter</filter-name>
       <filter-class>com.kuang.filter.CharacterEncodingFilter</filter- class>
   </filter>
   <filter-mapping> 
       <filter-name>CharacterEncodingFilter</filter-name>
       <!--只要是 /servlet的任何请求，会经过这个过滤器--> 
       <url-pattern>/servlet/*</url-pattern>
       <!--<url-pattern>/*</url-pattern>--> 
   </filter-mapping>
   ```

   