# 文件上传
在实际项目开发中，我们经常要用到文件上传的功能； 
在学习JSP的时候，我们也学习过文件上传；         
在JSP中，实现该功能的代码很多，也很复杂；        
在Struts2中，对文件上传进行了封装，大大简化了实现过程；       

在学习之前，要先做好准备工作：       
首先，把` commons-fileupload-1.2.2.jar  commons-io-2.0.1.jar`引入到项目中；  
准备好帮助文档，随时查看：   `struts-2.3.4.1/docs/WW/file-uploda.html`

#### 实现步骤：
首先，在JSP页面实现在客户端选择上传文件；    
然后，配置`struts.xml ` ，这样，拦截器会自动接收上传的文件；     
接下来，在Action中写代码，把上传的文件存入到服务器中；     
最后，跳转至新页面展示上传的文件；          

#### 小结：
关键点：   
首先，`<form>`需要添加如下代码：     
`<form> `需要添加 `enctype="multipart/form-data" `属性            
这个是表单上传文件的专用编码格式；       
其次，在Action中添加接收文件的成员时，成员的setter命名规则如下：            
```
(x为上传文件的name)
setX(File file)
setXContentType(String contentType)
setXFileName(String fileName)
```

忘了的话，可以在帮助文档中查找；     
最后，设置文件保存路径的时候，可以使用如下代码：       
设置文件保存路径的时候使用    
```
ServletActionContext.getSevletContext().getRealPath("/")
```

取得项目真实路径    
再把文件保存到子目录中就可以了；          


### 限制上传文件的大小
在实际开发中，我们经常需要限制上传文件的大小和类型；     
为什么要限制上传文件的大小？              
客户端上传的文件过大会影响服务器的性能，消耗服务器的存储空间；          

要实现限制上传文件的大小有两种常用的方法：              
实现方法：          
- 第一种：    
在`<struts>`节点下配置常量：         
在`<struts>`节点下添加 `struts.multipart.maxSize`常量          
```
<constant name="struts.multipart.maxSize" value="5000000"/>
```

它是一个全局配置，可以实现对所有<action>的文件上传大小限制          
其中，`value="5000000" `指的就是上传文件的最大值为500万个字节；        

- 第二种：   
在`<action>`下配置拦截器：             
```
<interceptor-ref name="fileUpload">
     <param name="maximumSize">5000000</param>
</interceptor-ref>
<interceptor-ref name="defaultStack">
```

它可以限制单个`<action>`的文件上传大小；      

可以根据具体情况选择使用；           


### 在文件上传的时候，我们还可能需要限制上传文件的类型；          
为什么要限制上传文件的类型：            
这是因为：           
1.某些特殊类型的文件，如`:exe`文件可能包含病毒程序             
2.某些特定业务只需要上传特定文件类型，如：上传照片只需要上传图片类型文件；       
实现方法和限制大小类似；                  
实现方法：              
在`<action>`下配置拦截器：        
```
<interceptor-ref name="fileUpload">
     <param name="allowedTypes">image/pjpeg,image/jpeg</param>
</interceptor-ref>
<interceptor-ref name="defaultStack">
```

经验分享：         
1.在Action中判断文件大小后再限制上传，这样可以吗？     
实际上，这是一种错误的做法；             
因为上传是在拦截器中完成的；             
它运行在Action之前，到了Action再限制上传已经迟了；        

2.`struts.multipart.maxSize` 常量默认值为 `2M`，上传大于`2M`的文件一定要修改           
该值；        
例如：如果你要上传`5M`的文件，只设置`<action>`的拦截器是不够的；                
`struts.multipart.maxSize`常量值也需要设置大于`5M`；               

3.由于不同浏览器对文件类型的定义有区别，实现文件类型控制时需要考虑兼容性；  
如：JPG文件需要在allowedTypes参数中定义 `image/pjpeg,image/jpeg`两种类型；               

4.配置拦截器时，不要忘记配置` <interceptor-ref name="defaultStack">`这个拦截器栈；            


### 上传多个文件
一次性上传多个附件也是项目经常需要实现的功能；    
实现思路：      
首先，在客户端需要选择多个文件，该功能可以使用JavaScript添加多个file标签来实现；                
然后，在Actin类中接收多个文件；           
接着，把上传的多个文件都存入服务器中；         
最后，在新页面展示上传的所有文件；            



经验分享：        

1.操作视频中使用了JavaScript添加多个file标签，该功能使用jQuery实现会更简单；       
2.操作视频中接收文件的成员使用了List集合，使用数组也可以实现；          
这点在帮助文档上写的很清楚，并且附带了实现代码；             
所以：大家要培养查找并参考帮助文档的习惯；        


学习技巧：       
1.学会使用帮助文档；      
2.提高代码量，锻炼出扎实的代码功底；       
