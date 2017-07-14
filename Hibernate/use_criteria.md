# 使用Criteria查询数据
概述：        
SQL可移植性差，Criteria可移植性好；           
HQL语言是非常类似于SQL语言的一种查询语言，也就是说使用了特定于某一个数据库的SQL语句，那么所编写的程序本身就会依赖于特定的数据库，这就降低了程序的可移植性，也就是说，如果更换数据库，程序就无法运行。        
那么，现在，Hibernate为我们解决了这个问题。          
Criteria查询，就可以避免因为使用SQL语句所带来的不便；             
即使你不懂SQL语言，也可以使用Criteria查询方式来检索出你想要的数据。                 


## Criteria本身只是查询容器

Criteria查询又称作对象查询；       

Criteria采用面向对象的方式封装查询条件，并提供了Restrictions等类别做辅助。可以使编写查询代码更方便，而且代码更易读；   
这也就是说，Criteria查询通过面向对象化的设计，将数据查询条件封装为一个对象；当然这个对象主要是Criteria对象；     
我们可以把Criteria查询看做是传统SQL的对象化表示。           
如下代码：      
```
Criteria criteria = session.createCriteria(Login.class);
```

这里的criteria实例实际上是这样一条SQL语句，也就是：     
```sql
Select * from login ;
```
criteria实例就是将这条SQL语句封装了起来，以一个对象的形式展现；           
Hibernate在运行期会根据Criteria中指定的查询条件生成相应的SQL语句；           
这种方式的特点是比较符合Java程序员的编码习惯的，并且具备清晰的可读性；正因为这样，许多ORM框架中都提供了类似的实现机制；                   
Criteria查询是一个非常易于理解的查询手段；而且使用起来也非常简单；     


Criteria查询采用面向对象的方式封装查询条件；           
也就是对SQL语句进行封装；          
Criteria采用对象的方式来组合各种查询条件；           
并且，由Hibernate自动产生SQL查询语句。               

使用Criteria需要首先创建一个Criteria对象；          
与创建Query对象的语法很相似，但是需要传入的参数是对应实体类的类型对象；            
Criteria建立后，如果不指定任何的查询条件；那么，查询实体对象所对应数据表中所有的数据。然后，可以使用Criteria的`list()`方法获得数据；`list()`方法就返回一个List实例。         
如下代码，为不带查询条件的查询方式：      
```java
List<Login> result = null;
SessionFactory sessionFactory = null;
Session session = null;
try {
       sessionFactory = new Configuration().configure().buildSessionFactory();
       session = sessionFactory.openSession();
       Criteria criteria = session.createCriteria(Login.class);
       result = criterial.list();
       for(Login login:result) {
        System.out.println("用户名：" +Login.getUsername()+"密码："+Login.getPasssword()+"年龄："+Login.getAge());
}
} catch (HibernateException e) {
 e.printStackTrace();
} finally{
    session.close();
    sessionFactory.close();
}
```

#### 限制结果集内容

Criteria本身只是查询条件的容器；       
如果想要设定查询条件，则要使用Criteria的`add()`方法加入“条件实例”；      
条件实例是由Restrictions的各种静态方法返回的；           
返回的每个Criteria实例都代表着一个条件；              

例如：    
查询username为“马达”的用户信息，代码如下：         
```
Criteria criteria = session.createCriteria(Login.class);
criteria.add(Restrictions.eq("username","马达"));
result = criteria.list();
```  

在这段代码中，我们创建Criteria对象，以及使用Criteria对象的`list()`方法获得数据的代码都没有变；只是在中间又加了一条语句；在这里使用Criteria对象的`add()`方法，然后在`add()`方法中加入了Restrictions对象的`eq()`方法来设置查询条件；Restrictions对象的`eq()`方法可以传入两个参数，第一个参数用来代表实体类属性的名称，而不是真正的数据库字段的名称，但是属性名称通过配置文件会映射到数据库中的某个字段，接下就管它叫字段名，但实际上它对应的是实体类的属性名称。第二个参数代表字段值；                      
那么，现在这段代码就相当于下面这条SQL语句：     
```sql
Select * from login where username='马达'
```
也就是查询login表中的所有信息，然后查询条件是`username = '马达'`

Restrictions对象的`eq()`方法，就是用来判断字段值是否等于传入的参数；     
 
在这里，就是查询 `username='马达'` 的记录；      
**记住**：      
	Restrictions对象的eq()方法，是用于表示 "等于"条件的；
	le()方法表示“小于等于”条件，ge()方法表示“大于等于”条件；

那么，上面这段代码就是采用面向对象的方式封装查询条件，并且采用对象的方式来组合各种查询条件，最终由Hibernate来自动生成SQL语句；              
也就是说，我们如果没有学过SQL语句，使用Hibernate框架中的Criteria查询方式，也可以完成查询数据的功能。         



Restrictions对象的`eq()`方法，就是用来判断字段值是否等于传入的参数；       
Restrictions对象的`eq()`方法，是用于表示 "等于"条件的；           



在前面两个示例中，使用Criteria查询数据要么是无条件的，要么是单条件的，如果有多个查询条件，Criteria如何来实现呢？   

如果有多个查询条件，仍然可以使用Criteria查询机制来实现；       
Criteria查询机制可以实现多个查询条件；           

例如：         
>我们要查询登录表中的用户年龄大于等于21，并且小于等于22的所有用户信息，那么使用Criteria查询机制如何来实现呢？
如下代码：     
```
Criteria criteria = session.createCriteria(Login.class);
criteria.add(Restrictions.le("age",22));
criteria.add(Restrictions.ge("age",21));
result = criteria.list();
```

