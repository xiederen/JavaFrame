# Hibernate是一个开放源代码的对象关系映射框架。
它对JDBC进行了封装。          
Hibernate提供了强大、高性能的对象到关系型数据库的持久化服务。    
利用Hibernate，开发人员可以按照Java的基础语义进行持久层开发。        
Hibernate提供的HQL是面向对象的查询语言。             
它在对象型数据和关系型数据库之间，构建了一条快速，高效，便捷的沟通渠道。    
在本次中要掌握持久化以及对象-关系映射等概念。           

### 并且掌握如何配置Hibernate；  
理解Hibernate的基础语义和配置文件中的各个元素。  
我们要了解Hibernate的体系结构，和Hibernate中事务管理。以及简单了解HQL语言。最后要学会如何使用Hibernate开发应用程序。     

## Hibernate框架简介：
我们在开发软件的时候使用Hibernate框架对我们有什么帮助？   
我们开发的大多数软件产品都是基于数据库的，也就是说我们在一个软件中的操作多数都是对数据库中的数据进行的操作。我们对数据库大量操作就会导致数据访问层的代码变得非常繁琐，并且直接降低了我们的开发效率；   
然而，现在企业开发软件对效率的要求是非常高的；如果能提高效率，也就意味着在降低成本，那么有没有一套更好的数据库访问框架，来使我们从编写DAO层的困境中脱离出来呢？      
很显然，我们要学习的Hibernate框架就是这样的一个框架；         

Hibernate是一个优秀的Java持久化层解决方案，是当今主流的对象-关系映射`(ORM,ObjectRelationalMapping)`工具；   


### 理解持久化
在了解持久化之前先了解两个概念：  
- 瞬时状态    
>在程序运行的时候，有些程序数据保存在内存中，当程序退出后，这些数据就不复存在了，所以我们称这些数据的状态为瞬时的。
- 持久状态
>在使用一些软件的时候，有些数据，在程序退出后，还以文件等形式保存在硬盘中，那么我们称这些数据的状态是持久的；

那么，将数据存储在数据库中，数据的状态是持久状态吗？   
这当然也是持久状态；      
所以在三层结构中，DAO层在有的地方也称为持久化层。       

### 持久化
持久化就是将程序中的数据在瞬时状态和持久状态之间转换的机制；    
JDBC就是一种持久化机制；    
将程序数据直接保存成文件也是持久化机制的一种实现，        
但我们常用的是将程序数据保存在数据库中。        
所以，持久化通常都是将程序数据保存在数据库中或是将数据库中的数据读取出来；               

## ORM概述
对象-关系映射 简称为 `ORM`            
我们现在所编写的程序都是基于面向对象的思想设计的；        
那么，瞬时的数据，也就是程序运行时放在内存中的数据，它们也是以对象的形式存在的；        
而，持久的数据多数是保存在关系型数据库中，或者把关系型数据库中的数据读取出来，以对象的形式封装。       
所以，持久化工作主要是在对象和关系型数据之间完成；           
也就是说，我们在编写程序的时候，以面向对象的方式处理数据；         
而，保存数据的时候，却以关系型数据的形式存储到数据库中。            
所以，需要一种能在两者之间进行数据转换的机制，将这种机制称为对象-关系映射机制，简称为ORM；       
我们使用这个机制保存对象和关系数据库表的映射信息；              
当数据在对象和关系数据库中转换的时候，协助正确地完成转换。            

### ORM框架介绍

ORM ： 对象关系映射       
Hibernate框架 ： 能够实现ORM的框架         
对于Hibernate的称呼有很多种，比如工具、技术、框架以及解决方案等。           
能实现ORM这个功能的框架有很多，Hibernate可以说是这些框架中最流行，最受开发者关注的；         
Hibernate这么流行的原因：             
首先，假设在理想情况下，我们在程序中需要获得任何Java对象并将它保存到数据库中都应该是很轻松的，不应该为此编写特殊的代码，也不会有性能的损失。而程序是完全可移植的，也就是说如果不发生特殊情况，我们就不需要为类与数据库表的关联做任何额外的工作；那么，这就是假设的理想状态；                 
而，Hibernate框架已经非常接近这个目标了；            

