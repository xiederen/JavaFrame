# 实体对象
概述            
Hibernate是一个开放源代码的对象关系映射框架，它对JDBC进行了封装；Hibernate提供了强大、高性能的对象到关系型数据库的持久化服务。利用Hibernate，开发人员可以按照Java的基础语义（包括关联、继承、多态、组合以及Java的集合架构）进行持久层开发。Hibernate提供的HQL(HibernateQuery Language)是面向对象的查询语言，它在对象型数据和关系型数据库之间构建了一条快速、高效、便捷的沟通渠道。     

在Hibernate的应用中，实体对象的生命周期是一个很关键的概念；         
那么，正确理解实体对象的生命周期，将对我们使用Hibernate做持久化设计起到很大的作用；       
要理解实体对象的生命周期，就要首先明白什么是实体对象；       

## 实体对象：
实体对象指的是Hibernate的`O/R`映射关系中的域对象，(即O/R中的O)；    

### 实体对象的生命周期：
所谓的实体对象的生命周期就是指实体对象由产生到被GC回收的一段过程；   
在这段过程中我们需要理解的就是实体对象生命周期中的三种状态；      

### 实体对象的三种状态：
在实体对象的生命周期中，可以分为三种状态：   
这三种状态就是**自由状态、持久状态、游离状态**；     

#### 自由状态(Transient)      
所谓自由状态，就是实体对象在内存中自由存在；     
此时，它与数据库无关；       
举例：       
如下代码：     
```
Login login = new Login();
login.setPassword("123456789");
login.setUsername("马达");
```
在这段代码中，Login对象就是我们所谓的实体对象；       
在这里，我们首先创建了实体对象；然后，给这个对象的password属性和username属性赋值；       
但此时，我们的Login对象只是在内存中存在的；没有和数据库产生任何关系；         
也就是没有存储在数据库中；           
因此，此时实体对象的状态就是自由状态；           

##### 实体对象在自由状态的特征主要有以下两点：
第一：       
当实体对象处于自由状态时，它并不处于Session的缓存中。      
也就是说，它不被任何一个Session实例关联。     

第二：      
此时，这个实体对象在数据库中没有对应的对象；      

#### 持久状态(Persistent)：
所谓持久状态，就是指实体对象处于Hibernate的管理情况下的状态；        
在这种状态下，实体对象的引用被纳入到Hibernate实体容器中加以管理；       
那么，处于此状态的实体对象，会由Hibernate固化到数据库中；            
如下代码：         
```
session.save(login);
session.getTransaction().commit();
```

在这段代码中，当session调用`save()`方法的时候，就会将login对象纳入Hibernate的管理；而，当session获取Transaction，也就是事务，并提交的时候，就会将这个对象固化到了数据库中；      
因此，此时的实体对象的状态就是持久状态；        
实际上，持久状态还包括另一种情况：             
就是当我们需要将数据库中的某一条数据调出来的时候，如下代码：     
```
session.load(Login.class,new Integer(1));
session.getTransaction().commit();
```

在这段代码中，当session调用`load()`方法的时候，也就是一个实体对象由Hibernate加载；     
那么此时，它也是处于持久状态；            

总之，简单的说，如果一个实体对象与某个Session实例发生了关联，并且处于这个Session的有效期内，那么它就处于持久状态。 

##### 实体对象处于持久化状态时的特征：     
第一：           
持久化对象位于一个Session实例的缓存中，也可以说，持久化对象总被一个Session实例关联；          

第二：         
持久化对象和数据库中的相关记录对应；        

第三：        
Session在清理缓存时，会根据持久化对象的属性变化来同步更新数据库中的数据；             

第四：          
Session的`save()`方法把实体对象从自由状态转变为持久状态；         

第五：         
Session的`load()`方法或`get()`方法返回的实体对象总是持久状态；         

第六：       
Session的`update() 、 saveOrUpdate()`和`lock()`方法使实体对象从游离状态转变为持久状态；           


#### 游离状态(Detached) :        
当处于持久状态的实体对象，对应的Session关闭以后，这个实体对象就处于了游离状态；     
实际上，我们可以认为，Session对象是实体对象在持久状态的宿主；          
而，一旦实体对象失去了这个宿主，也就是这个宿主失效，那么这个实体对象就处于游离状态了；        
如下代码：         
```
session.close()；
```

##### 游离状态时实体对象的特征：
第一：        
处于游离状态时的实体对象不再位于Session的缓存中，也可以说游离状态的实体对象不被Session关联。     

第二：          
游离状态是由持久化对象转变过来的，因此在数据库中可能还存在与它对应的记录；         

**注意**：     
	在有些地方，Session共有四个状态，第四个状态为：删除状态；     
	删除状态是Session已经计划将其删除的状态；
	比如：我们调用Session的delete()方法时被认为实体对象处于删除状态；
	但是，此时Session并没有真正删除；


##### 状态之间的转换：
一个实体对象由自由状态到持久状态，最后到游离状态，这三种状态之间的转换：     
代码如下：      
```
Login login = new Login();
login.setPassword("123456789");     //自由状态
login.setUsername("马达");

session.save(login);               //进入持久状态
session.getTransaction().commit();

session.close()；                  //进入游离状态
```

注意，这里给出的是伪代码，并不是实际可执行的代码；       
在这段代码中，当执行Session的`save()`方法的时候，并且调用事务的提交操作；那么一旦对象被持久化到数据库，此时的实体对象就由自由状态进入到了持久状态；当Session调用`close()`方法的时候，也就是Session已经关闭；那么，此时实体对象就由持久状态进入到了游离状态。                
需要注意这几个状态之间的区别，实际上就是游离状态和持久状态的区别。         

- 游离状态和持久状态的区别在于：          
游离状态下，实体对象虽然失去了Session宿主，但是它可以再找一个宿主；也就是说，它可以与另一个Session实例对象进行关联；一旦关联了Session实例，那么实体对象就又进入持久状态；而处于持久状态的实体对象，如果它的宿主还在，那么就没有必要再重新去找一个新的宿主。     

