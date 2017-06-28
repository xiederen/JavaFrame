# Spring概述
Spring是一种开源轻量级框架；     
Spring是为解决企业应用程序开发复杂性而创建的；          
Spring致力于`Java EE`应用的各层解决方案，而不仅仅专注于某一层的方案；           
可以说，Spring是企业应用开发的`一站式`选择；               
Spring贯穿于表现层、业务层、持久层；        
然而，Spring并不想取代那些已有的框架，而以高度的开放性与这些框架无缝整合；      
Spring是一个轻量级框架，它大大简化了Java企业级开发，提供了强大、稳定的功能，又没有带来额外的负担；        
### Spring有两个主要目标：
一是让现有技术更易于使用；   
二是促进良好的编程习惯；      
Spring是一个全面的解决方案；    
### Spring坚持一个原则：  
不重新造轮子；    
已经有较好解决方案的领域，Spring绝不做重复性的实现；    
比如，对象持久化和OR映射，Spring只是对现有`JDBC、Hibernate`等技术提供支持；                
使之更容易使用，而不是重新做一个实现；         
Spring框架包含很多特性，这些特性由七个定义良好的模块组成；          


## Spring框架体系结构
在Spring的各种版本中，Spring的功能和特点大致上被组织成了七个部分；               
- 第一个部分：              
`Spring Core` ：         
Spring框架的核心，提供IOC和依赖注入特性；          
- 第二个部分：       
`Spring Context` ：     
它提供了一种框架风格的方式来访问对象；有些像JNDI注册表。       
它继承了Beans包的功能；同时增加了国际化、事件传播、资源装载，以及透明创建上下文；        
- 第三个部分：
`Spring AOP` :     
通过配置管理，`Spring AOP`直接将面向方面编程功能集成到了Spring框架中；        
- 第四个部分：      
`Spring DAO` :       
提供了JDBC的抽象层；      
它可以消除冗长的JDBC编码和解析数据库厂商特有的错误代码；          
- 第五个部分：       
`Spring ORM` ：        
Spring框架插入了若干个ORM框架，从而提供了ORM对象关系工具；         
其中包括`JDO、Hibernate和iBatis`等；           
所有这些都遵从Spring的通用事务和DAO异常层次结构；         
- 第六个部分：
`Spring Web`模块：       
Spring的Web上下文模块建立在应用程序上下文模块之上；       
为基于Web的应用程序提供了上下文；        
所以，它支持与`Jakarta Struts`的集成；        
- 第七个部分：    
`Spring MVC`框架：       
MVC框架是一个全功能的构建Web应用程序的MVC实现；        

当然，这七个部分也不是绝对的，这是最早的体系结构的划分方法；        
也就是在Spring框架的1.2版本的时候提出的；        
在Spring框架的2.0版本，这七个部分被优化成了六个部分；            


## Spring框架的各个部分的功能：
`Spring Core`         
即，Spring的核心；     
它是框架的最基础部分，提供IOC和依赖注入特性；     
管理bean与bean之间的依赖关系        

`Spring Context`      
即，Spring上下文容器；    
它是BeanFactory功能加强的一个子接口；        

`Spring Web`     
它提供Web应用开发的支持；        
也就是说，`Spring Web`这个部分主要提供Web应用开发的支持；           

`Spring MVC`       
它是针对Web应用中MVC思想的实现；      

`Spring DAO`   
它提供了JDBC的抽象层；        
简化了JDBC编码，同时使编码更具健壮性；     
支持包括一致的异常处理和编程方式；         

`Spring ORM`     
它支持用于与流行的ORM框架的整合；     
比如：        
Spring与Hibernate的整合、Spring与JDO的整合、Spring与iBATIS的整合等等；           

`Spring AOP`        
AOP 即，面向切面编程          
它提供与AOP联盟兼容的编程实现；          

实际上，这些模块之间并不是全部紧密联系的，大部分是可以独立使用的；        
也就是说，我们可以单独使用Spring框架中的一个模块或几个模块；          


### Spring Core 和 Spring Context

Spring核心以BeanFactory为基础，管理bean之间的依赖关系，它的核心机制就是依赖注入，以此达到bean对bean实现类的依赖解耦，变成对接口的依赖。        
程序从面向具体类的编程，转向面向接口编程。            
而，Spring Context是BeanFactory的加强，它提供了在J2EE应用中的大量增强版本，比如随Web应用启动的自动创建、程序国际化等；             

Spring以bean的方式组织、管理Java应用中各组件以及组件之间的依赖关系；松耦合运行良好；        
这一切都是因为Spring的核心机制；这个核心机制就是依赖注入；        
Spring使用BeanFactory作为应用中负责生产和管理各组件的工厂；         
同时，BeanFactory也是组件运行的容器；          
而，BeanFactory是根据配置文件，来确定容器中bean的实现；以及管理它们之间的依赖关系的；          
那么，bean之间的依赖关系由BeanFactory管理，bean的具体实例化过程也由BeanFactory来管理；            
这里，我们通过依赖注入，就可以实现bean对bean实现类的依赖解耦，变成对接口的依赖；               
程序从面向具体类的编程变为面向接口的编程；           
这样，就可以极大的降低应用中组件之间的耦合；        
而，`Spring Context`是BeanFactory的加强；       
也就是说，它是BeanFactory的一个加强版的子接口；          
它提供了在J2EE应用中的大量增强功能；            
比如，随Web应用启动的自动创建、程序国际化等；         


