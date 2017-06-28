#   概述
Spring作为一个优秀的开发框架，本身也提供了JDBC以及事务的管理；    
同时，本节课中将讲解如何将Spring与Struts2、Hibernate整合；    
最后，我们将讲解Spring中如何配置定时任务；         


## Spring持久层
概述     
首先，看一下，Spring对持久层是怎么支持的：    
持久层就是对数据库操作的业务层；     
Spring提供了DAO框架，让开发人员无须耦合特定数据库技术，就能进行应用程序的开发；    

Spring提供了DAO框架，让开发人员无需知道具体使用什么数据库；     
Spring封装好了操作`Oracle、MySql、DB2、SQL Server`等数据库的用法；        
它们都是实现同一个接口，方法也是一样；     
开发人员无需操心使用哪个数据库，无需耦合特定数据库技术，就能进行程序的开发；    

DAO全名 : `Data Access Object`   即：访问数据的对象    


那么，Spring对DAO是如何开发的呢？      
首先，要设计一个DAO的接口；     
然后，再设计出一个实现了DAO接口的一个实现类；   
把这个类通过Spring的依赖注入，注入到配置文件中；    
然后，通过接口获得当前被注入的实现了DAO接口的Bean，从而达到操作数据库的目的；     
示例：    
```java
//DAO接口   
UserDao
public void insert(User user);
public User find(Long id);
public void update(User user);
public void delete(User user);
//DAO实现类
UserDaoImpl
public void insert(User user){...}
public User fine(Long id){...}
public void update(User user){...}
public void delete(User user){...}
```
首先，设计一个DAO的接口：UserDao，包含增删改查方法；   
它的实现类UserDaoImpl实现了UserDao的这些方法；    
还习惯将接口声明为IUserDao；    
开发中命名最好能一目了然，区分接口和实现类；     

如何通过Spring配置文件获得UserDao接口的实现类呢？     
如下代码：      
```java
...
User user = new User();
//通过Spring配置文件获得UserDao实现类
UserDao userDao = getUserDao();
userdao.insert(user);
```
这里的`getUserDao()`为简写，应该调用`Context.getBean()`获得具体的DAO，之后通过UserDao对象直接操作；此时，UserDao对象实际上是UserDaoImpl对象；    


Spring对DAO对象的支持优势在哪里？       
   
由于依赖于接口，可以通过依赖注入随时替换UserDao接口的实现类，而应用程序完全不用了解接口与底层数据库操作的细节；                
   
由于业务依赖于接口，接口的实现类是通过Spring的配置文件，即IOC注入进来的，那么对于业务代码来说，它只能看到当前接口，这时候对于数据库操作底层的细节，用户是看不到的；                   
UserDao实现的这个类并没有在代码中明确指出，而通过Spring的配置文件获得了当前实现类的一个Bean；通过这种方式实例化接口的；此时是看不到UserDao的源代码的，只能知道高层的接口；实现了用户的业务逻辑与底层实现的解耦；                 
系统的数据对象对用户是透明的；               

首先，应用程序调用DAO接口；                                 
然后，DAO的实现类实现了DAO接口后，去真正操作数据库；                     
应用程序和数据库之间多了一个DAO接口；    
而，应用程序不与DAO接口的实现类直接联系，而是与DAO接口联系；   
这时候，应用程序就和DAO实现类解耦了；      



## 注入数据源
DataSource (数据源)    
使用Spring持久层，必须知道数据库在哪里、从哪里连数据库；   
此时就通过注入数据源的方式来注入数据库了；    
连接数据的方式称为 `数据源`。比如JDBC、连接池或者JNDI。     
Spring最大的优势有两个： IOC、AOP；       
这里，Spring通过依赖注入方式配置数据源 ；   
不同系统，数据源的管理更多是针对底层的行为，这些行为不应该影响业务。    
比如银行的转账，数据库操作不应该影响转账；     
底层和上层不该相互影响；     
通过Spring依赖注入方式配置数据源的好处为：    
更换数据源只需要修改数据源Bean定义的内容，而不需要修改任何一行代码。    
和JDBC不同，JDBC还要修改代码，这里不用修改任何代码；               
修改配置文件就可以更换数据库了；      


#### 配置文件中是怎样注入数据源的？
如下代码：   
```
<beans...>
   <bean id="dataSource"
        class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName">
		  <value>oracle.jdbc.driver.OracleDriver</value>
	</property>
	<property name="url">
		  <value>jdbc:oracle:thin:@10.0.0.11:1521:ORCL</value>
	</property>
	<property name="username">
		  <value>demo</value>
	</property>
	<property name="password">
		  <value>demo</value>
	</property>
</bean>
....
```
需要一个自定义标签 `<bean id="dataSource">`，类是DriverManagerDataSource，包在Spring中，这个类是Spring自带的；这里面使用的数据源就是 DriverManagerDataSource；     
指定驱动名称为Oracle以及url路径；用户名和密码均为demo；       


Spring使用持久层时，一定要设计一个DAO接口；      
这里设计一个PersonDao接口；        
- PersonDao接口：            
```java
 public interface PersonDao {
   public Person find(Long id);
}
```
它只有一个方法，根据id查找Person对象；     