- 自由状态和游离状态的区别：     
此时，实际上，实体对象都没有Session宿主，它们的区别是：        
从本质上讲，自由状态的实体对象与数据库中的数据没有对应关系；           
而，处于游离状态的实体对象在数据库中是存在对应记录的；          
只是游离状态的实体对象脱离了Session这个数据管理平台。           
因而，如果实体对象发生变化，无法更新到数据库中；              


## VO与PO :
- VO :     
将处于自由状态和游离状态下的实体对象称为值对象，即`VO(Value Object)`      

- PO :         
将处于持久状态的实体对象称为持久对象，即`PO (Persistent Object)`    

根据实体对象的三种状态，可知：            
### VO与PO的区别：         

VO对象包含两种状态，自由状态和游离状态，根据这两种状态可知： 
VO对象是相对独立的对象，处于非管理状态。       
而，                
po对象处于持久状态；         
PO是Hibernate纳入其管理容器的对象，代表与数据中某条记录的Hibernate实体，po的变化在事务提交时将反映到实际数据库中；   

如果一个PO与其对应的session实例分离，那么此时就会变成一个VO。     


#### Session详解
Session接口：       
Session接口，是Hibernate向应用程序提供的操作数据库的最主要接口；        
Session接口，提供了基本的保存、更新、删除和加载Java对象的方法；           
Session具有一个缓存，位于缓存中的对象称为持久化对象；它和数据库中的相关记录对应；        

#### 什么是清理缓存：
Session能够在某些时间点，按照缓存中对象的变化来执行相关SQL语句，来同步更新数据库，这一过程被称为清理缓存(flush)。   

##### 站在持久化的角度，Hibernate把实体对象分为三种状态：    
```
自由状态、持久状态和游离状态。
```

那么，实体对象的状态转换正是通过Session的特定方法来实现的。     
所以有必要了解一下，Hibernate中Session的管理和使用。         


#### Session管理：
在利用Hibernate开发DAO模块时，会对Session进行频繁的创建和销毁；     
```java
//Session的创建：      
SessionFactory factory = config.buildSessionFactory();
this.session = factory.openSession();
//Session的使用：
this.session.save(login);
```

Session对象是由SessionFactory负责创建的；而，SessionFactory的实现是线程安全的；也就是说，多个并发的线程可以同时访问一个SessionFactory，并从中获取Session实例；那么，Session是否也是线程安全的呢？       
Session不是线程安全的；      
Session中包含了数据库操作相关的状态信息；所以，试图让多个线程共享一个Session将会发生数据共享混乱问题。      
Hibernate为我们提供了很多的Session管理方案，有一种是TreadLocal模式的解决方案：        

#### TreadLocal模式的解决方案：
我们要学习这种解决方案，首先，就要明白TreadLocal是什么：      
- TreadLocal：      
TreadLocal并不是一个线程的本地实现，也就是说它不是一个Tread，而是`Tread local variable(线程局部变量)`。它的作用就是为每一个使用这个变量的线程都提供一个变量值的副本，并且每一个线程都可以独立地改变自己的副本，而不会和其它线程的副本冲突。         
从线程的角度来看：                       
就好像每一个线程都完全拥有一个该变量一样。         

##### 具体如何利用TreadLocal来管理Session：       
```java
private static final ThreadLocal <Session>s = new ThreadLocal<Session>();
```

在LoginDAO类的开头，增加了ThreadLocal变量的声明代码；也就是说，我们创建了一个TreadLocal对象；并且，这个变量是用static和final修饰符修饰的；       
在此程序中，只会创建一个TreadLocal对象的实例；而，在构造方法中，获取Session实例的时候也做了部分修改；此时的Session实例的获得，通过TreadLocal工具来获取。TreadLocal会给每一个线程提供一个变量的副本；通过如下代码，就可以实现线程范围内的Session共享：       
```java
this.session = (Session)s.get();
if(session == null) {
    this.session = factory.openSession();
    s.set(this.session);
 }
```
从而，避免线程中频繁的创建和销毁Session实例；        
注意：在用完之后需要手动的去关闭Session。   

实际上，在我们使用Hibernate框架进行开发的时候，多数的DAO并不会涉及到多线程的情况。因为，我们总不会将DAO的代码写在Servlet中。           
通常的做法，是在Service层的代码里访问DAO的方法；          
这样的话，一般可以不考虑多线程的问题；            
但是我们不能只考虑现在的部分情况，我们还应该考虑将来的维护与升级。        
所以，建议在设计DAO类的时候，还是采用现在的这种方式。             

#### openSession和getCurrentSession之间的区别
实际上，Hibernate的SessionFactory类除了为我们提供了`openSession()`方法来返回Session实例以外，还提供了一个`getCurrentSession()`方法；通过这个方法，同样可以得到一个Session；       

这两者的区别：     
```
getCurrentSession()创建的Session会绑定到当前线程，而openSession()则不会；
getCurrentSession()创建的Session会在事务回滚或事务提交后自动关闭，而openSession()创建的必须手动关闭(调用Session的close()方法)；
```
注意：          
getCurrentSession的使用方法；         
在我们使用`getCurrentSession()`方法时，还需要在配置文件里进行一些设置：          
当使用本地事务时（jdbc事务）：        
需要在`hibernate.cfg.xml`文件的`<session-factory>`节点中添加如下代码： 
```
<property name="hibernate.current_session_context_class">thread</property>
```
当使用的是全局事务时（jta事务）：         
需要在`hibernate.cfg.xml`文件的`<session-factory>`节点中添加如下代码：      
```
<property  name="hibernate.current_session_context_class">jta</property>
```

假如，当我们使用了`getCurrentSession()`方法，并且我们又没有在配置文件中添加以上这些内容，那么我们的工具编译不会出现问题；只是当我们运行程序的时候就会出错；         
程序会报一个`No CurrentSessionContext configured!`的错误        

另一点需要主要的是：             
当我们使用`getCurrentSession()`方法的时候，是不需要手动关闭Session的；所以，我们不需要调用Session的`close()`方法；     

#### Session的使用
在学习Session的使用时主要介绍Session的一些常用方法；      
Session的`save()`和`persist()`方法：     

