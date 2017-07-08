# result的name属性：
问题： 
实现用户登录时，在Action中`execute()`返回 "success" 就可以呼应结果视图，为什么？       
原因：           
result表示输出结果；       
每个Action返回一个字符串，Struts2会根据这个返回的字符串值来决定响应什么结果；      

## result的name属性：
表示result的逻辑名。   
这个逻辑名会去和Action中返回的字符串匹配，默认取值 "success"          

### result的值： 
指定对应的实际资源位置；        

如下代码：      
```java
public String execute() {
 return "success";
}
```

```
<action name="login" class="com.pb.web.action.UserAction">
   <result name="success">/loginSuccess.jsp</result>
</action>
```

Action默认执行方法就是 `execute()`，这个方法返回了字符串 "success"；       
Struts2就根据这个返回值去和result的name属性匹配；                 
这样匹配成功的话，就可以返回登录成功页面`loginSuccess.jsp`作为结果视图了；      


result的name取值可以是除 "success" 以外的其他字符串；      
Action中可以返回除 "success"以外的其他字符串；             


#### result的name属性小结：     
result的name属性和Action返回的字符串去匹配决定结果视图； 
Action中有很多预定义的字符串常量，可以拿来使用；   
比如： `SUCCESS  INPUT  ERROR `等等；  可以查看源码获取到；           
除了这些字符串常量外，name可以取值任意定义的字符串；只要和Action返回的字符串结果一致就可以了；          
result配置当然可以使用绝对路径，也可以使用相对路径；         
result配置一般使用绝对路径，绝对路径以` (/) `开头；        


### result的type属性
type属性：     
指定了result的类型；            
不同类型的result代表了不同的结果输出；        
默认取值是dispatcher；          
dispatcher是将请求转发(forward)到本应用程序中指定的资源；          
可以在 `struts-default.xml` 中查看 result 的类型； 
其中，还有几个预定义结果类型：          

- redirect ： 请求重定向到指定的 URL        
- redirectAction  ： 请求重定向到指定的Action；    
- chain ： Actin链式处理，将请求转发(forward)到指定的Action；     
- json : 实现Ajax时返回JSON对象给客户端；           


可以自定义结果类型；            


#### result的type属性小结：
##### dispatcher与redirect的区别： 
dispatcher是result的type默认取值；             
dispatcher的后台使用RequestDispatcher类的`forward()`转发请求；          
dispatcher转发形式，参数等资源可以被转发；把参数转发给本资源；          
redirect后台使用HttpResponse的`sendRedirect()`将请求重定向至指定的URL；     
参数等资源将被丢失；            
 
和JSP中学习过的转发和重定向 相似；         


##### redirect 与 redirectAction 的区别：
redirect ：        
redirect跳转页面或Action；  
可以跳转到本应用程序内部或者外部；     

redirectActin：  
redirectActin跳转Action；     
只能跳转本应用程序内部；        


全局结果       
想一想：         
	场景一：
有些操作需要跳转到公共页面；   
比如，注册成功或者登陆不成功时我们都要返回到登录首页面；          

	场景二：
一个应用程序中，包含不同的Action；         
每个Action进行逻辑控制时，可能会遇到异常的情况；此时为了页面友好，我们都需要跳转到一个公共的错误页面；      

解决方案：     
方案一：
	在 `struts.xml` 中的每个 <action> 中都配置一个 <result>   
比如：         
```xml
...省略代码...
<action name="addHouse" ....>
  <result>error.jsp</result>
</action>
<action name="deleteHouse" ...>
  <result>error.jsp</result>
</action>
...省略代码...
```

>弊端：代码繁琐，修改麻烦       


方案二：   
	全局结果 (global-results)  
我们完全可以声明全局结果来解决这个问题；       
全局结果可以满足多个Action共享一个结果；       

```
<package .....>
  <global-results>
    <result name="error">/error.jsp</result>
  </global-results>
 ....
</package>
```

#### 全局结果小结：
使用 <result>来配置，只不过不是在 <action>中嵌套，而是在 <global-results>中嵌套；       
当所有Actin需要共享某个结果时，可以定义为全局结果。      
比如，类似登录页面这样的公共页面、错误页面、异常页面；      
全局结果的影响范围为整个包的所有Action，如果不是对所有Action都有效的结果，不要定义在 <global-results> 中；        

在配置 `struts.xml` 时，各 xml节点顺序不能写错；           
 
操作技巧：                  
查看` struts-2.3.dtd `确定各 xml节点在 `struts.xml` 中的顺序；          
       
当多个result元素出现在 `struts.xml `中时，result会按照一定顺序搜索和匹配；        
### result的搜索顺序：       

1.首先找自己的 <action> 元素内的 <result> 元素是否有匹配的，如果有，就执行这个 result；如果没有，下一步；    
2.其次，再找自己的 <action> 所在的包的全局 result，看看是否有匹配的；如果有，就执行这个result；如果没有，下一步；  
3.再次，递归的寻找自己的包的父包、祖父包中的全局result是否有匹配的；如果有，就执行这个result；如果没有，下一步；  
4.最后，如果上述三种情况都没有匹配的result的话，则抛出 Exception ；   


动态结果            
想一想：         
有些执行结果在配置时并不知道，在运行时刻才能确定。这时该怎么办？     
也就是意味着配置文件中的 <result>元素的值，我们没法确定；            

解决思路：            
在配置结果时使用表达式；        
在运行时刻，struts2框架解析并计算表达式；        
根据表达式的值来动态确定要执行的结果；       
也就是使用 动态结果；                    

要使用这种方式，需要在配置文件中使用表达式： `${}`        

```java
public class UserAction extends ActionSupport {
 private User user;
 private String nextDispose;

 public User getUser() {
   return user;
}
 public void setUser() {
   this.user = user;
}
 public String getNextDispose() {
   return nextDispose;
}
 public void setNextDispose(String nextDispose) {
   this.nextDispose = nextDispose;
}

public String login() {
 if(user.getUserName().equals("common") && user.getPassword().equals("common")) {
      nextDispose = "common";
      return SUCCESS;
    }else if(user.getUserName().equals("admin") && user.getPassword().equals("admin")) {
      nextDispose = "admin";
      return SUCCESS;
    }else {
       return INPUT;
   }
}
}
```
```
<package ....>
  <action name="login" class="com.pb.web.action.UserAction" method="login">
     <result name="success" type="redirectAction">${nextDispose}</result>
     <result name="input">/login.jsp</result>
  </action>
.....
</package>
```

>当 nextDispose = "common" 时，则跳转到 name="common" 的Action；
>当 nextDispose = "admin" 时，则跳转到 name="admin" 的Action；


#### 总结：
name属性：       
result逻辑名；和Action里的返回值匹配，默认 "success"；       

type属性：    
结果类型，默认取值dispatcher；以转发的形式到本应用程序的资源；     

全局结果：       
当遇到一些操作需要返回到公共的页面，比如：登录页面、错误页面等；         
通过 <global-results> 配置全局结果；对包内所有Action都有效的结果；            

动态结果：  
执行结果在配置时并不知道，在运行时刻才能确定；       
通过在配置文件中使用表达式的动态结果实现；          


学习方法：               
复习转发和重定向          
学着去查看 `struts-default.xml  struts-2.3.dtd`