### Hibernate框架简介：
我们之前学习的课程都是针对JDBC进行编程的；    
然而，Hibernate框架对JDBC进行了封装；          
如果我们使用Hibernate框架将会大大简化数据访问层繁琐的重复性代码，使得Java程序员可以随心所欲的使用面向对象编程思维来操作数据库。            
Hibernate可以应用在任何使用JDBC的场合；            
既可以在Java的客户端程序中使用，也可以在Servlet或JSP的Web应用中使用。       
那么，在Hibernate中，类与数据库表是如何来关联的呢？              
在Hibernate中，我们使用XML格式的配置文件来存储映射信息；              

### Hibernate框架下载
我们要使用Hibernate，首先，要先下载并配置它。
Hibernate的官方主页是：  `www.hibernate.org`            
我们需要的Hibernate框架的jar包可以从官方网站下载      
一般我们配置hibernate框架的时候IDE首选为MyEclipse；            
不过MyEclipse的集成度太高，有点牛刀小试的架势。           
我们作为hibernate的初学者，为了更为清晰地认识hibernate工作原理。         
我们使用最原始，最纯洁的eclipse；                         

在eclipse下配置hibernate框架的步骤：          
首先，下载`3.3.2`版本的Hibernate框架的压缩文件。         
`hibernate-distribuion-3.3.2.GA-dist.zip`           
然后将下载的Hibernate压缩包解压；          
Hibernate解压后的目录结构：                  
要用到的是lib目录下的所有jar包文件和`hibernate3.jar`包。             
但是我们下载的Hibernate压缩包下有几个开源的jar包是没有的。             
还需要我们手动下载，其中包括：          

```
commons-lang-2.3.jar
slf4j-api-1.5.8.jar
log4j-1.2.16.jar
slf4j-log4j12-1.6.1.jar
```

我们要连接Oracle数据库还需要`ojdbc14.jar`包。     
但是这个包我们可以在Oracle数据库的安装目录下找到。          
然后，我们创建一个Java项目，要有如下的目录结构：      
其中，hib是我们项目的名称。也就是整个项目最外层的目录。            
src是源文件的存放目录。          
WebRoot目录下的`WEB-INF`目录是我们存放编译后的文件和第三方jar包的目录。          
编译后文件放在classes目录；              
第三方jar包放在lib目录下。                 
我们在创建好项目之后，将我们需要的所有jar包都放在项目的lib目录下。        
我们有很多的jar包，这里面除了我们单独下载的4个jar之外，其余的都是我们下载的Hibernate解压后获取的jar包。          

### Hibernate配置 
Hibernate配置文件设置：   
我们已经做好了编写hibernate程序的准备；          
下面，我们首先来加载jar包；           
我们首先将lib目录下的jar包都加载到项目中，然后我们在新建工程项目hib的根目录(src)下面创建一个名为`hibernate.cfg.xml`的文件。这是hibernate的配置文件；           
这个文件中主要存放连接数据库的配置信息，我们在这里将它放在src的根目录；            
这个文件的内容：            
这个配置文件很简单；        
主要就是记录了数据库的url地址，登录数据库的用户名、密码、jdbc驱动程序的位置；          
现在我们假设在Oracle数据库中有一个记录用户登录信息的表login，表中有两个字段username和password；那么接下来我们将使用Hibernate框架针对login表实现增、删、该、查的功能；            

#### 添加持久化类及映射配置文件
接下来添加可持久化类；        
在这里的可持久化类使用普通的Java对象就可以了。习惯上叫做`POJO`；           
持久化类叫什么不重要，关键我们要知道它是什么。        
一个`POJO`很像`JavaBean`，它的属性是通过getter和setter方法来访问的；       
也就是说它对外隐藏了实现细节；           
然后我们来添加一个对象和关系型数据库表的映射文件；文件名为：`Login.hbm.xml`。         
```xml
<?xml version="1.0"?>
<!DOCTYPE hibernate-mapping PUBLIC
        "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
        "http://hibernate.sourceforge.net/hibernate-mapping-3.0.dtd">

<hibernate-mapping >
   <class name="com.pb.hibernate.po.Login" table="LOGIN" schema="sjjys">
       <id name="userName" type="java.lang.String">
           <column name="USERNAME" length="20"/>
           <generator class="assigned"/>
       </id>
        <property name="password" type="java.lang.String">
            <column name="PASSWORD" length="20" not-null="true"/>
        </property>
   </class>
   </hibernate-mapping>
```