- PersonDao实现类PersonDaoImpl  :    
```java
public class  PersonDaoImpl implements PersonDao {
 private DataSource dataSource;
 public DataSource getDataSource() {
    return dataSource;
 }
 //用来注入dataSource
 public void setDataSource(DataSource dataSource) {
    this.dataSource = dataSource;
 }
 public Person find(Long id) {....}
}
```
它有一个私有成员变量DataSource；    
注意：Spring在依赖注入的时候，如何给Bean中的属性赋值？    
当前这个Bean的属性有个Set方法；    
Spring就是通过查找Set方法来为Spring中的属性赋值的；     
Spring通过`setDataSource()`方法给dataSource属性赋值；      


PersonDaoImpl实现的find核心内容    
Person中find方法的细节：     
```
public Person find(Long id) {
  Connection conn = null;
  PreparedStatement pstmt = null;
  //获取连接，剩余操作与JDBC一样
  conn = dataSource.getConnection();
  pstmt = conn.prepareStatement("select * frome person where id= ?");
  pstmt = seltLong(1,id);
  ResultSet rs = pstmt.executeQuery();
  if(rs.next()) {
        Long p_id = rs.getLong("id");
	String name = rs.getString("name");
	int age = rs.getInt("age");
	Person p = new Person();
	p.setId(p_id);
        p.setName(name);
	p.setAge(age);
	return p;
     }
  
}
```
这里使用的是JDBC方式获得连接，不用指定驱动；    
因为我们已经注入了数据源了；    
怎么获得数据源呢？    
```
Connection conn = dataSource.getConnection();
```
后面与使用JDBC的方式是一样的；   
从person表中找出所需数据；    
之后，为` ?`号占位符赋值；然后，执行查询语言；   
之后，从ResultSet对象中读取数据，并返回Person对象；    


### 配置 PersonDaoBean
```
<bean id="personDao" class="demo.spring.jdbc.datasource.PersonDaoImpl">
    <property name="dataSource" ref="dataSource"/>
</bean>
```
`id="personDao"`,class就是PersonDaoImpl；       
property中的ref属性值为刚才配的数据源dataSource；     


####  JDBCTemplate   
为什么需要JDBCTemplate (JDBC模板)?     
使用JDBC进行开发时，总是需要进行固定的步骤，比如：Connection、Statement的获取、关闭、异常处理等等，牵扯精力。
##### JDBCTemplate作用：   
Spring将JDBC常用的操作封装到JDBCTemplate中，简化了使用JDBC的开发流程。      

##### 使用JDBC的步骤：
首先要设置驱动名` Class.forName()`;       
然后，通过 DriverManager 获得 Connection；   
使用 PreparedStatement获得ResultSet结果集；      
此时，至少需要固定的三步：指定驱动名、获取连接、开启Statement；       
而且，我们在使用完JDBC后还需要关闭结果集；之后，关闭Statement，最后关闭连接；     
这里其中有六步是重复的；当我们使用频繁，重复写很麻烦；            
此时，Spring为了方便开发人员使用，避免过多重复劳动；             
聪明人想到了把JDBC封装到Spring中；                             
Spring将JDBC常用的操作封装到JDBCTemplate中；                         
此时，使用JDBCTemplate不用关心获得连接和关闭连接；JDBCTemplate全部搞定了；              
你只需要关注于执行SQL语句以及如何获得所要的结果即可；                
大大提高了开发效率，简化了使用JDBC的开发流程；                            


##### JDBCTemplate的用法
使用JDBCTemplate开发的PersonDaoImpl :     
```java
private JdbcTemplate jdbcTemplate;
//获得jdbc模板
public void setDataSource(DataSource dataSource) {
  jdbcTemplate = new JdbcTemplate(dataSource);
 }
public Person find(Long id) {
 List results = jdbcTemplate.queryForList("select * from person where id="+id.longValue());
 Iterator it = results.iterator();
 if(it.hasNext()) {
   Map personMap = (Map) it.next();
   Long p_id = ((BigDecimal)PersonMap.get("ID")).longValue();
   String name = (String)personMap.get("NAME"));
   int age = ((BigDecimal) personMap.get("AGE")).intValue();

   Person p = new Person();
   P.setId(P_id);
   p.setName(name);
   p.setAge(age);
   return p;
 }
}
```
首先，要有一个类PersonDaoImpl类；    
有一个私有成员jdbcTemplate，然后使用Spring IOC注入的时候的一种另类用法；   
这里有个setDataSource方法，它将为我们数据源注入；    
注意：Spring只找Set方法，不会看有没有当前的属性；    
这里，我们通过Bean注入后，把DataSource注入到setDataSource方法中；      
之后，通过`setDataSource()`方法中的数据源参数初始化jdbcTemplate；    
将数据源作为参数传递到`setDataSource()`中初始化jdbcTemplate，而不能用数据源直接给jdbcTemplate赋值；     
特别注意：        
Spring中属性赋值时，与你当前是否有一个数据源成员无关，而是与当前是否有一个Set数据源方法有关；      
注意：              
jdbcTemplate返回之后，将Person对象放到一个列表中；              
列表保存的是一个Map集合，而Map中保存的是当前的键；                        
Person中有`ID，NAME，AGE`三个属性，那么Map中就又分别表示三个属性`ID，NAME，AGE`；           
这是 `jdbcTemplate.queryForList()`时，它里面元素的特点，包含的是一个Map；     


