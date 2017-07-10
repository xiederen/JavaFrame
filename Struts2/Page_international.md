#  实现页面国际化
## 国际化简介
国际化，又称 I18N ;即  internationalization           
国际化的特征 ：         
在程序不做修改的情况下，可以根据不同语言环境显示相应内容         
#### 国际化框架的优点：  
简化国际化工作量          
便于修改国际化内容          


### Java内置国际化的实现步骤
#### 步骤一：   
准备存放国际化信息的全局资源文件    

通常国际化资源文件至少三个文件为一组，分别是： 
```
默认语言资源文件 ： message.properties
中文语言资源文件 :  message_zh_CN.properties
英文语言资源文件 :  message_en_US.properties
```
当然，也可以根据自己实际需求，加入更多其他语言的资源文件；        
要求：             

放置位置 ： src根目录下，通常和`struts.xml`文件在同一目录        

资源文件的名字都有相同的后缀 `".properties"`，相同的前缀 "message"     
中间是两组字母以下划线相连；            
这两组字母中前面小写的部分是语言代码，后面大写的一组是国家或地区的代码；         

##### 资源文件的命名规则：      
```
前缀名_语言代码_国家代码.properties 
```
其中，后缀 `".properties"` 是资源文件的扩展名，是固定格式；     
前缀 "message" 是自定义的，可以更换；但是要保证同一组资源文件的前缀要统一；         
因为前缀是获取资源文件的关键标示；也就是说程序是要根据资源文件的前缀名去找这组资源文件的；     
那么，中间的语言代码和国家代码也是固定的，需要哪种语言或国家就写相应的代码就行了；     
至于那个没有语言代码和国家代码的文件，就是默认语言的资源文件了；          

内容 ：    
内容格式 ：` key = value`     
英文资源文件的内容：      
```
userName = username
password = password
welcome = welcome!
```
它的内容都是以` key = value `的格式出现       
等号前面的就是 key，等号后面的就是 value；            


#### 步骤二：
使用Java代码实现        

问题：          
1.引用哪种语言？   
```
Locale locale = Locale.SIMPLIFIED_CHINESE;    //选择中文
```

2.引用哪组多语言文件？     
```
ResourecBundle bundle = ResourceBundle.getBundle("message",local);
```

3.引用哪条信息？     
```
String title = bundle.getString("userName");  //选择 key 为 userName的资源信息；
```

两个类：          
Locale类，ResourceBundle类；这两个类都是位于 `java.util`包下；       
Locale类负责选择语言；        
ResourceBundle类负责选择国际化信息的来源并返回信息；            

##### 具体的实现步骤：        

- 第一步，指定语言：  
```
Locale locale = Locale.SIMPLIFIED_CHINESE;    //选择中文
```

这句代码的作用是通过指定常量返回一个中文语言环境的locale对象；      

- 第二步，选择资源信息来源：
```
ResourecBundle bundle = ResourceBundle.getBundle("message",local);
//选择前缀为message这组文件
```

getBundle方法里一共有两个参数；      
第一个参数对应前面看到的前缀为 "message" 的一组资源文件；           
第二个参数是第一步取到的中文语言环境的locale对象；         
这句代码的作用是：             
根据locale对象，返回一个从前缀为 "message"的国际化资源文件里获取信息的资源包对象；                

有了资源包对象，就可以从它里面获取具体的资源信息了，接着第三步；        

- 第三步，返回指定的资源信息：       
```
String title = bundle.getString("userName");  //选择 key 为 userName的资源信息；
```

`bundle.getString` 方法返回一个key 为 "userName" 的资源信息；     
根据方法名可以猜到返回类型为 String类型；                  


经验：       
遇到不熟的Java类和方法可以查看JDK的API，可以快速掌握该类的常用方法及属性；         



### Struts2的国际化
Struts2的国际化框架，是在Java内置国际化的基础上的封装；        

Struts2如何实现JSP页面国际化        

### JSP页面国际化
首先，准备全局资源文件：通常至少三个文件；        
```
 命名规则 ： 前缀名_语言_国家.properties
 内容格式 ： key = value
```