在这个文件中，我们可以看到对象与数据库表的对应关系。     
首先，class标签中用于指定Login类与login表的映射。       
然后，接下来是id标签，这代表主键。因为这个表只有两个字段。username和password。所以我们把username看做是主键。也就是说username是唯一的并且不能为空。在id标签里面的name表示Login类中的属性名字，type是类型。      
然后再看column标签。它里面的name表示表中字段的名字；generator标签定义主键的生成方式。此处的设置表示，主键是指派的，就是由用户自行管理的。          
然后看id标签下面的property标签。           
这个标签中的内容表示Login类中与表中其他字段的映射。        
如果我们还有其他的字段，那么就要再增加property标签。           
用于其他字段的标签配置与id标签基本类似，但是在这里有一个`not-null`属性。我们用它来控制该字段是否可以为空。它的值为true，就表示该字段不能为空。          
然后，我们在添加完这个映射文件后，我们要在`hibernate.cfg.xml`文件中添加我们这个映射文件的声明。也就是声明`Login.hbm.xml`文件是一个映射配置文件；这条声明语句添加在`session-factory`的结束标签之前就可以。    
```
<mapping resource="com/test/Login.hbm.xml"/>
```

#### 通过Hibernate实现增加功能
现在我们通过视频已经了解了如何来添加持久化类和如何编写映射配置文件，接下来我们来看一下如何实现增、删、改、查的功能。  
实现对数据库的操作，也就是要实现DAO层，那么我们添加一个LoginDAO类，在这个类中要用到Session对象和Transaction对象；    
这个Session对象是`hibernate3.jar`包中的Session对象；            
我们对数据库的所有操作都要通过Session对象来完成；        
Transaction对象在这里用来处理对数据库操作的事务。           
例如，提交功能；           
然后，我们在构造方法中实例化Session对象；实例化的过程是：         
首先找到Hibernate配置，然后从配置中取出SessionFactory，也就是Session工厂。最后从SessionFactory中取出Session。  
我们接下来添加一个向login表中增加数据的功能。             
我们添加一个save方法，将Login对象作为形参传入方法中。          
我们在对数据库操作的时候有可能会抛出运行时异常，所以我们用`try-catch`结构来捕获异常。          
然后，我们对login表来进行增加的操作；               
首先，我们来实例化Transaction对象；         
Transaction对象是用来处理事务的，那么我们实例化它就是开启事务；然后我们调用session对象的save方法，并将login对象传入该方法。然后提交到数据库。并关闭session对象。          
当然，在这个过程中如果出现异常，我们就会将事务进行回滚。            
在这里，我们没有编写SQL语句，就完成了增加记录的功能。在这里只是调用了Session对象的save方法；              
不用去管save中如何实现增加功能，只要知道这个save方法相当于insert语句就可以了。                 

通过实现增加的功能，我们就能够总结出在Hibernate中执行持久化操作的步骤。        
在Hibernate中执行持久化操作的步骤：          

- 第一步：
读取并解析配置文件       
```
Configuration config = new Configuration().configure();
```

- 第二步：     
读取并解析映射信息，并且创建SessionFactory      
```
SessionFactory factory = config.buildSessionFactory();
```

- 第三步：     
打开session         
```
this.session = factory.openSession();
```

- 第四步：        
开始一个事务（增删改操作必须，查询操作可选）      
```
tran = this.session.beginTransaction();
```

- 第五步：        
执行持久化操作         
```
this.session.save(login);
```

- 第六步：
提交事物       
```
tran.commit();
```

- 第七步：    
关闭session     
```
this.session.close();
```

#### 通过Hibernate实现修改、删除功能
- update()方法      
仍然将Login对象作为形参传入该方法中；         

##### 如何实现修改的功能：         
首先仍然是实例化Transaction对象，开启事务，然后调用Session对象的update方法，并将login对象传入update方法中，之后我们提交到数据库，关闭Session释放资源。       
这里调用的update方法，就相当于执行了update语句；          
##### 如何实现删除功能：                 
删除功能和前面的增加，修改功能很相似；       
我们在这里调用了Session对象的delete方法；      

#### 通过Hibernate实现主键查询功能
查询功能与前面的增加，修改和删除不一样，因为我们要返回一个查询结果；     
而且，查询也分为通过主键查询，查询所有记录，模糊查询等。           
如何实现通过主键查询：          
由于主键是唯一的；      
所以通过主键查询也可以理解为精确查询；        

### Hibernate的体现结构：
Hibernate使用数据库和配置文件数据来为应用程序提供持久化服务的过程。     