### Spring Web和Spring MVC

Spring的Web框架围绕分发器(DispatcherServlet)设计，DispatcherServlet将请求分发到不同的处理器；Spring的MVC框架提供了清晰的角色划分：控制器、验证器、命令对象、表单对象、模型对象、分发器、处理器、映射和视图解析器。          
Spring支持多种表现层技术 : Velocity、XSLT等等，甚至可以直接输出PDF电子文档或Excel文档；          

对于Spring体系结构中的Web和MVC这两个模块主要是提供Web应用程序方面的支持；           
#### 它们之间也是有区别的；
比如：     
Spring的Web框架围绕分发器DispatcherServlet设计；               
DispatcherServlet将请求分发到不同的处理器；       
这个就是它的设计原理；           
而至于Spring的MVC框架；         
我们以前学过MVC；        
MVC也就是指，模型，视图，控制器；        
Spring的MVC框架提供了清晰的角色划分；      

##### 这些角色包括：            
控制器、验证器、命令对象、表单对象、模型对象、分发器、处理器、映射和视图解析器等等；     
Spring支持多种表现层技术 ： Velocity、XSLT等等；         
甚至可以直接输出也就是返回PDF电子文档或Excel文档；          
这就是Spring的强大之处；           


## Spring 面向切面编程

面向切面编程(AOP)完善了Spring的依赖注入，而它的完善作用主要表现在：        
- 面向切面编程提供声明式事务管理；   
- Spring支持用户自定义切面    

Spring还可以与AspectJ整合；整合后允许通过依赖注入配置AspectJ的切面；      


### Spring AOP
AOP，也就是所谓面向切面的编程；        
实际上，AOP是起源于OOP；          
面向对象编程过程中实际上是有一定局限性的；比如，事务处理；            
那么，AOP就是对OOP思想的补充和完善；它解决了OOP无力解决的问题。         
但是，AOP和OOP之间不能互相替代。             
也就是说，虽然AOP是在OOP上发展而来，但是AOP并不能完全替代OOP；        
它们之间只能是互为补充、共利共赢。        
`切面`，可以想象成一根长木棒切断后形成的`截面`，就可以理解为`切面`；         
在OOP面向对象的操作过程中，程序代码是上下衔接的；         
比如，某行对象操作出现了异常，对于OOP接下来就是异常处理；           
那么对于过程，可以说抽象的可以理解为程序是上下结构的；            
而，`AOP`则面向`截面`；                  
比如，当程序在某个点出现异常，接下来程序中不必写异常处理代码；而是，直接执行代码后面的程序；我们可以把异常看做是透明的；那么，异常到底怎么办呢？                 
当程序抛出异常时，AOP会将程序拦住，执行拦截器中的异常处理代码；这段代码可以专门用作异常处理；简单的说：                  
AOP就是可以拦截程序流程，执行`截面`里面的代码；               

对于Spring框架中的面向切面编程模块，完善了Spring的依赖注入；     
它的完善作用主要变现在两方面：              
- 面向切面编程提供声明式事务管理；   
- Spring支持用户自定义切面；    

实际上，Spring还可以与AspectJ整合；整合后允许通过依赖注入配置AspectJ的切面；       


### Spring框架的持久化支持

Spring对各种持久化技术提供了一致的编程方式；    
不管最直接的JDBC，还是各种流行的ORM框架；        
比如，`Hibernate、iBatis、JDO`等；                    
Spring都提供一致的异常继承体系。        
它使用模板封装持久化访问的通用步骤；来自底层数据库的异常都是难以恢复的；             
因此，Spring将数据库访问的checked异常转换为运行时异常，避免繁琐的 `try....catch`块。          

Spring并没有实现自己的持久化方案；               
它集合了现有的流行持久化框架；        
比如：Hibernate、iBatis、JDO等框架；         
另外，对于喜欢使用JDBC完成数据持久化的程序员，Spring采用JDBC模板封装了JDBC的数据库操作；          
那么，程序员可以以更加方便的方式使用JDBC；            
Spring对各种持久化技术提供了一致的编程方式；                
不管直接使用JDBC，还是使用各种流行的ORM持久化框架，Spring都提供了一致的异常继承体系；                 
它使用模板封装持久化访问的通用步骤；         
来自底层数据库的异常都是难以恢复的；         
因此，Spring将数据库访问的checked异常转换为运行时异常。          
这样，在编译时就不会出现编译异常；            
避免繁琐的 `try....catch`块；           
这是Spring框架提供的便捷点；                        



### Spring总体架构

