# 概述：
## Ajax是几个单词首字母的缩写：         
`Asynchronous JavaScript And xml`     
Ajax并不是一种全新的技术；       
而是整合了几种现有的技术：` JavaScript`、`XML`和`CSS`。          
Ajax是一种创建交互式网页应用的网页开发技术。               


## Ajax简介：
传统Web应用是如何处理请求的：            
```html
<from action="" method="post" >
 <table align="center">
   <tr><td><p>用户账户：
        <input type="text" name="username"/>
       </p></td>
	</tr>
   <tr><td><p>用户密码：
        <input type="password" name="passwd"/>
       </p></td>
	</tr>
   <tr><td><p>重复密码：
        <input type="password" name="passwd"/>
       </p></td>
	</tr>
    <tr><td><input type="submit" value="提交"/>
         </td></tr>
   </table>
</form>
```

这是一个用户注册页面的部分代码；            
在这个用户注册页面，它有三个输入框，分别用来输入用户名、密码及重复密码；当点击提交的时候，这里是通过`POST`方式提交数据；
这个就是传统的Web应用发送请求的过程的一个简单示例；                    
当然，在这个示例中，除了使用`POST`方式提交数据以外还可以使用`GET`方式。

但是不论使用什么方式，当前页面都会发生跳转。                             
也就是说，一旦点击提交按钮，那么当前页面会跳转到其他页面，同时当前页面的数据也会传递过去。                 
本身这是没有什么问题的，完全满足平时的需要。                            
但是，`Web2.0`时代，要求更高的用户体验。                       
那么，这里的用户体验就不太理想；                                
比如，我们输入了用户名，但是实际上当前用户名已经在数据库中存在的话，我们是不能立即通知用户的，用户必须一次性的全部输入注册信息后，点击提交才能与后台进行数据交互，从而判断出用户名已经存在。那么对于这种情况，即使我们返回注册页面，用户也要重新进行输入一遍注册信息；如果再次用户名重复，那么用户可能就会崩溃了，甚至可能放弃注册。                             
那么，我们想要的到底是什么呢？我们如何来提高用户体验度呢？                                 

为了提高用户体验度，那么我们希望在用户注册页面，当用户在用户账户输入框输入完用户名，失去焦点的时候，发送请求到服务器，判断用户名是否存在，如果已经存在则弹出窗口提示，“用户名已经存在”。完成这个功能的过程中页面还不能刷新。                            
现在分析一下，当用户账户输入框失去焦点的时候，我们要去检查用户名是否存在；失去焦点，就需要使用输入框的onblur事件；                     
如下代码：                 
```html
<tr>
  <td><p>用户账户：
  <input type="text" name="username" style="width: 150px"
   onblur = "checkUser(this);/></p></td>
</tr>
```
在用户账户输入框，使用`onblur`事件，这个事件调用了`checkUser`方法，并且把当前的值当做参数传了进去。因此，我们需要再创建一个checkUser的Javascript方法；            
JavaScript的checkUser方法的实现代码：               
```javascript
<script type="text/javascript">
function checkUser(ouser) {
 var user = ouser.value;
if(!user){
  alert("用户名不能为空");
  ouser.foucs();
  return;
}
//发送请求到服务器，判断用户名是否存在
//Ajax代码实现
}
</script>
```
首先，要判断用户账户输入框是否为空，为空的话给出提示；如果不为空，就可以使用Ajax技术实现在不刷新页面的情况下，去与后台服务器进行交换的操作；                       
接下来就学习如何使用Ajax技术来实现这个需求：                        


#####  异常处理请求


在前面的伪代码中，最关键的代码没有给出，也就是：                        
如何使用Ajax来发送请求到服务器部分代码没有实现；                            