在这段代码中，添加了两个查询条件：          
第一个查询条件使用了Restrictions对象的`le()`方法；        
它表示年龄也就是age字段小于等于22；        
第二个条件使用了Restrictions对象的`ge()`方法，       
它表示年龄字段大于等于21；           
通过两次调用Criteria的`add()`方法增加了两个查询条件；         
这段代码也就相当于下面这条SQL语句：               
```sql
Select * from login where age<=22 and age>=21
```
也就是查询login表中所有的信息，查询条件是 `age<=22` 并且 `age>=21`      
通过这个例子，就已经知道了：             
在使用Criteria查询时如何添加多个查询条件，但是，我们也注意到，添加这两个查询条件是 与 的关系，也就是 并且 的意思，我们是用 and 来连接的两个查询条件；如果我们添加更多的`add()`方法加入查询条件，Criteria同样也是使用add来组合这些查询条件。      


>le()方法表示“小于等于”条件，ge()方法表示“大于等于”条件；


现在已经知道了如何来添加查询条件，并且用 与 的关系来组合查询条件；  
另外，我们如果要查询年龄在21到22之间的记录，也就是查询在一定范围之内的记录，在使用SQL语句时通常使用between来完成。那么，      

	Restrictions对象 也同样 提供了 between方法；

可以将刚才的代码修改一下，如下代码：       
```
Criteria criteria = session.createCriteria(House.class);
criteria.add(Restrictions.between("age",21,22));
result = criteria.list();
```

在这里，使用了Restrictions对象的between方法，为between方法传入的参数也很简单。         
第一个是字段名，后两个参数用于确定取值范围；        
这段代码就相当于如下的SQL语句：     
```sql
Select * from login where age between 21 and 22
```

现在我们已经知道了如何使用and组合多个查询条件，也知道了如何使用between来限定查询范围；           
但是，我们在学习数据库的时候，在SQL语句中可以使用 或 的关系来组合查询     
条件，也就是使用 `or` 来连接查询条件；               
接下来学习一下使用Criteria查询时，如何使用 or 来连接查询条件；         


##### Restrictions对象的or方法：

如果要使用 `or` 来连接查询条件，可以使用Restrictions对象的 `or`方法来实现；           
例如：       
>查询年龄等于20，或者用户名中含有 “马”字的记录。         
那么，就可以通过如下的方法来实现：      
```
Criteria criteria = session.createCriteria(House.class);
criteria.add(Restrictions.or(
            Restrictions.eq("age",20),
            Restrictions.like("username","%马%")));
result = criteria.list();
```

在这里可以看到，我们使用了Restrictions对象的or方法，为or方法传入了两个查询条件，那么，查询条件就是：     
年龄等于20、或者用户名中包含 “马”字。         


Restrictions对象的like方法就能够实现SQL语句中的like功能 

我们使用Restrictions对象的like方法，能够实现SQL语句中的like功能；        

Hibernate提供了相当多的查询限定方法，但尤其有用的是可以允许程序员直接使用SQL语句；         
例如：             
可以将上面的代码替换为如下代码：       
```
Criteria criteria = session.createCriteria(House.class);
criteria.add(Restrictions.sqlRestriction("age=20 or username like '%马%' "));
result = criteria.list();
```

可以看到，在这里可以直接使用SQL语句来编写查询条件，这样可以使我们更加灵活的限制查询结果集。   



Restrictions对象常用的限定查询的方法：           

|方法  |                        说明|
|----|-----|
|Restrictions.eq()   |       对应sql的等于('=')|
|Restrictions.gt()          |对应sql的大于('>')|
|Restrictions.ge()          |对应sql的大于等于('>=')|
|Restrictions.lt()          |对应sql的小于('<')|
|Restrictions.le()          |对应sql的小于等于('<=')|
|Restrictions.between()     |对应sql的between子句|
|Restrictions.like()        |对应sql的like子句|
|Restrictions.in()          |对应sql的in子句|
|Restrictions.and()         |对应sql的and|
|Restrictions.or()          |对应sql的or|


现在，我们已经学会了如何使用Criteria查询数据，接下来，来学习如何对查询结果进行排序：     
 
#### 结果集排序：   
使用Order关键字对Criteria查询结果进行排序；    

我们可以使用Order关键字对Criteria查询结果进行排序；        
我们在SQL中实现排序的关键字为asc和desc，也就是升序和降序。        
对应于 Order的方法也就是 `asc()`和 `desc()`；     

例如：     
>我们要查询所有用户的信息，并且按年龄降序排列。
那么就可以通过如下代码来实现：         
```
Criteria criteria = session.createCriteria(House.class);
criteria.addOrder(Order.desc("age"));
result = criteria.list();
```

**注意**：
	在加入Order条件时，使用的是addOrder()方法，而不是add()方法；
	然后，我们对年龄，也就是age字段进行降序排列；
	那么，升序排列和降序排列相似，将desc换成asc即可。


##### Example示例查询

我们已经学习了Criteria查询中，如何使用Restrictions添加查询条件；但是，在使用Criteria查询时，设定查询条件并非一定要使用Restrictions。       
例如：     
属性条件很多，我们使用Restrictions也不是很方便。 
假设如果已经有一个对象，那么就可以根据这个对象作为查询的依据，查询出属性与之类似的对象。也就是：          
 
>依照已有对象，查询与其属性相同的其它对象。

这种方式，也可以称为 根据示例查询。             

如下代码：       
```java
List<Login> result = null;
SessionFactory sessionFactory = null;
Session session = null;

Login user = new Login();
user.setAge(21);

try {
       sessionFactory = new Configuration().configure(). buildSessionFactory();
       session = sessionFactory.openSession();
       Criteria criteria = session.createCriteria(Login.class);
       
       criteria.add(Example.create(user));

       result = criterial.list();
       for(Login login:result) {
        System.out.println("用户名：" +Login.getUsername()+"密 码："+Login.getPasssword()+"年龄："+Login.getAge());
}
} catch (HibernateException e) {
 e.printStackTrace();
} finally{
    session.close();
    sessionFactory.close();
}
```