##### Save()方法：
Session的`save()`方法使一个自由对象，也称临时对象，转变为持久对象；   
如下代码：      
```java
Login login = new Login();
login.setPassword("123456789");
login.setUsername("马达");
Session session = sessionFactory.openSession();
Transaction tran = session.beginTransacion();
session.save(login);
tran.commit();   //清理缓存，提交事务
session.close();   //此时login对象变为游离状态
```
其中，`session.save(login)`;      
就是Session调用其`save()`方法；                
当Session调用其`save()`方法的时候主要完成了三项操作；       

这三项操作就是：       
1.把Login对象加入到Session的缓存中，使它进入到持久化状态；         
2.选用映射文件指定的标识符生成器，为持久化对象分配一个唯一的OID。        
如下：       
```
<id name="id" column="ID" />
   <generator class="increment" />
</id>
```

这就是Login的映射文件；也就是 `Login.hbm.xml`中；     
其中，`<generator class="increment" />`用来指定标识符生成器；        
假如现在login表中没有任何记录；那么，我们执行完`save()`方法之后，Login对象的OID为1；那么，这个OID是由Hibernate的特定标识符生成器产生的；                
当执行事务的`commit()`方法之后，login表中的新加的这条记录的ID字段就会自动赋值为1；       

3.计划执行一个insert语句，把Login对象当前的属性值组装到insert语句中；如下代码：     
```sql
insert into LOGIN(ID,USERNAME,PASSWORD)values(1,"马达","123456789");
```

这里的话就会计划执行这样的一个insert语句；注意：这里是计划执行，而不是立即执行；真正的执行是在Session进行清理缓存的时候，才会执行SQL的insert语句；也就是，当我们执行`tran.commit()`方法提交事务的时候，才会执行这个SQL语句；  

还需要注意：      
如果在`save()`方法之后，在事务的提交方法之前，又修改了持久化对象的属性，当Session在清理缓存的时候，会额外的执行一个`SQL update`操作；             
因为执行`save()`方法之后，对象就是持久化状态了，处于Session的时刻的管理和监控之中，当执行提交操作的的时候，Session就会对该对象进行同步，发现其内容的修改；所以就无需再执行一次`save()`方法了；             
如下代码：    
```java
Login login = new Login();
login.setPassword("123456789");
login.setUsername("马达");
Session session = sessionFactory.openSession();
Transaction tran = session.beginTransacion();
session.save(login);
login.setUsername("小明");
tran.commit();   //清理缓存，提交事务
session.close();   //此时login对象变为游离状态
```

其中：      
```
session.save(login);
login.setUsername("小明");
```

在执行Session的`save()`方法之后，又对Login对象属性进行了修改；把姓名换成了小明；那么，当执行`save()`方法的时候，实际上会计划执行两个SQL语句；          
一个是要插入马达这条数据的SQL语句；        
另一个，就是修改这条数据，把姓名改成小明；    

#### Session的persist()方法：
Session的`persist()`方法和`save()`方法的作用是类似的，也能把一个临时对象转变为持久对象；    
注意：        
`persist()`方法和`save()`方法的区别在于persist方法是在`Hibernate3`版本中才出现的，它实现了`EJB3`规范中定义的持久化语义。 

当调用`persist()`方法时，并不一定会立即为我们的持久化对象的OID赋值，而是有可能在最后Session清理缓存的时候才为OID赋值。       
另外，如果在事务边界以外调用`persist()`方法的话，这个方法不会计划执行一个SQL的insert语句；那么，这样可以提高负责处理长时间运行事务的程序的健壮性；而对于`save()`方法，不管是在事务边界以内，还是以外，只要调用它，就会计划执行SQL的insert语句；     

#### Session的update()方法：
Session的`update()`方法使一个游离对象转变为持久对象，并且会计划执行一条update语句；     
如下代码：  
```java
Login login = new Login();
login.setPassword("123456789");
login.setUsername("马达");
Session session = sessionFactory.openSession();
Transaction tran = session.beginTransacion();
session.save(login);
tran.commit();   //清理缓存，提交事务
session.close();   //此时login对象变为游离状态

Session session2 = sessionFactory.openSession();
Transaction tran2 = session.beginTransaction();
login.setUsername("小明");  //在和Session2关联之前修改Login对象的属性
session2.update(login);    //Login对象和session2关联
tran2.commit();    //清理缓存，提交事务
session2.close();
```

就像使用`save()`方法和`persist()`方法一样，当我们执行Session的`update()`方法时，也会完成一些操作；   
第一个操作就是把Login游离对象加入到当前的`session2`的缓存中，使它变为持久化对象；           
第二个操作是计划执行一个update语句；         
同样值得注意的是：          
`session2`只有在清理缓存的时候才会执行update语句，并且执行时才会把Login对象的当前属性的值，组装到update语句中。同时，即使程序中多次修改了Login对象的属性，在清理缓存的时候，只会执行一次update语句；     

#### Session的saveOrUpdate()方法
`saveOrUpdate()`方法同时包含了`save()`和`update()`方法的功能，如果传入的参数是临时对象，就调用`save()`方法；如果传入的参数是游离对象，就调用`update()`方法；而如果传入的参数是持久化对象，那就直接返回；          
在传入参数不是持久化对象的前提下，`saveOrUpdate()`方法如何判断一个对象处于自由状态还是游离状态呢？       
为此，我们给出了几种情况，如果满足以下情况之一，Hibernate就把它作为临时对象，否则就作为游离对象；       

第一：          
Java对象的OID取值为null；      

第二：       
Java对象具有version版本控制属性并且取值为null；        

第三：      
在映射文件中为`<id>`元素设置了`unsaved-value`属性，并且Java对象的OID取值与这个`unsaved-value`属性值匹配；    

第四：        
在映射文件中为version版本控制属性设置了`unsaved-value`属性，并且Java对象的version版本控制属性的取值与映射文件中`unsaved-value`属性值匹配；     
 
第五：        
为Hibernate的Internet(拦截器)提供了自定义实现，并且Interceptor实现类的`isUnsaved()`方法返回`Boolean.TRUE`;    