主要是JavaScript。                   
通过JavaScript的`XMLHttpRequest`对象，完成发送请求到服务器并获得返回结果的任务；然后使用JavaScript更新局部的网页。                             
需要注意的是这几个单词中的的“异步”：` Asynchronous`       
### 异步：
异步指的是，JavaScript脚本发送请求后并不是一直等着服务器响应，而是发送请求后继续做别的事，请求响应的处理是异步完成的。     
- XML ：        
XML一般用于请求数据和响应数据的封装；                

- CSS :          
CSS用于美化页面样式。            

根据Ajax对应的几个首字母缩写，可以看出，JavaScript和XML是Ajax技术中最重要的技术元素；但是，Ajax技术还包括CSS样式表、XMLHttpRequest数据交互对象和DOM文档对象等等；这些都算Ajax的关键元素；             
下面一起来认识以下这些关键元素：  

#### Ajax的关键元素：
- JavaScript语言    

- DOM文档对象

- CSS样式表

- XMLHttpRequest对象


#### JavaScript语言：
JavaScript是Ajax技术的主要开发语言；               
JavaScript是Ajax技术的主要开发语言，其开发的代码可以嵌入到浏览器中，通过浏览器解释执行；                        

#### DOM文档对象：
HTML页面元素在DOM中以树形结构存在；                   
在Ajax技术中，通过HTML DOM获取某个元素，然后通过DOM的属性或方法修改局部元素，就可以实现局部的刷新；                 
在Ajax应用中，DOM文档对象分为两种：            

`HTML DOM`  和 ` XML DOM`

#### CSS样式表：
在Ajax应用中，用户界面的样式可以通过CSS定义或修改。               
大多数情况下，Ajax应用不仅要实现局部内容的改变，往往还伴随着样式的变化，产生更加炫目的效果。               

#### XMLHttpRequest对象：            
Ajax技术的新鲜之处，就在于对`XMLHttpRequest`数据交换对象的使用。                        
`XMLHttpRequest`对象允许以异步的(Asynchronous)方式从服务器端获取数据以及向服务器端发送数据；                  
异步的优点在于可以减少用户访问网页的时间，它基本不会中断用户的正常操作。                           
`XMLHttpRequest`对象是大部分浏览器都支持的；               
可以使用JavaScript语言创建这个数据对象；                      


### 怎么使用Ajax：
我们知道了什么是Ajax以及Ajax的关键元素；             
那么怎么使用Ajax？先不着急，在学习怎么使用Ajax之前，对于前面的示例，我们先要开发出服务器端的程序。              
示例：                    
服务器端代码：           
```java
public void doGet(HttpServletRequest request,HttpServletResponse response) throws ServletException,IOException {
 response.setContentType("text/html");
 PrintWriter out = response.getWriter();
 String uname = request.getParameter("uname");
 boolean uExists = false;
 if(uname.equals("abcd"))
 {uExists = true;
   out.print(uExists);
 }else{
      out.print(uExists);
 }
}
```

在这里，创建了一个Servlet；                        
在`doGet()`方法中，首先获取用户账号的参数值，然后与`abcd`进行比较；
如果用户注册的时候，输入的注册账户是 `abcd`，表示当前账号已经存在，在Servlet中直接输出true，相反就输出false；

到现在，用户显示部分的HTML代码和服务器端的Servlet代码都已经实现了，剩下的只有Ajax的核心使用部分了；              
下面就来学习Ajax的使用；                             

首先，来认识一个JavaScript中的对象，这个对象就是`XMLHttpRequest`。                

#### XMLHttpRequest对象：
`XMLHttpRequest`对象在大部分浏览器上已经实现而且拥有一个简单的接口允许数据从客户端传递到服务器端，传递的过程是异步过程，不会打断用户当前的操作；并且使用`XMLHttpRequest`传输的数据可以是任何格式；                 
需要注意的是：                                      
`XMLHttpRequest`不是W3C标准，可以采用多种方式创建`XMLHttpRequest`对象。                    