在前面介绍了Spring的体系结构，分析了Spring体系结构的七个部分以及它们之间的关系；     
实际上，Spring总共有十几个组件；          
这十几个组件分别隶属于Spring体系结构中的不同模块。                     
根据前面学过的体系结构，可以分析出，这十几个组件中有几个组件是它的真正核心组件。                  
如下：   
```
 Spring框架
      Spring的特性组件
            Web    JDBC       ......
      Spring的核心组件
            Context    Beans     Core
```
这几个核心组件就是： `Context、Beans`和`Core`；
这三个核心组件就构建起了整个Spring的骨骼架构；     
如果没有这几个核心组件：Context、Beans和Core，就不可能有AOP、Web等上层的特性功能；          


### Spring设计理念
Spring的三个核心组件，构建起了整个Spring的骨骼架构。   
如果再在这三个核心组件中选出更核心的组件的话，那就非Beans组件莫属了；     
实际上，Spring就是面向Bean的编程；    
Bean在Spring中才是真正的主角。           
Spring容器使用DI，也就是依赖注入，管理所有组成应用系统的组件。这些组件也就是Beans。                

Beans组件又是三个核心组件中的核心。                 
Bean在Spring中的作用就像Object对OOP的意义一样；              
没有对象的概念就没有面向对象编程；               
Spring中没有Bean也就没有Spring存在的意义。                

为什么Beans在Spring中如此重要呢？                      
这其实是Spring框架的设计目标决定的；             

Spring解决了一个非常关键的问题。                      
那就是，让我们把对象之间的依赖关系用配置文件来管理。                
也就是，我们在对象与对象之间的依赖的时候，我们不再通过对象去创建对象了。               
它们之间的关系，改用配置文件来管理了。                                
这也就是它的依赖注入机制；                        
而，这个注入关系在一个叫IOC的容器中管理；                        
在这个容器中，Spring通过把对象包装在Bean中而达到对这些对象的管理；                           


核心组件如何协同工作                  
前面说，Bean是Spring中的关键因素。                      
那么，同为三大核心组件的Context和Core又有什么作用呢？                 
我们可以把Bean比作一场演出中的演员。Context可以理解为这场演出的舞台背景。Core就是演出的道具；`Beans  Context` 和` Core`就是这样的一种关系；                        
只有它们在一起才能完成一场优秀的演出；这三者都是完成演出的基础条件；                       
当然，仅仅只有这些基本条件还不能把演出做到最好；                 
要想在此基础上更添香添色，我们还需要Spring提供特色组件实现的功能；                 

我们知道Bean包装的是Object；而，Object必然有数据；                     
如何给这些数据提供生存环境，就是Context要解决的问题；                          
对于Context来说，它的作用就是发现每个Bean之间的关系，为它们之间建立好这种关系并进行维护。   所以，可以把Context看做一个Bean的关系集合。                              
这个关系集合，就称为 `IOC容器`。                
那么，一旦，建立了这个IOC容器后，Spring就可以为我们工作了。                            
这就是Bean和Context之间的协同工作。             
####  Core组件的作用：
Core就是发现、建立和维护每个Bean之间关系所需要的一系列工具。      
因此，可以把Core组件想象成util工具包。                         
Core组件就是Spring框架提供的工具组件。                                                                                                                                                                                                                                                         ### Spring入门

`Spring初探               开发第一个Spring项目`
项目需求：    
编写HelloSpring类，要求如下：        
此类要求输出  `"Hello,Spring!"  `     
其中的字符串 `"Spring" `是通过Spring框架 `注入`到HelloSpring类中；             
实现步骤：     

- 首先，创建一个工程。下载Spring并添加到工程项目中；            
- 第二步：编写Spring的配置文件。          
- 第三步：编写代码获取HelloSpring实例，并且同时把 `"Spring"`字符串注入到实例中。                                                                                                                                                                                                                                                                                                         