#### 体系结构：
- SessionFactory：          
属于单一数据库的编译过的映射文件的一个线程安全的，不可变的缓存快照。Session的工厂。        
有可能持有一个可选的数据缓存。          
可以在进程级别或集群级别保存可以在事务中重用的数据。        
- 会话，Session：  
单线程，生命期短暂的对象，代表应用程序和持久化层之间的一次对话。封装了一个JDBC连接。        
Transaction的工厂。          
保存有必须的持久化对象的缓存。            
用于遍历对象，或者通过标识符查找对象；        

持久化对象(Persistent Object)及其集合(Collection) ：    
生命期短促的单线程对象，包含了持久化状态和商业功能。             
普通的`JavaBeas/POJOs`。 
唯一特别的是他们现在从属于一个Session。      
一旦Session被关闭，他们都将从Session中取消联系。             
可以在任何程序层自由使用；比如：直接作为传送到表现层的DTO，数据传输对象；         
临时对象(Transient Object)及其集合(Collection) ：      
目前没有从属于一个Session的持久化类的实例。       
他们可能是刚刚被程序实例化，还没有来得及被持久化，或者是被一个已经关闭的Session所实例化。     
- 事务，Transaction ：    
（可选）单线程，生命期短促的对象，应用程序用它来表示一批工作的原子操作。是底层的JDBC，JTA或者CORBA事务的抽象。   
一个Session某些情况下可能跨越多个Transaction事务。        

- ConnectionProvider ：          
（可选）JDBC连接的工厂和池。             
从底层的Datasource或者DriverManager抽象而来。      
对应用程序不可见。但可以被开发者扩展并且实现。         

- TransactionFactory ：       
（可选）事务实例的工厂。         
对应用程序不可见。但可以被开发者扩展和实现。        


### 配置文件--XML格式
Hibernate同时支持xml格式的配置文件，以及传统的properties文件配置方式；不过在这里建议采用xml形式配置文件。因为这种配置文件提供了更易读的结构和更强的配置能力。可以直接对映射文件加以配置。而在properties文件中则无法配置。必须通过代码中的`Hard Coding`加载相应的映射文件。下面如果不作特别说明，都指的是基于xml格式文件的配置方式。         

配置文件名默认为 `hibernate.cfg.xml`           

Hibernate初始化期间会自动在`CLASSPATH`中寻找这个文件，并读取其中的配置信息。为后期数据库操作做好准备。    
配置文件应部署在`CLASSPATH`中；         
对于Web应用而言，配置文件应放置在 `/WEB-INF/classes` 目录下；     

#### 配置文件中的内容：
首先是，数据库URL，数据库JDBC驱动，数据库用户名和数据库用户密码；     
`hibernate.show_sql`，是否将运行期生成的SQL输出到日志以供调试。     
`hibernate.use_outer_join`   是否使用数据库外连接事务管理类型，这里使用 `JDBC Transaction` 。        

最后是，映射文件配置，注意配置文件名必须包含其相对于根的全路径。          


## Hibernate O/R映射
概述        
理解持久化、ORM           
### 持久化：       
将程序中的数据在瞬时状态和持久状态之间转换的机制。      
### 对象-关系映射：
我们在编写程序的时候，以面向对象的方式处理数据；         
然而，保存数据的时候，却以关系型数据的形式存储到数据库中；     
所以，我们需要一种能在两者间进行数据转换的机制，我们将这种机制称为         
对象-关系映射机制；即：         
能在对象和关系型数据两者进行数据转换的机制。       


### Hibernate基本数据类型：
Hibernate基本数据类型是一个基础的并且非常强大的体系结构元素；       
Hibernate的类型对象将一个Java类型映射到数据库字段的类型。          
然后，持续类所有的持续属性。包括关联，都有一个对应的Hibernate类型；       
这种设计使Hibernate变得极其灵活并易于扩展。                 
下面将Hibernate中的数据类型与Java中的数据类型做个对照：          

Hibernate中的数据类型是非常丰富的；     
Java中的原始类型或者是原始类型封装类以及日期时间类型等等，都可以在Hibernate中找到相对应的数据类型。        
Hibernate也支持自定义的数据类型；          