只需要将Bean配置中的class属性修改，而不需要修改测试代码；                          
这就是IOC的强大之处；                           


#### JDBCTemplate操作数据库          

- 执行DDL与更新：
如果想要执行DDL语句的时候，使用JdbcTemplate的`execute()`方法可以执行DDL语句；   
执行update或者insert，可以使用`update()`方法；       
实例：          
执行DDL :       
```
template.execute("create table test(test VARCHAR2(255 CHAR))");
```
这句代码就是创建一个test的表，它包括一个test的字段；    
这时就可以通过 `template.execute`这个语句来执行DDL语句；     
更新与插入:    
```java
//更新
template.update("update person set name='老人' where age > 23");
//插入
template.update("insert into person values(12,'张三',24)");
```
更新和插入分别调用`update()`方法，执行的SQL语句是不同的；   


使用 ` ?`  占位符进行操作     
有时，我们无法明确在某一时刻给出当前要进行插入、更新操作时的参数；         
这时，可以使用  ` ? ` 占位符来进行操作；    
实例：         
```java
public void insert(Person person) {
//参数1:带有 '?'占位符的sql语句
jdbcTemplate.update("insert into person values(?,?,?)",
//参数2:传递参数的对象数组
new Object[] {person.getName(),person.getAge(),person.getId() });
}
```

`jdbcTemplate.update("insert into person values(?,?,?)"`,      
这时不知道要传递的参数，通过后面的对象数组来传递参数：              
```
new Object[] {person.getName(),person.getAge(),person.getId() }
```
person就是Person类的对象；     

这时，jdbcTemplate的update方法有两个参数：              
- 第一个是带有 `?`占位符的预处理sql语句； 
- 第二个是传递参数的对象数组；       


#### 使用JdbcTemplate执行查询
使用JdbcTemplate进行查询时，可以使用`queryForXXX()`方法。         
提示：XXX表示执行查询语句后返回的数据类型；        
 

实例：     
返回单条数据：          
```
int count = jdbcTemplate.queryForInt("select count(*) from person");
```
计算person表中数据的总数，它返回一个整数，使用`queryForInt()`；      
这是单条数据的操作；    

##### 返回多条数据：
返回多条数据用 `queryForList()` ：
```
List results = jdbcTemplate.queryForList("select * from person");
```
把person表中所有数据放到List对象中；    


#####  使用JdbcTemplate进行批量更新操作
BatchPreparedStatementSetter接口可以将集合对象中的值与 `?`占位符参数对应，从而完成批量操作      
实例：          
```java
public static int count = 2000;

//设置插入内容列表
final List id = new ArrayList();
final List username = new ArrayList();
final List password = new ArrayList();

//设置插入内容
for(int i=0;i<count;i++){
  id.add(+i);
  username.add("username_"+i);
  password.add("password_"+i);
 }

 //预处理语句
 String batch_update = "insert into users values(?,?,?)";

//设置批量预处理操作类
BatchPreparedStatementSetter bps = new BatchPreparedStatementSetter() {
 @Override
 public void setValues(PrepareStatement ps,int index) throws SQLException {
   ps.setInt(1,(Integer) id.get(index));
   ps.setString(2,(String) username.get(index));
   ps.setString(3,(String) password.get(index));

  }
  @Override
  public int getBatchSize() {
      return count;
  }
}

//执行批量处理
jt.batchUpdate(batch_update,bps);
```


##### JdbcTemplate以对象方式操作数据库
刚才内容都是让JdbcTemplate使用SQL语句进行操作；     
jdbcTemplate也可以操作对象，以对象方式进行数据库操作；      
用对象方式操作的好处？                
可以使底层数据库操作进一步封装，开发人员甚至不用接触到Sql语法；                   
可以通过对象的方式提供给开发人员使用；               
某些场合下，开发流程更加简化而高效；                 
对于数据的性能和安全要求不是很高的前提下；                    
比如：一个表，有上千上亿条数据，此时需要优化的SQL、存储过程；               
这种情况下，用对象方式可能解决不了；                  
使用对象方式，只能将对象与表中某部分进行映射，或对象中封装一些操作语句；                     
这些语句在某些场合不高效，但大多数情况使用对象方式可以简化开发流程。                  


#### 使用对象方式主要需要两个类：
- RdbmsOperation抽象类：       
	- 它的子类实现了查询、更新等操作；    
	- 它的子类都是线程安全的，可以用于分布式环境；    

- SqlFunction :    
可以用来执行sql函数；      
例如：` select count(*) from user  `就是sql函数；    



##### 使用SqlFunction和RebmsOperation子类的使用过程：

一般为这两个类的子类设置`DataSource、Sql`以及相关参数；    
然后，执行`compile()`；保证当前操作可以被运行；        


一个继承SqlFunction的PersonFunction类，用于算出有多少条数据：   
```java
public class PersonFunction extends SqlFunction {
  public PersonFunction(DataSource ds) {
    super(ds,"select count(*) from person");
         complie();
    }
}
```
为PersonFunction类添加构造，带有一个数据源参数；      
通过`super()`把数据源和带有函数的SQL语句作为参数；          
最后，`compile()`; 编译并运行；     


**注意**：    
第一：       
一定要有数据 源；      

第二：      
有SQL语句；     

第三：    
有compile语句；     