#### Session的load()和get()方法
Session的`load()`和`get()`方法，都能根据跟定的OID从数据库中加载一个持久化对象；      
##### Session的load()和get()方法之间的区别：      
- 第一个区别:       
当数据库中不存在与OID对应的记录时，`load()`方法抛出`org.hibernate.ObjectNotFoundException`异常，而`get()`方法返回null；   

- 第二个区别：       
两者采用了不同的检索策略；      
在默认情况下，所有的持久化类都是采用延迟检索策略的；如下映射文件：    
```
<class name="com.test.Login" table="LOGIN" lazy="true">
```

在这个映射文件中，注意：`lazy="true"`         
lazy的属性的值默认是true；即，不加这个属性的时候，默认值就是true；     
那么，在默认的情况下，`lazy="true"`，`load()`方法会采用延迟检索策略加载持久化对象；        
假如，把lazy属性改为false；那么，`load()`方法会采用立即检索策略；       
而；       
`get()`方法则会忽略class元素中的lazy属性；        
也就是说，不管lazy属性取什么值，`get()`方法总是采用立即检索策略；       
延迟检索和立即检索又是什么呢？         
举例：            
假如一个对象中组合了另一个对象，当我们使用延迟检索的时候，如果我们现在操作外面对象的某个属性，这个属性又不是此对象中被组合的对象；那么，检索的SQL只会执行外面对象的数据查询。而当操作组合对象的属性的时候，才会检索组合对象的数据。
立即检索则是当执行检索的时候，就把所有数据一次性的都检索了出来。          

根据这两个方法的检索策略不同，就可以做出一些选择：         
如果加载一个对象的目的是为了访问它的各个属性，就可以使用`get()`方法；       
如果加载一个对象的目的是为了删除它或者建立与别的对象的关联关系，就可以使用`load()`方法；         

#### Session的delete()方法
Session的`delete()`方法用于从数据库中删除一个Java对象；       
`delete()`方法既可以删除持久对象，也可以删除游离对象；    
当调用`delete()`方法的时候，如果传入的参数是游离对象，那么会先使游离对象与当前的Session关联，从而使它变为一个持久对象；而，当我们传入的参数是一个持久对象的时候，则会忽略这一步。        
当，对对象持久化之后，第二步就会计划执行一个delete语句；           
最后，当对Session进行缓存清理的时候，就会执行这个计划的delete语句，从而从数据库中删除对应记录。      
需要注意：       
Session的`delete()`方法一次只能删除一个对象；      

通过一个示例来看一下如何使用Session的`delete()`方法：         
代码如下：  
```
Session session = sessionFactory.openSession();
Transaction tran = session.beginTransaction();
//先加载一个持久化对象
//加载持久化对象是为了删除此对象而无需访问它的属性；因此，用load()
Login login = (Login)session(Login.class,new Long(1));
//计划执行一个delete语句
session.delete(login);
tran.commit();   //清理缓存，提交事务
session.close();
```

现在要删除一个对象；        
首先，需要去得到一个持久化对象，因此首先使用`load()`方法得到一个持久化对象；实际上使用`get()`方法也是可以得到一个持久化对象的，但是因为不访问这个对象的任何属性，仅仅只是为了删除这个对象，因此使用`load()`方法；默认的情况下会使用延迟检索策略。         
得到这个持久化对象之后，就可以使用Session的`delete()`方法对这个对象进行计划删除；    
注意：         
当执行`delete()`方法的时候，此时会计划执行一个delete的SQL语句。但这个SQL语句还没有执行，也就是数据库中此时这个数据还没有被删除；等清理缓存的时候，才会真正的执行这个SQL语句，并从数据库中删除对应的数据。     


## Hibernate的映射类型
在对象-关系映射文件中，Hibernate采用映射类型作为Java类型和SQL类型的桥梁；        
举例：      
```
<id name="id" column="ID" type="int">
<generator class="increment"/>
</id>
<property name="username" type="string">
```
在`<id> <property> <version>`等元素中，type属性用来指定Hibernate映射的类型；         
如上代码，id元素的`type="int"`和property元素中的`type="string"`，就是Hibernate的映射类型；      
#### Hibernate映射类型分为两类：      
内置映射类型和客户化映射类型；        

- 内置映射类型：    
负责把一些常见的Java类型映射到相应的SQL类型；     
此外，        
Hibernate还允许用户实现UserType或CompositeUserType接口，来灵活的定制客户化映射类型。          
客户化映射类型，能够把用户定义的Java类型映射到数据库表的相应字段。          

- 内置映射类型：     
Hibernate的内置映射类型通常使用和Java类型相同的名字，它能够把Java基本类型、Java时间和日期类型、Java大对象类型及JDK中常用的Java类型映射到相应的标准SQL类型；        

#### Java基本类型的Hibernate映射类型：

 
##### Java时间和日期类型的Hibernate映射类型：
在Java中，代表时间和日期的类型包括:`java.util.Date`和`java.util.Calendar`
。此外，在`JDBC API`中还提供了3个扩展了`java.util.Date`类的子类：
`java.sql.Date、java.sql.Time`和`java.sql.Timestamp`，这3个类分别和标准SQL类型中的`DATA`、`TIME`和`TIMESTAMP`类型对应。    


#### Java大对象类型德尔Hibernate映射类型：

JDK自带的个别Java类的Hibernate映射类型：       


##### 操作Blob和Clob类型数据
在前面，介绍二进制大对象及字符串大对象的映射类型的时候，介绍过：       
在持久化类中，二进制大对象可以声明为`byte[]`或`java.sql.Blob`类型；         
字符串大对象可以声明为`java.lang.String`或`java.sql.Clob`类型。     

`java.sql.Blob`和`java.sql.Clob`是`JDBC API`中的接口。     

在默认的情况下，Blob和Clob接口实现会使用SQL定位器。       
换句话说，就是：          
当程序从数据库加载Blob类型或Clob类型的数据的时候，并没有把数据从数据库中读出来；仅仅是加载了一个Blob类型或Clob类型的数据的逻辑指针，我们得到这个指针之后，还需要通过`Blob.getBinaryStream()`或`Clob.getCharacterStream()`方法得到Blob或Clob类型的数据的输入流，之后才可以真正的读取到大对象数据。        