### 实体映射： 
在Hibernate中的对象关系映射就是把实体类与数据库中的表相对应，实现实体类中的属性与数据库表中的字段一一对应。       
对象和关系数据库之间的映射是用一个XML文档来定义的。           
这个映射文档被设计为易读的，并且可以手动修改。            
映射语言是以Java为中心的。意味着：         
映射是按照持久化类的定义来创建的，而非表的定义。         
**注意**：              
>虽然很多Hibernate用户选择手动定义XML映射文档，但也有一些工具可以生成映射文档；        
>如果我们使用MyEclipse的话，我们可以直接通过数据库中的表生成映射文件和实体类；              
>在实际开发中，大多数公司可能都使用MyEclipse，也有一些公司喜欢用其他的生成工具，这些就要我们在实际工作中去学习了；


映射文件配置 `--Login.hbm.xml`      
```xml
<?xml version="1.0"?>
<!DOCTYPE hibernate-mapping PUBLIC
        "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
        "http://hibernate.sourceforge.net/hibernate-mapping-3.0.dtd">

<hibernate-mapping >
   <class name="com.pb.hibernate.po.Login" table="LOGIN" schema="sjjys">
       <id name="UserName" type="java.lang.String">
           <column name="USERNAME" length="20"/>
           <generator class="assigned"/>
       </id>
        <property name="PassWord" type="java.lang.String">
            <column name="PASSWORD" length="20" not-null="true"/>
        </property>
   </class>
   </hibernate-mapping>
```

这个配置文件中主要包含以下几个元素：     
```
Doctype hibernate-mapping class id property
```
其中id元素中包含column和generator元素。     
property元素中包含column元素。     
下面分别来介绍这些元素：        

- Doctype ：      
所有XML映射都需要定义Doctype；         
也就是，DTD可以从我们指定的URL中获取，或者在`Hibernate3.jar`文件中找到。Hibernate总是会在它的classpath中首先搜索DTD文件。           

- hibernate-mapping ：     
这个元素包括四个常用的可选的属性。 

|参数  |                  描述            |        类型      |      必须|
|----|-----|-----|---|
|schema| 指明这个映射所应用的表所在的schema名称        | Text         |    N|
|default-cascade| 指定了Java会采取什么样的默认级联风格。  |       Text     |        N|
|auto-import| 指定了我们在查询语言中是否可以使用非全限定名的类名 |  Text|             N|                   
|package |指定一个包前缀，如果在映射文档中没有指定全限定名，就使用这个包名|   Text  |           N|
                   
>第一个是schema属性，它指明这个映射所引用的表所在的schema名称；假若指定了这个属性，表名会加上所指定的schema的名字扩展为全限定名。假若没有指定，表名就不会使用全限定名；          
>第二个是 `default-cascade`；它指定了Java会采取什么样的默认级联风格；           
>第三个是`auto-import`属性，它指定了我们在查询语言中是否可以使用非全限定名的类名。      
>第四个是package，它用于指定一个包前缀；如果在映射文档中没有指定全限定名，那么就使用这个包名。            


- Class元素： 
Class元素主要用于定义一个持久化类；       
Class元素中的属性：其中，每一个都是可选的；      

|参数 |               描述            |         类型         |   必须|
|----|------|------|----|
|table | 对应的表名默认值：当前类名 |  Text  |           N|
|dynamic-update | 生成update SQL时，仅包含发生变动的字 段默认值：false |Bool |  N|
|dynamic-insert|生成Insert  SQL时，仅包含非空(空)字段 默认值： false |Bool  | N|
|Proxy|代理类  默认值：空 |         Text|             N|
|discriminator-value|   子类辨别标识，用于多态支持。|   Text|    N|
|where   | 数据甄选条件，如果只需要处理数据库表中某些特定数据的时候，可通过此选项设定结果集限定条件|Text|    N|

>table属性，代表对应的表名，默认值是当前的类名；       
>dynamic-update属性，生成Update SQL语句时，表示仅包含发生变动的字段，它的类型是布尔型的；       
>dynamic-insert属性，它也是布尔型的；生成Insert SQL时，仅包含非空(空)字段     
>Proxy属性，也就是代理的意思；
>如果我们添加属性，为它赋的值也就是它的一个代理类；默认值为空；
>discriminator-value属性，这个属性值代表子类辨别标识，用于多态支持。
>where属性，数据甄选条件，如果只需要处理数据库表中某些特定数据的时候，可通过此选项设定结果集限定条件。


- id元素：
    