### 用Java代码获取国际化信息 
```
引用哪种语言？          指定语言
引用哪组多语言文件？    选择资源信息来源
引用哪条信息？          返回指定的资源信息
```

在这，第一步就可以省略了；   
如果不指定语言，Struts2会获取一个本地默认的语言；      
所以，可以直接进行下一步，指定资源文件来源；          

#### 第二步：          
指定资源文件 (设置常量)        
Struts2是通过配置常量的方式指定资源文件的；          

方式一：              
在`struts.xml`文件里面指定常量       
	struts.xml`文件 :    
```
<constant name="struts.custom.i18n.resources" value="message"/>
```

value 里面的 "message" 对应资源文件的统一前缀，和Java指定资源文件类似；     

方式二：          
在struts的配置文件`struts.properties`里面；
	struts.properties :
		struts.custom.i18n.resources=message

这个常量可以到 struts的 `default.properties`里找；


#### 第三步：
JSP页面输出国际化信息     
JSP页面获取国际化资源信息，只需要通过一个标签就搞定了；     
```
输出文本信息 ：  <s:text name="key"/>
<s:text>标签：这个标签里有个name属性，里面的值对应的就是资源文件里的 key；
```

JSP通过 `<s:text>` 可以获取并显示国际化的文本信息；          

问题：   
`<s:text>`虽然很容易就获取到了国际化了的文本信息，但是，如果想在UI标签上使用国际化该怎么办？ 以 `<s:textfield> `为例；     
```
   传统做法：  <s:textfield label="userName"/>
   国际化  ：  <s:textfield key="userName"/>
```
通常，我们都是使用label属性来输出标签前面的提示信息；      
在这里，只需要把 "label"属性换成 key属性，就可以实现UI标签的国际化了；              

小经验：    
1.使用key属性或label属性时应保持当前标签的theme属性为默认的 "xhtml"类型       
2.资源文件不支持中文显示，真正项目开发中国际化字段较多，一般使用插件来辅助中文显示         


### 小结：
框架的封装总会比基础实现要简单方便很多；         
更主要的是，框架还有更多的实现方式和功能；            

注意细节：         
在准备资源文件的时候要注意命名规范；       
Struts2通过设置常量去指定资源文件的时候，要注意常量的写法；           
以及在使用UI标签实现国际化时，需要注意theme属性的类型等等；       

准备资源文件         
设置常量指定资源文件          
`struts.xml`或`struts.properties`文件指定     
JSP页面输出国际化信息               
```
 文本信息国际化 ： <s:text name="key"/>
 UI标签国际化   ： <s:textfield key="key"/>
```
UI标签国际化时需要注意标签的theme属性应为 "xhtml"类型；       


### Action类国际化
问题：         
假如在访问某Action类的业务方法时，需要返给页面一段提示信息，这种情况该如何实现国际化？       

#### Action实现国际化
在Action实现国际化，首先需要继承ActionSupport类；    
ActionSupport类除了可以实现数据校验等功能以外，还提供了一系列的`getText()`方法；它可以实现从资源文件里获取国际化信息；       
访问无参资源信息 ： `getText(String key)`;   
这里的参数 "key" 对应资源文件里的 key值；        

问题：        
假如登录成功后，需要返回一个包含登录用户名的有参动态国际化信息，该怎么办？   
如果需要获取动态国际化信息如何实现？    
实现步骤：   
```
1.修改资源信息： welcome = {0} welcome!
2.访问有参资源信息： getText(String key,String[] args);
```

参数key对应资源文件的key，数组args对应需要传入的参数；       
首先，需要在资源文件里定义一条资源信息，不同的是，value里用 `{0}` 这样的占位符来表示参数；     
接着在Action类里还是使用          `getText()`方法，去获取国际化信息；但是，这个方法里有两个参数，第一个参数与上面方法的参数一样对应资源文件的key，而第二个参数是个String类型的数组；这里用来存放需要传的参数；        

问题：        
如果需要多个参数呢？        
因为第二个参数为数组，所以它可以将多个参数放到数组里，然后传入资源文件；只需要相应的修改资源文件的value，定义相同数量的占位符就可以了；             


问题1：      
既然在Action类可以访问有参国际化信息，那JSP页面是否同样可以？   如何实现？           

JSP页面如何访问有参国际化信息               
资源文件：  `welcome={0} welcome!`          
使用 `<s:text>`标签，添加 `<s:param>`           
```
  <s:text name="welcome">
        <s:param>userName</s:param>
  </s:text>