在这里，首先声明一个Login对象user，并为它的age属性赋值；        
那么，现在，user这个引用变量的age属性就有了一个值；        
接下来，其它的代码没有变；只是在增加查询条件的时候做了一个改动。            

在`add()`方法中加入了Example对象的`create()`方法，并为`create()`方法传入声明的user这个引用变量；      
那么，我们执行程序就会查询出所有年龄等于21的记录。           
虽然，我们只为一个属性赋了值，其它的属性都是空值，但并不影响我们获取查询结果。这就是说：        

>Hibernate在自动生成SQL语句的时候，将自动过滤掉对象的空属性，根据有非空属性值的属性生成查询条件。    

如果我们要想更精确的查找，可以在实例化user引用变量时多设置几个属性值。         


#### 统计、分组、限定返回数据行数
在学习数据库的时候，可以使用SQL语句中的聚合函数对查询结果进行统计、分组等操作，接下来学习在Criteria查询机制中如何实现这些功能。         

>使用投影(Projections)来实现SQL语句中的聚合函数的功能

我们在Criteria中可以使用投影来实现SQL语句中的聚合函数的功能，投影就是Projections；      
所以，首先，我们要引入`org.hibernate.criterion.Projections`资源。              

例如：         
我们要统计所有记录数量。         
我们就可以通过这段代码来实现：     
```
Criteria criteria = session.createCriteria(Login.class);
criteria.setProjection(Projections.rowCount());
result = criteria.list();
System.out.println("记录数："+result.iterator().next());
```

在这段代码中，统计记录数时，首先使用了Criteria对象的`setProjection()`方法；然后，为该方法传入了Projections对象的`rowCount()`方法来统计记录数。         
在这里还可以统计平均值、最大值,分组等等，如下代码：             

平均值：      
```java
Criteria criteria = session.createCriteria(Login.class);
criteria.setProjection(Projections.avg("age"));
result = criteria.list();
System.out.println("平均年龄："+result.iterator().next());
```

最大值：    
```java
Criteria criteria = session.createCriteria(Login.class);
criteria.setProjection(Projections.max("age"));
result = criteria.list();
System.out.println("最大年龄："+result.iterator().next());
```

分组：         
```java
Criteria criteria = session.createCriteria(Login.class);
criteria.setProjection(Projections.groupProperty("username"));
result = criteria.list();
Iterator it = result.iterator();
while(it.hasNext()) {
 System.out.println("用户名："+it.next());
}
```
实现这些功能与统计记录数的方法非常相似，使用起来也非常简单；        

Projections常用聚合查询方法           

|方法 |                                    说明|
|-------|------|
|Projections.rowCount()          |统计记录数|
|Projections.avg()               |统计平均值|
|Projections.max()               |统计最大值|
|Projections.min()               |统计最小值|
|Projections.groupProperty()     |分组|
|Projections.count()             |统计某一字段的非空记录数|
|Projections.sum()               |针对某一字段求和|


现在我们已经知道了Criteria查询机制中的一些聚合查询方法，但是在使用起来时，可能感觉比较麻烦；    
因为，我们每次实现一个功能的时候，都要重新创建一个Criteria对象，并且调用`list()`方法返回数据。      
那么，如果我们同时要实现多个聚合查询功能，就会出现许多冗余的代码；       
下面，来学习一下projections的另一个方法：               

	projectionList() ：实现一次查询调用多个聚合查询方法。

	projectionList()方法，可以实现一次查询调用多个聚合查询方法。

实例：      
>我们要统计登录表中所有的记录数、用户的平均年龄、以及最大年龄；       
如下代码：   
```
Criteria criteria = session.createCriteria(Login.class);
criteria.setProjection(Projections.projectionList()
.add(Projections.rowCount())   //统计所有记录数
.add(Projections.avg("age"))   //统计平均年龄
.add(Projections.max("age"))   //统计最大年龄
);
result = criteria.list();
```

首先，仍然是使用Criteria对象的`setProjection()`方法；  
然后，加入了Projections对象的`projectionList()`方法，         
接下来，添加了三个`add()`方法来分别进行统计；         
这样，就实现了一次查询统计出多个结果。             


无论是学习SQL，还是Hibernate的HQL查询方式，我们都学习过限定查询返回的数据行数。Criteria查询当然也会提供这种功能：  

	Criteria的setMaxResults()方法，可以限定查询返回数据的行数；
	Criteria的setFirstResult()方法，设定查询返回结果的第一行数据的位置
	Criteria的setMaxResults()方法，可以限定查询返回数据的行数；
然后，再配合：       
	Criteria的setFirstResult()方法，设定查询返回结果的第一行数据的位置；
那么，就可以实现简单的分页了；        

例如：        
传回第1行数据之后的两行数据，也就是从第2行到第3行这两行数据。    
代码如下：       
```
Criteria criteria = session.createCriteria(House.class);
criteria.setFirstResult(1);
criteria.setMaxResults(2);
result = criteria.list();
```
首先，使用`setFirstResult()`来设定返回结果的第一行数据的位置，在这里设为1；就是表示从第一条记录之后开始；然后，使用`setMaxResults()`来设定返回数据的行数。在这里设为2，表示返回两条记录；      
那么，现在，获取的查询结果就是第二和第三条记录。            


### 使用命名查询
概述：        
我们在前面已经学习了Criteria查询机制，而且在前面的课程中，也详细的学习了HQL查询机制，这两种查询机制的应用场合就要看程序员对SQL语言的理解程序而定；但是，我们选用HQL查询机制的话，我们都知道HQL查询语句都嵌入在程序代码中；如果HQL查询语句很复杂，并且跨越多行，那么就不得不进行判断并使用StringBuffer对象连接HQL语句。这样做可能会影响程序代码的可读性和可维护性，也许我们在实际工作中也会遇到这样的编码规范。例如：代码中不允许出现HQL语句；我们已经知道,HQL语句混杂在代码之间将破坏代码的可读性；并使得系统的可维护性降低；          