如果有参数，还需要配置参数；    


##### 继承SqlUpdate的PersonUpdate类： 
```java
public class PersonUpdate extends SqlUpdate {
  public PersonUpdate(DataSource ds) {
  super(ds,"insert into person values(?,?,?)");
  int[] types = {Types.VARCHAR,Types.INTEGER,Types.BIGINT};
  setTypes(types);
  compile();
  }
}
```
和刚才的PersonFunction类类似，但是，PersonUpdate类已经有参数了；    
```
  super(ds,"insert into person values(?,?,?)");
```
这时候，参数如何传呢？            
当前，我们使用这个类时，这个类应该是一个通用的、封装的类；             
它没法指定具体的参数；                        
所以，这里我们不能指定具体的参数，但是我们可以指定参数的类型；         
```java
  int[] types = {Types.VARCHAR,Types.INTEGER,Types.BIGINT};
```   

之后，`setTypes(types)`；把类型设置进来，分别对应了三个 `?`的位置；        
最后，不要忘了compile语句，让SQL语句可以执行；           


##### 查询：
查询要与对象进行映射，查询之后应该返回对象；    
这里，需要继承MappingSqlQuery类；    

继承MappingSqlQuery的PersonQuery类:     
```java
public class PersonQuery extends MappingSqlQuery {
  public PersonQuery(DataSource ds) {
        super(ds,"select * from person");
        compile();
    }
  //将结果集与指定对象的属性对应
  protected Object mapRow(ResultSet rs,int rowNum) throws SQLException {
   Person p = new Person();
    p.setId(rs.getLong("id"));
    p.setName(rs.getString("name"));
    p.setAge(rs.getInt("age"));
      return p;
  }
}
```

将结果集与指定对象的属性对应，通过`mapRow()`方法；    
```java
   Person p = new Person();  //p为一个Person对象；
   p.setId(rs.getLong("id"));
   p.setName(rs.getString("name"));
   p.setAge(rs.getInt("age"));
```
p的id等于结果集中的id；p的name等于结果集中的name；age等于结果集中的age；      
这个就是Spring中使用封装对象；                   
这就是jdbc通过对象方式进行查询时，如何与对象进行对应；                         
Hibernate中查找也是这样的步骤；                         
它也是将Person中的属性用这样类似的写法与结果集中每列对应；                        
使用Spring时，若当前列顺序不一样；对应不上；是因为早期版本使用1、2、3来对应；                       
后来则用列名来对应；                             

**注意**： 
我们实现了`mapRow()`方法，将Person的对象p与我们的sql语句返回结果集的各个字段进行了对应；         



##### 实现新的PersonDao接口
修改PersonDao接口：     
```java
public interface PersonDao {
 public List find();
 public void insert(Person person);
 public int getPersonCount();
}
```

把`find()`方法改成没有参数的，返回结果集；       
`insert()` : 插入一个对象，之后要返回person这个表中的所有数据的数目；     

实现类：               
```java
public class JDBCTemplatePersonDaoImpl implements PersonDao {
 private SqlUpdate personUpdate;
 private SqlQuery personQuery;
 private SqlFunction personFunction;
 public void setDataSource(DataSource dataSource) {
	personUpdate = new PersonUpdate(dataSource);
	personQuery = new PersonQuery(dataSource);
	personFunction = new PersonFunction(dataSource);
 }
 public List find() {
   return personQuery.execute();
 }
 public int getPersonCount() {
     return personFunction.run();
 }
 public void insert(Person person) {
  personUpdate.update(new  Object[]{person.getName(),person.getAge(),person.getId()});
 }
}
```
我们通过setDataSource初始化了相应的实现了三个类的对象；这三个类就是在上面实现的三个类；       
然后，`find()`方法返回的是`personQuery.execute()`方法；          
`getPersonCount()`方法返回`personFunction.run()`方法；                         
`insert()`方法调用`personUpdate.update()`方法并传入3个参数：Person的`id、name、age`             

配置Spring配置文件     
```xml
<bean id="objectDao" class="demo.spring.jdbc.objec.JDBCTemplatePersonDaoImpl">
   <property name="dataSource" ref="dataSource"/>
</bean>
```


## Spring与其它框架的整合   
我们已经学习过Struts2、Hibernate和Spring；    
在我们开发的时候，现在有很多公司都习惯使用SSH这种开发方式；    
下面来讲解Spring如何与Struts2和Hibernate整合；        

### Spring整合Struts2 :    
整合过程步骤如下：    
第一：            
导入Struts相关jar包，并导入 `struts2-spring-plugin-2.0.1.1.2.jar`

第二：         
配置 `web.xml`；       

第三：     
配置spring配置文件；

第四：    
配置struts配置文件；

第五：    
Struts的Action类需要继承spring提供的ActionSupport。                
就可以保证Struts的Action可以使用了；          

需要修改配置文件的地方并不多，接下来注意看；                 



#### 配置 web.xml文件:
首先，配置一个Struts2的Filter(过滤器);             
同平时的配置一样；                   
配置Spring的配置文件在哪里，上下文监听器；        
```java
.....
  <filter>
        <filter-name>struts2</filter-name>
        <filter-class>org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter</filter-class>
    </filter>

    <filter-mapping>
        <filter-name>struts2</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
.......

//配置Spring
...
<context-param>
 <param-name>contextConfigLocation</param-name>
 <param-value>/WEB-INF/classes/applicationContext.xml</param-value>
</context-param>
<listener>
 <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
....
```


