# 概述
Spring为企业应用程序的开发提供了轻量级的解决方案；              
其中，包括许多的技术应用，比如：依赖注入的核心机制、AOP的面向切面编程技术、Spring的MVC构架以及轻松的与现有的开源项目如hibernate，Struts等框架的整合等等；                         
Spring为表现层，业务逻辑层以及数据的持久层提供了很好的解决方案；            
Spring提供的不只是框架的技术，而是提供了一种企业应用开发的规范；                      
Spring将开发进行了抽象，从而大大简化了应用程序的开发；                           
Spring支持对java普通对象进行的管理，尤其是对javabean的管理使之更加的灵活。                      


## Bean管理

### BeanFactory介绍             
在学习beanfactory之前，首先来了解一下什么是bean？              

Bean ：                       
Bean在spring技术中是基于组件的；                 
它是spring容器所管理的最基本也是最常用的单元。                     
在spring的应用场合中，bean可以是数据源、java的普通类；                      
如果要与开源的框架结合，如：hibernate、struts等，bean可以是hibernate框架的`sessionFactory`、事务组件等等。         
bean作为组件，其 实例保存在spring的容器当中；这种方式是spring的核心思想所在；                   
所以，Bean通常被定义在配置文件当中，bean的实例化由spring的IoC容器进行管理；                                    
bean的实例可以通过beanfactory进行访问；                            
实际上，大部分的j2ee应用，bean是通过`applicationContext`来访问的；                     
`applicationContext`是beanFactory的子接口，其功能要比beanFactory强大的很多。               


BeanFactory介绍                         
Spring容器有一个被称为beanFactory的接口；               
有时，它被称为spring的上下文；它是产生bean的工厂，是Spring依赖注入的核心；                
### BeanFactory作用：
配置、创建以及管理bean对象；            
维持bean对象之间的依赖关系；                   
负责bean对象的生命周期；                         

应用程序需要通过beanFactory接口才可以获得bean的实例；                               

##### BeanFactory通常有如下方法：

- containsBean(String beanname)          
该方法用于判断在spring的容器当中是否有一个id为beanname的bean对象；      

- getBean(String beanName)          
返回spring容器中id为beanname的bean对象；           

BeanFactory有很多的实现类，但通常我们使用XmlBeanFactory类；           


##### ApplicationContext
对于j2ee应用程序的开发，建议使用applicationContext；               
它是beanfactory的子接口，其实现类通常使用`FileSystemXmlApplicationContext`；
ApplicationContext是基于BeanFactory而建立的；               
ApplicationContext继承了BeanFactory的所有功能，并提供了更多而且丰富的功能，如：         
```
读取Bean ， 定义文件
维护Bean之间的依赖关系；
国际化的支持；
资源访问；
事件传播；
多配置文件的加载；
```
前两点(读取Bean ， 定义文件)和beanfactory所实现的功能类似；     
同时，它还支持国际化；国际化的支持在实际的开发当中是比较常用的；             
对于根据需求支持不同的语言环境的系统而言，通常采用一个独立的资源文件(如一个properties的文件)完成语言信息的配置；             
spring对这种传统的方式进行了改进；即，通过在配置文件中定义相应的bean来实现资源文件的加载；                 
applicationContext同时提供了对资源文件进行访问的支持：           
如，通过getResource方法可以很轻松的获得resource这个对象，从而进行资源的访问；                
它同时还提供事件传播机制；                         
applicationContext提供了对bean的事件传播功能；               
通过application的publishEvent方法，可以实现将事件信息通知系统内所有实现了applicationListener接口的类；
最后一个功能就是多配置文件的加载；                                  


### Bean定义
下面来看一下bean在配置文件中的基本定义：    
配置文件        
```
<beans/>是spring配置文件的根节点；
<bean>节点是<beans/>元素的子节点；
```
一个<beans/>节点里面可以有多个<bean>节点；           
每个<bean>节点对应spring容器当中的bean的实例。                   

#### 在定义bean节点时，通常需要指定两个属性：              
- Id : 用来指明bean的标识符，这个标识符具有唯一性；           
这个属性比较重要；因为，spring容器对bean的管理，以及bean之间的依赖关系都需要这个属性；                 
- class : 该属性用来指明该bean的具体实现类，注意：这里不能是接口；               

在spring容器中所管理的bean，其bean的实例可以通过 BeanFactory的`getBean(String beanid)`方法获得；其中的beanid就是和上面说到的id属性是一致的；               

##### Bean在spring容器中通常有两种行为：             
- singleton :              
当设置bean的行为是singleton的时候，整个spring的容器只有一个共享实例存在；           
每次请求该bean，spring都会返回该bean的共享实例；                 
- non-singleton :          
当设置bean的行为是 `non-singleton` 时，每次请求该bean，spring容器都会新建立一个bean的实例，然后返回给程序；


### 创建Bean
那么，如何创建Bean呢？我们先介绍一下Bean的命名和别名            

### Bean的命名
对于bean的命名，可以按照java的命名规则，即：         
使用清晰的，描述性的，一致的命名规范；           

### Bean的命名机制
当在spring的容器当中，查找某个bean对象时，首先根据id进行查找；            
这种方式，将其作为bean的默认名称；                
如果id属性不存在，则根据name属性进行查找；                        
如果id属性和name属性都不存在，则按照类的名称进行查找；             


##### 下面，定义了3个简单的Bean：                
- 第一个Bean：    
```
<bean id="id1" class="实现类">
```
第一个Bean，指定了id属性值为 "id1"，以及class里面的实现类；            
当我们通过id属性对这个Bean进行查找的时候，可以很轻松的得到该Bean的实例；             

- 第二个Bean：        
```
<bean name="name1" class="实现类">
```
这个Bean当中，定义了一个name属性，以及它相应的class实现类；                 
这时，如果我们还按照id属性进行查找的话，那么这个id属性就会与这个name名称进行匹配；               
如果匹配上了，它就会得到这个Bean的实例；            