##### XMLHttpRequest对象的创建：
正是因为`XMLHttpRequest`不是W3C标准，那么对于不同的浏览器，创建`XMLHttpRequest`对象的方法也不相同；甚至同一浏览器不同的版本也有可能需要不同的创建方法；            
对于老版本的IE，比如IE5和IE6，它的创建语法和新版本的IE以及其他浏览器是不同的；它的创建语法是：
```
new ActiveXObject("Microsoft.XMLHTTP");
```

虽然`XMLHttpRequest`不是W3C标准，但是随着Ajax的应用越来越广泛，行业间也在逐步向一个标准统一；所以现在对于IE的高级版本，也就是IE7及以上的版本和其他的现代浏览器，比如火狐、谷歌浏览器、苹果公司的浏览器等；                
它们创建`XMLHttpRequest`对象的语法逐渐统一为：       
```
new XMLHttpRequest();
```

##### 创建XMLHttpRequest对象的语法：
老版本`Internet Explorer` (IE5和IE6)                           
```
xmlHttpRequest = new ActiveXObject("Microsoft.XMLHTTP");
```

新版本`Internet Explorer` 和其他大部分浏览器            
```
xmlHttpRequest = new XMLHttpRequest();
```

为了适用于所有浏览器，包括IE的低级版本及其他浏览器，我们需要判断，然后再创建`XMLHttpRequest`对象；这里给出了一个创建`XMLHttpRequest`对象的统一方法；            
##### 创建XMLHttpRequest对象的统一方法：
```
if(window.XMLHttpRequest)//新版本IE浏览器(IE7及以上版本)或其他浏览器
xmlHttpRequest = new XMLHttpRequest();
}else {  //老版本IE浏览器(包括IE5和IE6)
xmlHttpRequest = new ActiveXObject("Microsoft.XMLHTTP");
```
我们使用这个方法创建`XMLHttpRequest`对象的话，基本上就能满足绝大多数情况；                 
在这里，使用的是一个if判断；                                   
window对象中，有两个属性，可以帮助我们判断当前浏览器是什么类型的浏览器；                               
一个是`ActiveXObject`，一个是`XMLHttpRequest`。                          
`window.ActiveXObject`的返回值是一个布尔类型变量；                         
当返回的是true的时候，能根据此判断出当前浏览器是老版本的浏览器；            
`window.XMLHttpRequest`的返回值是一个布尔型变量；                              
当返回值是true的时候，则表示当前浏览器是IE7及以上版本或其他浏览器；                      

这里只用一种判断方法就可以了；            
因此，我们if条件中就是`window.XMLHttpRequst`                
假如是真，就表示当前是新版本IE浏览器或其他浏览器；               
我们就使用 `new XMLHttpRequest()`方法来创建 `XMLHttpRequest`对象；             
否则，我们就使用 `new ActiveXObject("Microsoft.XMLHTTP")`;来创建`XMLHttpRequest`对象；     

```javascript
function checkUser(ouser) {
 var user = ouser.value;
if(!user){
  alert("用户名不能为空");
  ouser.foucs();
  return;
}
   xmlHttpRequest = createXmlHttpRequest();
}
function createXmlHttpRequest() {
if(window.XMLHttpRequest) {
 return new XMLHttpRequest();
}else {
 return new ActiveXObject("Microsoft.XMLHTTP");
 }
}
```

#### XMLHttpRequest对象的方法和属性

`XMLHttpRequest`对象的方法：                      

对于Ajax技术来说，主要就是`XMLHttpRequest`的使用了；         
Ajax内容不多，但是几乎所有的都是非常重要的；                
对于`XMLHttpRequest`对象的方法包括：                 
```
open()
send()
abort()
setRequestHeader()
getResponseHeader()
getAllResponseHeaders()
```

对于这些方法，常用的其实只有前面三个；                        
因此，我们需要熟练掌握前三个方法，并了解后三个方法的作用。              

##### XMLHttpRequest对象的属性
```
readyState
responseText
responseXML
status
statusText
```

