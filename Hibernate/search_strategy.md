# Hibernate检索策略
Hibernate检索策略概述：        
所谓检索策略就是指查询策略，但是在Hibernate中一般称之为检索策略；      
问题：            
当在Hibernate中建立了对象的关联映射之后，当使用对象关联映射时，在使用`get()`、`load()`或者HQL查询等方式，来查询班级信息的时候，Hibernate会不会自动同时查询与之关联的Student信息呢？而我们又是不是希望Hibernate立即查询与之关联的学生信息呢？       

分析：       
	是否同时查询Student信息应根据应用需求而定。
	如果需要同时使用其全部关联的Student信息，则可以立即加载。
	如果只是为了使用Grade信息，同时查询Student将导致多余的查询操作，浪费内存空间；
	如何来解决这个问题呢？
Hibernate就提供了多种检索策略供我们来选择；        

#### Hibernate提供了三种检索策略，分别为：        
```
立即检索、延迟检索和迫切左外连接检索；   
```
|检索策略      |              允许机制|
|------|-----|
|立即检索|立即加载与当前对象关联的对象，需要执行多条select语句；|
|延迟检索|不立即加载与当前对象关联的对象，在第一次访问关联对象时才加载其相关信息|
|迫切左外连接检索|通过左外连接加载与当前对象的关联对象。为立即检索策略，但执行的select语句少，只执行1条select连接查询语句；|

##### 立即检索：
所谓立即检索就是指在加载当前对象的时候，会立即加载与之关联的关联对象，这需要通过执行多条select语句来实现；    
比如，在查询班级信息的时候，需要执行一条查询语句；然后，在根据编号查询每个班级的学生信息的时候，会再分别执行一条select语句；           

##### 延迟检索：        
指的是在加载当前对象的时候，不会立即加载与之关联的关联对象；何时来加载这些关联对象呢？在第一次访问关联对象的时候才加载其信息；比如，在查询班级信息的时候，不会同时查询学生信息，什么时候查询呢？在我们第一次访问学生信息的时候，再执行相应的select语句查询其信息；               

##### 迫切左外连接检索：   
所谓迫切也就是立即的含义；              
该检索策略实际上也是立即检索策略；            
两种检索策略的不同之处在于，迫切左外连接检索只需要执行一条select语句就可以了，而不是执行多条；它是通过一条左外连接查询语句来加载当前对象，以及当前对象的关联对象，减少了查询语句的数量；         



### 检索方法
通过Session类的`get()`和`load()`方法加载指定OID的对象       
通过`HQL`、`QBC`等检索方法加载满足条件的对象             

检索方法主要分为两类，一类是通过Session类的`get()`和`load()`方法加载指定OID的对象；只能够按照ID进行查询；第二类就是通过Hibernate所提供的HQL或者QBC等专业的检索方式来加载满足条件的对象；    

#### 检索的作用域

- 类级别：           
作用于当前对象。        
对当前对象是立即检索，还是延迟检索。默认延迟检索。只对`load()`有效。               

- 关联级别：  
对关联对象是立即检索、延迟检索还是迫切左外连接检索。    
默认延迟检索。对所有检索方法有序。               

检索按照作用域的不同，可以分为`类级别的作用域`和`关联级别的作用域`；                 
所谓关联级别的作用域，就是指我们在检索对象的时候，对其关联对象是立即检索，延迟检索、还是迫切左外连接检索；默认将采用延迟检索；该检索作用域对所有的检索方法有效；不管是`get()`、`load()`，还是`HQL`、`QBC`等检索方法；          

还有一种是类级别的作用域，它将只作用于当前对象，注意而不是关联对象；用来决定对当前对象是立即检索还是延迟检索；注意，类级别的作用域不包括迫切左外连接检索；默认也是延迟检索；该作用域的检索只对`load()`有效；注意只对`load()`有效，而对`get()`、`HQL`或者`QBC`等检索方法无效；   

我们还是通过一个例子来说明：            
比如，当我们加载班级信息的时候，对学生信息是立即检索、延迟检索还是迫切左外连接检索。这属于关联级别的作用域；它用来决定对关联对象，学生对象执行哪种检索策略；这其中还存在另外一个问题；当我们在加载班级信息的时候，对班级信息，也即是当前对象是立即检索呢还是延迟检索；这个作用域就属于类级别的作用域；          



#### 类级别的检索策略：
该策略只对`load()`有效；      

##### get()和load()的比较：    

- 相同点：                
按照OID查询指定对象；都支持并且只支持按照OID进行查询；       
如果按照OID进行查询，推荐使用`load()`或`get()`实现，而不是使用`HQL`或者`QBC`等进行间接查询；按照OID进行查询使用这两种方式效率更高一些；；  

