# Listener监听器

*实现一个监听器的接口；（有N种）*

1. 编写一个监听器

   实现监听器的接口… 

   ```java
   //统计网站在线人数 ： 统计session 
   public class OnlineCountListener implements HttpSessionListener {
       //创建session监听： 看你的一举一动
       //一旦创建Session就会触发一次这个事件！ 
       public void sessionCreated(HttpSessionEvent se) { 
           ServletContext ctx = se.getSession().getServletContext(); 
           System.out.println(se.getSession().getId());
           Integer onlineCount = (Integer) ctx.getAttribute("OnlineCount");
           if (onlineCount==null){
               onlineCount = new Integer(1);
           }else { 
               int count = onlineCount.intValue(); 
               onlineCount = new Integer(count+1); 
           }ctx.setAttribute("OnlineCount",onlineCount);
       }
       //销毁session监听
       //一旦销毁Session就会触发一次这个事件！ 
       public void sessionDestroyed(HttpSessionEvent se) { 
           ServletContext ctx = se.getSession().getServletContext(); 
           Integer onlineCount = (Integer) ctx.getAttribute("OnlineCount");
           if (onlineCount==null){
               onlineCount = new Integer(0);
           }else {
               int count = onlineCount.intValue();
               onlineCount = new Integer(count-1); 
           }ctx.setAttribute("OnlineCount",onlineCount);
       }
       /*Session销毁：
       1. 手动销毁 getSession().invalidate(); 
       2. 自动销毁
       */
   }
   ```

2. web.xml中注册监听器

   ```xml
   <!--注册监听器--> 
   <listener> 
       <listener-class>com.kuang.listener.OnlineCountListener</listener- class> 
   </listener>
   ```

3. 看情况是否使用！