##### Hibernate操作Blob：
示例需求：       
在LOGIN表中增加一个Blob类型的字段       
使用Hibernate操作Blob字段添加一张图片     
使用Hibernate操作Blob字段读取字段中的数据重新生成一张图片        

实现示例：      
首先，需要修改数据库，修改数据库增加Blob字段；        
IMAGE字段，类型为Blob类型；         

然后，修改映射文件以及bean文件，为映射文件和bean文件添加image属性的代码；         
那么，映射文件就是`Login.hbm.xml`；我们是对Login表进行的修改，因此我们还需要修改Login对象对应的bean文件；在这两个文件中都要添加image属性的代码；        
`Login.hbm.xml`文件中添加：   
```
<property name="image" type="java.sql.Blob" column="IMAGE"/>
//Bean文件中添加：
private Blob image;
//get方法和set方法
```
经过以上两个步骤，前期准备工作就完成了，接下来，        
创建带Blob类型字段的保存方法 `save()`方法；             
当我们调用`save()`方法的时候，我们希望传入一个Login对象和一个图片的地址。就能对这个对象进行保存操作。    
```java
public int saveBlob(Login login,String imagePath) throws IOException{
    InputStream in = this.getClass().getResourceAsStream(imagePath);
    byte[] buffer = new byte[in.available()];
    in.read(buffer);
    in.close();
			
    login.setImage(Hibernate.createBlob(buffer));
	 		
     tran = session.beginTransaction();
     session.save(login);
     tran.commit();
   return login.getId();
}
``` 

到现在，就已经创建完保存方法，接下来：         
创建从数据库中获得持久化对象，并把此对象的Blob字段的图片数据保存到某一个特定路径下的方法；       
```java
public void getBlob(int id,String targetPath) throws SQLException, IOException{
			
	//使用Hibernate来读取Blob字段
	Login login = (Login)session.get(Login.class, id);
	Blob image = login.getImage();
			
        //使用Java的IO功能将Blob字段内容复制到指定文件
	InputStream in = image.getBinaryStream();
	OutputStream out = new FileOutputStream(targetPath);
		
	int n = -1;
	while((n =in.read()) !=-1){
	out.write(n);
	}
			
	in.close();
	out.close();
		
		}

```

最后，需要去创建一个测试类，来测试代码；       
```
public class Test {
 public static void main(String[] args) throws Exception {

   Login login = new login();
   login.setPassword("23456789");
   login.setUsername("马达");

   int id = new BlobLoginDao().saveBlob(login,"orm.jpg");
   System.out.println("id=" + id);
   new BlobLoginDao().getBlob(id,"c:\\orm2.jpg");

}

}

```

##### Hibernate操作Clob
Clob类型：     
字符串大对象类型，      
涉及的信息量特别大，同时是字符数据；比如：用户的备注信息，用户的简历信息；         

示例需求：         
在数据库中的Login表中增加一个Clob类型的字段description
（因为这个字段内容比较多，所以采用`java.sql.Clob`类型）；      
使用Hibernate操作Clob字段添加用户描述信息        
使用Hibernate操作Clob字段读取用户描写信息          

实现示例：         
首先，修改数据表增加Clob字段；      
`DESCRIPTION字段`，类型为Clob；               
         
修改映射文件和bean文件增加description属性的代码；        
映射文件就是`Login.hbm.xml`；我们是对login表进行的修改，因此我们还需要修改Login对象对应的bean文件；在这两个文件中都需要添加description属性的代码；          

`Login.hbm.xml`文件中的修改：
```xml
<property name="description" type="java.sql.Clob" column="DESCRIOPTION"/>
//bean文件中添加：
private Clob description
//get方法和set方法
```

经过以上两个步骤，前期准备工作就完成了；接下来：   
创建带Clob类型字段的对象的保存方法`save()`方法；      
当调用此方法的时候，希望传入一个Login对象和一个使用String变量保存的长字符串；就能对这个对象进行保存操作了； 
在`ClobLoginDao.java`中：            
定义保存方法：     
```java
public int saveClob(Login login,String description){
tran = this.session.beginTransaction();
login.setDescription(Hibernate.createClob(description));
session.save(login);
tran.commit();

return login.getId();
}
```

创建从数据库中获得持久化对象，并把此对象的Clob字段的数据保存到某一个特定的String变量中；	       
```java
public String getClob(int id) throws SQLException IOException {
 //获取Clob字段
 Login login = (Login)session.get(Login.class,id);
 Clob desc = login.getDescription();

 //将Clob类型转换为String类型
 Reader rd = desc.getCharacterStream();
 BufferedReader br = new BufferedReader(rd);
 String sdesc = br.readLine();

 //返回String类型数据
 return sdesc;
}
```

建立测试类，测试代码；       
```java
public static void main(String[] args) throws Exception {
 Login login =  new Login();
 login.setPassword("1234567");
 login.setUsername("马达");

 String description = "马达学习刻苦，成绩优异！";
 int id = new ClbLoginDao().saveClob(login,description);
 System.out.println("id=" + id);
 String desc = new ClobLoginDao().getClob(id);
 System.out.println(desc);
}
```

## 集合映射
值类型集合的映射方法        
在值类型集合中存放的对象没有OID，它们的生命周期依赖集合所属对象的声明周期。        
Hibernate采用`<set>`、`<list>`和`<map>`元素，来映射`java.util.Set、java.util.List`和`java.util.Map`

### 映射Set(集)
Set(集)：       
Set是最简单的一种集合，集合中的对象不按特定方式排列，并且没有重复的对象；         
Set接口主要有两个实现类HashSet和TreeSet           
HashSet类按照哈希算法来存取集合中的对象，存取速度非常快；        
HashSet类还有一个子类，LinkedHashSet类；LinkedHashSet类不仅实现了哈希算法，而且实现了链表数据结构；          
TreeSet类实现了SortedSet接口，具有排序功能；           

#### Set的一般用法：
Set集合中存放的是对象的引用。并且没有重复对象；    
示例：      
```java
Set set = new HashSet();
String s1 = new String("Hello");
String s2 = new String("Hello");
String s3 = new String("World");
set.add(s1);
set.add(s2);
set.add(s3);
System.out.println(set.size());
```