`XMLHttpRequest`对象的属性，每一个都有其特殊的意义和作用；                     

`XMLHttpRequest`对象的方法：             

|方法 |               语法  |                             说明|
|-------|-------|---------|
|open()|xmlHttpRequest.open(strMethod,strUrl,strAsync,strUser,strPasswd)|此方法用于创建一个新的http请求，并指定此请求的方法、URL以及验证信息，参数strMethod表示http请求方式，比如POST、GET和PUT等；参数strUrl表示请求的URL地址；参数strAsync是可选参数，布尔型，表示请求是否为异步方式，默认为true；参数strUser和strPasswd也是可选参数，如果服务器需要验证，这两个参数表示验证信息中的用户名和密码；|
|send()|xmlHttpRequest.send(varObject)|此方法用于发送请求到服务器端并接受回应，此方法是采用同步或异步取决于open方法的strAsync参数；|
|abort()|xmlHttpRequest.abort()|此方法的作用是取消当前请求，XMLHttpRequest对象调用此方法后，当前请求返回 UNINITIALIZED 状态。|
|setRequestHeader()|xmlHttpRequest.setRequestHeader(strHeader,strValue)|此方法的作用是单独指定请求的某个http头，这个方法包含两个参数，strHeader参数是字符串类型，表示头名称；strValue也是字符串类型，表示值。注意：如果已经存在此名称的http头，则会覆盖，并且此方法必须在open方法后调用；|
|getResponseHeader()|strValue = xmlHttpRequest.getResponseHeader()|此方法用来从响应中获取指定的http头，注意：当send方法成功之后才能调用该方法；|
|getAllResponseHeader()|strValue = xmlHttpRequest.getAllResponseHeaders()|此方法与getResponseHeader()方法类似，它是获取响应的所有http头，注意：当send方法成功之后才能调用该方法并且获取的每个http头名称和值用冒号分割，并以 \n 结束。|

##### XMLHttpRequest对象的属性

- readyState属性            
语法：                 
```
varState = xmlHttpRequest.readyState
```

此属性返回请求的当前状态； 它的值有五个：             

```
属性名      值                     含义
readyState  0    未初始化，即对象已创建，但尚未调用open方法
            1    初始化，即对象已经创建，但尚未调用send方法
            2    发送数据，即send方法已经调用，但当前状态和http头未知
            3    数据传送中，即已经接受部分数据，但数据不全
            4    数据接受完毕
```

- status属性           
语法：           
```
varStatus = xmlHttpRequest.status
```

此属性的作用就是返回当前请求的http状态码；                 
注意：返回类型是长整型的；                      
http的状态码是很多的；比如：                   
```
属性名          值                         含义
status          200                 请求正确返回
                404                 请求找不到访问对象
```
这两个状态码是我们在Ajax中常用的，其他还有很多；              
状态码主要用于表示网页服务器HTTP的响应状态；                 
以` 3 `位数字表示；                                     

- 1** 表示消息；                
这一类型的状态码表示请求已经被接受，需要继续处理；          

- 2** 表示成功；                        
这一类型的状态码表示请求已经成功被服务器接受；          

- 3** 表示重定向；                                 

- 4** 表示请求错误；              

对于状态码的知识，可以去查询相关资料；           

其他属性             

|属性名 |                        说明|
|------|-------|
|responseText       |     将返回消息作为文本字符串|
|responseXML     |        将返回消息视为XML文档，|
|statusText  |   将返回当前请求的响应行状态|                    
|onreadystatechange  |    设置回调函数|



#### Ajax的使用
概述：         
当我们了解了`XMLHttpRequest`对象的各种方法和属性之后，接下来就学习如何使用Ajax技术来提高我们的用户体验度；                  
我们使用Ajax的目的主要是为了实现无刷新访问后台服务；              
因此，我们要学习如何使用Ajax来发送请求；                      
并且，我们还要学习如何使用Ajax来处理服务器返回的响应；             
对于发送请求，主要讲解如何发送GET请求和POST请求；                          
对于处理响应，也分为两部分来说，一个是如何处理文本响应，一个是如何处理XML响应；                      