为了避免这样的情况出现，在使用JDBC编程时，我们采取将SQL配置化的方式将SQL保存在配置文件中，需要调用的时候再进行读取。
而，Hibernate则允许在映射配置文件中定义字符串形式的查询语句。        
将这种查询方式称为  **命名查询**；           

接下来，使用命名查询实现用户登录，来体会Hibernate命名查询的优势；        


##### 命名查询--ORM映射文件配置
要使用命名查询；首先，我们要修改用户实体的映射文件；    
关键代码：        
```	xml
<hibernate-mapping>
 <class name="com.test.Login" table="LOGIN">
    <!--..省略其他配置  -->
 </class>
 <query name="loginUser">
   <![CDATA[
    from Login login where login.username =:username and  login.password =: password
    ]]>
  </query>
</hibernate-mapping>
```

这是Login实体的映射配置文件，也就是`Login.hbm.xml`文件；   
在这个映射文件中，我们添加了一个<query>元素；           
<query>元素用于定义一个HQL查询语句，它和<class>元素并列；       
以上HQL查询语句被命名为 "loginUser"；      
也就是我们为<query>元素命名一个名称。         
那么，在这个映射配置文件中保存HQL语句的格式是固定的；         

>以<![CDATA[HQL]]>方式保存HQL语句； 

总结：         
映射配置文件主要就是使用<query>元素定义HQL语句，并且<query>元素要和<class>元素并列，然后为HQL查询语句命名；接下来，按照格式编写HQL语句就可以了；          

	在这个映射配置文件中保存HQL语句的格式是固定的；
	以<![CDATA[HQL]]>方式保存HQL语句；


现在，配置文件就已经编写完毕，接下来在程序中实现登录验证功能。          


我们在程序中可以使用，Session对象的`getNamedQuery()`方法获取在映射配置文件中添加的HQL查询语句。    
代码如下：    
```java
public static void main(String[] args) {
SessionFactory sessionFactory = null;
Session session = null;

try {
  sessionFactory = new Configuration().configure().buildSessionFactory();
  session = sessionFactory.openSession();

  Query query = session.getNamedQuery("loginUser");
  Login lo = new Login();
  lo.setUsername("马达");
  lo.setPassword("123456789");
  query.setProperties(lo);
  List result = query.list();

  Iterator it = result.iterator();
  if(it.haxNext()) {
	Login login = (Login)it.next();
	System.out.println("欢迎："+login.getUsername()+"   年龄："+login.getAge());
}
}catch (HibernateException e) {
  e.printStackTrace();
} finally {
  session.close();
  sessionFactory.close();
}

}
```

在这段代码中，我们调用Session对象的`getNamedQuery()`方法，并将映射配置文件中为HQL查询语句命名的名称传入该方法。这样，我们就获得了一个Query对象；接下来，我们再实例化一个Login对象；然后，将用户登录的信息保存到该对象中；然后，调用Query对象的`setProperties()`方法，并把Login对象传入该方法，也就是将用户登录信息作为查询条件，组合到HQL查询语句中。
最后，调用Query对象的`list()`方法，来获取查询结果。      
那么，这样，我们就在程序中看不到查询语句了；       


##### 执行NativeSQL并获取结果

使用SQLQuery执行本地SQL：          
大家都知道HQL作为Hibernate的查询语言，提供了丰富的特性和强大的数据查询功能；不过，HQL并不能涵盖所有查询特性，有时我们不得不借助SQL以达到我们期望的目标；      
这就是接下来要学习的 NativeSQL；        

>NativeSQL， 也就是 本地SQL        

Hibernate对本地SQL查询提供了内置的支持，这对程序员来说是非常有用的；           
这也能够帮助Hibernate的初学者，把原来直接使用SQL JDBC的程序迁移到基于Hibernate应用的道路上。        


>使用SQLQuery执行本地SQL

下面通过一个示例来了解如何使用本地SQL查询；   
例如：         
我们要使用本地SQL查询用户名为 "马达"的用户信息。       

为了把SQL语句查询返回的数据映射为对象；因此，需要在SQL语句中为实体设置别名。     
代码如下：    
```java
public static void main(String[] args) {

SessionFactory sessionFactory = null;
Session session = null;

try {
  sessionFactory = new Configuration().configure(). buildSessionFactory();
  session = sessionFactory.openSession();

  String sql = "select {l.*} from login l where l.username='马达' ";
  SQLQuery query = session.createSQLQuery(sql).addEntity("l",Login.class);
  List result = query.list();

  Iterator it = result.iterator();
  if(it.haxNext()) {
	Login login = (Login)it.next();
	System.out.println("欢迎："+login.getUsername()+"   年龄："+login.getAge());
}
}catch (HibernateException e) {
  e.printStackTrace();
} finally {
  session.close();
  sessionFactory.close();
}

}
```

在这段代码中，我们在SQL语句中为表login起了一个别名l，这个 l 同样也是我们指定的实体对象的别名；然后，我们使用 `{}`表示引用实体的属性；
在这里使用 `l.*`表示所有的属性；我们在执行本地SQL查询的时候，将不再使用Query接口了，而使用SQLQuery接口；使用Session的`createSQLQuery()`方法，        
来利用传入的sql参数获得SQLQuery实例；在使用这个方法的时候，还需要传入查询的实体类；因此，需要配合SQLQuery的`addEntity()`方法一起使用。`addEntity()`方法是将别名与实体类关联在一起。       


##### 命名SQL查询
>我们现在已经知道了如何通过SQLQuery使用本地SQL查询，我们在前面也讲了HQL语句的命名查询，那么本地SQL可以使用命名查询吗？
答案是肯定的。         
本地SQL查询也可以使用命名查询的方式实现。        
如果我们要使用本地SQL的命名查询，也需要将查询语句定义在映射配置文件中；然后，就可以像调用命名HQL查询一样调用本地SQL的命名查询。           

	配置映射配置文件；

