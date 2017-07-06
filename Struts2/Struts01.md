# Struts2体系结构
Struts2 发展史      
概述                
 Struts 2 是一个全新的、非常先进的Web应用程序框架，在WebWork 2的基础上进行开发的，以webwork优秀的设计思想为核心；                       

- Struts 2 发展史    
- struts1简介      
- struts1优点和缺点          
- Webwork设计思想           
- Struts2的诞生               

## Struts1简介
Struts1是apache公司的开源项目，是基于mvc的web应用程序框架；             
Struts1发布于2001年6月，是世界上发布最早的mvc框架；                 
Struts1的成功得益于其丰富的开发文档以及广泛的开发群体；                  
开发者在互联网上可以搜索到很多关于Struts1开发的文档。                
## Struts1的基本框架
Struts1框架以ActionServlet为核心控制器；              
整个应用由客户端请求发起；                  
当客户端向web应用程序发送请求时，请求被Struts1的ActionServlet，也就是核心控制器拦截处理，通过`Struts-config.xml`这样的配置文件找到实际的处理类并将请求发送到具体的业务逻辑控制器中；然后，将业务逻辑控制器的处理结果返回给客户端并将数据显示出来。                                 
对于整个Struts1的框架而言，最核心的是控制器；                 
Struts1的控制器可以分为两种：                        
一种是业务逻辑控制器；另外一种是核心控制器；                
核心控制器就是之前所说的ActionServlet。                  
业务逻辑控制器是开发者根据具体的需求所定义的控制器；                                              
核心控制器通过配置文件找到业务逻辑控制器并将请求转发给客户端进行数据的显示；                  
那么，两个控制器是相互关联的关系。                     

### Struts1缺点
- 第一：        
视图的支持比较单一；   只支持JSP视图；

- 第二：
`Servlet API`严重耦合，难于测试；

- 第三：      
`struts API` 严重耦合   execute方法中包含过多的框架对象     


### Webwork优秀的设计思想
webwork是优秀的开源组织opensymphony开发的框架，其设计理念要比struts1先进很多；    
但是，二者还是有很大差别的，具体表现在：           

- 第一、          
Struts1只支持jsp作为视图层，但webwork可以支持更多的视图技术；                         
如：`freemarker、velocity、jsp、json、xslt`等；            
相对于Struts1框架中出现大量的`servlet api，webwork`的业务逻辑控制器可以是一个简单的java类，即pojo；其采用了一种松耦合的方式让框架不再与`servlet api`耦合，或者与框架的api耦合在一起；                
使单元测试更加的方便；                   
允许系统从`b/s`结构向 `c/s`结构进行转换；          

- 第二：                   
Ognl表达式便捷的数据访问；               
采用ognl强大的表达式语言，可以轻松的访问valueStack也就是值栈；                 
ognl对集合和索引属性的支持同样也非常强大；                   
Struts1与webwork在执行流程上看，还是非常相似的；都是由核心控制器和业务逻辑控制器组成；                                
Struts1核心控制器是`ActionServlet，webwork`的核心控制器是ServletDispatcher；它们都负责处理用户的请求并将请求发送到相应的业务逻辑控制器当中进行处理然后转发；                          

### Struts2的诞生
Struts2的诞生源于著名的开源组织opensymphony的web应用程序框架webwork；             
和Struts1一样，webwork也是流行框架之一；                
webwork要晚于Struts1，所以其技术先进性要优于Struts1；               
Struts1在这几年的发展过程中，其稳定性和可靠性得到了开发者的认可；                  
但，Struts1在整个市场需求不断的变化当中，其设计缺陷也逐渐显现出来；                  
需要对其框架进行有效的改善；                     
所以，2005年，webwork团队和Struts1团队决定合作开发下一代的web应用程序框架；          
Struts2很好的继承了webwork的核心思想，并且吸收了Struts1的设计理念；               
于是，Struts2诞生了；                         
在这里需要说明的是，Struts2与webwork框架在很多地方都有着共同点，这并不像Struts1框架；这是因为Struts2是以webwork为核心的，而不是以Struts1为核心；                       
所以，之前做webwork框架开发的程序员可以很轻松的转向Struts2框架中去。                             

