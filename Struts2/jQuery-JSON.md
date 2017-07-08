# 回顾Ajax
Ajax的使用场合及工作原理：     
在百度搜索中，输入java后文本框下提示了很多查询关键词；    
这些关键词就是使用Ajax方式从服务器取得的； 
新浪首页的登录框，成功登录后该网页只刷新登录框部分，并不会全部刷新，其实也是使用了Ajax技术；         
	Ajax : Asynchronous (异步的) JavaScript And Xml
Ajax就是异步的`JavaScript And Xml`，它是一种异步请求技术；   
即，在不刷新整个网页的情况下，实现浏览器与服务器的交互；      
它在多种平台和框架下都有实现；          
在一种平台下，也可能有多种实现方式；       
今天要学习的是 javaEE中，最典型的一种实现方式：   

## jQuery+Struts2+JSON 实现Ajax 的典型方式
基本执行过程：  
在浏览器端使用jQuery发送XMLHttpRequest，服务器接收请求后，使用Action处理请求；然后，以JSON的数据格式发回处理结果；最后，jQuery接收处理结果，并根据结果更新局部网页；          
不难发现，要掌握这种Ajax的实现方式，必须掌握以下知识点：     

- 了解JSON  
- 在服务器端实现Action类，并在 `struts.xml`中配置好 <action>使服务器能接收并处理 XMLHttpRequest；
XMLHttpRequest，可以简称 Ajax请求          
- 在浏览器端使用jQuery发送Ajax请求，并接收处理结果更新网页；     


## JSON简介
JSON 是一种对象；       

JSON就是浏览器和服务器之间交换数据的一种轻量级对象； 
在Java中，我们可以定义实体类来传递数据；     
那么，在JavaScript中，如何传递数据？       
我们可以使用JSON对象来传递数据；                
如下代码：         
```
var user = {"id":1,"username":"admin","password":"123"};
```
我们可以直接用这种方式创建JSON对象；     
这是一个用户对象；            
id、username、password为属性名；            
admin、123则为属性的值                         
如果想访问user对象的id属性，可以这样写:`user.id`                     
如果想把这个对象转换成字符串，如下：     
```
var str = JSON.stringify(user);
```
使用 `JSON.stringify()`这个方法就可以了；      
转换以后，str的值就是如下这个值：         
```
'{"id":1,"username":"admin","password":"123"}'
```
值得注意的是，str放在一对单引号中;它是字符串，不是user对象；      
JSON对象转换成字符串以后，就可以很容易的在浏览器和服务器之间传递了；          

要学会JSON的使用，必须掌握以下知识点：             

1.创建JSON对象的语法；           
2.JSON和字符串之间的转换方法；        

##### 对象
以 `{key:value,key:value,......}`格式书写，属性名和属性值以键值对表示；        
键和值用 `:`隔开，键值对之间用 `,`整个表达式放在 `{}` 中；        

key为string类型，value可以是string、number、boolean、null、对象、数组；             
例如：            
```
var person = {"name":"张三","age":"30","wife":null};
```

如果只有一个值，把它当成只有一个属性的对象即可，例如： `{"name":"张三"}`       


##### 数组        
以 `[value,value,......]`格式书写，元素之间用 `,`隔开，整个表达式放在 `[]` 中；          
- 字符串数组 举例： `["中国","美国","俄罗斯"]`                  
- 对象数组   举例： `[{"name":"张三","age":30},{"name":"李四","age":40}]`     

```jQuery
$(document).ready(function(){
	var person = {"name":"张三","age":20,"wife":null};
	$("#msg").append("姓名："+person.name+"<br>");
	$("#msg").append("年龄："+person.age+"<br>");
	$("#msg").append("配偶："+person.wife+"<br>");

        //JSON转换为字符串
       var str = JSON.stringify(person);
        //字符串转换为JSON
        var obj = JSON.parse(str);
});
```

### 服务器端实现Ajax
有了JSON之后，浏览器和服务器之间就可以使用它来传递数据了；    
接下来，就来学习在服务器端如何实现Ajax；         
在前面已经学过，在Struts2中实现服务器端只需要编写Action类和配置<action>就行了；           
接下来就以下面的Action类为例，讲解 `struts.xml`中的<action>该如何配置；                   

讲解之前，先区别普通请求和Ajax请求的区别；         
对于普通请求，服务器会返回一个页面；在这个页面中显示Action中的数据；            
所以，<action>中的<result>中配置一个页面就可以了；            
但是对于Ajax请求，服务器应该直接返回Action中的数据；       
所以，<action>中的<result>应该配置要返回哪些数据；              
```java
public class DataAction extends ActionSupport {
 private String name;
 private int age;
 private User user;
 private List<User> users;

 //处理请求的方法
 public String getData() {
   //代码省略
   return SUCCESS;
  }
  //省略getter和setter
}
```