```

首先，还是需要先在资源文件里定义好有参国际化信息；      
然后还是使用之前学过的` <s:text>`标签，这次只不过是利用 `<s:param>`标签，给他传入一个指定的参数就行了；这里的 "userName" 就是传入的参数值；      

问题2：         
既然 `<s:text>`可以使用`<s:param>`传入参数访问动态国际化信息，那UI标签是否同样可以呢？         
UI标签不可以；               


有参资源文件格式：          
```
 key = {0} value{1}
```
Action类国际化             
```
 访问无参国际化资源信息  getText(String key)
 访问有参国际化资源信息  getText(String key,String[] args)
```
JSP页面：         
```
 使用<s:text>和<s:param>传参访问有参国际化资源信息
 UI标签不能使用<s:param>传参
```

有参资源文件的新格式：         
key等于占位符value或者多个占位符；       
Action类的国际化：         
用getText的多态方法分别访问了无参国际化资源信息和有参国际化资源信息；            
JSP页面的有参国际化：          
使用`<s:text>`和`<s:param>`传参访问有参国际化资源信息；       
UI标签不能使用`<s:param>`传参；            


资源文件的查找顺序         

前面介绍了Java国际化和struts2国际化，应该知道国际化都离不开资源文件；    
之前也介绍了全局资源文件的命名规则；书写格式和放置位置；             
其实，除了全局资源文件以外，还有包级资源文件和Action类级资源文件；               
那么，它们的作用是什么？和之前介绍的全局资源文件有什么不同？        
资源文件根据不同有效范围可分为：          

|类别名称  |             命名规则             |   放置位置|
|-----|----|----|
|全局级资源文件  |     前缀名_语言_国家.properties |      src根目录(和struts.xml同一目录)|
|包级资源文件     |    package_语言_国家.properties |     在本包目录下(只在本包有效)|
|Action类级资源文件  | Action类名_语言_国家.properties|   和Action同一目录(只对该Action有效)|

全局级资源文件的前缀可以自定义的；      
包级资源文件的前缀 "package" 是固定写法，不能变的；       
类级资源文件的前缀是， "Action类名"           
如果想为某个单独的Action类定义国际化资源文件，就需要保证这些资源文件的前缀名，和目标国际化Action类名一致；           

假如为某个Action类自定义了一个类级国际化资源文件，同时在该Action类所在包中也存在一个包级的资源文件；      
在访问这个Action类的时候，会优先获取类级国际化信息；          
如果，类级国际化信息不存在，会访问包级资源信息；         
如果，包级国际化信息不存在，就去访问全局信息；           
如果都没有，就返回输入的 "key"字符串；            

概况来讲，        
查找顺序：           
```
类级 > 包级 > 全局级
```

JSP页面通过 `<s:i18n>`获取指定的国际化资源文件信息          

用法： `<s:i18n name="message">...</s:i18n>`        
```
struts.xml : <constant name="struts.custom.i18n.resource" value="message"/>
struts.properties : struts.custom.i18n.resources = message
```

struts2提供了一个`<s:i18n>`标签，用来临时指定国际化资源信息来源； 
这个标签的 name属性值对应之前讲过的 "struts2使用常量指定资源文件"里面的value值；      
同样的，只要name里填入要指定的资源文件的前缀，就可以临时指定标签内所有国际化信息的来源了；        


#### 经验总结：
使用UI标签实现国际化时，需要注意theme属性的类型；     
遇到问题查API、看源码、帮助文档或google都是解决问题的好办法，      
另外亲自动手去验证是巩固知识的最好方式；            