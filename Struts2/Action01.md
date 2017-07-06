# Action
## Struts2与MVC
Struts2其实就是一种基于MVC的Web应用框架；          
同MVC一样，做的事情就是将请求和展现分开；         

## Struts2和MVC的对应关系：
M(模型层)，包含了应用程序的业务逻辑和业务数据；         
由封装数据和处理业务的JavaBean组成；          
V(视图层)，封装了应用程序的输出形式，也就是通常所说的页面或者是界面；                 
比如：` JSP  HTML`          
C(控制层)，负责协调模型和视图；            
根据用户请求来选择要调用哪个模型来处理业务；以及最终由哪个视图为用户做出应答；          
比如：`Servlet`；          

Struts2框架其实就是一种对MVC的封装；        
不过，C控制层，注意：           
一般来说，控制层部分，Struts2包括核心控制器以及业务控制器两部分；         
核心控制器就是在`web.xml`中配置的 ： StrutsPrepareAndExecuteFilter；           
它负责过滤所有的用户请求，通过 `/*` 来实现；根据请求的不同，分发给不同的Action处理；       
这里的业务控制器就是Action；             


## Action的作用
Action是业务控制器，控制业务；   
也就是说，它并不做具体的业务逻辑操作；            
如果业务逻辑很复杂的话，可以把业务逻辑构建为业务类，然后再在Action中调用这个业务类；         
比如，使用Struts2输出 "Hello Struts"  案例：
```java
public class HelloAction {
 public String execute() {
    return "success";
  }
}
```

在Action中返回一个字符串 "success" ;         
当然，不止可以返回success，也可以返回别的字符串；        
实际上，Action可以根据业务逻辑执行的返回结果判断，返回不同的结果字符串；         
Struts2框架就根据Action返回的结果字符串选择对应的视图呈现给用户；通过 result；                   
Action还可以很方便的处理数据；        
比如，在Action中可以以属性的方式保存用户数据，接受参数；页面中就可以方便获取；        
 

#### Action接受参数方式一：  
属性方式小结：    
首先，表单参数名称必须在Action中有对应的setter以及getter方法；   
即，表单参数名称可以不和Action中的属性名称相同，但必须和setter与getter方法中的后缀名相同；             
同时，如果JSP页面与Struts2中编码格式不同还可能出现乱码；        
此时，可以修改 `struts.xml`配置文件，添加常量设置编码格式；            
添加代码：            
```
<constant name="struts.i18n.encoding" value="编码格式"/>
```

在页面获取并显示参数值时：     
使用Struts2标签：        
首先通过 taglib 指令引入 Struts2标签库；           
```
<%taglib uri="/struts-tags" prefix="s" %>
```

然后，通过 `<s:property>`标签输出值：
```
<s:property value="属性名"/>
```

应用场景：    
Action这种通过属性方式接收参数值的方式比较适合参数字段个数少的场景； 
比如：搜索应用中接收用户输入的搜索条件；             


当接收参数字段个数多的时候，Action可以通过JavaBean的方式接收参数；       


操作提示：    
`struts-core-2.3.4.1.jar`是一个压缩文件，它打包了实现功能所需要的各种文件，解压后就能看到里面的文件了；   


#### Action接收参数方式二：
Action接收参数(JavaBean方式) : 将模型数据从Action中分离了出来；             
这种方式将模型数据从Action中分离了出来，使Action结构更清晰；           
具体做法：     
首先，定义了实体类，并为实体类添加了相应属性以及setter、getter方法；             
比如： `com.pb.entity.User`          
接下来，编写相应的Action，并为Action添加实体属性以及setter和getter方法；       
编写相应的Action并为它添加了实体类User的属性。以及setter和getter方法： `User user`;         
然后，修改表单参数 name 为 实体对象名.属性名;          
比如，修改用户名为 `user.userName`   
```
<input type="text" name="user.userName"/>
```
这和第一种方式使用属性接收是不同的；    
第一种方式，用户名直接写 userName， 不用 `user.  `了；                     
页面取值时通过` <s:property value="对象名.属性名" /> `的形式

比如： `<s:property value="user.userName"/>`

编写`struts.xml`配置Action时的小技巧：          
设置XML自动提示：
```
Window -> Preferences -> XML Catalog -> Add 
```
在 Key type 中 选择 URL            
在 Key 中 输入   `http://struts.apache.org/dtds/struts-2.3.dtd`  
在location 中 选择  dtd文档的相应路径；          


#### Action接收参数三：
Action接收参数(ModelDriven方式)： 
具体做法：   
需要定义实体类，为实体类添加属性以及setter和getter方法；             
然后，创建一个Action，该Action实现ModelDriven接口，重写`getModel()`方法；          
Action中要提供JavaBean类型的属性，需要实例化，但不需要相应的setter和getter方法；      
Form表单项的name属性以及页面取值时通过 `<s:property value="属性名"/>`的形式；             


ModelDriven方式与Action接收参数前两种方式的相同点：         
与Action中定义JavaBean属性的方式在Action中的写法是类似的；  
与Action中定义属性方式在JSP页面的写法上一样；      
  
ModelDriven方式与Action接收参数前两种方式的不同点：         

- 第一：    
`Model Driven`方式，一个Action对应一个Model，用于接收参数。    
属性中可以有多个Model，但是只有一个可以被`getModel()`返回；这个实体用于接收参数；       
而，JavaBean方式，一个Action可以对应多个Model去接收参数；            

- 第二：     
Action接收参数的三种方式可以混合使用；      
ModelDriven方式和属性方式同时存在Action中时，对于相同的属性，ModelDriven方式优先被赋值；               

## 总结：
Action接收参数的三种方式：  
属性方式： 
 在Action中： 
在Action中，需要有对应表单参数的属性以及相应的setter和getter方法；          
 在Form表单元素的name属性和在页面上取值的时候：    

1.Form中元素name取值属性名；         
2.取值： `<s:property value="属性名"/>`
 应用场景：     
接收的参数字段个数少；             

### JavaBean方式：
 在Action中： 
需要一个实体属性以及对应的setter和getter方法； 
当然，实体类是需要提前定义的，这个实体对象在声明时不需要实例化；  
 在Form表单元素的name属性和在页面上取值的时候：     

1.Form中元素name取值对象名.属性名；    
2.取值： `<s:property value="对象名.属性名"/>`        
 应用场景：     
字段多，可以封装为一个对象；将模型数据从Action中分离出来时；            

### ModelDriven方式：
 在Action中：  
Action需要实现ModelDriven接口，重写方法`getModel()`;Action中需要提供JavaBean类型的属性，需要实例化，但不需要相应的setter和getter方法；           
 在Form表单元素的name属性和在页面上取值的时候：

1.Form中元素name取值属性名；            
2.取值： `<s:property value="属性名"/>`          
 应用场景：       
和JavaBean方式相似，不常用；         

后两种方式将模型数据从Actin中分离出来了，使Action结构清晰；    
当然，这三种方式都可以混合使用，比如：      
可以将JavaBean方式和属性方式一起使用；      
  
而且，JavaBean方式和属性方式是三者中最常用的两种方式；         
 
学习方法：
不用记忆文字，通过案例代码去了解；