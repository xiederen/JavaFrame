# 使用验证框架实现数据校验
## 验证表单介绍
为什么要进行数据验证：    
数据验证就是，对数据的合法性进行检查，只允许合法的数据进入应用程序； 
在哪里实现数据验证：         
在客户端和服务器端都可以实现数据的验证；         

## 客户端验证：
数据提交之前在客户端实现验证；    
可使用JavaScript或jQuery实现；       
特点：       
```
1.减少客户提交数据后等待验证结果的时间；
2.减小服务器的压力；
```

## 服务器端验证：
数据提交之后由服务器端进行验证；     
特点：   
```
1.防止“绕过”客户端验证提交非法数据；
比如：他把我们的页面另存到本地后，修改页面中的代码，再提交页面到服务器端；
2.服务器端验证可以在服务器端处理数据之前，确保数据的合法性；
```
#### Struts2有两种方式实现服务器端验证
- 使用ActionSupport编码实现验证
- 使用验证框架实现验证


### 使用ActionSupport编码实现验证
在业务方法中实现验证      
首先，我们创建的Action类要继承ActionSupport，接下来就可以在Action类中实现验证；        
按照验证部分出现的位置可以分为三种情况：          

- 第一种情况：
在Action类的业务方法中直接验证；   

- 第二种情况：
重写`validate()`方法；然后把验证的部分写到这个方法中；

- 第三种情况：      
使用`valiateXXX()`方法；然后把验证的代码写在这个方法中；         

我们在验证的时候，如果有某些数据验证不通过，可以添加相应的错误信息； 
添加错误信息：     
```
addFieldError(String fieldName,String errorMessage)
addActionError(String anErrorMessage)
```

添加错误信息的时候可以使用 `addFieldError()`方法和`addActionError()`方法；    
`addFieldError()`方法是用来添加与字段相关的错误信息；   
什么算是与字段相关的错误信息？        
比如：用户名这个字段为空，年龄字段的值不合理，这些都算是与字段相关的错误；       
这些错误信息，我们就可以使用`addFieldError()`方法来保存；        
`addActionError()`方法是用来添加与Action所处理的业务相关的错误信息；         
比如：用户名密码都填了，但填的不正确，无法登陆；这就算是业务相关的错误；       

验证以后的错误信息是要显示给用户的；这时就需要在页面上输出验证结果；     
在页面输出验证结果：              
```
<s:fielderror/> :输出一个或所有字段的错误信息；
<s:actionerror/> :输出所有Action的错误信息
```
>注意：   
<action>的配置   
在<action>标签中，要配置一个name属性为input的<result>节点；         
因为，当验证不通过的时候，会返回input，从而跳转到指定的页面；          

使用 `hasFieldErrors() hasActionErrors() hasErrors() `来判断是否有错；    
```

 public String execute(){
	   //验证用户名
	   if(null == user.getUserName() || "".equals(user.getUserName())){
		   this.addFieldError("user.userName", "用户名不能为空(业务方法)");
	   }
	   if(null ==user.getPassword() || "".equals(user.getPassword())){
		   this.addFieldError("user.password","密码不能为空(业务错误)");
	   }
            //验证Action错误信息
	   addActionError("用户名或密码错误！");
	   if(this.hasErrors()){
		   return INPUT;
	   }
	   return SUCCESS;
   }
```

```
<s:if test="hasFieldErrors()">
<div>
   用s:fielderror输出一个字段的错误信息：
   <s:fielderror fieldName="user.userName" />
   <br/>用s:fielderror输出所有字段的错误信息：
   <s:fielderror/>
 </div>
 </s:if>
 <s:if test="hasActionErrors()">
 <div>
   用s:actionerror输出所有Action错误信息：
   <s:actionerror/>
 </div>
 </s:if>
```

### 使用 validate()方法实现验证       
在Action类的业务方法中完成验证，Action的业务处理和验证的代码混在一起，不方便验证部分的复用和维护；  
有没有办法把业务处理和验证部分分离开呢？       
在 `validate()` 方法中实现验证；     
我们可以通过重写 ActionSupport类的 `validate()`方法；     
然后，在`validate()`方法中实现验证；                
这样就可以将数据验证和业务处理分离开了；       
这种方式，添加错误信息和显示错误信息的方式不变，还跟在Action类的业务方法中是一样的；  

两个小经验：           

- 接收参数时，数据转换失败也会调用 `validate()`方法；     
比如：当用户在输入年龄的时候，输入的字符串"abc"，把"abc"字符串转换为整数年龄的时候就会发生数据转换失败，在这种情况下`validate()`方法还是会执行的；   
- `validate()`方法验证不通过的时候，Action类的业务处理方法是不会被调用的；    


#### Action类中的业务方法可以有多个；
`validate()`方法会对Action类中的所有业务方法都起作用； 
  
如果仅想对某个业务方法进行数据验证怎么办？     
这时，就可以使用 `validateXXX()` 方法实现验证了；       