#### 下载Spring       
目前，Spring的官方[下载地址](http://www.springsource.org)
在官方网站上提供了各种版本的下载地址；                   
下载的压缩包不仅包含Spring的开发包，而且包含Spring编译和运行所依赖的第三方类库。

 解压后的目录结构中，其中：     
- dist文件夹：                
该文件夹下存放Spring的jar包。               
通常，我们只需要`spring.jar`文件。                                                                    
- docs文件夹：    
该文件夹下包含Spring的相关文档，包括开发指南、API参考文档等等。    
需要帮助时可以在这里查阅相关文档或资料。          
- lib文件夹：   
该文件夹下包含Spring编译和运行所依赖的第三方类库。             
该路径下的类库并不是Spring必需的。                                                            
- samples文件夹：    
该文件夹下包含Spring的几个简单示例。       
这些示例可以作为Spring的入门学习案例。                                                                   
- src文件夹：    
该文件夹下包含Spring的全部源代码。                 
如果开发过程中有地方无法把握，可以参考此处的源文件，了解底层的实现。                           
- test文件夹：
该文件夹下包含Spring的测试用例，它们可以作为学习Spring的入门代码。                                                                                                                             

Spring在项目中的部署            
下载好Spring之后，下面在项目中部署Spring框架：          


创建项目并添加Spring所需的jar文件。               

首先，创建一个项目，命名为HelloSpring。              
接着，添加`spring.jar`到项目中。     

为了方便观察Bean实例化过程，Spring采用log4j作为日志输出。           
所以，还需要将` log4j-1.2.15.jar`添加到项目中。         

考虑到以后可能使用 `jakarta-commons`的各种jar文件，因此，找到Spring解压缩后lib文件夹下面的
`jakarta-commons`，将其下的jar文件全部添加到HelloSpring项目中。

```
window ->Preferences -> Java -> Build Path 
```
中的 `Output folder name` 默认为 bin；       
现在改为  `WebRoot\WEB-INF\classes` ，创建一个Web工程。         


创建`log4j.properties`文件        
在前面介绍，Spring采用log4j作为日志输出形式，同时也已经将log4j支持的jar文件添加到项目中了。        
接下来，为该项目添加 `log4j.properties`文件。           
这是log4j的配置文件，用来控制日志输出。      

#### log4j.properties的内容如下：       

- rootLogger是所有日志的根日志，修改该日志属性将对所有日志起作用    
- 下面的属性配置中，所有日志的属性级别是info，输出源是console      
```
log4j.rootLogger=info,console
```

- 定义输出源的输出位置是控制台   
```
log4j.appender.console = org.apache.log4j.ConsoleAppender
```
- 定义输出日志的布局采用的类     
```
log4j.appender.console.layout=org.apache.log4j.PatternLayout
```
- 定义日志输出布局
```
log4j.appender.console.layout.ConversionPattern=%d %p [%c] - %m%n
```
log4j是Apache的一个开放源代码的项目           
通过使用log4j，我们可以控制日志信息输出的目的地           
比如控制台、文件、GUI组件等等            
我们还可以控制每一条日志的输出格式，每一条日志的信息级别等等               
这样，我们就可以更加细腻的控制日志的生成过程。                   

#### log4j的使用步骤：
因为Spring默认就是使用log4j作为日志输出的。                  
- 第一步就是要导入log4j的jar包。                        
- 第二步就是为项目添加 `log4j.properties`文件，并且在此文件中进行配置。                
对于log4j来说，主要有三个组件，分别是 `Logger、Appender`和`Layout`。    

log4j允许开发人员定义多个Logger，每个Logger有自己的名字，Logger之间通过名字来表明隶属关系。        
Appender则是用来指明将所有的日志信息存放在什么地方。            
log4j中支持多种的Appender，比如 console、files等等。                    
```
log4j.appender.console = org.apache.log4j.ConsoleAppender
```
这句代码就表示我们定义的输出源的输出位置是控制台。           
在运行的时候，日志就会在控制台进行输出。                  
Layout组件的作用是控制日志信息输出方式，也就是格式化输出信息。                      
这里的配置文件中的最后一行就是对日志信息的格式化配置。                   
- `%d`  表示输出日志时间点的日期或时间       
比如：我们想要显示 年、月、日，而不显示具体的小时、分钟、秒，我们就可以在 `%d`后面紧接着加一对大括号，同时，里面就是格式化的字符串，比如：` yyyy mm dd`         
这样，就只输出年、月、日，而不输出小时、分钟、秒了。         
即：  `%d [yyyy mm dd]`        
而，上面的配置文件中， `%d`后面没有大括号及格式化字符串，表示此时采用默认的日期加时间格式。           

`%p` 表示输出的优先级， `%c` 表示输出所属的类目，这里就是输出类的全名。      



##### 编写程序的主体代码：

当我们完成Spring的导入及log4j的配置之后，下面我们来编写程序的主体代码。
首先，创建包：`com.pb`；     
然后，在这个包下面创建HelloSpring类。     
```java
 public class HelloSpring {
   //定义who属性
   private String who = null;
   public void print() {
      System.out.println("Hello,"+this.getWho()+"!");
   }
   //获得who  return the who
    public String getWho() {
        return who;
   }
   //设置who
  public void setWho(String who) {
       this.who = who;
   }
   public static void main(String[] args) {
   ApplicationContext context = new ClassPathXmlApplicationContxt                                                         ("applicationContext.xml");
   //通过HelloSpring bean的id来获取bean的实例
   HelloSpring helloSpring = (HelloSpring)context.getBean("helloSpring");
   //执行print()方法
    helloSpring.print();
  }
}
```
因为我们需要打印 `"Hello,Spring!"`；其中，Spring字符串是通过Spring框架注入的，因此，我们首先定义一个属性：who;这个属性就是用来给Spring框架提供注入的；            
既然需要Spring框架提供注入，那么我们需要对此属性添加 get和set方法。                 
我们的需求是最终要在控制台打印一个字符串，因此，我们还需要定义一个打印方法`print()`；    
在这个方法中，我们进行字符串输出；             
注意，` "Hello"`这个字符串在这里是写死的。         
而，`"Hello"`后面的内容，是使用 `getWho()`，从who属性中取出Spring的注入信息，并和前面的`"Hello"`一起拼接，最后形成一个完整的字符串进行输出。             
最后，我们还需要一个main函数来启动我们的程序；               
在这个main函数中，我们首先要创建一个`ApplicationContext`对象；               
`ApplicationContext`是一个接口，负责读取Spring配置文件，管理对象的加载、生成，维护Bean对象与Bean对象之间的依赖关系，负责Bean的生命周期等等。               
而，`ClassPathXmlApplicationContxt`是`ApplicationContext`接口的具体的实现类。     
该类用于从classpath设置路径中读取Spring配置文件。         


##### 编写Spring配置文件        
最后一步，我们来编写Spring的 配置文件。      
也就是，在前面主体代码中所需要读取的`applicationContext.xml`文件。      
如下代码：    
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE beans PUBLIC "-//SPRING//DTD BEAN 2.0//EN"
"http://www.springframework.org/dtd/spring-beans-2.0.dtd">
<beans>
  <bean id="helloSpring" class="com.pb.HelloSpring">
        <property name="who">
                 <value>Spring</value>
        </property>
  </bean>
</beans>
```
在Spring的配置文件中，使用<bean>来创建Bean实例；     
这个元素有两个常用属性；    
一个是id，表明定义的Bean实例的名称。       
另一个是class，表明定义的Bean的类型。         

通过这个配置文件，结合前面的主体代码，不难发现，Spring会自动接管每个Bean定义里面的property元素。                 
Spring创建默认的Bean实例后，调用对应的setter方法为程序注入属性值。                  
property元素定义的属性值将不再由该Bean主动创建、管理，而是改为被动接收Spring的注入。              



##  Spring核心概念
前面，已经介绍了Spring的体系结构、核心组件、核心组件之间的关系。               
现在，来看Spring的核心概念；                      

### Spring中设计模式分析              
在Spring框架中使用到了多种设计模式，其中最主要的两种基本设计模式是：              
```
工厂模式
单态模式
```
Spring容器就是实例化和管理全部Bean的工厂。          
工厂模式可以将Java对象的调用者从被调用者的实现逻辑中分离出来。            
调用者只关心被调用者必须满足的某种规则，这里的规则可以看做是接口。              
而不必关心实例的具体实现过程，具体实现由Bean工厂来完成。                       
而对于单态模式，也叫做单例模式，Spring默认将所有的Bean设置成单例模式。                     
即，对于所有相同id的Bean的请求，都将返回同一个共享Bean实例。                     
这样，就可以大大降低Java对象创建和销毁时的系统开销。                              
使用Spring将Bean设置成单例行为，则无需自己完成单例模式；                       


####  单态模式回顾
单态模式限制了类实例的创建。      
采用这种模式设计的类，可以保证仅有一个实例，并提供访问该实例的全局访问点。         

对于J2EE应用的大量组件，都需要保证一个类只有一个实例。                
比如，数据库引擎的访问点只能有一个；                
同时，更多的时候，为了提高性能，应用程序应尽量减少Java对象的创建和销毁时的开销。                
那么，使用单态模式可以避免Java类的频繁实例化。                        
让相同类的全部实例共享同一个内存区。                          

为了防止单态模式的类被多次实例化，应该将类的构造器设置成私有的。                
这样，保证只能通过静态方法获得类的实例。                 
而，该静态方法则需要保证每次返回的实例都是同一个。                   
那么，这就需要将该类的实例设置成类属性。                 
该属性需要被静态方法访问，因此该属性还应设成静态属性。             
如下示例：         
```java
public class SingletonTest {
  //普通属性
  int a;
  //使用静态属性类保存类的一个实例
  private static SingletonTest s;
  //私有构造函数
  private SingletonTest() {
         System.out.println("正在执行构造函数。。。");
  }
  //返回该类的实例的静态方法
  public static SingletonTest getS() {
      if(s==null) {
          s=new SingletonTest();
      }
      return s;
   }
   //a的get和set方法
   public int getA() {
       return a;
   }
   public void setA(int a) {
       this.a = a;
   }
   public static void main(String[] args) {
     SingletonTest t1 = SingletonTest.getS();
     SingletonTest t2 = SingletonTest.getS();
     ts.setA(9);
     System.out.println(t1.getA());
   }
}
```

现在执行这个程序，打印结果是9；                         
可以看出，t1和t2这两个实例完全相同。         
这就证明了单态模式的类的全部实例是一个共享实例。       

在这个测试类中，声明的是一个私有的构造函数，这样就可以避免此类被多次实例化了。            
也就是，我们不能通过new 去多次实例化这个对象；                 
那么，我们必须提供其他返回这个类的对象的方法；                  
因此，在这个类里先使用静态属性类保存了类的一个实例。              
然后，提供一个返回这个类实例的静态方法。                  

注意：                
在返回这个类的实例的静态方法中，在实例化类实例前，要先检查该类的实例是否存在；              
如果不存在，则新建一个实例，并且把这个实例返回；                   
如果存在，则不需要创建新的实例，直接把这个实例返回就可以了。               



#### 工厂模式回顾
工厂模式根据调用数据返回某个类的一个实例。            
此类可能是多个类中的某一个类；             
通常，这些类满足共同的接口或父类；                       
而，调用者不关心工厂里有多少个类，它只关心工厂生产出的实例是否满足某种规范；                    
也就是，实现了某个接口，或者说工厂返回的实例是否可以供自己正常调用。                    
这个模式提供各对象之间清晰的角色划分，降低程序的耦合。                 
在工厂模式中，接口产生的全部实例通常实现相同的接口。                    
接口里定义全部实例共同拥有的方法。                   
但是，这些共同拥有的方法在不同的实现类中的实现方式可能是不相同的。                     
程序调用者并不关心这些具体实现类的具体实现过程，从而降低了系统异构的代价。                

如下实例：                    
```java
 public interface Ink {
    //打印墨盒颜色
    public String getColor();
 }
```
首先，定义一个接口，Ink，在这个接口中有一个打印墨盒颜色的方法；                 
这个接口就要求它的所有实现类必须实现这个方法。              

接下来，来创建Ink接口的两个实现类：               
这两个实现类，一个是ColorInk类，另一个是GreyInk类，如下代码：       
```java
 //ColorInk类实现Ink接口
 public class ColorInk implements Ink {
    public String getColor() {
       return "使用彩色墨盒打印";
    }
 }

  //GreyInk类实现Ink接口
  public class GreyInk implements Ink {
     public String getColor() {
        return "使用灰色墨盒打印";
     }
  }
```

这两个类都实现了Ink接口，并且实现了`getColor()`；      
但是，这两个实现类对`getColor()`的实现是不相同的。           
ColorInk类打印的是` "使用彩色墨盒打印"`      
GreyInk类打印的是` "使用灰色墨盒打印"`          
它们对这个接口的`getColor()`的具体实现是不相同的；                 

那么，这两个实现类将要被工厂管理；            
工厂将要根据调用者的不同请求，返回不同实现 类创建的请求实例；              

这个示例主要是为了表达工厂模式的实现形式，接下来就是重点，如下代码：                      
```java
  //Ink工厂的实现
 public class InkFactory {
   public Ink getInk(String e) {
     //根据参数返回Ink接口的实例
     if(e.equalsIgnoreCase("color")) {
	  return new ColorInk();
      } else {
 	  return new GreyInk();
      }
   }
}
```

这就是Ink工厂的实现代码；         
对于这个Ink工厂的实现，它提供了一个`getInk()`；         
此方法需要一个字符串输入参数并返回Ink实例。      
`getInk()`的具体实现会根据传入不同的参数返回不同的Ink接口的实例；          
当参数是color的时候返回的是ColorInk实现类实现的Ink类型实例；          
而，其他的参数则返回的是GreyInk实现类实现的Ink类型的实例；              

最简单的工厂模式的框架，基本上就是以上给出的这四个实例类的样子；                       
我们的程序的主体部分就仅仅需要与工厂类耦合就可以了，而无需与具体的实现类以及接口耦合在一起，如下代码：               
```java
public class FactoryTest {
  public static void main(String[] args) {
    //创建InkFactory的实例，获得工厂实例
    InkFactory f = new InkFactory();
    Ink p = null;
    //使用工厂获得Ink实例
    p = f.getInk("color");
    System.out.println(p.getColor());
    //使用工厂获得另一个Ink实例
    p = f.getInk("grey");
    System.out.println(p.getColor());
  }
}
```

我们的测试程序就完全从Ink接口的具体类中解耦出来了。     
而且，程序调用者无须关心Ink的实例化过程；           
这样，角色的划分非常清晰；               
测试程序仅仅与工厂服务结合在一起，获得工厂的引用；              
根据此引用，我们的测试程序就可以获得所有工厂能产生的实例；                      
这样，具体的Ink接口的实现类发生了变化，但是只要接口不发生任何变化,那么，调用者程序代码部分几乎无需任何改动；       


### Spring对单态与工厂模式的实现：
现在，在以上两个示例的基础上做一下修改，看一下Spring是如何对单态与工厂模式进行实现的；               

#### 首先，Spring对工厂模式的实现：  

- Spring配置文件

在这里，我们无需修改程序的接口和实现类；            
也就是说，我们不用修改Ink接口、ColorInk类以及GreyInk类这两个实现类；                
但是，因为我们要使用Spring提供的工厂模式来实现，          
因此，InkFactory这个工厂类就不需要了；          
这个工厂所提供的服务，我们就交给Spring来实现；                   
Spring通过配置文件就可以管理所有的bean，而这些bean就是Spring工厂能产生的实例；             
因此，首先，我们在Spring的配置文件中对这两个实例进行配置；         
如下代码：                  
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE beans PUBLIC "-//SPRING//DTD BEAN 2.0//EN"
"http://www.springframework.org/dtd/spring-beans-2.0.dtd">
<beans>
  <!-- 定义第一个bean，该bean的id是color -->
  <bean id="color" class="com.pb.ColorInk"></bean>
  <!-- 定义第二个bean，该bean的id是grey-->
  <bean id="grey" class="com.pb.GreyInk"></bean>
</beans>
```

在Spring的配置文件中，我们配置了两个bean；            
第一个bean的id是color，第二个bean的id是grey，分别表示彩色墨盒和灰色墨盒；            


- 主程序部分代码

在完成在Spring配置文件中对两个bean的配置之后，下面来看程序的主体部分代码，如下代码：      
```java
public class SpringTest {
  public static void main(String[] args) {
    //实例化Spring容器
    ApplicationContext context = new ClassPathXmlApplicationContext                                                          ("applicationContext.xml);
    //定义Ink接口的实例
    Ink ink = null; 
    //通过Spring上下文获得color实例
    ink = (Ink)context.getBean("color");
    //执行color实例的方法
    System.out.println(ink.getColor());
    //通过Spring上下文获得grey实例
    ink = (Ink)context.getBean("grey");
    //执行grey实例的方法
    System.out.println(ink.getColor());
  }
}
```

在这里，我们不再使用InkFactory工厂类来创建Ink实例，而是把这部分的工作交给了Spring来处理；    
因此，首先，我们需要实例化一个Spring容器，也就是ApplicationContext；          
然后，我们需要定义一个Ink接口实例；                  
因为，我们的实例在这里不再由InkFactory创建，而是由Spring容器注入；               
我们使用Spring容器的`getBean()`，根据我们传入的参数就可以在配置文件中找到对应id的bean；         
然后，Spring会自动创建这个bean的实例并进行返回；              
我们得到具体的实例之后就可以调用其方法；                    

在这里，我们一样可以打印出和使用InkFactory相同的执行效果；         
这说明使用Spring至少有一个好处，即使没有工厂类InkFactory，程序一样可以使用工厂模式；                 

所有工厂模式的功能，Spring完全可以提供；                           




在我们分析完Spring对工厂模式的实现之后，现在来看Spring对单态模式的实现；                 

#### Spring对单态模式的实现
在这里，我们继续使用前面Spring配置文件中配置的bean，我们不再重新定义新的bean了；              
在原来的基础上进行新的操作；             

主体程序部分代码，如下：            
```java
 public class SpringTest {
   public static void main(String[] args) {
    //实例化Spring容器
    ApplicationContext context = new ClassPathXmlApplicationContext(
				"applicationContext.xml");
   //定义两个Ink接口实例
   Ink ink1 = null;
   Ink ink2 = null;
   //通过Spring上下文获得color实例
   ink1 = (Ink) context.getBean("color");
   ink2 = (Ink) context.getBean("color");
   System.out.println(ink1 == ink2);
  }
}
```

以上为主体程序部分代码，我们对前面的代码进行了简单的修改；       
在这里，我们定义了两个Ink接口实例，然后分别通过Spring上下文获得了两个color实例；            
然后使用 `==` 对这两个实例进行比较；
我们知道，对于对象，当两个对象是同一个对象的时候， `==`的比较才为真，其他为假；              
程序的执行结果是 true；                       
这表明Spring对接收容器管理的全部bean，默认采用的是单态模式管理的；                            
我们来观察一下这些代码，不难发现，我们使用Spring的一些特点：                        

## Spring的特点：
- 第一：    
除了进行测试的主体程序部分代码，其他部分代码并没有出现Spring特定的类和接口，仅仅只有一个配置文件；     	     

- 第二：    
调用者的代码，也就是现在看到的主体部分代码，是面向Ink接口的编程；       
其实，并不需要知道Ink接口的实现类的具体名称，Spring的配置文件自动为我们完成了调用过程；          

- 第三：       
工厂通常无需多个实例，因此工厂应该采用单态模式设计；           
而，Spring的上下文，也就是产生Spring实例的工厂，已经默认被设计成了单态模式；                
Spring实现的工厂模式，不仅提供创建bean的功能，还提供对bean生命周期的管理。                    
最重要的是，还可以管理bean与bean之间的依赖关系，以及bean的属性值；                  
这就是使用Spring的好处；                


#### Spring的核心机制
在前面，我们说，Spring的核心机制就是依赖注入；现在，就来看什么是依赖注入；         

#### 控制反转和依赖注入
当某个角色需要另一个角色的协助时，在传统的程序设计过程中，通常由调用者来创建被调用者的实例。    
但在Spring里，创建被调用者的工作不再由调用者来完成，而是由配置文件，也就是Spring来完成的，因此被称为控制反转。    
创建被调用者实例的工作通常由Spring容器来完成，然后注入调用者，因此也称为依赖注入；           

可以说，依赖注入和控制反转是同一个概念；       

那么，到底什么是依赖注入呢？      
要理解依赖注入，我们首先需要明白什么是依赖；     

依赖                               
两个元素中，一个元素定义发生改变则会引起另一个元素发生变化，则称这两个元素之间存在依赖关系；                            

那么：                  
一个类要发送消息给另一个类；                
一个类将另一个类作为数据的一部分；                  
一个类操作另一个类作为其参数；                     
等等.....                    
这些情况都是一个类依赖另一个类；                        

类与类之间的依赖关系增加了程序开发的复杂程度；                

我们在开发一个类的时候，还要考虑相关联的其他类的属性和行为；                  

最开始的时候，程序只用来处理计算问题，没有复杂的逻辑，面向过程就可以搞定；               
后来，程序的规模越来越大，需要处理的逻辑越来越复杂；                 
类与类之间的依赖关系也越来越复杂，面向对象就有了必要；                           

当我们用程序模拟一个柜子的组装过程时，我们把挡板、框架、钉子等柜子的组件作为不同的对象；                      
当我们组装的时候，只要把这些对象组合在一起就可以形成一个柜子；                    
因此，以这种方式的编程可以把面向对象的优势明显地表现出来；                         


软件需求的增长是没有止境的。                    
当我们实现的系统越来越复杂，面向对象也有苍白的时候；                 
我们 就需要更多有效的理论和方法来解决问题；                                   

系统变得复杂是因为系统各部分关联程度太高；                        
也就是说，各模块间`依赖`程度太高；             
我们如果采用组件化的思想降低系统各组件的依赖关系；实现每个组件的时候，只需要考虑这个组件需要实现的功能，以及各个组件和其他部分的接口就可以了；            

##### 思考一个问题：
电脑是怎么做出来的呢？                     
电脑，有专门生产主板的厂商、有专门生产CPU的厂商、有专门生产硬盘的厂商.....                     
硬盘厂商设计产品的时候，不需要考虑CPU支持什么指令集，不需要考虑主板采用什么芯片组；
他只知道别人会留好接口，他也只会按照行业标准给别人留好接口。                       
电脑的各个组件间当然是有依赖关系的，但由于明确定义了它们之间的接口；                    
在各个组件实现的时候，完全不用考虑这些依赖关系，从而使我们获得了构建更复杂系统的能力；               
组件间的依赖关系和接口的重要性，在将各个组件组装在一起的时候得以体现。                       

通过这种开发模式，我们还获得了一种能力：                     
可以随意地更换接口的实现；                       
注意：是随意更换接口的实现，而不是随意更换接口；                              
例如：                
采用什么显示器，随你便；                         
你可以采用实现了VGA接口的液晶显示器，也可以采用实现了VGA接口的等离子显示器；                            
那么，只要它们实现了VGA接口即可，甚至可以接个投影仪当家庭影院。                                

### 软件设计与此类似；              

Spring提倡面向接口编程；    也是基于这种考虑；   
所谓依赖注入就是，当某个角色，需要另一个角色协助时，   
在传统的程序设计过程中，通常由调用者来创建被调用者的实例；   
但是，在Spring里，创建被调用者实例的工作由Spring容器来做；   
然后，Spring容器会把被调用者实例注入调用者中，依赖注入；    
让Bean与Bean之间的关系以配置文件的形式组织在一起，而不是以编码的方式耦合在一起；     


不管是依赖注入，还是控制反转，都说明Spring采用动态、灵活的方式来管理各种对象；       
对象之间的具体实现互相透明。       
例子：      
吃饭：       
第一种情形：    
自己做饭吃；    
第二种情形：     
下馆子；        
第三种情形：     
电话点餐；    
这个电话点餐 就对应Spring的依赖注入；     

##### 分析：   
- 第一种情况：   
对应自己做饭吃；     
Java实例的调用者创建被调用者的Java实例，必然要求被调用者的Java类出现在调用者的代码里，这种情况无法实现二者之间的松耦合。       

- 第二种情况：   
对应我们去饭馆吃饭；   
调用者无需关心被调用者具体的实现过程；     也就是我们不需要关心菜的具体的制作过程；    
只需要找到符合某种标准，或者可以理解为接口的实例，即可使用。       
此时的代码，可以说是面向接口的，可以让调用者和被调用者解耦；      
这正是工厂模式的使用原因。     
但是调用者却又和特定的工厂耦合在了一起；      

- 第三种情况：
对应电话点餐的情况；   
调用者无需自己定位工厂；  
程序运行到需要被调用者时， 程序自动提供被调用者实例；    
事实上，此时，调用者和被调用者都处于Spring的管理之下；    
二者之间的依赖关系由Spring提供；那么，这就是依赖注入；       



## Spring的核心概念

### 面向方面编程     

面向方面编程和面向对象编程之间的关系            

面向方面编程也就是所谓AOP。                
它并不会取代面向对象编程，也就是OOP；                
面向方面编程是面向对象编程的补充。       
AOP从动态的角度考虑程序结构，从而使OOP更加完善。      
面向对象编程将程序分解成各个层次的对象；                
而，面向方面编程将程序运行过程分解成各个切面；                

Spring体系结构中的AOP模块是Spring的一个关键组件。    
AOP组件是`Spring IOC`依赖注入的完善和补充。使之成为更加有效的中间件。    
当然，IOC容器并不依赖于AOP组件，因此，我们可以认为AOP组件不是必须的；      

`Spring AOP`的目标是提供与`Spring IOC`容器紧密结合的AOP框架；    
而，`Spring AOP`并不致力于提供完善的AOP实现，而是以 `实用主义`为目标，提供AOP框架与IOC容器的完美结合。    
AOP从程序运行角度考虑程序的结构，提取业务处理过程的切面；       
比如：             
我们可以定义一个事物提交的切面；               
程序自上而下执行；                               
当程序执行到事务提交操作的时候，程序的提交判断就可以交给切面监控程序去处理；                              
而，主程序则继续向下执行；                   
切面程序对于主程序来说是透明的；                 
这就是面向切面编程；              

AOP可以面对程序运行中的各个步骤；                  
这样可以很好的降低各个步骤之间的耦合从而提高步骤之间的隔离；              