- 第三个Bean：
```
<bean class="实现类">
```
最后一个Bean的定义中，我们只定义了一个class实现类的属性；                
我们既没有定义id的属性，也没有定义name的属性；                
那么，Spring会根据这个实现类的名称来对这个Bean进行实例的返回；                      


### Bean的别名：
通过 alias属性指定；     
```
<alias name="fromname" alias="toname"/>
```
在对bean进行定义的时候，除了使用id属性指定名称之外，为了提供多个名称，通常需要通过alias属性来进行指定；        
通常，别名的定义如下：       
```
<alias name="fromname" alias="toname"/>
```
fromname用于指定关联的Bean的名称，toname是该关联类的别名的名称；             

需要注意的是：     
所有的名称实际上都指向了同一个bean；        


#### 如何声明一个bean呢？分为4个步骤：

声明一个简单Bean :            
1.创建一个xml的配置文件                        
2.将相关的spring框架配置信息写入到配置文件当中；                     
3.根据需求生成相应的bean类；                     
4.加入bean的配置到配置文件当中；               




#### 注入属性
注入基本类型和String       
 - 用到value元素                        
 - xml解析器以string类型解析出数据           

注入Bean       
 - ref元素进行标识

Ref元素通常有两个属性
 - bean    
 - local     

对于基本数据类型和string类型的注入，需要用到value元素；它用于确定字符串参数；                
xml解析器以string类型解析出数据，之后通过PropertyEditors完成类型转换。                      
即，从字符串类型转换到其他的基本数据类型；                       
如下，这个bean的定义：          
```xml
<bean id="test" class="com.spring.test">
<property name="username">
<value>2.3</value>
</property>
</bean>
```

该Bean有一个属性值 2.3，这个属性值就会通过PropertyEditors转换到float类型当中；                 

如果bean的属性需要注入另外一个bean的实例，通常用 ref元素进行标识；              
ref元素可以防止出现引用错误；用于设置属性值为其它的bean；                    
属性值设置为容器中其它bean时，通常采用ref元素进行指定，而不是之前提到的value元素；                     

##### Ref元素通常有两个属性
- bean :     
用于指定不在相同xml配置文件中的bean；    

- local :
用于指定在相同xml配置文件中的bean；     

下面来定义一个id属性为 `test` 的Bean :     
```xml
<bean id="test" class="com.spring.test">
      <property name="username">
      <ref local="相同XML的bean id"/>
      </property>
</bean>
```

该Bean中包含了一个username的属性；    
同时，username属性当中引入了另外一个Bean；       
这个Bean要和 id 为 `test` 的Bean在同一个XML配置文件当中；          

- 另一个Bean：
```xml
<bean id="test" class="com.spring.test">
      <property name="username">
                <ref bean="其他的bean id"/>
      </property>
</bean>
```
这个Bean中，同样定义了一个 id为 `test` 的Bean；有一个 username的属性；          
同时，username中，我们通过ref关联到另外一个Bean；        
这个Bean可以和 id 为 `test`的Bean不在同一个 XML文件当中；              


那么，ref和value都可以指定bean，二者有什么区别呢？                                     
- 使用ref元素，可以让spring在部署时验证依赖的bean是否真实存在；          
- Value元素进行指定，仅在创建bean实例时做验证；             
这样会导致错误的延时出现，而且还会带来额外的类型转换开销；                    


如果bean的属性是集合，则可以通过使用集合元素，如：`List、set、map`以及`props`，用来为bean注入集合值；     
下面看一下list注入：            

- List         
```xml
<bean id="test" class="com.spring.test">
    <property name="lists">
          <list>
		<value>1</value>
		<value>2</value>
		<value>3</value>
          </list>
    </property>
</bean>
```

list注入，我们采用list标签；                   
它的值，我们采用value标签来进行指定；                    

下面看一下Map注入：
- Map          
```xml
<bean id="test" class="com.spring.test">
     <property name="maps">
             <map>
		  <entry key="key1">
		  <value>value1</value>
		  </entry>
		  <entry key="key2">
		  <value>value2</value>
		  </entry>
             </map>
      </property>
</bean>
```
Map注入，通常用map标签来表示；              
该标签下面有一个entry标签；entry标签中有个key的属性；                  
这个key和map对象中的key是对应的；                 
同样，用value标签来指定map中的值元素；                 

下面来看一下 props注入：              
```xml
<bean id="test" class="com.spring.test">
     <property name="props">
	       <props>
		    <props key="key1">value1</props>
		    <props key="key2">value2</props>
	       </props>
     </property>
</bean>
```

props的注入，通常采用props标签来表示；            
这个标签当中的key以及它相应的value，与properties配置文件中的 `key-value`键值对是一样的；         


下面看一下set集合的注入：
- set    
```xml
<bean id="test" class="com.spring.test">
  <property name="sets>
	<set>
	<value>value1</value>
	<bean class="com.spring.tt"/>
	......
	</set>
   </property>
</bean>
```
set集合注入，我们通常用set标签来表示；                    
set标签里可以加入普通的数据类型，bean类型，以及引用bean类型等等；                    



### Spring的自动绑定
对于Spring的自动绑定，通常使用bean元素的autowire属性来配置自动绑定类型；             
其中，autowire可以有如下的值：       
- No :           
不使用自动绑定；               
bean依赖通过ref元素决定；        
这是默认的绑定类型配置；            
- byName :       
根据属性的名称进行自动绑定；          
- byType :           
根据属性类型自动进行绑定；         


### Bean的作用域             
在spring2.0版本之前有两种作用域，即：           
`singleton  以及  prototype`          
那么，在spring2.0版本以后则增加了session和request的作用域；

- singleton :         
当bean的作用域设置为singleton后，那么，spring的容器只会存在一个共享的bean实例；             
并且所有针对该bean的请求只会返回同一个bean的实例；                

- prototype (non-singleton)                     
当bean的作用域设置为prototype后，对于每一次针对bean的请求都会生成一个新的bean实例；             
相当于java语言中 new 的操作；                         
有一点非常的重要：                                   
即，定义为 prototype类型的bean，其生命周期很长，不易回收，通常要进行额外的清除处理；              