- 区别：  
区别主要表现在两个方面：    

第一：              
当数据库中不存在指定ID记录的时候处理方式不同；            
记录不存在时处理方式不同。`get()`返回null，也就是空；`load()`抛出HibernateException异常。     

第二：   
`load()`可以返回实体的代理类实例，`get()`永远都是直接返回实体类。或者说`load()`方法支持延迟加载，而`get()`总是立即加载。     
即：          
`get()`只支持立即加载，而`load()`既支持立即加载也支持延迟加载；        
或者说，当使用`get()`查询的时候，永远都是直接返回实体类对象，而采用`load()`进行查询，立即加载将返回实体类对象，延迟加载将返回实体类的代理类对象；            


只对`load()`有效，只有立即加载和延迟加载；      
我们来看`load()`的执行，在立即加载和延迟加载情况下有什么不同；         
测试代码：     
```java
Grade grade =(Grade)session.load(Grade.class,110701);  //1
System.out.println(grade.getGid());   //2
System.out.println(grade.getGname());   //3
```

在测试代码中，一共有三行，已经进行了编号；             
该测试编码的作用就是，使用`load()`来加载编号为110701的班级信息；然后，输出其班级编号和班级名称；     
在立即加载情况下，`load()`的执行过程：          

因为是立即加载，所以在执行`load()`的时候，将会立刻产生相应的select语句并查询数据库；        
问题：                
>是在执行测试语句中的哪一条语句的时候，产生了select语句来查询数据库呢？       
很显然，这个时候是在执行第一条语句的时候，也就是我们调用`load()`的时候会查询数据库；然后，得到Grade类的对象；该对象中已经包括了班级的全部信息；然后，我们在输出其编号和名称的时候将无需再次查询，直接从查询结果Grade对象中获取相应的数据就可以；我们来看配置代码：         
配置代码如下：           
立即加载配置代码：     
```
<class name="com.pb.hibernate.po.Grade" table="GRADE" lazy="false">
```

因为类级别的检索策略默认是延迟加载，所以我们要进行立即加载的话需要进行设置；需要在Grade类的映射文件中进行设置；
在其`<class>`标签内部，添加lazy属性，并将其值设置为false；表示立即加载；        

- 延迟加载：     
将不执行select语句，而是创建Grade类的代理类实例，仅初始化其OID属性；       
访问Grade类的OID属性时不会查询查询数据库；             
第一次访问Grade类的非OID属性时查询数据库；             

延迟加载情况下，三行测试代码的执行过程：                  

>在我们执行第一条语句通过load()加载班级信息的时候，会不会执行数据库查询呢？        
此时将不执行数据库查询，而是创建班级类的一个代理类的实例；这个代理类就是班级类的一个子类；包括班级类的全部属性和方法；在这一步仅初始化其OID属性；也就是将班级编号110701赋值给代理类的id属性；        
>那么在执行第二条语句，输出班级编号的时候，会不会查询数据库呢？              
此时也不会查询数据库，因为在代理类中已经包括了班级的编号信息；          
在执行第三步，显示班级名称的时候，才会去查询数据库；因为此时代理类中并不包含班级的名称信息；也就是说，在第一次访问班级类的非OID属性的时候才会去查询数据库；这时，就是延迟加载；        
配置代码如下：          
延迟加载配置代码：        
```
<class name="com.pb.hibernate.po.Grade" table="GRADE" lazy="true">

或者
<class name="com.pb.hibernat.po.Grade" table="GRADE">
```
在班级类的映射文件中，需要在<class>标签中将其lazy属性设为true；       
因为其默认检索策略就是延迟加载，所以也可以不书写lazy属性；                



立即加载配置代码：       
```
<class name="com.pb.hibernate.po.Grade" table="GRADE" lazy="false">
```


延迟加载配置代码：        
```
<class name="com.pb.hibernate.po.Grade" table="GRADE" lazy="true">
或者
<class name="com.pb.hibernat.po.Grade" table="GRADE">
```

**强调**：
	类级别的作用域只对load()有效；
	它用来解决当前对象是立即加载还是延迟加载的问题；



##### 关联级别的检索策略：
在这里，以一对多的关联关系为例，进行讲解；     
测试代码：      
```
List<Grade> list = session.createQuery("from Grade").list();
```
测试代码将查询所有的班级信息，并将其内容放入一个List类型的集合对象中；  