#### 配置Spring配置文件
其实，Spring配置文件中最主要的功能是用来配置Struts2中的Action；           
之后，Spring配置文件中Action的ID需要被Struts配置文件调用；                    

例如：          
```xml
<bean id="loginAction" class="demo.struts.LoginAction">
   <property name="userDao">
         <ref local="userDao"/>
   </property>
</bean>
```
这里的loginAction，它是一个Struts2项目中的Action类；            
然后，需要把userDao注入进来；         


#### 配置struts配置文件
接下来看一下Struts配置文件，它主要需要修改两个地方；                      
- 在Struts配置文件中，添加一个 constant标签，内容如下：            
```
<constant name="struts.objectFactory" value="spring" />
```
这句代码的意思是，当前Action的对象工厂创建Action是通过Spring方式来实现的；            
也就是说，当我们想创建Action类的时候，需要和Spring联合；                     
看看Spring中是否配置了我们需要的Action；                     

- 调用Spring配置文件中配置的Action；        
其中，class属性的值与Spring配置文件中Action的Bean Id相同；               

我们的Struts中配置Action的时候，class属性应指定包名和类名；                          
但此次例外；                         
当和Spring整合之后， `name="login"` 不变，后面的result配置也不变；               
唯一改变的是 class属性；               
class属性不再指定包名 加 类名；                        
而是，指定Spring配置文件中对应当前Action的Bean的id；                 
这时候通过这一步，Struts2的action和Spring的action就结合起来了；                   
```xml
<action name="login" class="loginAction">
    <result name="success">success.jsp</result>
    <result name="fail">fail.jsp</result>
</action>
```

Struts原有Action需要继承ActionSupport           
需要将Struts原有Action继承ActionSupport，剩下的action中内容不变，仅仅是多继承了ActionSupport；                

原有Action  :              
```java
 public class LoginAction {
   .....
 }
```

修改后的Action :        
```java
 public class LoginAction extends ActionSupport {
   ....
}
```

#### Spring整合Hibernate
步骤：        
第一步：                    
导入Spring与Hibernate整合相关jar包；          
          
第二步：                   
在Spring配置文件中注入SessionFactory；           

第三步：                   
删除Hibernate配置文件；         


##### 如何在Spring中注入SessionFactory：

Spring配置数据源后，可以通过配置`org.springframework.orm.hibernate3.LocalSessionFactoryBean`来配置Session工厂，从而完成整合；                 

如下代码：          
```xml
<bean id="dataSource"...>
</bean>
<!--注入SessionFactory-->
<bean id="sessionFactory"
  class="org.springframework.orm.hibernate3.LocalSessionFactoryBean">
	<property name="dataSource">
	<!--连接数据源-->
	<ref bean="dataSource"/>
	</property>
	<!--配置数据库方言-->
	<property name="hibernateProperties">
            <props>
	    <prop key="hibernate.dialect">
	  org.hibernate.dialect.Oracle9Dialect
	    </props>
	</property>
	<!--配置ORM映射文件-->
	<property name="mappingResources">
	   <list>
	      <value>demo/entity/User.hbm.xml</value>
	   </list>
	</property>
</bean>
```

SessionFactory就是Spring和Hibernate整合的关键；         
关键的核心就是要给SessionFactory注入一个合适的数据源；             
那么这时候，这个数据源跟之前的JDBCTemplate数据源是一样的；        
```
<bean id="sessionFactory">
```
它的类就是`org.springframework.orm.hibernate3.LocalSessionFactoryBean`
属性dataSource，指到本地的dataSource数据源；              
这一步写完后就完成了Spring和Hibernate的整合；                     
之后，再把对象关系的映射文件映射一下就可以了；                      



#### Spring的事务管理
Spring对事务的支持             
在数据库中有一个非常重要的管理就是事务管理；         
事务是一组原子操作的工作单元；                 
它里面的操作要么都成功，要么都失败；                         
以转账为例：                
如果钱转出去了，而对方没有收到，那么钱应该退回你的账户中；             
而，转账成功的话，钱会转到对方账户；                       
在这个过程中，任意操作失败都会回到原始状态；                      


#### Spring管理事务方式
Spring管理事务方式有两种：              
一个是JDBC编程事务管理，一个是JDBC声明事务管理；            
##### JDBC编程事务管理：
可以清楚的控制事务的边界，事务控制粒度化细(编程的方式)；        
##### JDBC声明事务管理：         
事务相关API不用介于程序之中，将事务管理与实际业务代码解耦(配置xml的方式)；但是它对事务边界的控制就不能那么精确了；            

编程事务管理就是在应用程序代码中进行编码事务控制；             
声明事务管理是通过配置方式(配置XML文件)；                         

编程事务管理可以到指定代码中具体的某一行；             
声明事务管理仅仅能定义到方法上；         


##### JDBC编程事务管理
Spring提供两种方式实现编程式的事务管理；      
- 一种是：            
实现 PlatformTransactionManager 接口；           

- 一种是:
使用事务模板 TransactionTemplate            