|参数   |        描述    |       类型 |      必须|
|------|------|-----|----|
|column |主键字段名 默认值：当前类名    |     Text|           N|
|type|字段类型|Hibernate总是使用对象型数据类型作为字段类型，如int对于Integer，即使这里设置为基本类型，Hibernate内部还是会使用对象类型数据对其进行处理，只是返回数据的时候再转换为基本类型而已。|Text| N|
|length|        字段长度   |       Text |          N|
|unsaved-value|  用于对象是否已经保存的判定值|Text       |    N|
|generator-class | 主键产生方式，取值可为下列值中的任意一个：`assigned  hilo   seqhilo   increment  identity sequence native uuid.hex uuid.string foreigh` |    Text |         Y|

>只有generator-class是必须选的，其他是可选的；

- property元素： 
property元素是用于描述POJO中属性与数据库表字段之间的映射关系的。   
property的主要参数：         
property的所有的参数都是可选的；   

|参数 |         描述       |         类型       |     必须|
|-----|-----------------|--------------------|-----|
|column |    数据库表字段名 默认值：当前类名  |        Text  |           N|
|type  |     字段类型          |      Text      |       N|
|length   |  字段长度     |           Text         |    N|
|not-null  | 字段是否允许为空  |      Bool  |           N |
|unique |    字段是否唯一（是否允许重复值） | Bool |    N|
|insert   | Insert操作时是否包含本字段数据 默认：true |             Bool  |           N|
|update  |  Update操作时是否包含本字段数据 默认： true       |      Bool       |      N|


### 使用Hibernate
概述：     
Hibernate框架是一个开放源代码的对象关系映射框架   
对JDBC进行了轻量级的对象封装；         
可以使用对象编程思维来操纵数据库；            

基础语义：   
也就是Hibernate的核心接口；   
其中包括：      
Configuration，SessionFactory和Session；         

- Configuration类：   
Configuration类负责管理Hibernate的配置信息；        
Hibernate运行时需要获取一些底层实现的基本信息；       
其中几个关键属性包括：        
```
数据库URL   
数据库用户  
数据库用户密码  
数据库JDBC驱动类 
```

数据库dialect 它是用于对特定数据库提供支持，其中包含了针对特定数据库特性的实现；        如Hibernate数据类型到特定数据库数据类型的映射等；        
使用Hibernate必须首先提供这些基础信息以完成初始化工作，为后继操作做好准备；          
这些属性在hibernate配置文件中加以设定；         
当我们调用Hibernate配置时，Hibernate会自动在当前的CLASSPATH中搜寻配置文件；也就是`hibernate.cfg.xml`文件；并将其读取到内存中作为后继操作的基础配置；        
```
Configuration config = new Configuration().configure();
```

Configuration类一般只有在获取SessionFactory时需要涉及；        
当获取SessionFactory之后，由于配置信息已经由Hibernate维护并绑定在返回的SessionFactory之上，因此一般情况下无需再对其进行操作；         
我们也可以指定配置文件名；        
如果不希望使用默认的`hibernate.cfg.xml`文件作为配置文件的话，我们也可以指定一个自定义的配置文件，但一定要是xml格式的；         
```
File file = new File("c:\\sample\\myhibernate.xml");
Configuration config = new Configuration().configure(file);
```

- SessionFactory类    
SessionFactory负责创建Session实例；    
可以通过Configuration实例构建SessionFactory；      
Configuration实例config会根据当前的配置信息，构造SessionFactory实例并返回；SessionFactory一旦构造完毕，即被赋予特定的配置信息。           
也就是说，之后，config的任何变更将不会影响到已经创建的SessionFactory实例；     
如果需要使用基于改动后的config实例的SessionFactory，那么就要使用config重新构建一个SessionFactory实例；         
```
Configuration config = new Configuration().configure();
SessionFactory sessionFactory = config.buildSessionFactory();
```

- Session类：     
Session是持久层操作的基础，相当于JDBC中的Connection；      
Session实例通过SessionFactory实例构建；      
```
SessionFactory sessionFactory = config.buildSessionFactory();
Session session = sessionFactory.openSession();
```

之后我们就可以调用Session所提供的save、find、flush等方法完成持久层操作；代码如下：   

Find :          
```
String hql = "from TUser where name="'Erica'";
List userList = session.find(hql);
```

Save:         
```
TUser user = new TUser();
user.setName("Emma");
session.save(user);
session.flush();
```

