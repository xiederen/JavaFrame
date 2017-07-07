# OGNL：
学习方法：             
经常使用 `<s:debug/>`，便于理解原理，调试错误；   
常常使用 `<s:debug/>`，看清值栈和`Stack Context`中有什么，便于理解原理，调试错误；             

## 使用过的OGNL ：
页面获取并输出Action属性：     
```
 <s:property value="userName"/>
```
使用`Servlet API`处理数据时，页面中获取request保存的数据：   
```
 <s:property value="#request.userName"/>
```

实际上，这两句代码中的 `userName ,#request.userName` 就是 `OGNL`         

### OGNL是什么：
```
Object Graph Navigation Language
```
对象图导航语音；          
它是`Struts2`默认的表达式语音；开源，功能更强大；               

通过简单一致的表达式语法可以存取对象的属性、调用对象的方法；        
可以访问静态方法、静态属性              
可以访问值栈以及`Stack Context`；    
可以操作集合对象；      
支持赋值、运算操作、字段类型转化等             

### OGNL访问值栈
问题：   
通过 `<s:property value="house.title" />`为什么可以获取到房屋的标题信息？         
house为Action的属性；
而，Action的实例放在值栈`(Value Stack)`中; 
OGNL可以直接访问值栈中的值；              
因此通过`house.title`就可以访问到house，进而访问到它的title属性的值；      

#### 值栈 (Value Stack) 
可以把值栈理解为存放数据的一块内存；  
Action的实例就放在值栈中；   
这样，就可以通过OGNL来访问Action实例中的属性值了；              

注意：       
值栈中的值OGNL可以直接访问到；     


### OGNL访问Stack Context 
问题：          
为什么通过 `<s:property value="#request.userName"/>`可以取得request保存的值？           
request的相关属性以及参数都存放在`Stack Context`中；                        
OGNL是可以通过` #`号 访问 `Stack Context` 中的值的；        

#### Stack Context是什么
还是可以把 `Stack Context` 理解为存放数据的一块内存；          
`Stack Context` 中存放了：            
```
request的参数、request的属性、session的属性、application的属性以及 attr；
```

- attr :    
在所有的属性范围中获取值；      
attr就是依次按照页面page、请求request、会话session和应用application的顺序搜索；返回第一个符合条件的属性；其中还存放了其他内容；以上只是经常使用的；       

注意：         
`Stack Context`的值,OGNL可以通过` #`号访问到；     

OGNL一般都是和Stack2标签一起使用；          


通过Struts2标签 `<s:debug/>`可以看到值栈以及`Stack Context`的内容； 
以后要经常使用`<s:debug/>`,不仅可以解释原理，同时还能调试错误；        


### OGNL访问List
OGNL如何访问集合 (List)          
可以通过指明索引的方式访问List中的特定元素；         
如：    
```
<s:property value="streetsList[0]"/>
```
streetList为一个街道信息的集合List；     
可以通过索引 0 获得集合中的第一个街道信息；              

#### OGNL可以访问集合(List)的方法： 
如：        
```
<s:property value="streetsList.size()"/>
```
可以通过` size()`，来获取集合的元素个数；      
如：      
```
<s:property value="streetsList.idEmpty()"/>
```
可以通过 `isEmpty() `判断该集合是否为空          

由此可以推断，OGNL访问其他对象方法也是这种形式的；            

可以直接在OGNL中构造一个List；       
如：       
```
{1,2,3}
```
可以这样声明一个包含`1、2、3 `三个数字的List集合；  
然后，如果我们想取得特定值要通过索引；        
如：      
```
{1,2,3}[0]
```
这里通过索引0 获得集合中第一个元素；也就是 1；    
不过，这样直接构造List的做法并不常用；了解即可； 
一般都是动态获取集合内容的；          

OGNL访问数组与访问List类似；         


### OGNL访问集合 (Map)
OGNL如何访问集合(Map)          
Map是由键-值对组成的，因此当访问Map中特定元素时，需要通过如下方式：      
```
Map名称['键的名称']   或者  Map名称.键的名称       访问特定元素            
```

如：      
```
<s:property value="streetsMap['m1']/>
<s:property value="streetsMap.m1/>
```
streetMap为一个街道信息的集合Map；  
要获得这个集合中键为m1的值是什么，可以通过这两种方式；            

#### 可以访问Map的方法：       
如：      
```
<s:property value="streetsMap.size()"/>
<s:property value="streetsMap.isEmpty()"/>
```
可以直接在OGNL中构造Map:       
要以` #`号 开头，如下代码：  
```
#{'fist':'zhangsan','second':'lisi'}
```
我们构造了两个元素的Map，以 `#`号开头，键和值之间使用冒号隔开，两个元素使用逗号隔开；        
获取 键为 `first` 的值的写法：     
```
#{'fist':'zhangsan','second':'lisi'}['first']
#{'fist':'zhangsan','second':'lisi'}.first
```

不常用，了解即可；


### OGNL访问 Set
OGNL如何访问集合(Set) 
Set是无序的，想要访问Set中的指定元素，不可以直接通过索引访问；但是，      
可以把Set转换为数组，再通过索引访问；             
如：       
```
<s:property value="streetsSet.toArray()[0]"/>
```
streetsSet为一个街道信息的集合Set；         
要获得这个集合中的第一个元素，可以先转换成数组，再通过索引 0 获得；      

可以访问Set的方法：       
如：       
```
<s:property value="streetsSet.size()"/>
<s:property value="streetsSet.isEmpty()"/>
```

OGNL其他使用         
OGNL访问静态方法、静态属性：            
```
@类的完全限定名@静态方法名      
@类的完全限定名@静态属性名
```
强调： 
`Struts2.3.4.1`版本的默认静态方法调用是不允许的，要想使用OGNL访问静态方法，一定要修改`struts.xml`，为其添加如下代码：        
```
<constant name="struts.ognl.allowStaticMethodAccess" value="true" />
```

设置`struts.ognl.allowStaticMethodAccess`为true；          
不用死记硬背，可以去` default.properties `查找并复制；               


OGNL投影、选择         
实际上，OGNL还可以进行投影和选择；   
投影：      
选出集合中每个元素的相同属性组成新的集合；            
选择：        
过滤满足选择条件的集合元素；              