- request :              
Request作用域表示针对每一次的http请求都会产生一个新的bean实例；                   
同时，该bean仅在当前的 `http request` 范围内有效；             


- session :              
session作用域表示针对每一次的http请求都会产生一个新的bean实例；            
同时，该bean仅在当前`http session`范围内有效；                


### Bean的管理生命周期
Spring可以管理实例化bean之前以及销毁之前的行为；             
对于Spring的管理生命周期行为，主要分为如下两个时机：                            
- 注入依赖关系之后       
- 销毁bean之前


注入依赖关系之后                
那么，注入依赖关系之后，spring提供了两种方式，即：         
1)使用 `init-method`属性              

2)实现`InitializingBean`接口         

通过指定 `init-method`属性，确定某个方法应该在bean依赖关系结束以后自动执行；     
这种方式无需将代码与spring的接口耦合在一起；代码的污染极小；      
通常，通过在bean当中，进行方法的定义如 `init()`方法，然后通过在配置文件bean元素中加入
`init-method`属性来实现这个过程；     

另外一种实现是实现`InitializingBean`接口；               
这种方式无须指明 `init-method`属性；              
当容器依赖注入以后，会自动调用 `afterPropertiesSet`方法；它和 `init-method`执行的效果一样；         
但这种方式属于侵入性的代码设计，不推荐采用；                            


与初始化方法类似，spring也提供了与之对应的bean销毁之前的行为方式；             
有两种实现方法，即：     
- destroy-method属性       
- 实现DisposeableBean接口                       

`Destroy-method`属性用于执行在bean销毁之前所执行的方法；                 
这种方式和之前的`init-method`一样，无需将代码和spring的接口耦合到一起；其代码污染极小；             
通常，通过在bean当中进行方法的定义，如： `destroy()`方法；              
然后，在配置文件bean元素中加入 `destroy-method`属性来实现这一过程；              

另外一种是实现 DisposableBean接口；               
这种方式无须指明 `destroy-method`属性；                          
当容器依赖注入以后，会自动调用 destroy方法；             
它和 `destroy-method`执行的效果一样；            
但，这种方式属于侵入性的代码设计，不推荐采用；                   



### Spring中Bean的继承

Bean的继承   
 继承    
```
子Bean可以从父Bean继承配置信息
减少配置工作
```

Bean的模板    
```
   不需要被实例化
   使用abstract属性
```

什么是继承呢?         

#### 继承：
继承是指子bean可以从父bean继承配置信息；                   
也可以覆盖特定的配置信息；                                  
或者，在父bean的基础之上加入新的配置信息；                          
其实质类似于java语言中的子类与父类的继承关系；                     

利用继承可以节省很多的配置工作；                               
在实际的项目应用中，公有的配置会配置成模块，供子bean继承；                  
如果两个bean之间配置信息大致相同，可以采用bean的继承来减少配置工作；                        

那么，在spring中既然要将公用的配置信息配置成模板；              
那么，这个模板不需要被实例化，而仅仅作为子bean的模板来使用；                         
但是，applicationContext或者beanfactory默认会初始化所有的bean；                      
所以在这里，使用abstract属性；该属性可以阻止模板被实例化；                    
当abstract为true时，表示该bean是抽象的bean，不能被初始化；                   
所以，抽象的bean，即不能通过sessionfactory或者applicationContext的getBean方法获得；                         
也不能通过bean的ref属性指向这个bean；否则将导致错误的产生；                      




## 高级管理

Bean的注入方式     
Bean根据注入方式的不同，可以分为：      
```
构造方法注入
属性注入
```
### 构造方法注入
定义：      
所谓的构造方法注入是指通过构造函数来完成依赖关系的设定；          
构造方法注入通过 `constructor-arg`属性来指定；                    
在该标签下要指定索引的位置，通常用属性index来表示。             
index属性就是用于指定对象将注入到构造方法中的哪一个位置的参数；                    
参数的顺序指定中，第一个参数索引值是0，第二个参数索引值是1，以此类推；                        
以下为构造方法注入的代码段：                
```
<bean id="test" class="com.spring.test">
<constructor-arg index="0">
   <value>values1</value>
<constructor-arg/>
<constructor-arg index="1">
   <value>values2</value>
<constructor-arg/>
</bean>
```

这里我们用`constructor-arg`标签来指定构造方法的注入           
其中，index属性设置为0，另一个index属性设置为1；    
这样就要在构造函数当中定义两个属性值来完成依赖注入；            

#### 使用构造方法注入的优缺点：        
优点：          
在构造对象的同时，完成依赖关系的建立；           
对象一旦建立，与其关联的对象关系也就建立了；           

缺点：         
但，如果关联的对象很多，那么不得不在构造方法上加入过多的参数，会让程序代码难以维护；          
这个时候使用属性注入就比较好了；           


### Aware相关的接口
对于应用程序来说，应该尽量减少对` spring api` 的耦合程度；             
然而有些时候，为了运用spring所提供的一些功能，有必要让bean了解spring容器对其进行管理的细节信息；             
如，让bean知道在容器中以哪个名称被管理的；                   
或者，让该bean知道beanfactory或者applicationContext的存在；                     
也就是说，让该bean可以取得beanfactory或者applicationContext的实例；                          
如果bean可以意识到这些对象的存在，那么就可以在bean的某些动作发生时，做一些如事件发布等操作；                
Spring提供一些aware相关接口，如下：              

- BeanNameAware 接口： 
 获得本身bean的id属性     
 通过 `setBeanName(String name)`方法实现；       

如果某个bean需要访问配置文件中本身bean的id属性，通过实现该接口的bean类，在依赖关系确定之后，初始化方法之前，提供回调自身的能力，从而获得本身bean的id属性；                    
该接口提供了 `void setBeanName (String name)` 方法；         
需要指出的是，该方法的name参数就是该bean的id属性；                
回调该 setBeanName方法可以让bean获得自身的id属性；                  


- BeanFactoryAware接口：                    
  直接通过beanfactory来访问Spring的容器       
