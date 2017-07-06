# 使用method属性
访问Action时，默认调用的是 `execute()` 方法；
```java
public class HouseAction extends ActionSupport {
 public String execute() {
  return "success";
 }
}
```

其实，这个方法不是必须的，我们可以自己定义Action对应访问的方法；  
比如，我们可以在Action类中添加这样的方法：     
```java
public class HouseAction extends ActionSupport {
 public String add() {
  return "success";
 }
}
```

然后，在`struts.xml`中的action标签里配置method属性,来指定Action需要调用的方法：      
```
<action name="house_add" class="com.pb.web.action.HouseAction" method="add" >
```
这样的话，可以在Action中定义多个业务方法；    
这些业务方法的格式要与`execute()`方法相同，仅方法名不同；也就是说，要有相同的方法签名；            
然后在`struts.xml`中配置多个action标签；在action标签中使用method属性指定要调用的业务方法是哪个；      
在提交请求访问的时候，就可以根据action的配置，实现对不同业务方法的调用；                  


访问Action时，默认调用的是`execute()`方法；      
可以在Action中定义多个业务方法；           
	方法签名要与`execute()`方法相同；
配置多个`<action>`标签
	method属性指定调用的方法
```java
public class HouseAction extends ActionSupport {
 public String add() {
  return "success";
 }
  public String update() {
  return "success";
 }
}
```

```
<action name="house_add" class="com.pb.web.action.HouseAction" method="add" >
<action name="house_update" class="com.pb.web.action.HouseAction" method="update" >
```

### 使用method属性小结：
使用method属性可以指定这个Action对应的业务方法是哪一个； 
这样的话，就可以配置多个Action来对应一个Action类中的多个业务方法；       

##### 带来的问题：
这样的话，每个业务方法都要有一个对应的Action配置； 
需要配置的Action会过多；`struts.xml`中的内容会变得非常的庞大；       

##### 如何解决：     
我们只要使用`<include>`标签就可以解决这个问题；     
首先，我们按模块在不同的xml文件中配置Action；     
然后，在`struts.xml`中使用<include>标签包含各个xml文件，就可以分散众多的Action配置；       
struts_house.xml :
```xml
 <package  ...>
     <action  ...>
     </action ...>
 </package>
struts.xml :
 <struts>
   .....
    <include file="struts_house.xml"/>
   .....
 </struts>
```

这样做的特点是：          
配置文件的结构清晰直观，减少各个模块配置内容的相互干扰，每个模块的配置可以各司其职； 
   
使用method属性指定需要调用的业务方法，配置的Action确实是比较多的；     


使用动态方法调用
```
DMI (Dynamic Method Invocation)
```
动态方法调用也可以在一个Action类中有多个业务方法；    
只需要为这个Action类配置一个 `<action> `标签          
而且，不需要使用method属性，就可以指定需要调用的业务方法；    
使用这种动态方法调用的时候，需要注意的是：        
调用时指出 Action名和业务方法；            
在请求的时候，除了要指明Action以外，还要在后面写 `!` 号再加上业务方法的名字；        
如下请求：         
```
http://localhost:8080/struts2_03DMI/house!add
```

这个请求指出要访问的是house这个Action的`add()`方法；    
```java
public class HouseAction extends ActionSupport {
  public String add() {
     return "success";
 }
  public String update() {
     return "success";
 }
}
```

```
<action name="house" class="com.pb.web.action.HouseAction">

http://localhost:8080/struts2_DMI/house!add
```

在Form表单元素中：             
```html
<form action="house!add" method="post">
  <input type="submit" value="添加房屋信息"/>
</form>
<br/>
<form action="house!update" method="post">
  <input type="submit" value="修改房屋信息"/>
</form>
```

相应的url为：           
```
http://localhost:8080/struts2_DMI/house!add
http://localhost:8080/struts2_DMI/house!update
```

##### 动态方法调用的特点：
使用动态方法调用，可以在一个Action类中有多个业务方法；       
一个Action类只需要配置一个`<action>`标签；  
它不需要使用method属性；这点与使用method属性相比，可以减少Action的配置数量；      
请求时需要指明要访问的Action和需要调用的方法；          
     
动态方法调用可能会带来安全隐患，暴漏一些方法给用户；官方也不推荐使用这种方式；             

我们可以在 `struts.xml` 中加入如下这个设置来禁用动态方法调用：        
```
<constant name="struts.enable.DynamicMethodInvocation" value="false" />
```

请求后面的` .action `可以省略；       