#### Ajax的使用方法：
如何使用XMLHttpRequest对象来发送请求：                   
常用的http请求方式有两种：也就是GET和POST；                                    
所以对于发送请求，主要分为两部分来讲，分别是发送GET请求和发送POST请求；                     
首先，如何发送GET请求：                         

```javascript
function checkUser(ouser) {
 var uname = ouser.value;
if(!uname){
  alert("用户名不能为空");
  ouser.foucs();
  return;
}
//发送请求到服务器，判断用户名是否存在
//Ajax代码实现
//发送请求到服务器，判断用户名是否存在
//请求字符串
var url = "servlet/doReg?uname"+uname;
// 1.创建XMLHttpRequest对象
xmlHttpRequest = createXmlHttpRequest();
//2.设置回调函数
xmlHttpRequest.onreadystatechange = haoLeJiaoWo;
//3.初始化XMLHttpRequest组件
xmlHttpRequest.open("GET",url,true);
//4.发送请求
xmlHttpRequest.send(null);
}
```

在前面的课程中，我们只完成了JavaScript的checkUser方法的前部分代码，那么现在就来实现具体的Ajax代码；            
在前面讲解`XMLHttpRequest`对象的方法和属性的时候，介绍过`open`方法和`send`方法；这里就开始使用这两个方法来完成我们的实现；            
因为这里，打算使用`GET`方式发送请求，因此我们首先定义一个`url`；                                         
这个url字符串包含了要访问的目标地址和我们要传递的GET参数；                         
如下代码：               
```
var url = "servlet/doReg?uname"+uname;
```
创建好请求字符串之后，下面就开始对`XMLHttpRequest`进行操作；                
首先，我们要创建`XMLHttpRequest`对象，这里，我们是调用的`createXmlHttpRequest()`方法；得到了一个`XMLHttpRequest`对象。                               

当我们创建好`XMLHttpRequest`对象之后，下一步设置一个回调函数；                         
如下代码：                    
```
//2.设置回调函数
xmlHttpRequest.onreadystatechange = haoLeJiaoWo;
```
这里是给`XMLHttpRequest`对象的`onreadystatechange`属性赋一个值；这个值是一个JavaScript的方法名，这里我们赋值为 "haoLejiaoWo"；因此，后面还要实现一个haoLeJiaoWo的JavaScript方法；在这个方法里面，我们要对服务器返回的信息进行处理；                      
当我们设置好回调函数之后，第三步要对`XMLHttpRequest`对象进行初始化；             
初始化的方法就是调用这个对象的`open()`；因为，我们要实现的是发送`GET`请求；因此，我们对`XMLHttpRequest`对象的`open()`传递的第一个参数就是`GET`；第二个参数是我们要访问的目标地址；第三个参数是true，表示我们要使用异步请求方式。最后一步，我们来发送请求，这步比较简单；这里直接调用`send()`方法就可以了；对于`send()`方法中的参数，仅仅是当我们使用POST方式发送请求的时候才会有效，而在这里使用的是GET方式，所以我们直接给一个null就可以了；                   