下面，来看一下如何来配置映射配置文件：      
代码如下：     
```xml
<hibernate-mapping>
  <class name="com.test.Login" table="users">
    <!-- ...省略其他配置 -->
  </class>
  <sql-query name="findUser">
     <![CDATA[
         select {l.*} from login l where l.username=?
       ]]>
     <return alias="l"  class="com.test.Login"/>
   </sql-query>
</hibernate-mapping>
```
在这段代码中，可以看到本地SQL命名查询与HQL命名查询还是有一些区别的；       
在这里，我们使用 `<sql-query>`元素定义本地SQL查询语句；          
`<sql-query>`与`<query>`元素相同，都是和`<class>`元素并列的；      
我们定义的本地SQL查询语句被命名为 "findUser"，         
SQL语句的编写格式与HQL语句的格式是一样的；               
使用`<sql-query>`元素的子元素`<return>`指定别名与实体类的联系；     
表要和实体类相对应；           
其中，alias属性用于指定别名，class属性用于指定实体类；       
这也就是将查询的结果和实体类进行绑定。                

现在，映射配置文件就已经配置完毕了。           
接下来，看一下在程序中如何来获取查询语句。           


在程序中通过Session对象的`getNamedQuery()`方法调用命名SQL；      

我们在程序中同样也是通过Session对象的`getNamedQuery()`方法来获取该查询语句；       
代码如下：        
```java
public static void main(String[] args) {

SessionFactory sessionFactory = null;
Session session = null;

try {
  sessionFactory = new Configuration().configure().buildSessionFactory();
  session = sessionFactory.openSession();

  Query query = session.getNamedQuery("findUser");
  query.setString(0,"马达");
  List result = query.list();

  Iterator it = result.iterator();
  if(it.haxNext()) {
	Login login = (Login)it.next();
	System.out.println("欢迎："+login.getUsername()+"   年龄："+login.getAge());
}
}catch (HibernateException e) {
  e.printStackTrace();
} finally {
  session.close();
  sessionFactory.close();
}

}
```

在这段代码中，我们同样使用Session对象的`getNamedQuery()`方法获取查询语句；我们为该方法传入了在映射配置文件中为SQL查询语句命名的名称findUser；然后，我们使用Query对象的`setString()`方法为查询语句添加参数，最后就可以使用`list()`方法获取查询结果了。         

本地SQL的命名查询与HQL的命名查询使用方法是非常类似的，实现起来也非常简单。         


#### 定义SQL
我们已经学过了本地SQL的两种使用方式，接下来学习第三种使用方式：   
也就是  定制SQL        

Hibernate能够使用定制的SQL语句来执行`insert`，`update`和`delete`操作；     
映射标记：`<sql-insert>、<sql-update>、<sql-delete>  `    
对应session操作：`save、update、delete`      

Hibernate能够使用定制的SQL语句来执行insert，update和delete操作；      
在Hibernate中，持久化的类和集合已经包含了一套配置期产生的语句，也就是insert语句、delete语句和update语句。这些语句可以通过`save()、update()、delete()`方法来调用。             
然而，在Hibernate中还可以通过使用一些标记来重载这些语句，也就是`<sql-insert>`标记、`<sql-update>`标记和`<sql-delete>`标记；就相当于我们使用方法重载一样；如果我们使用这些标记来编写SQL语句，那么我们再使用session来执行save、update和delete操作的时候就会执行我们自己编写的SQL语句。        
那么，映射标记放在哪里呢？             
要将标记放在映射配置文件中，也就是`Login.hbm.xml`文件里。     
如下代码：        
```xml
<hibernate-mapping>
  <class name="com.test.Login" table="LOGIN" schema="MYHR">
   <id name="username" type="java.lang.String">
     <column name="USERNAME" length="50"/>
     <generator class="assigned"/>
    </id>
<property name="password" type="java.lang.String" >
  <column name="PASSWORD" length="20" not-null="true" />
</property>
<property name="age" type="java.lang.Integer" >
  <column name="AGE" length="20" not-null="false" />
</property>
   <sql-insert>
	insert into login (password,age,username) values(?,?,?)
   </sql-insert>
   <sql-update>
        update login set password = ?,age =? where username=?
   </sql-update>
   <sql-delete>
	delete from login where username=?
   </sql-delete>
  </class>
  
</hibernate-mapping>
```

这是Login类对应的映射配置文件的部分代码；      
我们在这里定义这些映射标记之后，我们再执行`save、update、delete`操作就会执行我们自己定义的这些语句； 
在自定义SQL语句的时候也要注意一些问题：    

- 第一点：      
我们编写的insert和update语句中定义的字段必须和映射文件声明的属性相对应，一个都不能少。     

- 第二点：      
在insert和update语句中，属性出现的顺序必须和映射配置文件中声明的顺序一致。      

- 第三点：      
在insert语句中，主键总是放在最后。       


在程序中调用这些语句：        
如下代码：     
```java
sessionFactory = new Configuration().configure().
                          buildSessionFactory ();
session = sessionFactory.openSession();
tran = session.beginTransaction();

Login login = new Login();
login.setPassword("123456789");
login.setUsername("秦岚");
login.setAge(25);
session.save(login);

tran.commit();
```

这就是在程序中实现我们自定义的insert语句；          
这段代码与我们最初学习Hibernate时，所接触的代码没有任何区别；           
仍然是使用Session对象的`save()`方法来保存Login对象，只不过在这里就不会使用自动生成的SQL语句，而是使用我们自定义的SQL语句。不过，我们自己编写的SQL语句，insert和update中都是包含了所有的字段；        
如果我们不想包含全部的字段，那怎么办呢？     
我们可以在属性定义时加上一些标记，例如：            

