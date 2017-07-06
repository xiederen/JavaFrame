# Struts2总结：
Struts2是一种基于MVC的Web框架，能将请求和展现的视图分开；   
通过 `struts.xml` 核心配置，通过Action控制业务；通过 Result 匹配相应的结果视图；       

## Action总结 
对应 MVC 中的 C(核心控制器和业务控制器)中的业务控制器；        

### Action作用：
返回结果字符串，控制业务；    
方便的处理数据(接收参数、数据封装、值传递)        
### 创建Action：   
普通的Java类；         
实现Action接口；     
继承ActionSupport类；        
继承ActionSupport类是最常用的，而且使用后两者可以使用其预定义的字符串常量作为结果字符串返回，比如：`SUCCESS INPUT ERROR...`       

#### 接收参数
1.属性方式           
2.JavaBean方式          
3.ModelDriven方式           

#### Action定义方法
1.默认执行`execute()`，也可以自定义与其签名相同的方法      
2.可以通过 `action!method` 的方式动态方法调用            
3.也可以通过 <action> 的method属性指定           
4.规则定义明确的前提下，可以使用`*`通配符；                    


### Result总结：    
Action的方法会返回一个字符串，Struts2根据这个值来决定响应什么结果；       
这个输出结果就是 Result；            

name属性： 和Action里的返回值匹配，默认success；            
type属性：结果类型，默认dispatcher；           
全局结果 `<global-results>` 可以满足多个 <action> 共享一个结果；         
比如：公共页面、错误页面等等；                 


### struts.xml总结
`struts.xml`是Strut2的核心配置文件；          
Struts2正是通过`struts.xml`配置文件页面导航，便于后期维护；            

##### 配置strut.xml时的操作小技巧：
设置XML自动提示；     
关联源码；           
设置开发模式         
```
  <constant name="struts.devMode" value="true" />
```
其中要设置的常量，可以通过查看`default.properties`这个文件获得；       
XML节点顺序正确与否去查看对应dtd文档；           
 


#### struts.xml
在`struts.xml`中，<package>元素可以把逻辑上相关的一组Action、Result等元素封装起来，形成一个独立的模块，用来避免同名Action的冲突；                
如果同一个包下，配置有多个name相同的<action>，则后一个会把前一个覆盖掉；            
package可以继承其他的package；也可以作为父包被其他的package继承；         
namespace指包的命名空间，它是和URL相对应的；             
可是，假设namespace取值为空字符串或者不配置时，                

#### namespace
`struts.xml`中 <package> 的 namespace：     
如果在 `struts.xml` 中对 namespace 进行了配置，在输入URL进行访问时，也需要加上命名空间；      
比如：         
```
struts.xml : <package namespace="/user"...>
```
则访问的URL:`http://localhost:8080/Struts2_05login/user/login `                 
访问的URL中需要加上 `/user`；         
namespace也可以省略不写；            
取值为` " "` 或者不配置时，代表不管路径有多深，只要Action的名字匹配上就可以；       
namespace默认取值就是 空字符串` " " `              
尽管namespace可以不配置，但是一般都会配置，并且：            
一般以模块进行命名；              
比如，用户管理的，可以为namespace取值为 user；            


### Action搜索顺序
问题：        
为什么用户登录不成功时，每次访问URL要不断的追加 `/user`  ?        
这个看上去错误的URL，可是运行效果不受影响，为什么？              
怎么避免这种情况出现？              

方法一：            
修改表单提交路径为绝对路径：            
```
<form action="<%=request.getContextPath()%>/user/login" method="post">
```

方法二：                
表单提交路径不修改，但是：          
1.页面中添加如下代码：       
```jsp
<%
String path = request.getContextPath();
String basePath = request.getScheme()
+"://"+request.getServerName()+":"+request.getServerPort()+path+"/"; %>
```

2.<head>中添加如下代码，为页面添加基链接：       
```
<base href="<%=basePath%>">
```

问题1：为什么用户登录不成功时，每次访问URL不断的追加 `/user` ?  
因为JSP页面中表单提交写的是相对路径：           
```
<form action="user/login" method="post">
```