### Struts2的工作原理
MVC工作原理；                 
Struts2采用了MVC的设计模式；                 
什么是MVC设计模式：                      
MVC是包含了多个设计模式的框架结构；                         
它强制性的使应用程序的输入、处理、输出分隔开来；                
MVC模式将应用程序分成三个基本部分，即：                            
`Model(模型)、View(视图)、Controller(控制器)`;                       
它们以最少的耦合方式协同工作，以提高应用程序的可维护性和可扩展性的目的；               
MVC模式的目的是将M(模型)和V(视图)代码进行有效地分离；                       
##### MVC当中的各个模块之间有什么作用呢？         
首先，M(模型)是用来存储数据的地方；V(视图)是用来显示M(模型)当中的数据；                       
C(控制器)是M(模型)和V(视图)之前的沟通桥梁；同时还负责分派用户的请求并根据视图当中的数据更新视图；            
##### MVC模式是如何工作的呢？
当视图发送请求到控制器当中，控制器将负责将数据填充到数据模型当中；然后进行转发，转发到视图页面中进行数据的显示；            
##### MVC模式带来了哪些好处呢？        
首先，M(模型)、V(视图)、C(控制器)三层各司其职；                   
一旦哪个层出现了问题或者发生了变化不会影响到其他层；              
其次，三层分工明确；                       
如：美工只需要负责关心视图部分；程序员只需要关心控制器和模型部分；                   
最后，有利于组件重用；                         


前面介绍了基本的MVC模式，那么Struts2在MVC模式当中是怎样的呢？                  
在该框架中MVC各个层扮演着什么样的角色呢？                                  
- Model(模型)是数据存储组件，是用来存储数据的地方；                      
对于Struts1，采用了FormBean的`Struts api`；               
这种方式过多的依赖框架，造成了可扩展性很差；                             
那么，Struts2对这个部分进行了很有创意也很先进的改良；                   
即，通过在业务逻辑控制器当中定义一些简单的属性，然后生成相应的set和get方法；这样就实现了数据从视图到控制器之间的通讯；            
这种方式设计的很巧妙，同时也很实用；                
- View(视图)，即视图组件；            
Struts2所支持的视图包括`jsp、velocity、freemarker、json`等等；            
相对于Struts1，Struts2在视图方面支持的很丰富；                     
该方式既要写相应的脚本文件，又要对脚本文件在Struts2配置文件当中进行配置；                     
这些都是开发Struts2应用程序最基础最常用的部分；                   
- Controller(控制器)，即 控制器组件；                
在Struts2框架中，控制器分两种，一种是核心控制器，另一种是业务逻辑控制器；              
它们的职责主要是用于转发和数据处理。                       

#### 下面简要介绍一下Struts2的mvc处理流程：
首先，客户端从视图层提交请求到服务器端；             
服务器端通过应用Struts2的核心控制器进行过滤处理；                  
filterDispatcher，也就是核心控制器；根据用户的配置文件信息找到相应处理该请求的业务逻辑控制器并转发到该业务逻辑控制器；在业务逻辑控制器中将具体进行业务逻辑处理，并将数据封装到模型层当中；这个模型通常是业务逻辑控制器当中的成员变量；最后，转发相应的视图文件并将模型当中的数据进行显示；                                   
#### Struts2的具体执行过程是什么样的呢？                                   

一共分为五个步骤：         
- 第一：客户端发送请求；                
- 第二： filterDispatcher，也就是核心控制器，根据请求调用相应的业务逻辑控制器；            
- 第三：Struts2的拦截器链自动对该请求应用不同的功能；             
如：上传和数据校验等等；                         
- 第四：回调业务逻辑控制器当中的execute方法；在该方法体内将数据填充到模型组件当中；             
- 第五：将业务逻辑控制器当中的execute方法执行结果信息输出到客户端进行显示；                      


## Struts2 体系结构
### XWork框架简介：               
Xwork是一个标准的用命令设计模式(command模式)实现的框架，完全从web层脱离出来；                         
Xwork提供了许多的核心功能，如：拦截器、运行时表单验证、类型转换、ognl、ioc控制反转，等等。                     
其目的是：       
创建一个泛化的、可重用、可扩展的框架，而不是只针对某个领域；              
### Xwork的特点：                      
- 第一：
只要一个简单的接口，就可以进行灵活且可自定义的配置；              

- 第二：
核心命令模式框架可以通过指定和扩展拦截器来适应任何请求或者响应环境；                  

- 第三：
框架通过类型转换和ognl的action属性来创建；            