##### 处理文本响应：
对于发送GET请求的checkUser方法的实现，我们已经分析完了；                   
在checkUser方法中，有一处设置回调函数的地方，指向的是一个haoLeJiaoWo的JavaScript方法；这个方法是用来实现处理服务器的响应的；                      
那么，现在就来分析如何实现这个方法；                        
如下：            
```javascript
function haoLeJiaoWo() {
    if(xmlHttpRequest.readyState == 4
          &&  xmlHttpRequest.status == 200) {
	  var b = xmlHttpRequest.responseText;
	  b = b.replace(/(^\s*) | (\s*$) /g,"");
	  if(b=="true") {
	       alert("用户名已经存在");
	  }else{
		alert("用户名可以使用");
	  }
     }
}
```
在这个方法里，我们使用了`XMLHttpRequest`对象的三个属性：
```
readyState、status和responseText；
```
`readyState`有5个当前状态，其中当值为4的时候表示数据接收完成；                        
`status`属性能返回当前请求的http状态码，其中200表示服务器已经正确返回；            
那么在这里，我们就要首先对`readyState`和`status`这两个属性进行一下判断；                    
当`readyState`的值为4并且`status`的值为200的时候我们才进行后面的处理；                   
那么，当满足条件的时候，接着使用`XMLHttpRequest`对象的`responseText`属性来获取响应的字符串信息；            
在前面实现服务器端代码的时候，是直接打印的true 或者是 false；                   
这里，我们就可以使用`XMLHttpRequest`对象的`responseText`属性来得到这个打印值。             
这里需要注意的是，我们的服务器端是使用Servlet来实现的；              
因此它打印结果的时候会有部分空格垃圾数据；                                      
而对于JavaScript没有为我们提供去删除首尾空格的方法；                          
因此这里，我们使用正则表达式来实现去除首尾空格；                                
当我们对返回值进行去除首尾空格之后，就能得到在Servlet中返回的true或false；                        
接下来就简单了，直接对b这个返回值进行判断；                  
如果b等于true，就表示用户名已经存在；相反，则用户名可以使用。                 



##### 发送POST请求
在前面的案例，已经实现完成，根据分析我们可以知道，                       
当使用Ajax技术进行请求操作的时候，我们使用的是GET方式；                     
接下来，我们来看一下如何来实现POST方式：                      
实际上，POST方式的实现跟GET方式是很类似的，如下代码：                        
```javascript
function checkUser(ouser) {
 var uname = ouser.value;
if(!uname){
  alert("用户名不能为空");
  ouser.foucs();
  return;
}
//发送请求到服务器，判断用户名是否存在
//Ajax代码实现
//发送请求到服务器，判断用户名是否存在
//请求字符串
var url = "servlet/doReg";
var userinfo = "uname="+uname;
// 1.创建XMLHttpRequest对象
xmlHttpRequest = createXmlHttpRequest();
//2.设置回调函数
xmlHttpRequest.onreadystatechange = haoLeJiaoWo;
//3.初始化XMLHttpRequest对象
xmlHttpRequest.open("POST",url,true);
xmlHttpRequest.setRequestHeader("Content-Type","application/x-www-form-urlencoded");
//4.发送请求
xmlHttpRequest.send(userinfo);
}
```

首先，在创建请求字符串的时候，我们的url要指明访问的目标地址；              
对于GET方式的话，还要把请求参数传递过去；                   
而对于POST方式，是以表单提交的方式进行数据传递的，因此我们可以省略问号后面的参数部分内容；但是，POST需要提交表单数据，而Ajax要实现无刷新提交数据，因此，我们还需要创建一个表单对象，就像这里的userinfo；                    
它是一个键值对，如果有多个键值对要传递的话，中间使用 `&`进行分割；                              

创建好表单对象和请求字符串之后，下面就是创建XMLHttpRequest对象了；                     
XMLHttpRequest对象的创建过程和前面的GET方式中的创建过程一样；                          
而创建好的这个对象的回调函数，也可以使用原来的代码；                                        
再接下来，初始化XMLHttpRequest对象的时候和GET方式略有不同；                            
在我们调用XMLHttpRequest对象的open()方法的时候，把第一个参数修改为POST；                   
然后，我们需要设置一下请求头，也就是调用XMLHttpRequest对象的setRequestHeader()；                       
这个方法用来设置请求的头部信息，`Content-Type`表示内容类型；那么，在这里把`Content-Type`设置为 `application/x-www-form-urlencoded`        表示发送的数据为表单数据；                  
那么，这样，使用POST方式的时候，服务器端才能获取这里我们提交的数据；                           
最后，调用XMLHttpRequest对象的send()发送请求；                     
注意，需要把前面创建的模拟表单对象当做其参数传进去。                                  