#### 使用 PlatformTransactionManger 管理事务大致流程：
- 首先，指定 PlatformTransactionManger 的实现类；                     
- 其次，定义事务属性 TransactionDefinition；              
- 之后，将事务定义传送给 TransactionStatus；            
- 之后，将欲进行的事务用 `try....catch`语句封起来；         
如果事务出错，调用PlatformTransactionManger 的rollback方法，回滚。将状态回滚到之前的状态；          


#### 使用TransactionTemplate
需要封装一个 TransactionManager  
创建事务回滚类            
执行TransactionManager的 `execute()`方法；        

它的使用比之前的PlatformTransactionManager方式要简单，因为它是一个事务模板，类似JDBCTemplate；       
提供一个模板化的事务管理方式，封装事务管理大部分功能；           


#### 回顾：什么是AOP       
AOP不会侵入开发代码，从而保证在不影响业务逻辑的前提下执行系统功能(日志、统计、事务管理等)；           

声明式的业务代码主要是通过Spring的AOP来实现的；            


#### JDBC声明事务管理 
Spring声明式事务管理依赖它的AOP框架来完成；            

- 第一个声明事务管理   
不需要修改原有DAO；     
使用 TransactionProxyFactoryBean指定要介入的事务以及其方法；     


配置TransactionProxyFactoryBean            
```xml
<!--指定TransactionProxyFactoryBean -->
<bean id="personDaoProxy"
  class="org.springframework.transaction.interceptor.TransactionProxyFactoryBean">
 <property name="proxyInterfaces">
   <list>
      <!-- 指定介入的DAO接口 -->
      <value>demo.dao.PersonDAO</value>
   </list>
 </property>
 <!--指定目标DAOBean -->
 <property name="target" ref="personDao"></property>
 <!-- 注入事务管理-->
 <property name="transactionManager" ref="transactionManager"/>
 <!-- 指定要管理的方法以及管理方法 -->
 <property name="transactionAttributes">
   <props>
     <!-- insert*表示所有以insert开头的方法 -->
     <!-- PROPAGATION_REQUIRED 表示在目前的事务中执行操作，
  如果事务不存在，就建立一个新的 -->
	   <prop key="batch*">PROPAGATION_REQUIRED</prop>
   </props>
 </property>
</bean>
```

Spring配置文件就是这样的；         
首先，指定TransactionProxyFactoryBean；       
指定bean的id是什么；之后，指定介入的DAO接口；          
然后，指定目标DAOBean，我们引入了personDao；            
这个personDao是当前PersonDao接口的实现类的bean；        
然后，注入事务管理器：transactionManager；            
之后，我们要指定将要管理的方法以及管理方式；                  
`batch*` 表示所有以batch开头的方法都要进行事务管理；              


#### 事务的属性      
接下来，我们来看在Spring中事务属性是怎么定义的；         

声明式事务以方法为边界；             
简单的说，一个方法内的全部操作可以看成一个事务；          
Spring中事务属性，可以分为四类：                    
```
传播行为 (Propagation Behavior)
隔离级别 (Isolation Level)
只读提示 (Read-only Hints)
事务超时期限 (The Trasaction timeout period)
```

只读提示指信息和数据是只读的；         
事务超时期限指当前事务执行的超时限制，超时则进行事务回滚；              


|传播行为                             |  说明|
|-----|-----|
|PROPAGATION_MANDATORY|方法必须在一个现存的事务中，否则丢出异常；|
|PROPAGATION_NESTED|在一个嵌入的事务中进行，如果不是，则同PROPAGATION_REQUIRED|
|PROPAGATION_NEVER|指出不应在事务中进行，如果有就丢出异常|
|PROPAGATION_NOT_SUPPORTED|指出不应在事务中进行，如果有就暂停现存的事务|
|PROPAGATION_RRQUIRED|支持现在的事务，如果没有就建立一个新的事务；|
|PROPAGATION_REQUIRES_NEW|建立一个新的事务，如果现存一个事务就暂停它|
|PROPAGATION_SUPPORTS|支持现在的事务，如果没有就以非事务的方式进行|


#### 隔离级别 (Isolation Level)
由于事务彼此独立，所以会造成如下问题：       
脏数据、非重复读、幻读           
脏数据就是读取数据的时候有一部分数据没有及时更新到库中；造成这部分数据没有更新，成脏数据了；          
非重复读就是在一个事务中两次读同一张表，但是出现的结果不一样；             
幻读就是在一个事务中读出一个表的内容与实际内容不符；             

|隔离级别                       |   说明|
|----|-----|
|ISOLATION_DEFAULT|使用底层数据库预设的隔离级别|
|ISOLATION_READ_COMMITTED|允许事务读取其它并行的事务已经送出的数据字段，可以防止脏读取问题|
|ISOLATION_READ_UNCOMMITTED|允许事务读取其它并行的事务还没送出的数据，会发生脏读取、非重复读、幻读等问题|
|ISOLATION_REPEATABLE_READ|要求多次读取的数据必须相同，除非事务本身更新数据，可防止脏读取、非重复读问题|
|ISOLATION_SERIALIZABLE|完整的隔离层次，可防止脏读取、非重复读、幻读等问题，会锁定相应的数据表格，导致使用此级别的应用效率降低|

根据自己项目中实际的需求来选择实际的具体的隔离级别


Spring2.0声明式事务管理中两种简化方式：          