上面是Find方法，下面是Save方法；        
在完成Save方法的时候，我们最后要进行flush操作，也就是调用`Session.flush`方法强制数据库同步；    
这里，即强制Hibernate将user实例立即同步到数据库中；       
如果在事务中则不需要flush方法；                   
在事务提交的时候hibernate自动会执行flush方法；            
另外，当Session关闭的时候，也会自动执行flush方法；          


## 事务管理：
Hibernate是JDBC的轻量级封装，本身并不具备事务管理能力；   
在事务管理层，Hibernate将其委托给底层的JDBC或者JTA，以实现事务管理和调度功能；          
Hibernate的默认事务处理机制基于JDBC Transaction；        
我们也可以通过配置文件设定采用JTA作为事务管理实现；        

### 基于JDBC的事务管理：
它是将事务管理委托给JDBC进行处理；这无疑是最简单的实现方式；      
Hibernate对JDBC事务的封装也极为简单；           
如下代码：              
```java
session = sessionFactory.openSession();
Transaction tx = session.beginTransaction();
.....
tx.commit();
```

从这段代码中可以看出，从JDBC层面而言，上面的代码实际上对应的是：     
```java
Connection dbconn = getConnection();
dbconn.setAutoCommit(false);
.....
dbconn.commit();
```

就是这么简单；Hibernate并没有做更多的事情；实际上也没有办法做更多的事情；它只是将这样的JDBC代码进行了封装而已；   
**注意**：        

	在sessionFactory.openSession()中，hibernate会初始化数据库连接；
	与此同时，将其AutoCommit设为关闭状态；也就是 false；
	而其后，在Session.beginTransaction方法中，Hibernate会再次确认
	Connection的AutoCommit属性被设为关闭状态；
	它是为了防止用户代码对session的Connection.AutoCommit属性进行修改；
这也就是说，我们一开始从SessionFactory获得的session，其自动提交属性就已经被关闭。下面的代码将不会对数据库产生任何效果；        
```
session = sessionFactory.openSession();
session.save(user);
session.close();
```

这实际上相当于`JDBC Connection`的AutoCommit属性被设为false；         
执行了若干JDBC操作之后，没有调用commit操作即将Connection关闭；          
如果要使代码真正作用到数据库，我们必须显式的调用Transaction指令；         
代码如下：      
```
session = sessionFactory.openSession();
Transaction tx = session.beginTransaction();
session.save(user);
tx.commit();
session.close();
```

这也就是我们通过Transaction来提交事务；          


### 基于JTA的事务管理
JTA提供了跨Session的事务管理能力；          
这一点是与`JDBC Transaction`最大的差异；             
JDBC事务由Connection管理；也就是说，事务管理实际上是在`JDBC Connection`中实现的；事务周期限于Connection的生命周期； 
同样，对于基于`JDBC Transaction`的Hibernate事务管理机制而言，事务管理在Session所依托的`JDBCConnection`中实现；事务周期限于Session的生命周期；         
而，       
JTA事务管理则由JTA容器实现；         
JTA容器对当前加入事务的众多Connection进行调度，实现其事务性要求；          
JTA的事务周期可横跨多个JDBC Connection生命周期；         
同样对于基于JTA事务的Hibernate而言，JTA事务可横跨多个Session；          
JTA事务是由JTA Container维护，而参与事务的Connection无需对事务管理进行干涉；        
这也就是说，如果采用`JTA Transaction`，我们不应该再调用Hibernate的Transaction功能；       
了解即可；        


## 锁机制
在业务逻辑的实现过程中，往往需要保证数据访问的排他性；       
例如，在金融系统的日终结算处理中，我们希望针对某个`cut-off`时间点的数据进行处理，而不希望在结算进行过程中，数据再发生变化；可能这个过程只有几秒钟，也可能是几个小时；        
此时，我们就需要通过一些机制来保证这些数据在某个操作过程中不会被外界修改；       
这样的机制，在这里，也就是所谓的“锁”。            
即，          
给我们选定的目标数据上锁，使其无法被其他程序修改；        
Hibernate中支持两种锁机制：        
```
悲观锁(Pessimistic Locking)
乐观锁(Optimistic Locking)
```

### 悲观锁(Pessimistic Locking)          
指的是对数据被外界（包括本系统当前的其他事物，以及来自外部系统的事物处理）修改持保守态度；因此，在整个数据处理过程中，将数据处于锁定状态；         
悲观锁的实现往往是依靠数据库提供的锁机制；       
也只有数据库层提供的锁机制才能真正保证数据访问的排他性；           
否则，即使在本系统中实现了加锁机制，也无法保证外部系统不会修改数据；          