在这里创建了三个引用变量`s1 s2 s3`；Set集合依次把这三个引用变量加入到集合中；那么，当在一个main函数中执行以上这段程序的时候，它的输出结果是2；实际上只向Set集合中加入了两个对象；         
实际上，当一个新对象加入到Set集合的时候，Set的`add()`方法会使用`equals()`方法比较两个对象是否相等，而不是采用`==`比较运算符。       
对于上面的代码，虽然s1和s2实际上引用的是两个内存地址不同的字符串对象，但是由于`s2.equals(s1)`的比较结果是true。因此， Set集合认为它们是相等的对象；因此，当使用`add()`方法对s2操作的时候，将不会把s2加入到集合中，因此程序输出为2。    
在这个示例中，使用的是HashSet类；        

- HashSet类：        
Hash类是Set接口的实现类；      
HashSet类按照哈希算法来存取集合中的对象，具有很好的存取性能，当HashSet向集合中加入一个对象时，会调用对象的`hashCode()`方法获得哈希码，然后根据这个哈希码进一步计算出对象在集合中的存放位置。        

- TreeSet类：   
TreeSet类实现了SortedSet接口，能够对集合中的对象进行排序；    
示例：    
```java
Set set = new TreeSet();
set.add(new Integer(8));
set.add(new Integer(7));
set.add(new Integer(6));
set.add(new Integer(9));
Iterator it = set.iterator();
while(it.hasNext()) {
System.out.println(it.next());
}
```

	在这段代码中，创建了一个TreeSet对象，然后向集合中加入4个Integer对象；    
	当执行这段程序的时候，会顺序输出： 6 7 8 9 
这是因为，当TreeSet向集合中加入一个对象时，会把它插入到有序的对象序列中；         
#### TreeSet支持两种排序方式：     
自然排序和客户化排序；       
在默认的情况下，TreeSet采用的是自然排序方法；      
当使用自然排序的时候，只能向TreeSet集合中加入相同类型的对象；并且，这些对象的类型必须实现了Comparable接口；       
而，              
客户化排序需要实现Comparator接口；此接口定义了一个`compare()`方法；          
我们去实现这个方法，在这个方法中定义我们自己的排序规则；           
那么，当我们创建TreeSet对象的时候，就把我们对Comparator接口的实现类的实例化对象作为其初始化参数；这样，当我们向TreeSet集合使用`add()`方法添加数据的时候，就会使用客户化排序规则进行排序。它的实现细节不需要了解；          

### 映射Set(集)
Hibernate中的映射Set；   
示例：           
假设有一个用户表Users，对应Users对象；        
然后，再假设Users对象中有一个images属性，它是一个集合属性，用于存放用户的所有收藏图片；         
这里要求images集合中不允许存放重复的照片文件名；             
对于这种要求，可以把images属性定义为Set类型的；       
接下来，通过实现这个需求来完成学习映射Set。       
在持久化类Users类中：            
如下代码：   
```
private Set images = new HashSet();
public Set getImages() {
 return images;
}
public void setImages(Set images) {
 this.images = images;
}
```

这段代码是Users类中的部分代码；           
在此主要为Users对象定义一个images属性，并且属性的类型指定为Set类型的；那么，还需要在数据库中定义两张表：一个是用户表，也就是Users表。另一个表是images表；对于images表，有两个字段，一个是userid，一个是filename字段；对于它的userid字段需要指定为users表的外键；             
由于需求中要求Users对象的images集合中不能出现重复的照片文件名，因此当创建数据库表的时候，尤其当创建images表的时候，需要把userid字段和filename字段作为联合主键；         

创建用户表：     
```
表名： USERS
字段： Name      Type
       ID        number
       NAME      varchar2(20)
       AGE       number
```
该表的主键对应的是ID字段，起名为`PK_USERS`

创建图片表：       
```
表名： IMAGES
字段   Name         Type
       USERID       number
       FILENAME     varchar2(100)
```
在该表同时使用`USERID FILENAME`作为主键，创建联合主键，起名为`PK_IMAGES`      
联合主键：      
即，通过`USERID FILENAME`可以唯一确定一条记录；     

在`IMAGES表`中通过`USERID字段`和用户表建立关联关系。     
将`FILENAME`作为主键，所以不允许在IMAGES表中在同一个用户中存放两张相同的图片；     

创建好对象类和数据库中的表之后，接下来看一下映射文件；          
也就是，要创建一个`Users.hbm.xml`文件，在`Users.hbm.xml`文件中映射Users类的images属性的代码；     
在映射文件中映射Users类的images属性的代码：     
```
<set name="images" table="IMAGES" lazy="true">
  <key column="USERID"/>
  <element column="FILENAME" type="string" not-null="true"/>
</set>
```

在`<set>`元素中包含了三个属性：        
```
name属性指定了Users类的images属性名；
table属性指定了和images属性应用的表是images表。
lazy属性如果为true，表示对images集合使用延迟检索策略。
```

默认的情况下，lazy的属性值为true，因此这里也可以不显示的进行设置。      
`<set>`元素的`<key>`子元素指定images表的外键为userid；             
`<element>`元素指定和images集合中元素对应的字段为filename字段；并且images集合中的元素为字符串类型；     

只需要创建Users表对应的持久化类Users类，不需要创建`IMAGES`表对应的持久化类，可以把`IMAGES`表看做`USERS`表的一部分。在`USERS`表中还有属性`IMAGES`属性。         

最后，需要创建以下方法：      
```
saveUser() :  保存一个Users对象
loadUsers() : 加载一个Users对象
printUser() : 打印Users对象的信息
```
```java
public int saveUsers(Users user) {
  tran = session.beginTransaction();
  session.save(user);
  tran.commit();
  return user.getId();
}

public Users loadUsers(int id) {
  tran = this.session.beginTransaction();
  Users user = (Users) this.session.load(Users.class,id);
  return user;
}

public void printUsers(Users user) {
 System.out.println(User.getUsername()+"  "+user.getAge());
 Set images = user.getImages();
 Iterator it = images.iterator();
 while(it.hasNext()) {
  String filename=(String)it.next();
  System.out.println(user.getUsername() + " " +filename);
  }
```