问题2：这个看上去错误的URL，可运行效果不受影响，为什么？           
这是由Action的搜索顺序决定的；      
示例：         
对于来自客户端的请求URL是：` http://localhost:8080/p1/p2/p3/login`        

### struts2的搜索顺序是：      
1.首先搜索namespace为 `/p1/p2/p3`    的package，如果存在这个package，则在其中寻找name为login的action；如若这个package不存在则转至步骤2；   

2.搜索namespace为 `/p1/p2` 的package，如果存在这个package，则在其中寻找name为login的action；如若这个package不存在则转至步骤3；

3.搜索namespace为 `/p1` 的package，如果存在这个package，则在其中寻找name为login的action；如若这个package不存在则转至步骤4；      

4.到默认namespace的package里面去搜索此action，所谓默认namespace的package，就是没有namespace或者namespace为空字符串 `(" ")` 的package；如若在默认namespace的package里面还是找不到该action，则报404提示找不到此命名空间和action。   


问题3：怎么避免这种情况出现？        

方法一：       
修改表单提交路径为绝对路径：     
```
<form action="<%=request.getContextPath()%>/user/login" method="post">
```

方法二：         
表单提交路径不修改，但是：            
1.页面中添加如下代码：           
```jsp
<%
String path = request.getContextPath();
String basePath = request.getScheme()
+"://"+request.getServerName()+":"+request.getServerPort()+path+"/"; %>
```

2.<head>中添加如下代码，为页面添加基链接：        
```
<base href="<%=basePath%>">
```

为了避免路径错误，尽量写绝对路径；


### Struts2异常机制     
通过在 `<action>` 元素中设置 `<exception-mapping>` 元素；这种是局部异常映射，只对该Action有效；           
#### <exception-mapping> :  

局部异常映射。            
指定在Action方法抛出指定异常的时候跳转到哪个指定的结果视图；        

使用Struts2的异常机制后，就不需要手动去`try-catch`了；

##### <exception-mapping> 元素的属性：

result属性：发生异常后要跳转到的结果视图；            
exception属性 ：指定了一个Exception的全类名；         

如果Action的方法抛出的异常是这个Exception类或子类的实例，就会跳转到result指定的结果去。 

当然，如果各种异常发生时，都要跳转到统一的一个异常处理结果视图，这时可以通过 `<global-exception-mappings>` 设置全局异常映射；           

##### `<global-exception-mappings> ：` 全局异常映射 

是 <package>元素的子元素，必须声明在 <package>元素内；       
既然要跳转到一个统一的结果，肯定要配置一个全局结果 `<global-results>`        
`<global-results>` 应该在 `<global-exception-mappings>` 之前；    
可以去查看dtd确定它们的顺序；      
如果这个结果要对其他包内的Action有效，可以把全局异常拿到一个父包里去，被其他包继承即可；            


在action中的相应方法上用 throws 抛出异常；               
```
<exception-mapping> :
<package ....>
 <global-results>
     <result name="error">/error.jsp</result>
 </global-results>
<action ....>
   ....
  <exception-mapping result="error" exception="java.lang.NullPointerExcpetion">
</action>
</package>
```

当action中发生空指针异常时，则跳转到error.jsp页面；           
```
<global-exception-mappings>
<package ....>
 <global-results>
     <result name="error">/error.jsp</result>
 </global-results>
<global-exception-mapppings>
   ....
  <exception-mapping result="error" exception="java.lang.NullPointerExcpetion">
</global-exception-mappings>
......
</package>
```

页面获得异常信息：      
```
 <s:property value="exception"/>     输出异常基本信息
 <s:property value="exceptionStack"/>   输出异常堆栈信息
```


总结：
namespace：
代表包的命名空间；
默认取值为 空字符串` " "` ，建议还是以模块为其命名；

Action搜索顺序；

Struts的异常机制：
```
 <excpetion-mapping> 局部异常
 <global-exception-mappings> 全局异常
 ```