可以加上 `insert=" false"` 和 `update=" false"`
如下代码：
```xml
<property name="password" type="java.lang.String" update="false">
  <column name="PASSWORD" length="20" not-null="true" />
</property>
<property name="age" type="java.lang.Integer" insert="false">
  <column name="AGE" length="20" not-null="false" />
</property>
<sql-insert>
	insert into login (password,username) values(?,?)
   </sql-insert>
   <sql-update>
        update login set age =? where username=?
   </sql-update>
   <sql-delete>
	delete from login where username=?
   </sql-delete>
```

我们分别为password属性添加了`update="false"`标记，和age属性添加了     
`insert="false"`标记；       
这就表示，我们插入操作的时候不能插入age字段；修改记录的时候，不能修改password字段；         
相应的，我们也要修改insert和update两条SQL语句；           
我们将insert语句中的age字段去掉，并且将update语句中的password字段去掉就可以了；            


#### Hibernate批处理数据与调用存储过程
概述：          
现在，我们对Hibernate有了一定的认识，并且对Hibernate中的各种查询方式有了一定的了解，但是在现实工作中我们有可能遇到一些特殊的问题；           
例如：            
>我们要对数据库中的某一个表进行大量的插入或是修改操作，那么我们如何来解决呢？
假设我们要向Login表中插入10万条记录，我们首先会想到利用循环来完成，     
例如，如下代码：       
```java
sessionFactory = new Configuration().configure().
                          buildSessionFactory ();
session = sessionFactory.openSession();
tran = session.beginTransaction();
for(int i=1; i<=1000000;i++) {
  Login login = new Login();
  login.setUsername(Integer.toString(i+1000000));
  login.setPassword("111111");
  login.setAge(21);
  session.save(login);
}
tran.commit();
session.close();
```

那么，这种做法理论上是没有错误的；但是在实际运行过程中，这段程序在循环5万次左右的时候就有可能会失败了，并抛出内存溢出的异常。这是由于Hibernate把所有新插入的Login实例，在Session级别的缓存区进行缓存的缘故。这也就是说，在一个Session对象的缓存中，只能存放数量有限的持久化对象；我们又只能等到Session对象处理事务完毕，才能关闭Session对象。从而释放Session的缓存占用的内存。这样就会造成内存溢出的结果。          
所以说，这种大量插入数据的方式有两个缺点：

1.占用大量内存。    
2.频繁访问数据库。降低应用的性能。         

一般来说，我们在进行程序设计时，应该尽量避免在应用层进行批量操作，而应该在数据层直接进行批量操作。     
例如：直接在数据库中执行用于批量插入、更新或删除的SQL语句；                 
如果批量操作的逻辑比较复杂，则可以通过直接在数据库中运行的存储过程来完成批量操作。但是，有些数据库不支持存储过程。例如，MYSQL就不支持存储过程。所以，我们还要了解一下在应用层如何实现批量操作。          



#### 批量处理数据
在应用层实现批量操作的方式有很多种，比较常用的一种方式就是：  
 
	通过Session来进行批量操作；

Session的`save()`及`update()`方法都会把处理的对象存放在自己的缓存中，如果通过一个Session对象来处理大量持久化对象，应该及时从缓存中清空已经处理完毕并且不会再访问的对象。         

>具体的做法是在处理完一个对象或小批量对象后，立刻调用`flush()`方法清理缓存，然后再调用`clear()`方法清空缓存。    

通过Session来进行批量操作需要注意以下几点问题：         

1.首先需要在Hibernate的配置文件中设置JDBC单次批量处理的数量；   
一般情况，会将这个数值设置为 10 - 50之间。    
例如，将它设为20；         
```
<!--设置JDBC单次批量处理的数目-->
<property name="hibernate.jdbc.batch_size">20</property>
```
将`hibernate.jdbc.batch_size`设置为20.       
然后，我们在进行批量操作时，应该保证每次向数据库发送的批量SQL语句数目与这个`batch_size`属性一致。       

2.如果对象采用"identity"标识符生成器，则Hibernate无法在JDBC层进行批量插入操作。             

3.进行批量操作时，建议关闭Hibernate的第二级缓存。        
我们使用的Session缓存为Hibernate的第一级缓存。通常，它是事务范围内的缓存；这也就是说，每个事务都有单独的第一级缓存。
而，SessionFactory的外置缓存为Hibernate的第二级缓存。它是应用范围内的缓存。也就是说，所有事务都共享同一个第二级缓存。但是在任何情况下，Hibernate的第一级缓存总是可用的；而，默认情况下，Hibernate的第二级缓存是关闭的。      
另外，也可以在Hibernate的配置文件中通过设置来关闭第二级缓存。           
```
<!--关闭第二级缓存-->
<property name="hibernate.cache.use_second_level_cache">false</property>
```

#### 批量处理数据       
下面，我们来看一下如何在应用层实现批量插入操作。     

>在应用层实现批量插入数据
如下代码：     
```java
Session session = new Configuration().configure().                                          buildSessionFactory().openSession();
Transaction tran  = session.beginTransaction();
for(int i=0; i<=1000000;i++) {
  Login login = new Login();
  login.setUsername(Integer.toString(i+1000000));
  login.setPassword("111111");
  login.setAge(21);
  session.save(login);
  if(i % 20 == 0) {   //单次批量操作的数目为20，与JDBC批量设置相同
     session.flush(); //清理缓存，批量插入20条记录的SQL语句
     session.clear(); //清空缓存中的login对象
   }
}
tran.commit();   //因为81 到 99都不满足 i%20==0，所以需要手动提交一次
session.close();
```

在这段代码中，我们仍然使用for循环来完成10万次插入操作，只是在循环中加入了一个if判断；      
判断条件是，够20条记录就将这20条记录批量插入到数据库中，并且清空Session缓存中的实例；20是之前设置的JDBC单次批量处理的数目。但这不是必须的，也可以设置为其他的数字。           

这就是插入数据的批量处理方式；             