Spring2.0声明式事务管理：基于`XML Schema`           
基于`XML Schema`文件进行简化；                      
Spring2.0中，简化了声明式事务的配置方式，可以依赖于<aop>与<tx>标签；            
步骤：                  
加入相关xml命名空间；        
使用`<tx:advice>`标签指定Advice；        
使用`<aop:config>`标签配置事务管理；              


Spring2.0声明式事务管理：基于注解                  
主要使用 `@Transactional` 来标示；         


#### Spring任务调度

计划任务             
我们常需要定时执行一些计划（定时更新等），这样的计划称之为计划任务；              
什么是任务调度：                      
一般来说，任务调度就是定时的执行一些计划任务；           
我们将这种需要定时执行的任务称为计划任务；              
比如说，有时，我们需要定时更新一张表、处理一些文件；                    

Spring抽象封装了Java提供的Timer与TimerTask类；               
也可以使用拥有更多计划功能的Quartz；                    
 
那么，Timer和Quartz最主要的区别就是，            
Timer只能指定每隔多少时间执行一次；                       
Quartz可以指定到具体的某个时间点去执行任务；                       


##### 使用TimerTask进行任务调度：
继承TimerTask的类          
使用TimerTask进行计划任务开发的步骤有这么几个：         
- 第一：
定义任务类继承TimerTask，并实现run方法； 

- 第二：
在Spring配置文件中，使用          
```
org.springframework.scheduling.timer.ScheduledTimerTask
```
来定义任务的执行周期；           

具体代码：        
```java
public class BatchUpdate extends TimerTask {
 private List update_command;
 public List getUpdate_command() {
      return update_command;
  }
 public void setUpdate_command(List update_command) {
      this.update_command = update_command;
   }
 public void run() {
   for (Iterator it = update_command.iterator();it.hasNext();){
          System.out.println(it.next());
    }
     System.out.println("更新操作执行完毕");
    }
 }
```
这里定义了一个BatchUpdate，它是一个批量更新的操作类；                 
它继承了TimerTask，之后实现一个`run()`方法；                     
`run()`方法中进行更新操作；            

然后，配置Bean；                       
配置任务Bean与TimerTask实现类；     
```xml
<bean id="update" class="demo.Task.timerTask.BatchUpdate">
  <property name="update_command">
    <list>
       <value>update Person set duty='考勤' where date=$(on)</value>
       <value>update Person set duty='缺勤' where date=$(no_info)</value>
       <value>update Person set duty='迟到' where date=$(late)</value>
    </list>
   </property>
</bean>
 <bean id="scheduleTask" class="org.springframework.scheduling.timer.ScheduledTimerTask">
   <!-设置第一任务要在1秒后执行-->
   <property name="delay">
      <value>1000</value>
   </property>
   <!-设置任务从启动后，每隔2秒执行一次-->
   <property name="period">
       <value>2000</value>
   </property>
   <property name="timerTask">
        <!--封装继承TimerTask类的任务-->
        <ref local="update"/>
   </property>
 </bean>
```
首先，我们有三条语句，分别是考勤、缺勤、迟到语句；              
之后，我们设置了ScheduledTimerTaskBean的实例；                
`bean id` 是 scheduleTask，里面有个迭代；                 
迭代的意思是，当任务启动后，                           
第一次任务要在1秒后执行，之后还有period；           
period这个时期就说明，当第一次任务启动后，每隔2秒执行一次；            
然后，`<property name="timerTask">`            
它指定一个具体实现了继承TimerTask接口的`Bean：update`；           


#### 配置启动任务
任务创建并配置好了，下面需要启动任务；           
通过什么启动任务呢？就是通过timerFactory类启动任务；            
通过这个类去启动我们的任务；                 
之后把scheduledTask引用到属性scheduledTimerTasks中来；               
当启动Spring后就启动任务了；                     
```xml
<!--启动任务调度-->
<bean id="timerFactory"
class="org.springframework.scheduling.timer.TimerFactoryBean">
   <property name="scheduledTimerTasks">
	<list>
	   <ref local="scheduleTask"/>
	</list>
   </property>
   <property name="daemon">
        <value>false</value>
   </property>
</bean>
```


##### 使用MethodInvokingTimerTaskFactoryBean进行任务调度：    

Spring提供了另一种方式来执行计划任务            
使用Spring提供的MethodInvokingTimerTaskFactoryBean定义计划任务；                 

使用这个类的时候，任务类不用再继承TimerTask；                
Spring中使用                      
```
org.springframework.scheduling.timer.MethodInvokongTimerTaskFactoryBean
```
这个类指定计划任务类和计划执行的方法；             
如下配置代码：             
```xml
<!--使用MethodInvokingTimerTaskFactoryBean定义任务Bean -->
<bean id="methodInvokongTask"
class="org.springframework.scheduling.timer.MethodInvokongTimerTaskFactoryBean">
  <property name="targetObject">
	<ref local="update"/>
  </property>
  <property name="targetMethod">
	<value>ren</value>
  </property>
</bean>
```
首先，配置MethodInvokingTimerTaskFactoryBean；              
它有两个属性：targetObject和targetMethod；               
targetObject指定刚才的update任务类；             
targetMethod指定任务类中要执行哪个方法；                 

接下来，修改之前定义的BatchUpdate；                
     