响应Ajax请求的 <action>配置             
```xml
<package name="data" namespace="/" extends="json-default">
  <action name="getData" class="action.DataAction" method="getData">
     <result type="json">
	 <param name="includeProperties">
                name,user.*
         </param>
     </result>
   </action>
</pacakge>
```

首先，package需要继承 `json-default`；    
`json-default` 是处理Ajax请求的配置文件；  
其次， <result> 的 type 要配置为 json，表示以JSON对象的方式返回数据；              
值得注意的是，<result>默认会返回Action的所有成员变量；       
如果只想返回部分数据，可以使用参数 includeProperties；               
用来设置需要返回的成员变量；                      
比如，该例子中设置了，只返回name和user对象的所有属性； 
经过这些设置以后，服务器端响应Ajax请求的Action就已经准备好了；    
接下来只要在浏览器发送请求就行了；                



### 回顾jQuery
浏览器发送Ajax请求，我们一般会使用jQuery来实现；   
jQuery是一个优秀的JavaScript库，它对发送Ajax请求进行了封装；    
能很方便的调用服务器端实现Ajax的Action；             

jQuery中最常用的发送Ajax请求的方法就是 ` $.ajax() `  方法；    
如下示例：        
```javascrpt
$.ajax({
   url: "getData",      //请求的url地址，Struts2中就是请求<action>的name；
   type: "post",        //提交方式
   data:  {"name":"admin"},   //提交的数据，一般使用JSON表示
   dataType : "json",         //接收的结果类型，一般为JSON类型
   success : function(data) { //请求成功后执行的操作
       //省略代码
   },
   error:function(data) {   //请求失败后执行的操作
       //省略代码
   }
});
```

这个例子中，列举了该方法最常用的几个参数；        
```
   url: "getData",      //请求的url地址，Struts2中就是请求<action>的name；
   type: "post",        //提交方式
   data:  {"name":"admin"},   //提交的数据，一般使用JSON表示
```

上面这3个参数是发送Ajax请求使用的；          
url为请求的地址；在Struts2中就是请求的 <action> 的name；       
type为提交方式，默认是 "get"；            
data是要提交到服务器的数据，一般使用JSON表示；         
```
   dataType : "json",         //接收的结果类型，一般为JSON类型
   success : function(data) { //请求成功后执行的操作
       //省略代码
   },
   error:function(data) {   //请求失败后执行的操作
       //省略代码
   }
```

这三个是接收响应结果时用到的参数；       

- dataType是接收的结果类型，一般为JSON类型； 
- success为请求成功后要执行的操作；     
- error则是请求失败后执行的操作；         


### 实现无刷新登录
无刷新登录的案例： 
当用户点击 “登录”按钮的时候，我们需要把用户名和密码的值提交到服务器；        
在Ajax中，如何提交数据呢？         
刚才讲解 `$.ajax() `方法时，data参数可以提交数据到服务器；使用它就可以了；         
首先，要使用这段代码：       
```javascrpt
 var params = {
    "user.username":$("#username").val(),
    "user.password":$("#password").val()
};
```

来取得文本框中的用户名和密码，创建params的JSON对象；    
再把他们交给 `$.ajax() `方法的data参数就可以实现数据提交了；                 
```javascrpt
$.ajax({
   data:params,
   .....
});
```

##### 无刷新登录的实现步骤：            
- 实现Action，接收参数，验证用户名、密码，返回验证结果    
- 配置<action>        
- 在客户端调用实现Ajax的Action，调用时，提交用户名和密码  
- 实现回调方法，根据验证结果更新登录框；     


### 知识点总结：
使用jQuery和Struts2框架实现Ajax 
<action>配置： 
```
extends="json-default"   <result type="json">   <param name="includeProperties">
```
`$.ajax()`方法：各个参数的使用    

练习 jQuery的使用                

#### JSON语法 
```
对象： {key:value,key:value,.....}
数组： [value,value,....]
```

经验分享    
使用jQuery和Struts2实现Ajax的这种方式运用广泛。    
Struts2还有一些Ajax标签实现了更深层次的封装，但其灵活性及性能有所限制，用的不多；             
Struts2实现Ajax采用的是dwr组件；它是一个非常优秀的服务器端Ajax实现组件。                 
Struts2把dwr、JSON、Action很好的融合在一起，简单易用，性能优秀，兼容性好。                  
HTML、CSS、JavaScript是Web前端技术的基石，掌握了JSON、jQuery、Extjs等其他Web前端知识后，能够大幅提升项目的开发速度，实现更加复杂的功能和效果。             
jQuery发送Ajax请求除了使用 $.ajax()方法外，还有 $.get()  $.post()等方法，但用的不多；
服务器端除了返回JSON数据类型外，还有xml script等类型；