现在，我们已经知道了插入数据如何实现批量处理。接下来，我们来学习更新数据的批量处理。      
我们在进行批量更新的时候，如果一下子把所有对象都加载到Session的缓存中，然后再一一更新它们，显然是不可取的。所以大量的更新操作也需要我们进行批量处理。              

可以使用可滚动的结果集ScrollableResults。         

如下代码：         
```java
Session session = new Configuration().configure().                                          buildSessionFactory().openSession();
Transaction tran  = session.beginTransaction();

ScrollableResults logins = session.createQuery("from Login").
      scroll(ScrollMode.FORWARD_ONLY);
int count = 0;
while(logins.next()){
  Login login = (Login)logins.get(0);
  login.setAge(login.getAge() +1);   //更新Login对象的age属性
  if(++count % 20 ==0) {//单次批量操作的数目为20，与JDBC批量设置相同
      session.flush();  //清理缓存，执行批量插入20条记录的SQL语句
      session.clear();  //清空缓存中的login对象
   }
}
tran.commit();
session.close();
```

在这段代码中，我们使用Session对象获取一个Query对象；
接下来，再使用Query对象的`scroll()`方法，返回一个ScrollableResults对象logins。但是，这个logins并不包含任何Login对象，它仅包含用于在数据库中记录的游标；只有当程序遍历访问该对象的时候，也就是logins对象中的特定元素的时候，它才会到数据库中加载相应的Login对象。                     
>为了保证程序顺利运行，需要注意两点：      
1.在Hibernate的配置文件中，应该把`hibernate.jdbc.batch_size`属性设置为20。      
2.关闭二级缓存。          


#### Hibernate批处理数据与调用存储过程

使用CallableStatement调用存储过程       

我们在软件开发过程中会遇到一些复杂的数据库操作，如果在应用层来实现这些操作会非常繁琐，我们多数情况下会选择使用存储过程；存储过程直接在数据库中运行，运行速度非常快；因此，使用存储过程来处理复杂的数据库操作非常方便。       
那么，在Hibernate中如何来调用存储过程呢？          

首先，来创建几个存储过程：          

存储过程保存数据库中；           

第一个是向login表中插入记录的存储过程：       
```sql
create or replace procedure login_insert(username in varchar,password in varchar,age in number) as begin     
insert into login (password,age,username) values (username,password,age);
end;
```

第二个是更新表中记录的存储过程：          
```sql
create or replace procedure login_Update(a in number) as begin
update login set age=a where username='Tom';
end;
```

第三个是删除表中记录的存储过程：        
```sql
create or replace produre login_delete(uname in varchar) as begin    
delete from login where USERNAME = uname;    
end;      
```

这几个存储过程都非常简单，我们就将它们作为示例在程序中调用一下。        

首先，我们来看一下如何调用插入数据的存储过程：         
```
//调用插入数据的存储过程
Session session = new Configuration().configure().                                          buildSessionFactory().openSession();
Transaction tran  = session.beginTransaction();

CallableStatement cst = session.connection().prepareCall("{
	call login_insert(?,?,?)
}");
cst.setString(1,"aaa");
cst.setString(2,"2222222");
cst.setLong(3,21);
cst.executeUpdate();

tran.commit();
session.close();
```

在这里，使用了Session获得Connection对象；然后，再通过Connection的`prepareCall()`方法来封装调用存储过程的语句；接下来，使用CallableStatement对象的`set**()`方法为存储过程传入参数；然后，使用CallableStatement对象的`executeUpdate()`方法来执行存储过程；最后，提交事务，释放资源。    
调用存储过程的步骤也非常简单；         

```java
//调用更新记录的存储过程
Session session = new Configuration().configure().                                          buildSessionFactory().openSession();
Transaction tran  = session.beginTransaction();

CallableStatement cst = session.connection().perpareCall("{
               call login_update(?)}");
cst.setInt(1,20);
cst.executeUpdate();

tran.commit();
session.close();
```

这段代码就是调用更新记录的存储过程；      
同样是通过Connection的`prepareCall()`方法来封装调用存储过程的语句，然后传入参数，最后执行并提交。       
```java
//调用删除记录的存储过程
Session session = new Configuration().configure().                                          buildSessionFactory().openSession();
Transaction tran  = session.beginTransaction();

CallableStatement cst = session.connection().perpareCall("{
               call login_delete(?)}");
cst.setInt(1,"aaa");
cst.executeUpdate();

tran.commit();
session.close();
```

它的调用方法和插入，更新存储过程的语法都是一样的；        

下面，再来看一下如何调用带有返回结果集的存储过程，也就是具有查询功能的存储过程：
首先，创建这个存储过程：            
```sql
create or replace procedure login_getlist(logins OUT SYS_REFCURSOR)     as
begin
  open logins for
     select * from login;
end;
```

这个存储过程带有一个输出参数logins，这个参数被定义为系统游标类型。       
然后，将一个查询结果赋给了logins； 
这个存储过程非常简单；      

```java
//调用带有返回结果集的存储过程
Session session = new Configuration().configure().                                          buildSessionFactory().openSession();
Transaction tran  = session.beginTransaction();

CallableStatement cst = session.connection().perpareCall("{
               call login_getlist(?)}");
cst.registerOutParameter(1,oracle.jdbc.OracleTypes.CURSOR);
cst.execute();
ResultSet rs = (ResultSet)cst.getObject(1);
while(rs.next()) {
 System.out.println("用户名: "+rs.getString("username"));
 System.out.println("密码  : "+rs.getString("password"));
 System.out.println("年龄  : "+rs.getString("age"));
 System.out.println;
}

tran.commit();
session.close();
```

在这段代码中，我们仍然使用Session对象，来获取Connection对象并调用`prepareCall()`方法；但是，不同的是，接下来要注册输出参数;使用CallableStatement对象的`registerOutParameter()`方法，来注册输出参数；   
为这个方法传入两个参数：                 

>第一个参数 1，表示输出参数的顺序。                
这个存储过程只有一个参数，所以用 1 表示。         