测试类：          
```java
public static void main(String[] args) {
 Set images = new HashSet();   //演示映射Set时使用
  images.add("images1.jpg");
  images.add("images2.jpg");
  images.add("images3.jpg");
  images.add("images4.jpg");
  images.add("images5.jpg");
  images.add("images6.jpg");
  images.add("images7.jpg");

 Users user = new Users();
 user.setUsername("马达");
 user.setAge(22);
 user.setImages(images);

 UsersDao udao = new UsersDao();
 int s = udao.saveUsers(user);
 Users nuser = new UsersDa().loadUsers(s);
 new UsersDao().printUsers(nuser);

```

### 映射List(列表)        
首先来回顾集合中的List：                
集合List：                     
List的主要特征是其对象以线性方式存储，集合中允许存放重复对象；        
List接口主要的实现类有LinkedList和ArrayList；         
LinkedList采用链表数据结构；          
ArrayList代表大小可变的数组；              
需要注意的是：             
List接口还有一个实现类，叫Vector；它的功能和ArrayList比较相似；           
两者的区别在于Vector类的实现采用了同步机制。而，ArrayList没有使用同步机制。             

List对集合中的对象按索引位置排序，允许按照对象在集合中的索引位置添加和查找对象；            
示例：           
```java
List list = new ArrayList();
list.add(new Integer(3));
list.add(new Integer(4));
list.add(new Integer(3));
list.add(new Integer(2));
for(int i=0;i<list.size();i++)
  System.out.println(list.get(i));
```

在这里，创建了一个List对象，并且向这个List对象中加入了4个Integer对象；接着，使用List的`get(int index)`方法，返回集合中由参数index指定的索引位置的对象；         
需要注意的是：         
第一个加入到集合中的对象的索引位置为0；      
那么，以后以此类推；         
因此，当我们执行这段程序的时候，它的输出结果是：`3 4 3 2`   
List的`iterator()`方法和Set的`iterator()`方法一样，也能返回Iterator对象；    
通过Iterator对象，也可以遍历集合中的所有对象；          
代码如下：       
```java
Iterator it = list.iterator();
while (it.hasNext()) {
 System.out.println(it.next());
}
```

它的用法和Set中的Iterator的用法是一样的；     

List只能对集合中的对象按索引位置排序，如果希望对List中的对象按其他特定方式排序，可以借助Comparator接口和Collections类；          
Collections类，是Java集合API中的辅助类；它提供了操纵集合的各种静态方法，其中，`sort()`方法用于对List中的对象进行排序：
```
sort(List list) ： 对List中的对象进行自然排序；
sort(List list,Comparator comparator) :对List中的对象进行客户化排序；Comparator参数指定排序方式；
```

自然排序的例子：     
```java
List list = new ArrayList();
list.add(new Integer(3));
list.add(new Integer(4));
list.add(new Integer(3));
list.add(new Integer(2));
Collections.soet(list);
for(int i=0;i<list.size();i++)
 System.out.println(list.get(i));
}
```

在这里，通过`Collections.sort(list)`方法，就可以完成对List集合的排序；     
经过排序后，再执行这段程序，输出结果是： `2 3 3 4`        

### 映射List(列表)
Hibernate中的映射List：     
实例：         
针对映射Set(集)中的示例，在其基础上进行一定的需求变更；        
这里假如希望images集合中允许存放重复的元素，并且能够按照索引位置排序，我们需要添加一个POSITION字段，代表每个元素在集合中的位置；         
因此，         
首先，需要更改数据库中的表结构。         
在IMAGES表中，定义一个position字段；因为，我们要用这个字段进行排序，因此修改IMAGES表的主键；此时，userid字段和position字段共同构成了IMAGES表的主键；          
创建用户表：        
```
表名： USERS
字段： Name      Type
       ID        number
       NAME      varchar2(20)
       AGE       number
```

该表的主键对应的是ID字段，起名为`PK_USERS`   
创建图片表：     
```
表名：IMAGES
字段  Name         Type
      USERID       number
      FILENAME     varchar2(100)
      POSITION     NUMBER
```

在该表同时使用`USERID POSITION`作为主键，创建联合主键，起名为`PK_IMAGES`      

在IMAGES表中通过USERID字段和用户表建立关联关系。    
POSITION字段作为排序的依据；对于同一个用户，POSITION必须是不同的值。        
FILENAME可以有相同的图片；          

修改好数据库中的表结构之后，接下来再来，         
修改`Users.hbm.xml`文件，映射Users类的images属性：        
代码如下：    
```
<list name="images" table="IMAGES" lazy="true">
  <key column="USERID"/>
  <list-index column="POSITION"/>
  <element column="FILENAME" type="string" not-null="true"/>
</list>
```

在这里，`<list>`元素与前面介绍的`<set>`元素的配置是很类似的；      
区别在于：`<list>`中增加了`<list-index>`子元素；它用于设置IMAGES表中代表索引位置的`POSITION字段`；       
修改好`Users.hbm.xml`映射文件后，还要修改Users类：         
因为数据库中增加了一个字段；映射文件中，也添加了对应的字段，因此还需要修改Users类；在其中添加这个属性；    

修改User类 Set类型的images属性为List类型，同时添加对应的get和set方法：
```
private List images = new ArrayList();
//get和set方法；
```
注意：       
无需为POSITION字段创建属性；在映射文件中通过`<list-index>`标签指定对应字段即可；     
POSITION字段不是针对IMAGES表中的所有记录的，而是针对某一个用来来设定的；每一个用户的图片索引都要从0开始；       

修改测试代码：           
在做完以上操作之后，需要修改测试代码；        
在创建Images对象的时候，不再使用Set集合，而是替换为了ArrayList；         
其他部分基本上也使用Set集合的时候没有什么区别；         
代码如下：            