```
   void setBeanFactory(BeanFactory beanFactory) 方法
```
实现BeanFactoryAware接口的bean类，可以直接通过beanfactory来访问spring的容器；               
当该bean被容器创建以后，会有一个相应的beanfactory的实例引用，该接口有一个方法即：                 
```
 void setBeanFactory(BeanFactory beanFactory) 方法
```
该方法有一个beanFactory的参数；            
该参数指向创建它的BeanFactory实例；                   
实现BeanFactoryAware接口，可以让bean拥有访问spring容器的能力；                         
但，它也有不好的地方，即代码污染，导致代码与spring的api耦合在一起；                           
因此这种方式是不推荐的；               

- ApplicationContextAware 接口：                  
注入 ApplicationContext实例；
```
  setApplicationContext(ApplicationContext context) 方法
```
实现`applicationContextAware`接口的bean类，在bean类被初始化后，将会被注入applicationContext实例；   该接口只有一个方法，即：     
```
   setApplicationContext(ApplicationContext context) 方法；
```
其中，参数context用来指向创建它的applicationContext实例；                     
其优缺点和beanFactoryAware接口相同。                          


#### BeanpostProcessor类
对容器中的Bean进行处理     
实现BeanPostProcessor接口
```
   Object postProcessBeforeInitialization(Object bean,String name)
   Object postProcessAfterInitialization(Object bean,String name)
```
Spring提供了一种特殊的bean，其并不对外提供服务，也无需id属性，它负责对容器中的bean进行处理；这种bean被称为bean后处理器；即：beanpostProcessor；         
它是在 bean实例在spring的容器创建 以后，针对这个bean进一步进行处理；                 
Bean后处理器必须实现BeanPostProcessor接口，该接口包含两个方法：          

- Object postProcessBeforeInitialization(Object bean,String name)       
该方法第一个参数是系统即将初始化的bean，第二个参数是bean实例的名称；          

- Object postProcessAfterInitialization(Object bean,String name)         
该方法第一个参数是系统初始化后的bean，第二个参数是bean实例的名称；                   

实现该接口的bean，必须实现这两个方法；              
这两个方法，在容器中实例化bean之前和之后被调用；                         
容器中一旦注册了这个BeanpostProcessor后，其在spring的容器当中会自动进行加载，并自动的进行bean的相应的处理工作；              


#### BeanFactoryPostProcessor类
与bean的后处理器对应的，spring还提供了容器后处理器，即：       
```
      BeanFactoryPostProcessor；
```
容器后处理器在容器实例化结束以后，对容器进行额外的处理；           
具体的，在beanfactory加载bean的内容，但还没有产生bean的实例之前，可以针对该beanfactory进行一些处理；           
容器的后处理器必须实现 BeanFactoryPostProcessor接口；              
该接口必须实现一个方法，即：         
```
 void postProcessorBeanFactory(ConfigurableListableBeanFactory  beanFactory)
```
有一点需要说明的是，applicationContext可以自动检测到BeanFactoryPostProcessor；           
然后，作为容器的后处理器进行注册；                     
可以直接将实现了该接口的bean以普通的bean的形式部署在容器中；                        
但如果使用的是beanfactory作为容器，则需要手动进行注册；                    
Spring提供了很多的后容器处理器，常用的有：                        
```
1、 PropertyPlaceholderConfigurer : 属性占位符配置器
2、 PropertyOverrideConfigurer : 另外一种属性占位符配置器；
```
他与propertyPlaceholder的区别在于他具有覆盖的性质；       


- PropertyPlaceholderConfigurer : 属性占位符配置器            
它是spring的内建PropertyEditor，它是beanFactoryPostProcessor的实现类；            
作用：                        
主要用于读取properties配置文件；                         
通过该实现类可以将spring的配置文件的某些属性值配置到properties文件当中；                   
从而，只要修改properties的文件，spring的配置文件即可进行相应的变动；                         

使用 PropertyPlaceholderConfigurer 修改某个部分的配置属性时，不需要打开spring的配置文件。                      
从而保证不会将新的错误引入到spring的配置文件中；                                 

- PropertyPlaceholderConfigurer : 属性占位符配置器 有如下优点：
```
1、可以从主 xml配置文件中分离出部分的配置信息；
2、可以支持使用多个属性文件；
```
通过这种方式，可以将配置文件分割成多个配置文件，从而降低修改配置文件的风险；            


`PropertyOverrideConfigurer`与`PropertyPlaceholderConfigure`不同的是，`PropertyOverrideConfigurer` ；

- 它是另一种形式的占位符配置器；      
- 该占位符配置器会覆盖掉xml配置文件当中的配置信息；      
也就是说，以 `.properties`文件的配置信息为主；       
如果没有对PropertyOverrideConfigurer 进行配置，则将会使用 xml配置文件当中的配置信息；         
PropertyOverrideConfigurer 的属性格式如下：     
```
beanName.propperty = value
```
其中，beanName是spring xml配置文件中的 bean id属性；    
value是对应的属性值；          

对于多个 properties配置文件，通过locations属性来进行指定；                


Bean属性编辑器                              

自定义属性编辑器                               

应用场景                       
类型就无法被识别，如日期等                       
实现                            
```
 继承PropertyEditorSupport
  重写其setAsText方法
```
自定义属性编辑器的应用场合是什么样的呢？                       
spring可以把普通的属性注入到bean对象中；                             
但是，像一些特殊的类型，如：日期等类型，这些类型是无法被正确处理的；                        
日期和字符串属于不同的对象类型，所以这个时候可以通过spring的自定义属性编辑器将字符串转换为相应的对象进行注入；                
自定义属性编辑器通过继承 PropertyEditorSupport类来实现；                   
并且需要实现 重写 其 setAsText方法，即：  `setAsText(String text)`                     
其中的参数text是配置文件中的值。                    
我们的目标就是将这个text按照需求进行转换；                         


下面以日期和字符串之间的转换的例子来说明自定义属性编辑器的使用；         
如下代码：         
```java
public class test {
  private Date date;

  public Date getDate() {
    return date;
  }

  public void setDate(Date date) {
    this.date = date;
  }
}
```