计划任务类 ：         
```java
public class BatchUpdate  {
 private List update_command;
 public List getUpdate_command() {
      return update_command;
  }
 public void setUpdate_command(List update_command) {
      this.update_command = update_command;
   }
 public void run() {
   for (Iterator it = update_command.iterator();it.hasNext();){
          System.out.println(it.next());
    }
     System.out.println("更新操作执行完毕");
    }
}
```
当前这个任务类和继承了TimerTask的任务类基本没区别；           
唯一的区别就是，把extends去掉了，其他什么都没有改变；               



##### 使用Quartz进行计划任务调度
Timer只能指定任务与任务之间的周期，无法指定某个时间点来定时执行某个任务；                
这时候，Quartz就能发挥作用了；                 
Quartz使用步骤：            
- 任务类继承QuartzJobBean；        
- 配置Spring配置文件；          

任务类继承QuartzJobBean            
```java
 public class QuartzJob extends QuartzJobBean {
  protected void executeInternal(JobExecutionContext arg0)
           throws JobExecutionException {
                    ......
      }
}
```
这里，QuartzJob继承了QuartzJobBean；           
之后，它里面实现了一个方法 `executeInternal()`；           

- 配置Bean
```xml
<bean id="quartzDetail" class="org.springframework.scheduling.quartz.JobDetailBean">
  <property name="jobClass">
     <value>demo.quartz.jobbean.QuartzJob</value>
  </property>
  <property name="jobDataAsMap">
    <map>
       <entry key="command">
	  <value>更新</value>
       </entry>
    </map>
  <property>
</bean>
```
配置任务Bean，id是quartzDetail；         
class指定到Spring提供的JobDetailBean；       
jobClass指定到任务类QuartzJob；               
jobDataAsMap将要执行的指令放到map里；            
key是command，value是更新；              


- 在Spring中配置Quartz    
```xml
<!--简单任务触发器-->
<bean     id="simpleTrigger"
  class="org.springframework.scheduling.quartz.SimpleTriggerBean">
   <property name="jobDetail">
	<ref bean="quartzDetail"/>
   </property>
   <property name="startDelay">
	<value>1000</value>
   </property>
   <property name="repeatInterval">
	<value>2000</value>
   </property>
</bean>
<!--表达式任务触发器-->
<bean id="cronTrigger2"
  class="org.springframework.scheduling.quartz.CronTriggerBean">
   <property name="jobDetail">
       <ref bean="quartzDetail2"/>
   </property>
   <property name="cronExpression">
	<!-Quartz表达式，这里每天18:21:35触发-->
        <value>35 21 18 * * ?</value>
   </property>
</bean>
```

##### Quartz与Timer相同和不同的地方            
先看这个简单的任务触发器相同的地方：                    
相同的地方就是，第一个jobDetail指定了我们刚才定义的任务类；             
startDelay就是开始的时候延迟多少毫秒执行；                   
还有一个就是，repeatInterval；                     
这个就是我们之前写的第一次任务执行之后每隔多少毫秒执行一次；                     
这个和Timer没什么区别；                    
关键是，后面有个表达式触发器；                       
这里，主要看cronExpression属性；                                 
它里面有个  `35  21  18 * * ?`
它表示的就是在每天的` 18:21:35` 的时候执行我们指定的任务；            
这是它最有优势的地方；              
它自己有 quartz表达式；                   
quartz表达式可以指定到具体某一天、某个时间；               
如果想要指定某一天，可以把后面的` * *` 替换掉就可以了；             
这个表达式写法很多，可以查询资料；              


配置启动任务     
```xml
<bean class="org.springframework.scheduling.quartz.SchedulerFactoryBean">
 <property name="triggers">
    <list>
	<ref bean="simpleTrigger"/>
	<ref bean="cronTrigger"/>
	<ref bean="cronTrigger2"/>
    </list>
 </property>
</bean>
```
之后，与Timer一样，把我们之前指定的任务Bean加载进来；        
放到 SchedulerFactoryBan的属性triggers的list下面；            
一个个引进过来就可以了；              
这时候就启动了我们的任务；                             


##### 使用MethodInvokingJobDetailFactoryBean进行任务调度
那么，与Timer一样，Spring对于Quartz也进行了封装；             
在这个过程中，使用了 MethodInvokingJobDetailFactoryBean；                
在这里，我们对比 Timer类来看，                
同样的，任务类不需要再继承QuartzJobBean了；                 
之后，因为Spring封装了Quartz功能在 MethodInvokingJobDetailFactoryBean 中，                  
所以，直接在Spring的配置文件中配置就可以了；             

##### 配置 MethodInvokingJobDetailFactoryBean 任务类
```xml
<bean class="org.springframework.scheduling.timer.MethodInvokingJobDetailFactoryBean">
  <property name="targetObject">
      <ref local="update"/>
  </property>
  <property name="targetMethod">
      <value>run</value>
  </property>
</bean>
```
配置的时候，我们先设置一个 Bean id，将我们要执行的 Quartz这个Bean注入进来；            
之后，我们注入 MethodInvokingJobDetailFactoryBean 这个对象；                     
它有两个属性，一个是 targetObject，一个是 targetMethod；                    
那么，targetObject 指定 testQuarz这个 bean；                 
targetMethod指定的是 testQuartz中的test方法；                    
那么，就完成了Quartz的调用，就是任务的调用；        