- 一个典型的依赖数据库的悲观锁调用：     
```sql
select * from account where name="Erica" for update
```

这条sql语句锁定了account表中所有符合检索条件的记录；        
本次事务提交之前，外界无法修改这些记录；          
Hibernate的悲观锁，也是基于数据库的锁机制实现；            
下面的代码实现了对查询记录的加锁：       
```java
String hqlStr = "from Tuser as user where user.name='Erica'";
Query query = session.createQuery(hqlStr);
query.setLockMode("user",LockMode.UPGRADE);  //加锁
List userList = query.list();   //执行查询，获取数据
```

`query.setLockMode`对查询语句中，特定别名所对应的记录进行了加锁；     
这里也就是对返回的所有user记录进行加锁；             
下面来观察一下运行期Hibernate生成的SQL语句：      
```sql
select tuser0_.id as id,tuser0_.name as name,tuser0_.group_id as group_id,tuser0_.user_type as user_type,tuser0_.sex as sex from t_user tuser0_where (tuser0_.name ='Erica')for update
```

这里Hibernate通过使用数据库的`for update`子句实现了悲观锁机制；    


### Hibernate悲观锁中的几种加锁模式：      
- LockMode.NONE ：无锁机制；      
- LockMode.WRITE :Hibernate在Insert和Update记录的时候会自动获取这个锁机制，也就是可写机制；         
- LockMode.READ :Hibernate在读取记录的时候会自动获取这个机制，也就是可读机制；      

>以上这三种锁机制一般由Hibernate内部使用；   
>例如：Hibernate为了保证Update过程中对象不会被外界修改，会在save方法实现中自动为目标对象加上WRITE锁；     

- LockMode.UPGRADE :利用数据库的for update子句加锁；   
- LockMode.UPGRADE_NOWAIT :Oracle的特定实现，利用Oracle的`for update nowait`子句实现加锁；      

上面这两种锁机制是我们在应用层较为常用的；     

加锁一般通过以下方式实现：        
```
Criteria.setLockMode
Query.setLockMode
Session.lock
```

**注意**：         
	只有在查询开始之前，设定加锁，才会真正通过数据库的锁机制进行加锁处理；否则，数据已经通过不包含`for update`子句的Select SQL加载进来；
	所谓的数据库加锁也就无从谈起；


### 乐观锁 (Optimistic Locking)
相对于悲观锁而言，乐观锁机制采取了更加宽松的加锁机制；       
悲观锁大多数情况下依靠数据库的锁机制实现；以保证操作最大程度的独占性；随之而来的就是数据库性能的大量开销；特别是对长事务而言，这样的开销往往无法承受；       
如一个金融系统，当某个操作员读取用户的数据，并在读出的用户数据的基础上进行修改时，如果采用悲观锁机制，也就意味着整个操作过程中，数据库记录始终处于加锁状态；可以想见，如果面对成百上千个并发，这样的情况将导致怎样的后果；     
乐观锁机制在一定程度上解决了这个问题；       
乐观锁，大多是基于数据版本记录机制实现；          
##### 什么是数据版本：         
就是为数据增加一个版本标识；       
在基于数据库表的版本解决方案中，一般是通过为数据库表增加一个"version"字段来实现；         
读取出数据时，将此版本号一同读出；           
之后更新时，对此版本号加一；         
此时，将提交数据的版本数据与数据库表对应记录的当前版本信息进行比对；           
如果提交的数据版本号大于数据库表当前版本号，则予以更新；否则，认为是过期数据；               

乐观锁机制避免了长事务中的数据库加锁开销，大大提升了大并发量下的系统整体性能表现；       
但是，需要注意的是：            
乐观锁机制往往基于系统中的数据存储逻辑，因此也具备一定的局限性；            
所以，我们在系统设计阶段应该充分考虑到各种情况出现的可能性；            
例如，将乐观锁策略在数据库的存储过程中实现；             
对外只开放基于此存储过程的数据更新途径，而不是将数据库表直接对外公开；             
Hibernate，在其数据访问引擎中内置了乐观锁实现，如果不用考虑外部系统对数据库的更新操作；我们利用Hibernate提供的透明化乐观锁实现，将大大提升我们的生产力；          