首先，定义一个名称为test的bean，该bean有一个日期属性date；             
同时，我们生成相应的setter和getter方法；                
接下来，再定义一个扩展了PropertyEditorSupport类的自定义属性编辑器类customerProperty           
```java
public class customerProperty extends PropertyEditorSupport {
  String format;
  public String getFormat() {
      return format;
  }
  public void setFormat(String format) {
      this.format = format;
  }
  @Override
  public void setAsText(String text) throws IllegalArgumentException {
    SimpleDateFormat sf = new SimpleDateFormat(format);
    try {
         this.setValue(sf.parse(text));
     }catch (ParseException e) {
	e.printStackTrace();
     }
  }
}
```
这里只需要重写setAsText方法，并且将转换后的对象通过setValue方法，重新设置属性即可；               

最后，在配置文件中，进行配置：
```java
<bean class="org.springframework.beans.factory.config.CustomEditorConfigurer">
    <property name="customEditors">
	<map>
	   <entry key="java.util.Date">
	        <bean class="customerProperty">
		    <property name="format" value="yyyy-mm-dd"></property>
		</bean>
	   </entry>
	</map>
     </property>
</bean>
<bean id="te" class="test">
   <property name="date">
      <value>2011-5-6</value>
   </property>
</bean>
```
定义一个CustomEditorConfigurer的bean；    
其中，entry中的key是需要进行转换的对象；      
里面的bean用于指明具体处理转换的bean；                


## Spring AOP
概述    
AOP，全称是：`aspect-oriented programming`，即，切面编程；它是面向切面编程的思想核心；                  
AOP并不是一个新的名词；                 
它之所以流行是近年来许多技术都支持了AOP的实现；                 
AOP是面向切面编程的思想核心；                       
OOP是面向对象的编程语言；                   
AOP与OOP思想并不冲突，它们是两个相辅相成的设计模型；                                 
AOP技术弥补了面向对象编程思想的不足；               
`Spring AOP` 是实现AOP的一种技术；                            
`Spring AOP`是Spring框架中某个子框架或者子功能所依赖的核心；                               
但，Spring的容器并不依赖于AOP；
这意味着程序员可以自己选择是否使用AOP技术；
AOP提供强大的中间件解决方案，这使得`Spring IoC`容器更加的完善；


### AOP简介
AOP术语介绍             
关于AOP有很多的抽象的名词术语，下面我们对相关的术语进行介绍：           
`Corss-cutting concerns` : 系统层面上的服务穿插到业务逻辑的处理流程之中；            
在应用程序当中，将安全验证，事务，日志等系统层面上的服务穿插到业务逻辑的处理流程之中；            
这些动作被称之为 `Cross-cutting concerns`            

系统级别的服务和业务逻辑混合在了一起；                
业务逻辑代码中被写入了大量的非业务逻辑代码，这样会使维护程序的成本大大增加；            

`Aspect` : 当需要时，将其放到应用程序之上；不需要时，将其从应用程序中脱离出来；           
将业务逻辑代码中的` Cross-cutting programming` 整合到一起，设计成各个独立的可以重用的对象；                            
当需要服务的时候，将其放到应用程序之上；              
不需要时，将其从应用程序中脱离出来；                  
这样，对应用程序侵入极小；            

`Adivice` : 是Aspect具体的实现                   
Advice包括了 `cross-cutting concerns` 的行为或所提供的服务；             


`Joinpoint` : 即，连接点；是Aspect在应用程序执行时加入业务流程的时机             
这个时机可能是某个方法被执行之前或之后，或者两者都有，或者某个异常发生的时候；              

`Pointcut` : 指定某个aspect在哪些joinpoint时被穿插至应用程序之上；                  
即切入点；                
通过定义pointcut可以指定某个aspect在哪些连接点时，被穿插至应用程序之上；              
可以在某个定义文件中编写相应的Pointcut；                    
其中，说明哪些aspect需要穿插到应用程序的joinpoint；                      

`Target` : 一个advice被应用的对象或者目标对象                         
是一个目标对象；                          
它是一个advice被应用的对象或者目标对象；                     

`Instruction`  : 为已编写，编译完成的类，在执行时期动态加入一些方法，而不用修改或者增加任何代码；                    
对于一个现存的类，Instruction可以为其增加行为，而不用去修改该类；                           
具体的，为某个已编写，编译完成的类，在执行时期动态加入一些方法，而不用修改或者增加任何代码；                         

`Weave` : 被应用到对象之上的过程                
即织入；              
被应用到对象之上的过程；                   
AOP织入有3个阶段，即： 编译、类加载、执行时期；               



### Spring对AOP的支持
纯java语言来编写     
定义pointcuts可以使用配置文件         
不支持属性成员的jointpoints               

不同的框架对AOP有不同的实现方式；            
主要差别在于，所提供的Pointcut，Aspect的不同；以及，它们如何被 weave到应用程序当中；                
Spring的advices采用纯java语言来编写，而不是使用特定的AOP语言；             
在定义pointcuts时，可以使用配置文件；          
这种方式，对于java程序员来说，不用使用AOP的特定语法，          
就可以用熟悉的java语言与xml格式来开发`Spring AOP`；             

`Spring AOP`当中，应该以实现接口的方式作为优先条件；             
这样可以降低应用程序组件之间的耦合程度；                              

Spring不支持属性成员的jointpoints，这是因为Spring的设计思想认为：          
支持属性成员的jointpoints会破坏对象的封装性；                            




#### Spring创建Advice

- Before Advice     
 目标对象的方法执行之前被调用          
 通过实现MethodBeforeAdvice接口来实现     
```
 before(Method method,Object[] args,Object target)
```    

`Before advice`会在目标对象的方法执行之前被调用。         
可以通过实现 MethodBeforeAdvice接口来实现其逻辑；               
该接口中定义了一个方法，即：      
```
before(Method method,Object[] args,Object target)
```

MethodBeforeAdvice继承自BeforeAdvice接口；           
而，BeforeAdvice接口又继承自Advice接口；              
before方法会在目标对象target所指定的方法执行之前被调用执行；            
before方法声明为 void，所以不会传回任何的结果；                  
在before方法执行完毕之后，才开始执行目标对象的方法；                     


