# Struts2 UI标签
从名字上看，`Struts2 UI`标签就是用来生成UI界面，也叫Web界面的标签。   
或者，为Web界面提供某些功能支持，比如，    
表单标签，就是把各种途径获取的需要展示的数据，通过动态生成HTML的形式展示在界面上；           

`Struts2 UI`标签的实现是基于模板和主题的； 
模板 (Template)       
模板其实就是一些代码;       
在Struts2中通常是用 FreeMarker 编写的文件 (.ftl)            
FreeMarker 是一个用Java语音编写的模板引擎；它也是一种被Struts2支持的表现层技术之一；  
模板用于生成HTML；    
说白了，一个标签最终生成什么样的HTML代码，就是由一组FreeMarker的模板来定义的；    
这组模板被存放在Struts2核心jar包的template包中；                 



刚才说模板生成最终的HTML代码；               
假设不同标签使用不同模板，一个页面有好多标签；这样，这个页面就有很多的风格；此时页面风格杂乱；  
那么，如何统一各标签风格呢？     
主题就能解决这个问题；           

### 主题 (Theme)
将一组具有共同风格和观感的模板组织到一起就形成了一个主题；   
Strut2使用目录名作为主题名； 
具有共同观感的模板文件放在同一个目录下；      
可以通过切换主题来切换标签生成的HTML的风格；         

设置了同一个主题，那页面中的所有标签就具有了共同风格； 

#### strut2内建主题
Strut2中有四个内建主题；它们是：    `simple   xhtml  css_xhtml  ajax`        
把它们理解为四种不同的风格就可以了；           
其中，xhtml是Strut2中默认的主题；可以通过查看 `default.properties`文件获取到这个值  
查看 `default.properties : struts.ui.theme = xhtml`        
从这里可以看出来，通过 `struts.ui.theme `就可以指定主题是 xhtml；                

问题：   
可以自己修改主题吗？  
可以复制 `strut.ui.theme `常量到 `struts.xml `配置文件中，然后配置它的值为相应的主题就可以了；       
例如：     
```
<constant name="struts.ui.theme" value="simple" />
```

这里，我们把主题由默认的xhtml修改为了simple； 
        
还有一个办法，就是可以直接设置相应标签的 theme属性；         



### form表单标签
Strut2的表单标签会在HTML标签中找到对应的标签，所以各标签属性可以对比之前HTML标签的属性，作用一样；  
比如：           

-  <s:form> 表单标签       对应HTML中的 <form>标签  `<from></from>`        
属性： `name、action、method `     
name属性 ：指定名字     
action属性 : 指定表单处理程序  
method属性 : 指定表单提交方式           


- <s:textfield>  用来生成单行文本框 对应HTML标签的text元素  `<input type="text"...>`               
属性 : `name value maxlength readonly `            

- <s:textarea>  生成多行文本框 对应HTML中的textarea标签   ` <textarea></textarea> `      
属性 :` name value cols rows readonly `    
强调：             
value属性；                       
之前在HTML标签中指定默认值时，必须使用<textarea>标签内的文本，value属性不起作用；        
即： <textarea>默认值</textarea>         
而在struts2标签中正好相反；必须使用value属性指定默认值；     

- <s:submit>  生成提交按钮 对应HTML标签中的submit元素  `<input type="submit"...>`      


- <s:select>  用来生成一个下拉框 对应HTML中的select标签；  `<select></select>`
	- name属性 : 代表下拉框的名字    
	- list属性 : 用于生成下拉框的集合     
	- listKey  : 代表生成的选项的value属性     
	- listValue : 代表生成的选项显示的文字   


- <s:dobuleselect> 用于生成两个联动的下拉框 
	- name属性 : 代表第一个下拉框的名称     
	- list属性 : 代表用于生成第一个下拉框的集合   
	- listKey属性 : 代表生成第一个下拉框的选项的value属性    
	- listValue属性 : 代表生成第一个下拉框的选项显示的文字      

因为它是联动的下拉框，所以还有一个相应的 `double**` 的属性，分别代表第二个下拉框的相应属性
doubleName属性 : 代表第二个下拉框的名字        
doublList属性 : 代表用于生成第二个下拉框的集合   
doubleListKey属性 : 代表生成第二个下拉框的选项的value属性     
doubleListValue : 代表生成第二个下拉框的选项显示的文字          


### Ajax标签 
datetimepicker标签 : 日期、时间     
datetimepicker标签可以实现一个日历效果；          
大家可以通过它来选择一个日期；           
name属性 :  代表它的名称                   
lable属性 : 用来显示它之前的文本              

注意:    
datetimepicker标签 属于 Ajax标签         
datetimepicker标签 的使用步骤：       
使用Ajax标签的时候，需要将 `struts2-dojo-plugin-2.3.4.1.jar`包引入项目中来；        
接下来，需要在页面上使用 taglib指令引用其中的标签库，为这些标签取一个前缀；        
比如：  
```
<%@taglib uri="/struts-dojo-tags" prefix="sx" %>
```
这里，定义了前缀为 `sx`；    
要使用 datetimepicker标签 ，实现日历的效果，还需要在<head>标签中加入如下代码：           
```
<sx:head parseContent="true" />
```
可以把它理解为设置了这个日历的样式； 
最后，就可以在页面中使用datetimepicker标签了；      


UI标签、Ajax标签实际使用并不多，通过帮助文档了解即可；      



问题一：doubleselect实现的地区与街道信息的联动下拉框，如何一行显示？  
方法一：       
添加CSS样式 
如： 
```
.add br{display:none;}
```
设置br,也就是换行的display为none就可以了； 
这种方法只能修改特定doubleselect的样式；     

方法二: 
可以修改不同页面的doubleselect的样式         
复制 `/template/simple/doubleselect.ftl`到项目中，修改去掉其中的<br/>，设置doubleselect标签的theme属性为修改后的模板；    


问题二：房屋信息发布失败时，返回房屋信息发布页面，此时，该页面之前填写的数据还能回显吗？ 
实现数据回显：            
确认表单标签name与Action属性一致；    
能找到对应的setter以及getter方法；           
添加信息失败后要跳转到添加信息页面之前的Action，并应该以转发形式，而不是重定向；  


学习方法：       
分清重难点复习        
多翻阅帮助文档        