Struts2框架中，各个对象之间是怎样协调工作的呢？                
首先，客户端提交一个请求到服务器端；                   
请求到达服务器端，并且通过过滤器进行拦截；              
过滤器包括：`ActionContextCleanUp、siteMesh、FilterDispatcher`等等；              

- `FilterDispatcher`是Struts2的核心控制器，它是基于mvc模式的Struts2核心控制器；所有的请求都会在这里进行处理和转发。

- `FilterDispatcher`询问`ActionMapper`来决定那个业务逻辑控制器用于处理该用户的请求；相应的`FilterDispatcher`会把请求的处理交给`ActionProxy。`             

- `ActionProxy`通过`configuration Manager`来查找相应的配置文件；找到需要调用的业务逻辑控制器类(Action类)；ActionProxy同时创建一个`ActionInvocation`实例；同时，ActionInvocation通过代理模式调用业务逻辑控制器类(Action)；但在调用之前，ActionInvocation会根据配置文件加载业务逻辑控制器相关的所有拦截器。            

当Action执行完毕，ActionInvocation负责根据`Struts.xml`中的配置找到对应的result结果，并将数据返回到视图组件当中；

```
ActionContextCleanUp:用于清除ActionContext中的数据内容；
filterDispatcher:用于转发和处理数据
ActionMapper:屏蔽了Action对request等java servlet api的依赖
ActionProxy:是一个Action的一个代理，由ActionProxyFactory所创建；
ConfigurationManager:用于加载Struts2框架配置文件；
ActionInvocation:用于执行Action和拦截器的功能；在ActionProxy代理类中被创建；
Interceptor:是拦截器；  进行功能拦截处理；
Result:输出结果类型
```
Result要`在Strus.xml`文件当中进行配置，常见的语法如下：
```
<result name="success" type="dispatcher">success.jsp</result>
```
这里的type可以是：
`dispatcher chain freemarker velocity redirect json` 等等         
功能：        

dispatcher是默认的结果类型；                         
chain结果类型用于处理多个action链；                                    
Freemarker结果类型用于处理freemarker模版文件；                     
Velocity结果用于处理velocity模版文件；                   
Redirect用于重新定向到一个url的结果类型；                                       
redirectAction用于重新定向到另一个action的结果类型；                    
Stream类型用于提供用户下载的结果类型；                         
Plaintext用于显示文件的源代码的结果类型；                            
Json将返回结果定义成一个json对象在页面中进行显示；                  


## 初始Struts2
在初识Struts2当中，我们将从最基本的关于Struts2环境的下载；安装到最后的由登录开始，一步一步的搭建起开发Struts2框架的基本构架；                 
最后通过上机练习在已经搭建好的框架基础之上，开发一个简单的登录例子，使大家了解Struts2开发的基本步骤。                    