实现的过程：            
首先，定义一个接口: IHello       
```java
 public interface IHello
 {
   public void sayHello(String str);
 }
```
该接口中定义一个 sayHello方法；这个方法没有返回值；           
接下来，定义一个IHello的实现类Hello类，并实现接口中的sayHello方法：            
```java
 public class Hello implements IHello {
   public void sayHello(String str) {
      System.out.println("你好"+str);
   }
}
```
可以看到，该方法中只传入了一个字符串变量；    

下面，我们要在 sayHello方法之前执行一些操作：                      
我们需要定义一个实现了 MethodBeforeAdvice 接口的实现类 sayBeforeAdvice   
```java
 public class sayBeforeAdvice implements MethodBeforeAdvice {
  public void before(Method method,Object[] args,Object target)
  {
    System.out.println("在方法执行前做的事情！");
  }
}
```
实现MethodBeforeAdvice接口的before方法；               
在该方法体中，我们简单的输出一个信息提示；                  

最后一步，我们要在XML配置文件中，建立Advice和Target对象实例；          
同时，我们还需要用到ProxyFactoryBean；                        
这个类被beanfactory或者applicationcontext用来建立代理对象；                   
同时，需要指定proxyInterfaces属性并设置其代理的接口；                    
在target上设定target对象；                  
在interceptorNames上设定advice的实例；              
```
<beans>
    <!--建立目标对象实例-->
    <bean id="hello" class="Hello"/>
    <!--建立advice实例-->
    <bean id="sba" class="sayBeforeAdvice"/>
    <!--建立代理对象-->
    <bean id="helloProxy" class="org.springframework.aop.framework.ProxyFactoryBean">	
    <!--设置代理的接口-->
    <property name="proxyInterfaces">
       <value>IHello</value>
    </property>
    <!--设定目标对象实例-->	
    <property name="target">
    	<ref bean="hello"/>
    </property>
    <!--设定advice实例-->
    <property name="interceptorNames">
    	<list>
    		<value>sba</value>
    	</list>
    </property>
    </bean>
</beans>

```

##### 编写测试类，测试运行结果
```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;


public class Test {

	/**
	 * @param args
	 */
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		 ApplicationContext context = new ClassPathXmlApplicationContext(
					"applicationContext.xml");
		 IHello iHello = (IHello) context.getBean("helloProxy");
		 iHello.sayHello("访客");
	}

}
```


#### After Advice
 在目标对象的方法执行之后被调用     
 通过实现AfterReturningAdvice接口来实现    
```
 afterReturning(Method method,Object[] args,Object target)
```
`After Advice`会在目标对象的方法执行之后被调用；      
可以通过实现AfterReturningAdvice接口来实现其逻辑；          
该接口中定义了一个方法，即：           
```
afterReturning(Method method,Object[] args,Object target)
```

AfterReturningAdvice继承自Advice接口；                       
afterReturning方法会在目标对象target所指定的方法执行之后被调用执行；                
afterReturning方法声明为void，所以不传回任何的结果；                   
如果要终止接下来的应用程序的执行，唯一的方式是通过抛出异常来实现；                                       


可以通过在上一个 before advice的基础之上，为hello类的sayHelo方法加入执行之后的操作；              

首先，定义一个SayAfterAdvice类，这个类实现了AfterReturningAdvice接口；                           
该接口中定义了一个afterReturning方法；                
在方法体内简单的输出 `在方法执行后所执行` 这样的信息；   
```java
public class sayAfterAdvice implements AfterReturningAdvice {

	@Override
	public void afterReturning(Object arg0, Method arg1, Object[] arg2,
			Object arg3) throws Throwable {
	 System.out.println("在方法执行后执行");

	}

}
```


接下来，在配置文件中，加入一个bean，并且将这个bean放入到interceptorNames属性的列表中；            
其他的配置和上一个BeforeAdvice是相同的；       
```
<beans>
    <!--建立目标对象实例-->
    <bean id="hello" class="Hello"/>
    <!--建立advice实例-->
    <bean id="sba" class="sayBeforeAdvice"/>
    <bean id="saa" class="sayAfterAdvice"/>
    <!--建立代理对象-->
    <bean id="helloProxy" class="org.springframework.aop.framework.ProxyFactoryBean">	
    <!--设置代理的接口-->
    <property name="proxyInterfaces">
       <value>IHello</value>
    </property>
    <!--设定目标对象实例-->	
    <property name="target">
    	<ref bean="hello"/>
    </property>
    <!--设定advice实例-->
    <property name="interceptorNames">
    	<list>
    		<value>sba</value>
    		<value>saa</value>
    	</list>
    </property>
    </bean>
</beans>
```

前面介绍了`before advice` 和` after advice` ；          
它们在方法的执行之前和执行之后来执行相应的操作；            


- Around Advice         
  在方法的执行之前和之后来执行相应的操作         
  要实现MethodInterceptor接口              

`Around Advice` 可以不用分别配置 `before advice` 和` after advice`；              
而，只需要实现 MethodInterceptor接口；               
该接口中有一个invoke方法；                             
其中，参数 methodInvocation有一个proceed方法，用来执行目标的方法；                      


下面，我们定义一个实现了MethodInterceptor接口的实现类 SayAroundAdvice类；             
该类中有一个invoke方法；                         
在方法体内，我们在调用proceed方法执行之前和之后输出两条简单的信息，从而达到和实现
`before advice` 和` after advice`一样的效果；    
```java
public class SayAroundAdvice implements MethodInterceptor {

	@Override
	public Object invoke(MethodInvocation arg0) throws Throwable {
		Object result = null;
		System.out.println("在方法执行前做事情");
		result = arg0.proceed();
		System.out.println("在方法执行后做事情");
		return result;
		
	}

}

```