>第二个参数，输入了一个 Oracle 的数据类型；        
因为存储过程是用系统游标类型的输出参数来返回的结果集，所以在这里要传入一个Oracle游标的类型。        
接下来，调用CallableStatement对象的`execute()`方法来执行存储过程。          
最后，使用ResultSet对象来获取存储过程的输出参数，也就是系统游标类型的输出参数。在这里注意，要使用强制类型转换；  
那么，利用Hibernate调用具有查询功能的存储过程的关键代码就这么多了。       
下面的代码是循环遍历结果集以及提交事务、释放资源等操作。             



现在我们已经学会了如何在程序中调用存储过程，也就是使用CallableStatement调用存储过程。      
然而，使用存储过程和使用Native SQL，就是本地SQL是非常相似的。         
之所以说使用它们非常相似，是因为它们都具有三种调用方式：           
首先，回顾一下本地SQL的三种使用方式：        

	1.使用SQLQuery来执行本地SQL语句。
	2.使用命名SQL。
	3.使用定制SQL。
然而，调用存储过程的三种方式和本地SQL的使用方式相似：         
调用存储过程的三种方式：               

	1.使用CallableStatement调用存储过程
	2.使用命名SQL调用存储过程
	3.使用定制SQL调用存储过程

我们现在已经学会了第一种调用存储过程的方式，也就是使用CallableStatement调用存储过程。它和第二种方式，使用命名SQL调用存储过程是比较常用的方式，而第三种方式，使用定制SQL调用存储过程，在现实开发中是很少使用的；        
在这里只讲第二种方式，使用命名SQL调用存储过程。             


#### 使用命名SQL调用存储过程

在这里，仍然以前面介绍的针对Login表的增、删、改、查的存储过程为例，来学习如何使用命名SQL调用这四个存储过程。      
在学习命名SQL的时候，我们知道，要使用命名SQL，首先需要在映射配置文件中声明`<sql-query>`元素；那么，使用命名SQL方式调用存储过程也是一样的。       
如下代码：      
```
<!--调用插入记录的存储过程-->
 <sql-query name="loginInsert">
    {call login_insert(?,?,?))}
 </sql-query>
<!--调用更新记录的存储过程-->
 <sql-query name="loginUpdate">
    {call login_update(?)}
 </sql-query>
<!--调用删除记录的存储过程-->
 <sql-query name="loginDelete">
    {call login_delete(?)}
 </sql-query>
<!--调用查询记录的存储过程-->
 <sql-query name="loginGetList" callable="true">
    {call login_getlist(?)}
    <return alias="I"  class="com.test.Login"/>
 </sql-query>
```

这是我们要在`Login.hbm.xml`映射配置文件中添加的`<sql-query>`元素，它们分别封装了调用插入记录、更新记录、删除记录、以及查询数据的存储过程的SQL语句。          
**注意**：         
	调用插入、更新 或者删除功能的存储过程和使用SQL语句没有什么区别。
	但是，调用带有返回结果集的存储过程，也就是调用查询功能的存储过程，一定要为 <sql-query> 元素添加一个callable属性，并且将其设置为 true。
	这代表声明的命名SQL支持存储过程。否则的话，使用这个命名SQL会抛出QueryException异常，并且不能够正确的获取查询结果集。


在程序中如何通过命名SQL调用存储过程           

调用插入记录存储过程的关键代码：     
```
Query query = session.getNamedQuery("loginInsert");
query.setString(0,"aaa");
query.setString(1,"222222");
query.setInteger(2,22);
query.executeUpdate();
```
在这里，仍然使用的是调用命名SQL的方式，也就是通过Session对象的`getNamedQuery()`方法来获取一个Query对象。为`getNamedQuery()`方法传入的参数loginInsert，就是在映射配置文件中声明的调用插入记录的存储过程的`<sql-query>`元素，然后传入要插入记录的信息；接下来，调用`executeUpdate()`方法执行命名SQL。这样就完成了通过命名SQL调用插入记录的存储过程。     

调用更新记录的存储过程的代码：           
```
Query query = session.getNamedQuery("loginUpdate");
query.setString(0,"80");
query.executeUpdate();
```

这段代码非常简单，与调用插入记录的存储过程的代码相同；          
在存储过程中只更改了一个字段，所以只传入一个参数就可以了。           


调用删除记录的存储过程的代码：             
```
Query query = session.getNamedQuery("loginDelete");
query.setString(0,"aaa");
query.executeUpdate();
```

这段代码与前面的代码也没有什么区别；       



首先，来看一下映射配置文件中是如何来声明具有查询功能的存储过程：      
```
<!--调用查询记录的存储过程-->
 <sql-query name="loginGetList" callable="true">
    {call login_getlist(?)}
    <return alias="I"  class="com.test.Login"/>
 </sql-query>
```

它就是我们声明的一个`<sql-query>`元素，用来封装调用具有查询功能的存储过程`login_getlist`；这个存储过程有一个系统游标类型的输出参数；那么，这个输出参数就是我们要获取的查询结果集。      
声明`<sql-query>`元素的时候，一定要添加callable属性，并将它的值设为true；同时，还要使用`<sql-query>`元素的子元素`<return>`指定查询结果与实体类的联系。           


如何调用这个命名SQL，并且获取查询结果集：       
首先，我们仍然是通过Session对象的`getnamedQuery()`方法来获取Query对象；接下来，使用一个List对象来获取查询的结果集；我们是通过Query对象的`list()`方法来获取的List对象；然后，使用迭代器来循环遍历List对象。         
代码如下：        
```java
Query query = session.getNamedQuery("loginGetList");
List list = query.list();
Login l = new Login();
Iterator iter = list.iterator();
while(iter.hasNext()) {
  l = (Login)iter.next();
  System.out.println("用户名："+l.getUsername()+" 密码："+
    l.getPassword()+"年龄："+l.getAge());
}
```