首先，下载和安装必要的lib包；                   
在这里，我们首先要登录apache公司的网站，下载 Struts2                                  
[登录站点](http://struts.apache.org/download.cgi#struts25101)                       
并将版本当中的lib包拷贝到当前的web应用的`WEB-INFO/lib`路径下；                                      
最后，我们需要配置`web.xml`的文件；           
我们要编辑web应用的`web.xml`配置文件，配置Struts2的核心filter。           

下面看一下`web.xml`配置文件的最基本的代码段部分；          
配置后的`web.xml`代码段如下：       
```xml
<filter>
  <filter-name>strutes2</filter-name>
  <filter-class>org.apache.struts2.dispatcher.FilterDispatcher</filter-                                                                  class>
</filter>
<filter-mapping>
  <filter-name>strutes2</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>
```
首先，FilterDispatcher是一个过滤器的类；它是Struts2的核心控制器，用来加载和初始Struts2的框架；          
其次，要配置Struts2的过滤器映射路径；确保对从客户端发送过来的请求都可以通过这个过滤器来进行拦截；过滤器是Struts2的重要的部分，一定要在`web.xml`配置文件当中进行配置；否则Struts2框架将无法被加载，更无从谈起基于Struts2框架下的web应用程序开发。               
到此为止，我们已经构建好了一个基本的Struts2开发构架，下面就可以进行开发了；              

### Struts2配置
概述：                        
之前，我们已经了解到了Struts2框架的基本构架；                   
Struts2的框架都是建立在配置文件的基础之上的，这些需要配置的文件是Struts2最重要也是最核心的部分；                  
那么，在什么时候需要用到这些配置文件呢？                   

- 第一：
当框架需要与web应用整合时，需要用到配置文件；     

- 第二： 
当Struts2创建action代理对象的时候，需要配置文件；              

- 第三：
当框架加载全局属性时，需要配置文件；        

##### Struts2的配置文件主要包括以下几个部分：
```
struts.xml  : 用于配置业务逻辑控制器、拦截器等的struts.xml配置文件；
struts.properties : 用于配置Struts2框架系统属性的struts.properties配置文件；
web.xml : Web应用的配置文件；即， web.xml文件；
struts-default.xml : struts-default.xml文件，是Struts2框架默认加载的配置文件；
struts-plugin.xml : 插件配置
```

struts.xml文件是Struts2的核心配置文件，其中包括了拦截器、业务逻辑控制器以及结果类型；               
struts.xml文件可以包含其他的配置文件，通常应用在一个系统当中的多个模块的开发的情况。                    
struts.properties配置文件定义了大量的框架属性；                      
开发者可以通过改动属性，来适应不同的项目需求；                              
该文件包含了大量的 `key-value `对象；                 
`web.xml`配置文件：当任何框架如果要与web进行整合的话，都需要配置 `web.xml`这样的文件；该文件用于加载并初始化框架。           
`struts-default.xml`文件：是Struts2默认加载的配置文件；其中定义了框架底层的拦截器等信息；            
`struts-plugin.xml` 文件提供了可选的插件配置；                       
在框架启动的时候被加载；                 


##### Web.xml文件配置       
加载并初始化FilterDispatcher过滤器；           
设置过滤器的路径映射；           
加载必要的初始化参数；                     

任何基于web的mvc框架整合都需要针对这个`web.xml`文件进行配置；                  
只有配置这个文件，框架的核心控制器或者是servlet才会被加载并初始化；                     
对于Struts1而言，在配置文件中需要加载ActionServlet这个核心控制器；                  
那么对于Struts2而言，则需要加载FilterDispatcher核心控制器；                    
那么不同的框架，其加载的核心控制器的类型是不同的；                           
开发基于Struts2的mvc框架就必须加载FilterDispatcher核心控制器；                        
只有通过它，才可以加载Struts2的框架；                       
Struts2将核心控制器设计成了一个filter，即过滤器，而不是一个servlet；                
同时需要将FilterDispatcher的拦截路径设置为对用户所有的请求进行拦截；                   
因此，在`web.xml`中需要加载这个过滤器，从而加载整个Struts2的框架；                         
下面看一下配置文件的代码段：                     
```
<filter>
  <filter-name>struts2</filter-name>
  <filter-class>
       org.apache.struts2.dispatcher.FilterDispatcher
  </filter-class>
  <init-param>
       <param-name>actionPackages</param-name>
       <param-value>指定需要加载action的包</param-value>
  </init-param>
</filter>
<filter-mapping>
    <filter-name>struts2</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

正如上面所看到的，当配置Struts2的核心控制器FilterDispatcher的时候，需要指定一系列的初始化参数；其中，比较常用的参数 有：                            
常用的配置参数：                     

- 1、Filter 和 filter-mapping ：           
```
Filter用来配置过滤器；其中：
filter-name : 指明过滤器的名称；
filter-class : 用来指明过滤器的类；
filter-mapping ：用来配置过滤器映射的路径(url)；
 /*是通配符，表示对所有的请求进行拦截；
```

- 2、Config ：                      
该参数接收配置文件的位置；                 
当有多个配置文件的时候，要用英文的逗号分隔开；              
Struts2会自动加载指定的配置文件。            

- 3、actionPackages ：              
用来指明包含有action类的包名；             
Struts2将搜索该路径下的action类；多个包名同样用英文逗号分隔开；                   

- 4、configProviders ：              
用户可以通过实现ConfigurationProvider这个接口类来实现自定义的ConfigurationProviders；之后将这些类的类名设置成该属性的值；多个类名之间用英文逗号分割；             

- 5、loggerFactory ：           
LoggerFactory实现类的类名；在实现类中要加载日志的文件；                  

- 6、taglib  标签库              
标签库是用于操作页面数据的工具；                   
其中，`taglib-url`这个标签是Struts2的uri，即统一资源标识符；                            
`taglib-location` 用来指明标签的实际位置。                      

提示：           
需要说明的是，                
在`servlet2.3`版本以前，不会自动加载struts2的标签库；       
所以在`web.xml`配置文件当中需要加载Struts2的标签库；              
```
<taglib>
  <taglib-uri>/struts-tags</taglib-uri>
  <taglib-location>/WEB-INF/struts-tags.tld</taglib-location>
</taglib>
```

那么如果高于这个版本，就不需要配置；            

Struts2的标签库文件在Struts的核心jar文件包里，这个包里面有一个`META-INF`文件夹；那么`struts-tag.tld`文件就在这个文件包下面；这个文件就是Struts2的标签库文件。                 
到现在基本上就完成了`web.xml`的配置工作。                            


##### struts.properties配置文件
`struts.properties`用于定义Struts2的框架属性，对于不同的需要可以改变其属性值；它包含了大量的值键对，即key和value的这种形式；那么，key 对应着属性；value对应着属性值；                     
`struts.properties`通常放在` WEB-INF/classes`路径下面；                      
下面，我们对常用的属性进行介绍：              
属性包括：               

- struts.configuration ：                  
用于加载struts2配置文件的配置文件管理器；              
默认是DefaultConfiguration这个类；那么它同时也是Struts2默认的配置文件管理器；            

- struts.locale :        
用于指定web应用程序的默认locale，通常用来实现国际化的部分；         

- struts.i18n.encoding :          
用来指定web应用程序的默认编码集；          
相当于调用HttpServletRequest的setCharacterEncoding方法。                  

- struts.objectFactory :         
用于指定Struts2的`objectFactory Bean`，默认是spring；           

- struts.objectFactory.spring.autoWire ：           
用于指定spring框架自动装配模式；                 
通常该属性默认值为name，即以bean的名称进行自动绑定；        

- struts.objectFactory.spring.useClassCache :          
该属性当使用spring，就是框架时，用于是否将bean的实例进行缓存；          
默认值是true；在这里，建议不修改该属性值；          

- struts.objectFactory.spring.autoWire.alwaysRespect :             
用于是否spring自动装配总是有效；              

- struts.multipart.parser :             
用于指定上传文件的解析器；     上传文件数据的解析；         

- struts.multipart.saveDir  :         
用于指定上传文件的临时保存路径；         
这个属性最好在开发Struts2开始时就进行设置；                  
否则当上传文件的时候，可能会报  `unable to find` 这个属性；                
这是因为系统默认使用 tempDir 作为上传的路径设置；这个路径通常在Tomcat下面的localhost下；                

- struts.multipart.maxSize :          
用于指定上传文件的最大字节数；         

- struts.devMode :     
用于是否启用开发模式；              
如果是true，则在应用出错时，可以显示更多的友好出错信息；           
那么，建议在开发阶段将其设置为true；当产品发布以后将其设置为false；                   
- struts.custom.properties :          
加载自定义属性文件；          
该属性不会覆盖掉`struts.properties`文件中定义的属性；             
多个自定义的文件要用英文的逗号分隔；             

- struts.action.extension :          
用于指定struts2处理请求的后缀名；               
默认是action； 说明所有以 `.action` 结尾的请求都将通过框架进行处理，多个后缀要用英文的逗号分隔开来；              

- struts.serve.static.browerCache :        
用于设置浏览器是否缓存静态内容；               
建议在开发阶段将属性值设为false；这样对于每次的http请求都是最新的数据；              
在发布的时候，可以缓存静态的页面；即，将该属性值设置为 true；          

- struts.enable.DynamicMethodInvocation :               
用于设置struts2是否支持动态方法调用；         
默认值为true；表示允许动态方法的绑定；           

- struts.enable.SlashInActionNames :            
用于指定是否允许在struts类名当中加入斜线；               

- struts.tag.altSyntax :            
用于指定是否允许在struts2标签中使用表达式语法；                   
在struts2当中经常会用到标签的表达式，所以建议不要修改其属性值；             

- struts.i18n.reload :            
用于指定是否每次http请求，系统都会重新加载资源文件；          
那么，在开发阶段建议将其设置为true；这样针对每一次的http请求都会重新加载这个资源文件；                
那么，当产品进行发布的时候，我们要将其设置为false；                   

- struts.custom.i18n.resources :           
用来指明国际化资源文件；                     
如果有多个资源文件，那么要用英文的逗号分隔开；          

- struts.ui.theme :          
用于指定视图标签默认的视图主题；                         

- struts.ui.templateDir  ：           
用于指定视图主题模版文件所在的位置；                
默认为template；即，默认加载template路径下的模版文件；                      

- struts.ui.templateSuffix :        
用于指定模版文件的后缀；                 
默认是ftl，即支持 freemarker的模版文件；                 

- struts.configuration.xml.reload :             
那么，当`struts.xml`文件改变以后，系统是否重新加载该文件；                      
那么，该属性值默认为false；            

- struts.velocity.configfile :          
用于指定velocity框架所需要的`velocity.properties`配置文件的位置；               

- struts.freemarker.templatesCache :           
用于是否启用freemarker模版的缓存功能；         

- struts.ognl.allowStaticMethodAccess :           
用于是否允许在ognl表达式当中调用静态方法；                 


##### struts-default.xml
`Struts-default.xml`是struts2默认的配置文件，其中定义了大量的拦截器，bean对象以及结果类型；  struts2每次都会自动加载这个配置文件；                
struts2很多核心的功能都是通过这些内置的拦截器、bean对象以及结果类型来实现的；          
如：从请求中把请求参数封装到action当中，文件上传，数据校验等等；                    
开发者只有继承了这个 `struts-default` 包 才可以实现上述的功能；                  
那么，在struts2的核心包当中，struts2会自动加载这个配置文件。              


##### struts.xml
struts2框架的核心配置文件是 `struts.xml`文件；                          
该文件负责加载struts2框架的拦截器、业务逻辑控制器、异常、结果类型等等；                   
struts2会自动加载位于`WEB-INFO/classes`下面的`struts.xml`文件；               
但随着需求的增长以及不断的变化，在一个文件中进行这样的配置，会使文件变得很庞大、臃肿，这样很不容易进行维护；              
所以struts2允许将一个`struts.xml`进行分解，并将分解后的多个配置文件，包含在`struts.xml`文件当中；                               
例如，我们有一个`struts1.xml`文件以及`struts2.xml`文件；最后，我们可以通过include标签将这两个文件分别包含到`struts.xml`文件当中；                  
这种方式，有利于增加配置文件的可读性和模块化，为程序的日后维护带来了很大的方便；                  

那么，`struts.xml`配置文件当中都包含哪些所要配置的元素呢？            
```
1、Bean的配置；
2、常量的配置；
3、包的定义；
4、命名空间配置；
5、拦截器配置；
```

- 1、Bean的配置：             
struts2是可以扩展的框架，框架中大部分为组件；         
struts2并不是完全硬编码的框架；              
其采用了类似于spring技术的ioc，即控制反转的技术来管理框架中的组件；                 
struts2以配置的方式管理组件；从而允许开发者在很大程度上扩展其现有的组件，而形成自己的一套组件；                               
细心的读者可能会发现，在`struts-default.xml`文件当中，包含了大量的bean定义；实际上这个就是该框架ioc的实现；也就是说，将组件部署在struts2的ioc容器当中；              

Bean元素的配置                    
Bean元素有如下属性：            
Bean元素的组成：              
```
Class ： 指定bean实例的实现类；
Type ： 指明bean实现类所实现的接口；
Name ： 指明bean的名称；
Scope ： 指定bean的作用域；
取值为 default、singleton、request、session或者是thread；            
Static ： 指定该bean是否使用静态方法注入；
Optional ： 指明该bean是否是一个可选的bean；
```

在struts当中定义bean，通常用于两种情况：             

第一种情况：               
创建bean的实例，并将该实例作为struts2的核心组件；             
定义bean的时候，需要指定该实例类实现的接口，以及实现类的路径；        

第二种情况：                    
Bean包含的静态方法需要注入一个值；             
对于绝大多数的java应用当中，我们一般都不需要重新定义struts2框架当中的bean对象；                          


#### 常量配置                     
三种方式：                    

在`struts.properties`中配置，如：                    
```
struts.devMode = true;
```
在`struts.xml`中配置，如：
```
<constant name="struts.devMode" value="true" />
```

在`web.xml`中配置，如：
```
<param-name>struts.devMode</param-name><param-value>true</param-value>
```

在`struts.xml`文件中定义的常量，等同于`struts.properties`里面所配置的struts2框架属性；       

在`struts.xml`文件当中通常通过constant元素配置框架属性，其中：
```
name ：指定了属性名称；  
value ： 指定了属性值；
```

对于`struts.properties`而言，其属性配置用`key-value`的形式进行表达；             
值得一提的是，还有另一种可以配置系统属性的方法，就是在`web.xml`文件当中，我们所定义的filter过滤器，我们可以加一个初始化的参数 `<param-name>`；通过这个name来指定相应的属性以及通过` <param-value>`指向它相应的属性值；             
那么，三个方法都可以实现常量的配置。                        
但是，这三种方式中，还是推荐采用struts.xml当中进行配置比较好。


#### 包定义
```
name : 包名称
extends : 扩展其他的包
abstract : 抽象包，不能包括action的定义；可选；
namespace : 包的命名空间；
```
示例：       
```
<package name="包名" extends="另一个包名" namespace="命名空间"                                                  abstract="true/false" />
```

Struts框架当中核心组件是业务逻辑控制器、拦截器等等，那么如何有效的管理这些组件呢？           
Struts2引入了 包 的概念；                  
每个包当中可以定义大量的业务逻辑控制器、拦截器等等组件；             
配置包的时候需要指明 name 属性 ： 即 包的名称；            
同时还要指定 extends ： 即 包可以扩展到另外一个包；                      
extends表示该包可以继承其他的包，同时该包就继承了其他包当中的资源；                   
在包的属性当中还有一个abstract属性；                
abstract包 ： 即抽象包，抽象包当中不能包含action的定义；                  
该属性是可选的；                          
namespace ： 用于定义包的命名空间；                                 
Struts2框架有一个默认的包，即 `struts-default`；                              
其中，`struts.xml`文件当中会继承该包；                  


#### namespace配置 (命名空间配置)        

在package包中指明namespace属性，设置包名；            

考虑到在同一个web应用当中可能出现相同名称的action，也就是业务逻辑控制器；                
那么，struts2以命名空间的形式管理业务逻辑控制器；                 
通常在package包当中指明namespace属性；该属性定义包的命名空间；命名空间将会影响到访问的地址的url；如果一个包没有指定namespace属性，那么struts2将采用默认的命名空间，即为空；                    

##### 命名空间的好处：

首先，从两点进行分析：      

- 第一点：          
同一个命名空间里不能有同名的action，也就是业务逻辑控制器；          
不同的命名空间里面可以有相同的业务逻辑控制器；          

- 第二点：            
有利于模块化；                 


#### Struts2拦截器
拦截器配置：             
用途：              

权限控制           
日志   

好处：                        
减少代码入侵到业务逻辑中；         
提高程序的可读性和可维护性；                    

##### Interceptor定义：
```
name : 拦截器的名称
class : 拦截器实现类
```
##### Interceptor引用：                 
用 `interceptor-ref` 来引用拦截器；

那么，拦截器其实可以理解为面向切面编程的编程思想；             
拦截器允许在业务逻辑控制器之前，以及之后做一些如日志、权限等的自定义开发；可以用拦截器来实现是否有权限和日志这样的操作；               
这种方式也解决了过多的非业务逻辑代码侵入到逻辑代码当中；从而增强了程序的可读性和可维护性；                    
业务逻辑控制器用Interceptor来表示；name用来指明拦截器的名称，class用来指明拦截器的类路径；                
同时用 `interceptor-ref `来引用相应的拦截器。                                         
Struts2当中允许有多个拦截器组合在一起；形成一个栈；我们称之为拦截器栈；              
`interceptor-stack` 就是用来定义这个拦截器栈，它是拦截器的集合。              


#### action配置：
处理业务逻辑的控制器类，它是struts2最核心的部分；              
配置：        
```
name : 控制器名称；
class : 控制器类的路径，默认为 ActionSupport；
```
##### Action：         

用来定义处理业务逻辑的控制器类；它是struts2最核心的部分；               
开发者需要在 `struts.xml` 配置文件当中进行相应的配置；并且根据具体的需求实现这个业务逻辑控制器；         
相对于struts1而言，struts2采用了低侵入性的设计方式；             
struts2不要求业务逻辑控制器继承特定的类；所以可以将这个业务逻辑控制器理解为一个普通的Java类；但要求这个业务逻辑控制器必须实现相应的execute方法；该方法要返回一个字符串类型的变量；             
这个，对于一个基本业务逻辑控制器就完成了；那么用它可以开发大部分的基于struts2的应用程序；                     
在配置文件`struts.xml`文件当中，要对相应的业务逻辑控制器进行配置，action标签当中需要配置两个属性，                
一是name : 用来指明控制器名称；                
二是class : 用来指明控制器的类的路径；如果没有指定 class，那么系统则使用ActionSupport作为业务逻辑控制器的处理类；             



#### result配置
```
name : 指定result的结果名称
type : 指定result的类型；
```
result代表输出结果；          
当一个业务逻辑控制器处理完毕以后，将返回一个字符串类型的结果。          
那么，这个结果将和result配置当中的name是相同的；               
result元素有两个可选的属性：                        
第一个属性name ： 用于指定结果的名称；               
第二个属性type ： 用于指定结果的类型；                
那么，不同的结果类型将代表不同的输出结果；            

struts2的结果类型包括：            
```
chain ： action链式处理
dispatcher ： web资源的集成，例如 jsp集成
freemarker ： freemarker集成；
redirect ： 重定向到另外一个url；
redirectAction ： 重定向到另外一个action资源；
stream ： 返回一个inputStream，通常用于文件下载；
velocity ： velocity集成
plainText ： 显示页面源代码
```

其中，dispatcher是struts2默认的结果类型；主要用于与jsp页面进行整合；


### 异常配置
```
exception : 异常类型
Result    : 当异常出现时，根据result返回结果；
```
#### 异常分类：
```
全局异常类型  : <global-exception-mapping>
局部异常类型  : <exception-mapping>
```
Struts2异常通过在`struts.xml`文件当中，配置` exception-mapping`这个节点来实现；配置需要指定两个属性：             
第一个属性是 exception ： 用于指明异常的类型；               
第二个属性是 result ： 用于指明当异常出现后，系统转向result属性所指向的结果；                
根据异常的位置又可分为 全局异常 以及 局部异常；                  
全局异常用 `global-excpetion-mapping` 这个节点来定义；与action的配置，它们是同级的；                     
那么，局部异常定义在action元素里面；               
全局异常对所有的action都有效，局部异常只对action范围内有效；           
如果全局异常和局部异常配置相同，那么在action内，局部异常将会覆盖掉全局异常。                      


#### 使用Annotation配置Action
- Annotation ：              
即通过注解来配置struts2的业务逻辑控制器；              
那么，通过这种方式，就不用将业务逻辑控制器的配置信息写入到之前我们所介绍的`struts.xml`文件当中；             
尽管`struts2.0`的版本对注解功能并不是很强大；官方提到`struts2.2`版本以后，注解的功能会很强大；完全可以抛离`struts.xml`文件来写程序；             
这样的话，每个人都可以配置好自己的程序，而不用集中在`struts.xml`文件当中进行配置了；                   
与业务逻辑相关注解包括：          

`ParentPackage、Namespace、Result`以及`Results`，这些注解都是类级别的；也就是说，需要写在类的最前面；           
ParentPackage的配置，可以省略掉；因为ParentPackage默认就是继承               
`struts-default`这个包；             
Namespace设置是位于默认的命名空间；所以可以省略，除非开发者自定义其命名空间；           
Result注解，默认是success；            
那么，在这里为了便于分析，指明了具体的name的属性值；                   

作用：               
通过注解来配置 struts2 中的 action            
分类：             
```
ParentPackage : @ParentPackage(value="struts-default")
Namespace : @Namespace(value=" ")
Result : @Result(name="success",value="success.jsp")
Results
```

当配置好上述的注解以后，还需要将配置信息配置在`web.xml`文件当中；         
在`web.xml`，代码片段如下：          
```xml
<init-param>
   <param-name>actionPackages</param-name>
   <param-value>使用注解的包路径</param-value>
</init-param>
```

那么，这里，我们指明了业务逻辑控制器所在的包；              
那么这样的话，struts2的框架将会加载这个包；                   

在action类中一般定义如下：            

```
@ParentPackage(value="struts-default")
@Namespace(value="")
@Result(value="success.jsp")
#Result(name="login",value="login.jsp")
```
ParentPackage，它有个`value；value`指向了 `struts-default`；            
也就是说，以默认的struts2的配置文件作为配置；                
那么，Namespace，它的值value等于空；也就是说，命名空间也采用这种默认的形式；                  
`Result ：value`等于`success.jsp`               
那么，在这里注意一下，我们没有指定 name 的属性；               
那么这样的话，struts2将会以默认的name名称作为这个结果类型的名称；                        
也就是说，success；                  
那么，在这里我们需要注意一下：                        
对于业务逻辑控制器的名称，我们要以action作为结尾；             