- 延迟检索：延迟加载当前对象的关联对象                       
在延迟加载情况下，该条语句的执行过程是怎样的？ ：         
既然是延迟加载，将只加载班级信息，而不加载其关联的学生信息；         
我们需要在班级类的映射文件中进行配置；        
需要在映射students属性的<set>标签内添加lazy属性，并将其值设置为true，表示延迟加载；因为默认就是延迟加载，所以我们也可以不添加lazy属性；          
配置代码         
``	`
<set name="students" cascade="save-update" inverse="true" lazy="true">
或者
<set name="students" cascade="save-update" inverse="true" >
```

最后，该条测试语句将只产生一条SQL语句，将仅执行一条查询语句：           
查询语句：     
```sql
select * from GRADE
```
从SQL语句中，可以看到，此时只查询了班级信息，而没有查询其关联的学生信息；         


- 立即检索：立即加载当前对象的关联对象         
下面来看一下同一条测试语句在立即检索情况下的执行过程；         
测试代码：    
```
List<Grade> list = session.createQuery("from Grade").list();
```

此时，将不仅加载班级信息；而且，还会同时加载与班级信息相关联的学生信息；        
首先，还是要在班级的映射文件中进行配置，在其与学生类建立关联的<set>标签内部，添加lazy属性，并将其值指为false，表示立即检索；           
配置代码：      
```
<set name="students" cascade="save-update" inverse="true" lazy="false">
```
该条测试语句最终生成的SQL语句，如下所示：             
查询语句     
```sql
select * from GRADE
select * from STUDENT s where s.gid = 110701
select * from STUDENT s where s.gid = 110702
select * from STUDENT s where s.gid = 110703
```

一共生成了四条SQL语句；     
首先，查询班级信息需要一条SQL语句；         
因为，该数据库中一共包括班级的三条记录；          
所以，下面会立即按照班级的编号查询该班级的所有学生信息；从而产生了三条查询语句；        
我们看这三条语句就是分别按照不同的班级编号进行查询的；                

如果在班级数据库中一共有n条班级信息的话，那就会另外产生n条的查询语句；所以，我们执行立即检索，将一共产生1+n条的查询语句；           
即，           
需要执行`1+n`条select语句；         


##### batch-size的作用

可以看到，在班级表中如果有n条班级记录的话，会额外产生n条select语句，分别按照班级编号进行查询，查询相应的学生信息；select语句明显太多了；          
实际上，在Hibernate中还提供了批量检索功能，可以批量的检索数据；这么做就可以减少执行select语句的条数，从而减少访问数据库的次数，提高软件的性能；这就需要用到`batch-size`属性；      
`batch-size`属性，其默认值为 1；          

还是以同一条测试语句为例，查询所有的班级信息；         
测试代码：      
```
List<Grade> list = session.createQuery("from Grade").list();
```

首先，需要在班级类的映射文件中配置批量检索；       
配置代码：       
```
<set name="students" cascade="save-update" inverse="true" lazy="false" batch-size="3">
```

我们要在<set>标签中添加`batch-size`属性，并将其值设置为3；含义就是，将一次查询三个班级的数据；而不是一次查询一个班级的数据；将该属性设置为3后，我们再次执行测试语句，将产生两条查询语句：   

将执行两条查询语句     
```sql
select * from GRADE
select * from STUDENT s where s.gid in (110701,110702,110703)
```

第一条查询语句查询出了班级的信息，第二条查询语句一次查出了三个班级的学生信息，减少了select语句的数目；   
我们再来修改`batch-size`的数目，将其值由3改为2；      
配置代码：        
```
<set name="students" cascade="save-update" inverse="true" lazy="false" batch-size="2">
```
在这种情况下，再执行测试代码，将产生三条SQL语句；         

将执行三条查询语句        
```sql
select * from GRADE
select * from STUDENT s where s.gid in (110701,110702)
select * from STUDENT s where s.gid = 110703
```

第一条，用来查询班级信息；         
然后，通过一条SQL语句查询出两个班级的学生信息；           
因为我们的数据库表中只有三条记录；            
最后一条SQL语句中，再查询出最后一个班级的学生信息；           

问题：      
>既然批量检索可以大大的减少查询语句的数量，我们是不是可以把批量检索的值设的很大，比如：10、20、50、100、1000；

注意：        
	如果将批量检索的数值设置的太大，可能会适得其反；       
	其默认值为1，合理取值一般在2 - 10 之间；        
	一定要注意这个合理的取值范围；       