使用 `validateXXX()`方法实现验证    
Struts2支持使用`validateXXX()`方法，针对`XXX()`方法进行数据验证； 
比如：     
使用`validateRegister()`方法实现针对`register()`方法的数据验证；       
这样，在注册时，就可以通过`validateRegister()`方法验证注册时才涉及到的内容；比如，年龄在注册业务的时候有，在别的业务方法中不一定会有；        

在注册和登录时都要验证的内容就可以在`validate()`方法中完成；    
比如，验证用户名和密码非空，在注册和登录时都要验证；      


小经验：           
`validate()`方法和`validateXXX()`方法同时存在时都会起作用； 
`validateXXX()`方法的调用要先于`validate()`方法；              


### 小结：
在Action类中添加错误信息，我们使用`addFieldError()`方法和`addActionError()`方法；      
这两个方法，一个是添加字段相关的错误信息，一个是添加Action业务处理相关的错误信息；      

在页面中显示错误信息，可以使用 `<s:fielderror/>`标签，输出一个或所有字段的错误信息；        
`<s:actionerror/>`，输出所有Action业务处理相关的错误信息      

##### 在Action类中实现验证的位置可以有三处：
- 第一处： 在业务方法中直接实现数据验证；     

>这种做法适合有针对性的仅验证个别的字段,不建议使用；
>因为业务与验证混在一起，不方便验证的复用和维护；

- 第二处： 在`validate()`方法中实现数据验证；      
这样就可以对Action类中所有的业务方法进行验证；          

- 第三处： 在`validateXXX()`方法中实现数据验证；  
这样可以针对Action类中某个业务方法进行验证；     
开发中使用的较多，可以更有针对性的进行验证；        


使用 `ActionName-validation.xml `实现验证 
回顾之前的验证方式，验证方法与Action类都耦合在一起；   
其实，使用Struts2的验证框架，就可以把验证方法从Action类中分离出来；         
这样，验证实现的部分更方便复用、扩展；            
还可以让验证的实现与业务处理分离；        

### 使用验证框架的过程：
1.编写JSP数据输入页面；         
2.编写Action类及其配置文件；         
3.在Action类同目录下创建验证文件 `ActionName-validation.xml`
这个文件名的前面ActionName这部分要与Action类的名字一致；
后半部分 `"-validation.xml"` 就是 固定写法了；         
这个文件只要命名符合这个要求，而且与Action类在同一目录下就会被识别；        
4.在这个xml文件中编写验证规则；         


### 小结：
使用验证框架这种验证方式就可以从Action类中分离验证部分的实现；         
这种做法要求在Action类同目录下创建 `ActionName-validation.xml`文件；       
至于验证规则的编写，可以查看帮助文档来参考验证规则的写法；     
Struts2支持两种验证配置的风格；          
将来，在工作的时候，所在的项目组只要统一使用一种就可以了；       

举例：      

字段验证器 (Field-validator)     
```
<field name="email_address">
  <field-validator type="required">
      <message>邮箱不能为空！</message>
  </field-validator>
</field>
```
<field>标签实现针对一个字段的验证规则配置；        
<field>标签中name属性指定的是要验证的字段名；                  
<field-validator>标签中的type指定的是要做什么样的验证；            
>这里的 "required"　指的是　"必填"　　　　　　　　　　　　  
<message>标签中的内容就是验证不通过的时候要提示的内容               

非字段验证器 (Non-Field validators)    
```
<validator type="required">
  <param name="fieldName">email_address</param>
  <message>邮箱不能为空！</message>
</validator>
```
<validator>标签可以配置一个验证器，其中： 
<param>标签中 name属性是fieldName时，表示该标签中的值是要验证的字段名；       
以上实例中，表示 email_address 是要验证的字段名；            
<message>标签中的内容是验证不通过的时候要提示的内容；           


### 使用ActionName-alias-validation.xml实现验证     

- `ActionName-validation.xml`对Action类中所有业务方法生效； 
- `ActionName-alias-validation.xml`对某一业务方法配置验证规则；       

问题：        
如果我们想对某一业务方法配置验证规则该怎么做呢？  
解决方法：     
首先，要对Action类中的某一个业务方法单独编写<action>配置，并使用method属性指定该业务方法；    
然后，提供 `ActionName-alias-validation.xml`文件，就可以针对某一个业务方法进行验证了；        
这里的 alias 是别名的意思，就是在 <action>标签中 name属性所配置的值；       
这个xml文件中验证规则的编写与刚才一样，没有变化；                


小结：            
`ActionName-validation.xml`文件可对Action类中所有业务方法起作用；实现验证；
`ActionName-alias-validation.xml`文件可对Action类中某一业务方法起作用，进行验证； 
如果以上两个文件同时存在，两个文件都会起作用；       
使用验证框架的验证顺序：       
```
1、Action父类-validation.xml
2、Action父类-alias-validation.xml
3、Action类-validation.xml
4、Action类-alias-validation.xml
```