最后，需要创建以下方法：    
```
saveUser() :  保存一个Users对象
loadUsers() : 加载一个Users对象
printUser() : 打印Users对象的信息
```
```java
public int saveUsers(Users user) {
  tran = session.beginTransaction();
  session.save(user);
  tran.commit();
  return user.getId();
}

public Users loadUsers(int id) {
  tran = this.session.beginTransaction();
  Users user = (Users) this.session.load(Users.class,id);
  return user;
}

public void printUsers(Users user) {
 System.out.println(User.getUsername()+"  "+user.getAge());
 List images = user.getImages();
 Iterator it = images.iterator();
 while(it.hasNext()) {
  String filename=(String)it.next();
  System.out.println(user.getUsername() + " " +filename);
  }

//测试类：
public static void main(String[] args) {
  List images = new ArrayList();   //演示映射List时使用
  images.add("images1.jpg");
  images.add("images2.jpg");
  images.add("images3.jpg");
  images.add("images4.jpg");
  images.add("images5.jpg");
  images.add("images6.jpg");
  images.add("images7.jpg");

 Users user = new Users();
 user.setUsername("马达");
 user.setAge(22);
 user.setImages(images);

 UsersDao udao = new UsersDao();
 int s = udao.saveUsers(user);
 Users nuser = new UsersDa().loadUsers(s);
 new UsersDao().printUsers(nuser);
```

### 映射Map

Map(映射)：         
Map(映射)是一种把键对象和值对象进行映射的集合，它的每一个元素都包含一对键对象和值对象，而值对象仍可以是Map类型，以此类推，这样可以形成多级映射；         
需要注意的是：          
向Map集合中加入元素时，必须提供一对键对象和值对象；      
从Map集合中检索元素时，只要给出键对象，就会返回对应的值对象；        

示例：    
```java
Map map = new HashMap();
map.put("1","Monday");
map.put("2","Tuesday");
map.put("3","Wendsday");
map.put("4","Thursday");
String day=(String)map.get("2");
System.out.println(day);
```

首先，创建一个Map集合对象，然后通过Map的`put()`方法向集合中添加元素；  
通过Map的`get()`方法来检索与键对象对应的值对象；           
>输出结果：Tuesday          
需要注意的是：        
Map集合中的键对象不允许重复；        
也就是说，任意两个键对象通过`equals()`方法比较的结果都是false；        
而，对于值对象，则没有唯一性的要求；可以将任意多个键对象映射到同一个值对象上；         

Map中的元素不能保证有序；         


#### Hibernate中的映射Map：
对于Hibernate中的映射Map，继续在映射Set的示例基础上进行需求变更。    
示例：        
如果Users类的images集合中的每一个元素包含一对键值对对象，那么应该把images集合定义为Map类型；       
代码如下：     
```java
private Map images = new HashMap();
public Map getImages() {
 return images;
}
public void setImages(Map images) {
  this.images = images;
}
```

这里是Users类中的部分代码；      
定义的images属性的类型设置为Map类型；         
那么此时，在数据库的images表中，需要定义一个`images_name字段`，这个字段使其和images集合中的键对象对应。而，值对象就是filename；        
Map集合的键对象具有唯一性要求，因此对于数据库中的images表，还需要修改它的联合主键；定义联合主键为 `userid`和`images_name`；       
创建用户表：          
```
表名：USERS
字段：Name      Type
      ID        number
      NAME      varchar2(20)
      AGE       number
```

该表的主键对应的是ID字段，起名为`PK_USERS`
创建图片表：
```
表名：IMAGES
字段  Name         Type
      USERID       number
      FILENAME     varchar2(100)
      IMAGES_NAME  VARCHAR2(20)
```

在该表同时使用`USERID IMAGES_NAME`作为主键，创建联合主键，起名为`PK_IMAGES`     

在IMAGES表中通过USERID字段和用户表建立关联关系。   


修改好数据库表中的表结构之后，再来修改`Users.hbm.xml`文件；    
修改`Users.hbm.xml`文件，映射Users类的images属性，代码如下：      
```
<map name="images" table="IMAGES" lazy="true">
 <key column="USERID"/>
 <map-key column="IMAGE_NAME" type="string"/>
 <element column="FILENAME" type="string" not-null="true"/>
</map>
```

在这里，`<map>`元素与`<set>`元素的配置很类似；         
只是，区别在于：             
`<map>`元素中增加了`<map-key>`子元素；它用于设置IMAGES表中和images集合的键对象对应的`images_name`字段；     
需要注意的是：        
前面的UserDao类中的printUsers方法中，都是使用Iterator迭代器进行数据打印的；        
而，对于Map集合来说，不能直接使用迭代器进行迭代输出；          
因此，先要转换为Set类型或者其他类型的；           

最后，需要创建以下方法：    
```
saveUser() :  保存一个Users对象
loadUsers() : 加载一个Users对象
printUser() : 打印Users对象的信息
```

```java
public int saveUsers(Users user) {
  tran = session.beginTransaction();
  session.save(user);
  tran.commit();
  return user.getId();
}

public Users loadUsers(int id) {
  tran = this.session.beginTransaction();
  Users user = (Users) this.session.load(Users.class,id);
  return user;
}

public void printUsers(Users user) {
 System.out.println(User.getUsername()+"  "+user.getAge());
 Map images = user.getImages();
 Set keys = images.keySet();
 Iterator it = keys.iterator();
 while(it.hasNext()) {
  String key = (String) it.next();
  String val = (Stirng) images.get(key);
  System.out.println(key + "-----------" + val);
}
```

最后，同样需要修改测试代码，在创建Images对象的时候将其类型替换为HashMap；其他部分基本上和原来的代码没有什么区别；  

测试类：        
```java
public static void main(String[] args) {
  Map images = new HashMap();   //演示映射Map时使用
  images.put("image1","images1.jsp");
  images.put("image2","images3.jsp");
  images.put("image3","images3.jsp");
  images.put("image4","images4.jsp");
  images.put("image5","images5.jsp");

 Users user = new Users();
 user.setUsername("马达");
 user.setAge(22);
 user.setImages(images);

 UsersDao udao = new UsersDao();
 int s = udao.saveUsers(user);
 Users nuser = new UsersDa().loadUsers(s);
 new UsersDao().printUsers(nuser);
```