在配置文件中，加入一个bean，并且将这个bean放入到interceptorNames属性的列表中；             
该bean的实现类指明了SayAroundAdvice实现类；               
其他的配置与 `before advice` 和 `after advice` 配置相同；        
测试类也相同；     
```
<beans>
    <!--建立目标对象实例-->
    <bean id="hello" class="Hello"/>
    <!--建立advice实例-->
    <bean id="sad" class="SayAroundAdvice"/>
    <!--建立代理对象-->
    <bean id="helloProxy" class="org.springframework.aop.framework.ProxyFactoryBean">	
    <!--设置代理的接口-->
    <property name="proxyInterfaces">
       <value>IHello</value>
    </property>
    <!--设定目标对象实例-->	
    <property name="target">
    	<ref bean="hello"/>
    </property>
    <!--设定advice实例-->
    <property name="interceptorNames">
    	<list>
            <value>sad</value>
    	</list>
    </property>
    </bean>
</beans>
```

- Throw Advice     
  异常发生的时候，通知某个服务对象做处理      
  实现ThrowsAdvice接口；           

`Throw Advice`可以在异常发生的时候，通知某个服务对象做处理；      
实现这个异常通知，必须实现ThrowsAdvice接口；                
这个接口当中没有定义任何的方法；这个方法可以自己来定义；             

throw advice是如何实现的呢？                    

首先，我们定义实现了ThrowsAdvice接口的通知类，并且定义一个 afterThrowing 的方法；       
该方法的参数method，是异常发生时的方法；ta是所发生的异常；          
```java
public class sayThrowAdvice implements ThrowsAdvice {
public void afterThrowing(Method method,Object[] objs,Object target,Throwable ta) {
 System.out.println(" Exception happened "+ta+" and was thrown in "+method);
  }
}
```

我们定义了一个实现了 ThrowsAdvice接口的实现类 sayThrowAdvice；        
在 afterThrowing这个方法里面，我们将异常发生时的方法名称以及异常发生时的信息输出；               

为了测试，我们需要在实现类和接口中加入异常；      
我们在IHello接口的实现类Hello类当中，在sayHello方法中抛出一个异常；                        
并且，传入一个字符串参数，异常发生；      
```java
 public interface IHello {
  public void sayHello(String str) throws Exception;
 }
 public class hello implements IHello {
 @Override
 public void sayHello(String str) throws Exception {
  System.out.println("你好"+str);
  throw new Exception("异常发生！");
```

配置文件的配置部分与之前Advice中的配置是相同的；
```
<beans>
    <!--建立目标对象实例-->
    <bean id="hello" class="Hello"/>
    <!--建立advice实例-->
    <bean id="sta" class="sayThrowAdvice"/>

    <!--建立代理对象-->
    <bean id="helloProxy" class="org.springframework.aop.framework.ProxyFactoryBean">	
    <!--设置代理的接口-->
    <property name="proxyInterfaces">
       <value>IHello</value>
    </property>
    <!--设定目标对象实例-->	
    <property name="target">
    	<ref bean="hello"/>
    </property>
    <!--设定advice实例-->
    <property name="interceptorNames">
    	<list>
    		<value>std</value>
    	</list>
    </property>
    </bean>
</beans>
```

需要注意的是：        
`throw advice` 只是会感知异常的行为；         
他不会具体处理掉异常；            
这个工作还需要应用程序本身来负责处理完成。         

测试类：    
```java
public class Test {


	public static void main(String[] args) {
		// TODO Auto-generated method stub
		 ApplicationContext context = new ClassPathXmlApplicationContext(
					"applicationContext.xml");
		 IHello iHello = (IHello) context.getBean("helloProxy");
		 try {
			iHello.sayHello("访客");
		} catch (Throwable e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}

}
```


基于 `XML Schema`           

对于Spring的AOP技术，我们同样可以采用基于 `xml schema`技术的开发；     
以这种方式进行开发可以简化代码的实现，更容易的对代码进行维护；       

在Spring配置文件当中，切面，切入点等所有元素都必须定义在 `<aop:config>`标签元素当中；    
配置文件中可以定义多个 `<aop:config>`元素；      
同样，该元素也可以包含多个pointcut，advisor和aspect等元素；     


##### 下面通过实例来说明一下实现的步骤：    
该实例实现了通过`Spring AOP`的`xml schema`技术，在输出信息之前和之后进行相应的操作处理；    

首先，定义一个切面；      
这个切面可以是一个简单的bean对象，也可以是负责相应的业务逻辑的处理对象；          
该切面当中，我们定义了两个方法：                  
startLog方法，用于开始记录；  endLog方法，用于结束记录；               
```java
 public class aspectBean {
  public void startLog() {
    System.out.println("开始记录！");
  }
  public void endLog()  {
    System.out.println("结束记录！");
}
```
同时，定义IHello接口，以及实现了该接口的Hello实现类；     
在该实现类中，定义了sayHello方法；           
方法体中简单的输出一条提示信息；               
```java
 public interface IHello {
  public void sayHello(String str);
  }
 public class Hello implements IHello {
  @Override
   public void sayHello(String str) {
    System.out.println("你好"+str);
   }
 }
```

下面来看一下相应的配置文件：
```
 <bean id="apbean" class="aspectBean"></bean>
 <bean id="he" class="Hello"></bean>
 <aop:config>
  <aop:aspect id="ap" ref="apbean">
   <aop:pointcut expression="execution(*.hello.*(..))" id="pc"/>
   <aop:before method="startLog" pointcut-ref="pc"/>
   <aop:after  method="endLog" pointcut-ref="pc"/>
  </aop:aspect>
 </aop:config>
```

该配置文件中：           

<aop:config>    : 包含多个切面，切入点，advice等标签元素；           
<aop:aspect>    : 定义一个切面；ref属性用于指向一个普通的bean对象；            
<aop:pointcut>  : 定义切入点信息，expression是执行的表达式；               

在执行表达式中最常用的是  `*`字符；它代表匹配任意的返回类型；       
```
 ()   :  匹配了一个不接受任何参数的方法
 (..) :  匹配了接受任意参数的方法，该参数可以是0个或者更多。

```
<aop:after> : 定义after advice；     
<aop:before> :定义before advice；        
<aop:around> :定义around advice；     



基于Annotation              