##### 处理XML响应
Ajax全称是“异步的JavaScript和XML”；                          
那么到目前为止，我们使用Ajax发送和接受数据都是采用简单的文本格式；            
并没有使用真正的XML格式的数据，然而XML数据格式仍然是很重要的；                     
很多的情况下还是要选择使用XML格式；                       
接下来通过一个示例来学习如何处理XML响应：                        
示例：                                    
在一个用户信息查询页面，当在下拉框中选择一个用户的时候，希望在不刷新页面的情况下，自动从服务器读取用户的账户信息，并自动填写到用户账户的输入框中；                  
即：                
我们要查询aptech用户的信息，当我们在下拉列表选择此用户的时候，要实现无刷新获取用户信息；                     

对于这个示例，首先来实现服务器端代码，如下：                
服务器端代码：             
```java
public void doGet(HttpServletRequest request,HttpServletResponse response) throws ServletException,IOException {
 String uname = request.getParameter("uname");
 response.setContentType("text/xml;charset=utf-8");
 PrintWriter out = response.getWriter();
 StringBuilder bd = new StringBuilder();
 bd.append("<userinfo>");
 bd.append("<username>");
 if(uname==null || uname.length()==0)
 {
     bd.append("请选择用户");
  }else{
     bd.append(uname);
  }
  bd.append("</username>");
  bd.append("</userinfo>");
  out.println(bd.toString());
  out.flush();
  out.close();
  }
```
在这里，创建了一个Servlet，关键是Servlet中的doGet()的实现；                 
在doGet()中，首先获取客户端的参数值；这里传进来的是一个用户名；                       
因为我们要处理服务器返回的XML响应；因此，服务器端返回的时候不能再使用`text/html`头；如下代码：                 
```
 response.setContentType("text/xml;charset=utf-8");
```
使用的是 `text/xml` ；                
然后，拼接一个XML格式的字符串；                  
那么，此时这个XML的根节点是<userinfo>；在<userinfo>节点中包含了一个<username>节点；那么，当客户端请求的参数为空或者长度等于0时，我们在这个<username>节点中添加“请选择用户”;相反，我们把用户名添加到<username>节点中，然后向客户端打印输出。                                        


客户端的HTML代码：                   
```html
<table align="center">
 <tr>
 <td><select name="user" onchange="getUser(this);">
 <option value="accp">accp</option>
 <option value="aptech">aptech</optiom>
 </select></td>
 </tr>
 <tr>
 <td><p>用户账户：<input type="text" id="username" /></p></td>
 </tr>
 <tr>
 <td><p>用户密码：<input type="password" id="passwd" /></p></td>
 </tr>
 <tr>
 <td><p>重置密码：<input type="password" id="passwdtwo" /></p></td>
 </tr>
 </table>
```

我们在表格中创建了四个表单元素，一个是下拉列表，剩下三个是文本框；
那么，我们在名字叫user的下拉列表中，添加了两个下拉项；并且，我们在它的onchange方法中调用了JavaScript的一个函数getUser()。            


JavaScript的getUser()的实现代码：            
发送GET请求           

```javascript
function getUser(ouser) {
 var uname= ouser.value;
 if(!uname) {
   alert("请选择查询用户");
   ouser.focus();
   return;
}
//发送请求到服务器，判断用户名是否存在
//Ajax代码实现
//发送请求到服务器，判断用户名是否存在
//请求字符串
var url = "servlet/doGetU?uname="+uname;
//1.创建XMLHttpRequest对象
xmlHttpRequest = createXmlHttpRequest();
//2.设置回调函数
xmlHttpRequest.onreadystatechange = haoLeJiaoWo;
//3.初始化XMLHttpRequest组件
xmlHttpRequest.open("GET",url,true);
//4.发送请求
xmlHttpRequest.send(null);
}
```

