## Struts2详解：
实际上，Struts2就是一种基于MVC的Web应用框架；               
而，MVC框架远不止Struts2这一种，比如Spring MVC、JSF等；        

这里先介绍两个MVC框架：                 
一个是Struts1；另一个是WebWork；                 
Struts2 = `Struts1的知名度和市场+WebWork的技术`；              
Struts2的核心是WebWork，而不是Struts1；                  
Struts2实现了MVC模式，结构清晰；                            
使开发者只关注业务逻辑实现即可，同时提供了丰富的标签；                     
大大提高了开发效率；提供了页面导航；                      
通过配置文件把握整个系统各部分之间的联系，便于后期维护；       
与`Servlet API`松耦合，便于测试；               
Struts2能像MVC一样，将请求和展现分离；          

#### 搭建Struts2开发环境小结：
首先，通过下列网址获取到最新版本的[Struts2.3.4.1](http://struts.apache.org/download.cgi)             
其中，提供了很多版本的下载，需要下载的是`Struts-2.3.4.1-all`的版本；      
这里面不仅包括Struts示例，还包括源码以及帮助文档；        

### Struts2.3.4.1的主要目录：
- apps : 该文件夹下包含了基于Struts2的示例应用；
- docs : 该文件夹下包含了Struts2的官方文档，包括Struts2的快速入门、API帮助文档等内容；
- lib  : 该文件夹下包含了Struts2框架的核心类库，以及Struts2的第三方插件类库；
- src  : 该文件夹下包含了Struts2框架的全部源代码；

### Struts2环境搭建所需jar包：
```
commons-fileupload-1.2.2.jar  Struts文件的上传和下载
commons-io-2.0.1.jar          文件读取
commons-lang3-3.1.jar         为java.lang包提供扩展
freemarker-2.3.19.jar         FreeMarker是一个模板引擎，一个基于模板生成文本输出的通用工具
ognl-3.0.5.jar                支持ognl表达式
struts2-core-2.3.4.1.jar      Struts2的核心包
xwork-core-2.3.4.1.jar        xwork的核心包
javassist-3.11.0.GA.jar       分析、编辑和创建JAVA字节码的类库
```

为了避免每次一个一个jar包引入，可以将所需jar包组成`User Library`一起引入；            

对于使用Struts2框架的项目，没必要所有配置文件都手敲，理解含义，直接去参考源码提供的示例即可。这也是一种使用框架开发的常用方法；            


Struts2能像MVC一样将请求和展现分开；              
案例需求：         
使用Struts2输出 "Hello Struts"          
实现思路：               

修改`struts.xml`配置<action>以及<result>          
创建`helloStruts.jsp`页面           

### Struts2流程分析：
当用户在浏览器中输入请求URL后，由Tomcat接受到该http请求并确定相应的WebApplication是哪一个；         
访问该WebApplication的`web.xml`过滤所有请求，我们是通过 `/* `来完成的；        
根据用户输入的URL去匹配和对应`struts.xml`；查找相同名称的namespace和action；             
通过result确定显示结果视图为 `helloStruts.jsp`；          
该页面中就可以输出 "Hello Struts" 了；             

##### 这个流程中有三点需要注意：
- 用户的URL对应`struts.xml`中的namespace以及action的name；
- struts.xml起到了一个核心的配置作用；         
- Struts2将展现和处理分开，更灵活；           
修改结果视图时只需要重新配置`struts.xml`文件即可；                
action负责处理请求，并根据请求决定result，result决定结果视图；           


创建Action：

实际上，我们也可以自己创建Action，而且有三种方式：
- 第一种：        
编写普通的Java类，编写 `public String execute()` 方法；    
即，在Java类中，添加一个返回值为String字符串的`execute()`方法即可；       

- 第二种：       
编写一个类去实现Action借口，重写`execute()`方法；         

第三种：        
编写一个继承自ActionSupport的类，重写`execute()`方法；         

>第三种是最重用的方式；    


升级需求：             
使用Strut2输出 "Hello Struts"            

操作小技巧：          
设置开发模式：         
在`struts.xml`中添加 `<constant name="struts.deMode" value="true"/>`                   

不用重启Tomcat；
```xml

    <constant name="struts.devMode" value="true" />
    <package name="default" namespace="/" extends="struts-default">

        <action name="hello" class="com.pb.web.action.HelloAction3">
            <result >
            /helloStruts.jsp
            </result>
        </action>
    </package>
```

#### Strut2流程再分析：
我们自己创建了Action类，并修改了`struts.xml`中的action元素：     
```
<action name="hello" class="com.pb.web.action.HelloAction1">
```

通过这个配置，用户输入的URL对应上相应的namespace以及action；       
通过action的class属性找到对应的Action类；          

之前，并没有给action添加class，同样也可以实现 "Hello Struts" 的输出；      
其实，如下代码：         
```
<package extends= "struts-default" />
```

这个package类似java中的包，extends类似java中继承的作用；        
`extends="struts-default" `代表继承了`struts-default`这个包；          
一旦继承这个包，就可以使用这个包里的设置；                  
而默认设置中action的class属性就是 ActionSupport；             
在 `struts-default.xml`配置文件中，              
`<default-class-ref class="com.opensymphony.xwork2.ActionSupport" />`               
即，默认类引用 为 ActionSupport；
```
 <action name="hello"> 
```
就相当于：
```
 <action name="hello" class="com.opensymphony.xwork2.ActionSupport">
```

操作小技巧：        
在Eclipse中导入源码的方法：          

>选择相应jar包，右键 --> Properties --> Java Source Attachment


### 总结：        
Struts2其实就是一种基于MVC的Web应用框架；        
Struts2做的事情就是将请求与展现分开；              
通过 struts.xml配置文件，将整个过程变得更加灵活；               

创建Action的三种方式：            

第一种：       
编写普通的Java类，编写 `public String execute()` 方法；                 
即，在Java类中，添加一个返回值为String字符串的`execute()`方法即可；       

第二种：       
编写一个类去实现Action借口，重写`execute()`方法；             

第三种：           
编写一个继承自ActionSupport的类，重写`execute()`方法；

>第三种是最重用的方式；