使用通配符          
使用通配符简化 `<action> `的配置  
```
 <action name="house_*" class="com.pb.web.action.HouseAction" method="{1}" >
      <result> /house_{1}_success.jsp</result>
 </action>
```

在这里，使用`house_*`的方式来匹配以`house_`开头的Action请求；        

如果我们访问`house_add`这个Action时， `*`号匹配到的内容就是 add;
house_add           
```
<action name="house_add" class="com.pb.web.action.HouseAction" method="add">
  <result>/house_add_success.jsp</result>
</action>
```

如果我们访问`house_update`这个Action时，`*`号匹配的部分就是update；        
house_update               
```
<action name="house_update" class="com.pb.web.action.HouseAction" method="update">
  <result>/house_update_success.jsp</result>
</action>
```

method属性里和`<result>`标签里，有一个`{1}`；        
在访问Action的时候，这个 `{1} `的位置就会用 `*`号匹配到的内容来替换；                 


##### 默认页设置：
在`struts.xml`配置文件中配置如下代码：      
```
<default-action-ref name="index" />

<action name="index">
     <result>index.jsp</result>
</action>
```

##### 匹配顺序：
- 问题1：当`struts.xml`中有多个 `<action>` 匹配的时候，会使用哪个？          

如下，有两个 `<action>`配置 ： 
```xml
<action name="house_add" class="com.pb.web.action.HouseAction" method="add">
  <result>/house_add_success.jsp</result>
</action>
<action name="house_*" class="com.pb.web.action.HouseAction" method="{1}" >
      <result> /house_{1}_success.jsp</result>
</action>
```

一个name属性是 `house_add` ，另一个是 `house_*` ，当我们访问 `house_add`的时候会匹配哪个？ 

结论：        
当有完全匹配的时候，优先使用完全匹配；    
没有完全匹配时，再使用有通配符的 `<action>` ;            

- 问题2 ： 如果有多个通配符匹配的情况呢？

比如：我们在访问 `house_add`的时候，有两个含有通配符的 `<action>`配置： 
```xml
<action name="house_*" class="com.pb.web.action.HouseAction" method="{1}" >
      <result> /house_{1}_success.jsp</result>
</action>
<action name="*_add" class="com.pb.web.action.{1}Action" method="add" >
      <result> /{1}_add_success.jsp</result>
</action>
```

一个是 `house_*`，另一个是 `*_add`，这时会使用哪个 `<action> `呢？      

结论：         
这个时候，要看这两个 `<action>` 位置的先后顺序；    

优先使用写在前面的  `<action>`

所以，当有多个使用了通配符的 `<action>`的时候，一定要注意先后的顺序；    


#### 使用通配符小结：
我们可以在 `<action>` 的 name 属性中使用通配符 `*`号；     
在method属性、class属性和 `<result>`标签中使用 `{数字} `的方式做占位符；      
用 `*`号匹配到的内容替换对应的大括号位置上的内容；      
`*`号可以出现多个；     
如下示例：        
```xml
<action name="*_*" class="com.pb.web.action.{1}Action" m ethod="{2}">
  <result>/{1}_{2}_success.jsp</result>
</action>
```
在这个例子中，name属性里的第一个 `*`号匹配到的内容会替换到后面 `{1} `中去；     
第二个 `*`号匹配到的内容会替换到 `{2} `中去；        

如果使用上面的配置，当访问 `House_add`时，相当于如下的配置：       
```xml
<action name="House_add" class="com.pb.web.action.HouseAction" method="add">
  <result>/House_add_success.jsp</result>
</action>
```

但这种方式用的不多； 
开发中一般会把一批Action配置到不同的package中；       

### 默认Action ：
当访问的Action找不到的时候会访问默认Action；           
如下，使用 `<default-action-ref> `标签设置默认Action              
```xml
<default-action-ref name="index" />

<action name="index">
     <result>index.jsp</result>
</action>

```

总结：

使用method属性指定调用的业务方法；       
这样会有很多的 `<action>`配置，不过没有关系，开发时一般会使用 `<include>`标签按模块分散这些配置；  

动态方法调用            
使用动态方法调用时，`<action>`配置中不需要配置method属性；
访问时使用 `action!method `这样的格式，比如： `house!add`

使用通配符简化配置
在使用通配符的时候，要注意 `<action>` 的匹配顺序是先完全匹配，没有完全匹配的时候再匹配有通配符的`<action>`，这时就会按顺序查找进行匹配；如果还是找不到，就会找默认的Action；