我们首先要判断查询条件输入框是否为空；            
为空的话给出提示；                   
如果不为空，我们就可以使用Ajax技术实现在不刷新页面的情况下去与后台服务器进行交互的操作；                  
这部分代码跟上一个示例使用GET方式发送请求时的代码实现没有什么不同；                     
这里，首先定义一个url；                           
这个url字符串包含了要访问的目标地址和我们要传递的GET参数；               
如下代码：          
```
//请求字符串
var url = "servlet/doGetU?uname="+uname;
```

创建好请求字符串之后，下面就开始对XMLHttpRequest进行操作；                       
首先，我们要创建XMLHttpRequest对象；那么，这里也是调用的createXmlHttpRquest()，获得一个XMLHttpRequest对象。                          

当我们创建好XMLHttpRequest对象之后；下一步，我们要设置一个回调函数；
如下代码：
//2.设置回调函数
xmlHttpRequest.onreadystatechange = haoLeJiaoWo;
这里，我们设置回调函数名为 "haoLeJiaoWo"；
因此，后面我们还要实现一个haoLeJiaoWo的JavaScript方法；
那么，在这个方法里，我们将要对服务器返回的信息进行处理；
haoLeJiaoWo方法将是这个示例中的关键代码；
当设置好回调函数之后，第三步，我们要对XMLHttpRequest对象进行初始化；
初始化的方法就是调用这个对象的open方法；
因为，我们要实现的是发送GET请求；
因此，这里我们对XMLHttpRequest对象的open方法传递的第一个参数是GET，第二个参数是要访问的目标地址；第三个参数是true，表示我们要使用异步请求方式；
最后，我们发送异步请求，调用XMLHttpRequest对象的send方法。

到目前为止，已经可以使用XMLHttpRequest对象的responseXML属性得到服务器端生成的XML文档。但是responseXML并不是简单的字符串，而是一种复杂的文档对象；因此我们要学会如何处理它才能得到我们需要的数据；在这里，我们使用 XmlDocument对象；对于XmlDocument对象，它提供了一些方法可以帮助我们正确的从XML文档中得到我们想要的数据；

##### XmlDocument对象的使用
从XmlDocument中获取节点对象            

|方法名                    |       说明|
|-----|------|
|getElementsByTagName()|用来获取指定节点名的节点对象集合，参数为节点名称字符串|
|selectSingleNode()|用来获取符合条件的单个节点对象，它的参数为Xpath表达式。Xpath是在XML文档中查找信息的一门语言，在本次课程中我们不介绍它；|
|selectNodes()|用来获取符合条件的节点对象集合，它的参数同样为XPath表达式；|

我们知道了XmlDocument对象的常用方法，下面就来看haoLe方法中如何处理XML响应，如下代码：           
```javascript
function haoLe() {
 if(xmlHttpRequest.readyState == 4
    &&  xmlHttpRequest.status == 200) {
	var dom = xmlHttpRequest.responseXMl;
	if(dom) {
	 var userNodes = dom.getElementsByTagName("username");
	   if(userNodes.length>0) {
	      var username = userNodes[0].firstChild.nodeValue;
	      document.getElementById("username").value = username;
	}
     }
   }
}
```

我们说，XMLHttpRequest对象的`responseXML`属性可以得到服务器端生成的XML文档；那么，这个文档其实就是一个`XmlDocument`对象。                         
当我们得到这个对象之后，就可以使用它的getElementsByTagName方法获取我们想要的指定节点的对象集合；这里，我们想要的节点集合是 <username>节点的集合。在前面服务器端代码中，实际上我们生成的XML文档中只有一个<username>节点；因此，我们可以使用 `userNodes[0].firstChild.nodeValue`就可以获得这个节点中包含的值；然后，我们把这个值赋值给username输入框，那么这个示例就可以实现了。                                