spring2.0以后，采用 `@AspectJ`，即以注解的方式对java的普通类进行注解；           
实际上，被`@AspectJ`元素进行修饰的类，被称为切面；           
spring可以将这些切面织入到目标bean当中；              
`@AspectJ`采用jdk5.0的注解技术；                          
所以，在开始使用aspectj注解技术之前，要确保 jdk是5.0或者以上的版本；                    

下面，通过实例说明一下实现的步骤：                   
该实例通过实现aspectj技术，在输出信息之前和之后进行相应的操作处理；          

首先，定义IHello接口，以及实现类Hello：        
```java
 public interface IHello {
  public void sayHello(String str);
  }
 public class Hello implements IHello {
  @Override
   public void sayHello(String str) {
    System.out.println("你好"+str);
   }
 }
```
类当中有一个sayHello方法，用于输出一条简单的提示信息；             

下面来定义一个切面：
```java
@Aspect  //声明切面
 public class aspectBean {

  @Pointcut("execution(* hello.*(..))")  //定义切入点
  public void log() {}

  @Before(value="log()")  //在切入点之前执行
  public void startLog() {
    System.out.println("开始记录！");
  }
  @After(value="log()")  //在切入点之后执行
  public void endLog() {
   System.out.println("结束记录！");
  }
}
```

在class定义之前，我们定义一个 `@Aspect`注解；
该注解表明了该类是一个切面的类；             
同时，我们在log方法之前注入`@Pointcut`注解，用于定义切入点；              
在startLog方法之前，我们注解`@Before`，该注解用于在切入点之前执行，相当于`before advice`的功能；     
接下来，我们在endLog方法之前注解`@After`；     
该注解是在切入点之后执行，相当于`after advice`；                               


最后，我们来看配置文件：            
```
<aop:aspectj-autoproxy/>
<bean id="apbean" class="aspectBean"></bean>
<bean id="he" class="hello"></bean>
```
首先，我们要在配置文件当中启动AspectJ；方法是，输入 `<aop:aspectj-autoproxy/>`       
这样就启动了一个AspectJ；                 
同时，我们需要将Hello类以及它的切面类配置到配置文件当中；                     


测试类：         
```java
public class Test {


	public static void main(String[] args) {
		// TODO Auto-generated method stub
		 ApplicationContext context = new ClassPathXmlApplicationContext(
					"applicationContext.xml");
          IHello ih = (IHello) context.getBean("he");
          ih.sayHello("访客");
		}
	}
```

运行结果：       
开始记录！        
你好访客      
结束记录！       



##### 定义 Pointcut、Advisor

前面所定义的advice都是直接织入到代理接口的执行之前，之后，或者在异常发生时；         
事实上，还可以对织入的时机定义的更细；             
pointcut定义了advice的应用时机；                       
在spring中，pointcutAdvisor将pointcut和advice结合成为一个对象；                     
spring内建的pointcut都有对应着的pointcutAdvisor；                       
常见的有两种：      

- NameMatchMethodPointcutAdvisor :    
它是最基本的pointcutAdvisor；  是静态pointcut的实例；     
你可以指定advice所要应用的目标方法名称；或者，用 `* ` 来指定；     
例如，       
hello`*` 表示以hello开头的所有方法名称，都会应用到指定的advice上；       

- RegExpMethodPointcutAdvisor  :       
可以让你用正则表达式的方式来定义 pointcuts     
它是静态pointcut的实例；        
在符合正则表达式的情况下，应用到指定的advice上；     
其中，bean中有一个pattern属性，用于写入该正则表达式；         

```
<bean id="helloPerson" class="org.springframework.aop.support.NameMatchMethodPointcutAdvisor ">
       <property name="mappedName">		
    		<value>hello*</value>
    	</property>
    	<property name="advice">
    		<ref bean="sba"></ref>
    	</property> 	
    </bean>


    <bean id="helloPerson" class="org.springframework.aop.support.RegexpMethodPointcutAdvisor  ">
       <property name="pattern">		
    		<value>.*hello.*</value>
    	</property>
    	<property name="advice">
    		<ref bean="sba"></ref>
    	</property> 	
    </bean>

```

#### 动态代理模式
通过动态的代理模式，在目标对象的方法调用的前后，插入相应的处理代码；      
任何的接口都能被代理；    这恰好符合面向接口编程的要求；      
spring所实现的动态代理，其内置的AOP，默认使用动态代理模式来实现；     
不必为特定的对象或者方法编写实现特定功能的代理对象；       
可以使用一个处理者handler服务于各个对象；        

首先，一个处理者的类必须实现InvocationHandler接口；      
   
关键部分的代码实现：     
```java
private Obejct proxy;
public Object bind(Object proxy)
 {
   this.proxy = proxy;
   return Proxy.newProxyInstance(proxy.getClass().getClassLoader(),
   proxy.getClass().getInterfaces(),this);
 }
@Override
public Object invoke(Object arg0,Method method,Object[] arg2)
 throws Throwable {
 Object result = null;
 System.out.println("proxy method start");
 result = method.invoke(proxy,arg2);
 System.out.println("proxy method end");
 return result;
}
```
这段代码的关键部分是 `Proxy.newProxyInstance()`静态方法；      
用该静态方法建立的一个代理对象；       
建立代理对象时，必须告知所要代理的接口；之后，可以操作这个代理对象；           
invoke方法中，传入被代理对象的方法名称与执行参数；                
实际上，真正要执行的方法交由 `method.invoke`方法来执行；     
可以在这个方法之前或者之后，加上操作；     
要实现代理对象同样必须定义所要代理的接口以及它的实现类；         

在这里，我们定义一个IHello接口，以及实现了该接口的类：Hello类：     
```java
public interface IHello {
 public void hello(String str);
 }
 public class hello implements IHello {
 @Override
 public void hello(String str) {
  System.out.println("你好" +str);
 }
}
```
在Hello类当中，我们简单的定义一个信息输出；   

最后，我们进行测试       
```java
logHandler lh = new logHandler();
IHello ih = (IHello) lh.bind(new hello());
ih.hello("代理！");
```
结果：     
```
proxy method start
你好 代理！
proxy method end
```