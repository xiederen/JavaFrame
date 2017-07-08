# 访问Servlet API
Struts2的Action实现了 MVC 中 C层的作用；        
在C层中要处理请求必然会需要访问 `Servlet API`；         
比如：       

针对请求向用户显示所在城市信息；这时需要根据请求中的IP地址判断出所在城市后，把相关的城市信息保存到请求中，再显示给用户；      
当用户登录成功后，要保存这个用户的信息；这个信息一般是保存在session会话中；       
保存当前在线人数；把数据保存在application中；           
像这些情况，我们都需要访问`Servlet API`；       
 
我们以前学习JSP的时候，使用的`Servlet API`是传统的类型；        
也就是说，访问request请求的时候，使用的类型是 HttpServletRequst；         
访问session会话的时候，使用的是 HttpSession 类型；            
访问 application的时候，使用的是 ServletContext类型；                   

在Struts2中，已经对传统的`Servlet API` 类型进行了封装；           
Struts2中传统`Servlet API`类型被处理成Map类型；           
这样可以让我们更方便的访问request、session和application这些对象；         
这种做法就可以让Struts2和传统`Servlet API`不再关联在一起；          
Struts2不再依赖像 HttpServletRequest这样的传统`Servlet API`类型；这也被称为 "解耦合"       
不依赖传统 `Servlet API`类型 -- 解耦合              


## 访问Servlet API
第一种解耦合方式访问`Servlet API` :           
在Action中访问`Servlet API`，我们可以使用 ActionContext类；     

### 关于 ActionContext ：           
所在包 ： `com.opensymphony.xwork2`                
ActionContext通常被称为Action上下文或Action环境；        
ActionContext可以提供每个Action运行时与之相关的所有信息；就是提供了Action运行时的周边环境；       
当前这个Action可以访问哪些数据、有哪些参数可以访问呀，等等；这些都算是Action运行的环境信息；               

##### 如何通过 ActionContext 访问 Servlet API ：   
ActionContext类提供`getContext()`方法获得实例；             
获得ActionContext的实例后，就可以通过ActionContext类的实例，获得Map类型的请求、会话等等这些对象来访问；       
得到Map类型的请求、会话或是application以后，通过 `put()` 和 `get()`  方法在Map类型的对象中存取数据；          


## 小结：
Struts2中使用`Servlet API`步骤：          

首先提交表单来访问Action； 
然后在配置文件中找到对应的Action类和方法；         
##### 在Action类中先声明Map类型的request等需要使用的对象：       
```
private Map<String,Object> request;
private Map<String,Object> session;
private Map<String,Object> application;
```
##### 获得ActionContext实例：
使用ActionContext类的`getContext()`方法；        
```
ActionContext ac=ActionContext.getContext();
```

##### 获取request等对象：    
通过ActionContext类的对象来调用get方法，把request作为参数，这样就可以获得request对象；`request = (Map<String,Object>)ac.get("request");`            

>注意：需要做类型转换；

##### 获取session和application对象：
调用`getSession()`方法就可以得到session对象：
```
session = ac.getSession();
```

##### 调用getApplication()方法就能得到application对象：
```
application = ac.getApplication();
```
得到这些对象以后就可以访问它们了；     

##### 在Map类型中存取数据：         
用`put()`方法就可以把数据以键值对的方式存进去了；        
```
request.put("loginAddress","您本次登录地点是：北京");
```

##### 取出Map中的数据用 get()方法：
```
request.get("loginAddress")
```

只要在`get()`方法中，把键写对了就行；      

##### 在JSP页面中获取保存数据提倡的方式：
```
<s:property/>标签：
<s:property value="#request.loginAccess"/>
```
>注意：不要忘记加 "#"；


### 以IoC的方式访问Servlet API
刚才的request、session和application对象，都是由我们自己调用方法来获取到的；      
自己获得 `request : ac.get("request")`       
自己获得 `session : ac.getSession()`          
都是我们自己想办法调用方法得到的对象；           

有一种简单的方法，可以让"别人"为你提供`Servlet API`对象；这种办法是：       
通过实现相应的接口来获得 `Servlet API`对象；          
```
获得 request对象，需要实现 RequestAware接口； 
获得 session对象，需要实现 SessionAware接口；         
获得application对象，需要实现 ApplicationAware接口； 
```

使用这种方式，在你的程序中，想获得什么对象，只要按照需求实现接口就能获取到对应的对象了；    

这种方式，叫做，`控制反转`，就是 `IoC`；也可以叫做，`依赖注入`，就是 `DI`      

### 控制反转 IoC (Inversion of Control)   
控制反转就是说，本来正常情况都是自己控制如何得到对象，如何实例化对象； 
但是现在不是自己控制了，是由类的外部，通过接口的方式为你提供了对象；      
对于对象的控制部分已经不在本类的内部了，而转移到了本类的外部了；            
这就是，对象的控制权发生了转移，称为控制反转；            

>控制反转还有另一个名字，就是 依赖注入；

### 依赖注入 DI (Dependency Injection)   
本类所依赖的对象是在类的外部注入到本类中来使用的；           
依赖注入减小了程序的耦合、提高了内聚；          
更加方便程序的移植、维护、扩展和测试；           


刚才用到的几个接口名字中都有个Aware；          
Aware的意思是“知道的，明白的，意识到的”；          
RequestAware就应该知道请求request对象；           
这样，我们就不必自己去动手获取request了；让它帮我们注入request对象就可以了；           
那么，注入是在哪里实现的呢？             
这几个Aware接口所在的包是 struts的interceptor包：               
```
org.apache.struts2.interceptor包；
```

interceptor是，`拦截器`的意思；      
Struts2是通过拦截器来完成对象注入工作的；       
拦截器和过滤器有点像，就是在你访问资源之前和访问资源之后，能帮你做些事情；        

有了这些接口，我们开发时需要哪个`Servlet API`对象，只要根据需求实现相应的接口就可以了；  
并实现接口中相应的方法；      
不需要使用的对象也不用去实现那个接口；   
通过这种IoC的方式就可以完成对象的获取，使用起来非常的方便；         

>提倡使用这种IoC的方式；


## 耦合方式访问Servlet API      
按照在Action中是否使用传统`Servlet API`类型，可以分为解耦合方式和耦合方式访问`Servlet API`    
- 解耦合方式：          
不使用传统`Servlet API`类型，而是使用Map类型；        

- 耦合方式： 
使用传统`Servlet API`类型；       
但这种情况并不多；   
假设你非要调用HttpSession中的某个方法，就可以使用这种耦合的方式；        
耦合的时候，访问request是使用HttpServletRequest；         
访问session时，使用HttpSession；                
访问application时，使用ServletContext；             
这时也有IoC和非IoC两种方式；         
这两种方式并不难；      


## 总结
解耦合方式访问`Servlet API`     
我们使用的`Servlet API`类型是Struts2为我们封装的Map类型；     
在使用非IoC方式的时候，我们需要用到ActionContext类；自己来获得需要用到的对象；     
在使用IoC方式的时候，我们需要使用到的是：    
```
RequestAware接口、SessionAware接口、ApplicationAware接口；      
```
通过实现接口就可以获得我们需要的对象；  

耦合方式访问`Servlet API`只需要了解就可以了；    

控制反转(IoC) 也称为 依赖注入(DI)          
就是说，本该由本类控制的事，发生了反转，转移到了本类之外，通过注入的方式为本类提供了对象；     

在页面中获取保存数据提倡的方式是：         
<s:property>标签            
```
<s:property value="#request.loginAccess"/>
```