##### fetch的作用
fetch的作用和取值：       
在Hibernate中，该关键字用来表示，加载关联对象时查询语句的形式以及加载关联对象的时机；    
fetch有三个取值，分别为： `select、subselect和join `
```
select : 加载关联对象时通过select语句实现。默认值；
subselect : 加载关联对象时通过带  子查询语句 实现；
join : 执行立即检索，采用迫切左外连接检索所有关联对象，lazy属性将被忽略；
```

当取值为select的时候，表示通过select语句来加载关联对象；这是其默认值；      
在前面加载班级类的关联对象学生类的时候，就是通过select语句来实现的；其实在那个时候，就已经使用了fetch关键字的默认值select；             
第二个取值是subselect，将通过带 子查询 的语句来加载其关联对象；            
第三个取值是join，将采用迫切左外连接来检索其所有的关联对象；              
也就是说，将采用一条左外连接查询语句来检索其关联对象；            
既然是迫切左外连接检索，肯定是立即检索而不是延迟检索；             
关于fetch关键字的作用及其取值，需要强调两点：                  
- 第一点：     
select和subselect属性值，适用于立即检索和延迟检索均可以；而，join属性值仅适用于立即检索；      

- 第二点：      
select和subselect属性值，决定了加载关联对象时查询语句的形式，是select语句，还是 子查询语句；而，join属性值，不仅决定了加载关联对象时查询语句的形式，是一条左外连接查询语句；而且还决定了加载关联对象的时机；是立即检索而不是延迟检索；            



>fetch关键字分别取其三个属性值时会有什么不同：

```
fetch="select"
```

还是使用相同的测试代码，查询所有的班级信息：        
测试代码：       
```
List<Grade> list = session.createQuery("from Grade").list();
```

在班级类的映射文件中进行配置；          
在students属性的<set>标签中添加fetch属性，取值为select，或者干脆不做任何的设置，因为select就是其默认值；    
配置代码：         
```
<set name="students" cascade="save-update" inverse="true" lazy="true" fetch="select">
或者
<set name="students" cascade="save-update" inverse="true" lazh="true">
```
执行该测试语句最终将产生四条SQL语句：        
使用多条select语句查询关联对象       
```sql
select * from GRADE
select * from STUDENT s where s.gid = 110701
select * from STUDENT s where s.gid = 110702
select * from STUDENT s where s.gid = 110703
```

其中，使用了三条select语句来查询其关联对象Studnet；   

这就是fetch关键字取select的作用：         
用来说明加载其关联对象Student时查询语句的形式是通过select语句；          


```
fetch = "subselect"
```

测试代码：      
```
List<Grade> list = session.createQuery("from Grade").list();
````
还是查询所有的班级信息；        
然后，在班级类的映射文件中将fetch关键字的取值变为subselect；         
配置代码：            
```
<set name="students" cascade="save-update" inverse="true" lazy="true" fetch="subselect">
```

执行该测试语句，所产生的SQL语句：      
 
使用子查询语句查询关联对象    
```sql
select * from GRADE
select * from STUDENT s where s.gid in(select g.GID from GRADE g)
```

一共产生了两条SQL语句；       
第一条查询所有的班级信息；       
第二条查询其关联对象； 采用了带  子查询 的语句；     
通过括号内的子查询语句得到了所有的班级编号信息；       
>这么做的好处：          
明显的减少了查询语句的数量；       

```
fetch="join"
```

在这里不能再使用原来的测试代码，因为原来的测试代码是使用HQL语言查询所有的班级信息，而fetch关键字取值为join时，对HQL查询是不起作用的；所以，在这里，将通过`get()`或者`load()`，来查询编号为110701的班级信息；        
测试代码：      
```
//List<Grade> list = session.createQuery("from Grade").list();
Grade grade = (Grade)session.get(Grade.class,110701);
//Grade grade = (Grade)session.load(Grade.class,110701);
```

同样，需要在班级类的映射文件中，添加fetch属性，并将其值设置为join；  
配置代码：       
```
<set name="students" cascade="save-update" inverse="true" lazy="false" fetch="join">
```

>执行该测试语句，最终将产生什么样的SQL语句呢？      

使用一条左外连接语句立即查询关联对象     
```sql
select * from GRADE g
left outer join STUDENT s
on g.GID = s.gid
where g.GID = 110701
```

它将产生一条左外连接查询语句；        

强调：             
	第一点：      
	join属性值仅适用于立即查询，而不适用于延迟查询；      
	当fetch取值为join时，lazy属性的值将被忽略；不管其取值是false还是true；        
	第二点：      
	fetch取值为join，对HQL查询是不起作用的；    
	也就是说，如果我们要验证fetch取值为join时的执行情况，不能够使用HQL进行查询；              
