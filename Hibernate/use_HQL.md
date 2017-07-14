# HQL查询概述   
Hibernate提供的查询方式：        
- OID查询方式                 
通过`get()`和`load()`方法加载指定OID的对象            

- HQL查询方式  
通过Query接口使用HQL语言进行查询       

- QBC查询方式 
通过Criteria等接口和类进行查询        

- 本地SQL查询方式    
使用原生SQL语言进行查询        

- 对象导航查询方式      
通过已经加载的对象，导航到其关联对象

HQL和QBC是Hibernate提供的专业查询方式          
HQL查询方式为官方推荐的标准查询方式                


HQL是Hibernate提供的面向对象的查询语言；       
HQL和SQL的语法格式很相似       
HQL操作持久化类，而不是数据库表。         

### 使用HQL进行查询需要的步骤：
- 第一步：      
得到session对象；     

- 第二步：       
使用HQL语言来编写HQL语句；

- 第三步：         
创建Query对象；         
使用HQL进行查询是离不开Query对象的，就是调用Query对象的各个方法来实现查询。       

- 第四步：         
执行查询，得到结果；            
就是调用Query对象的相应方法，来执行查询，从而得到查询的结果；            
查询结果是一个转化好之后的List集合对象；        
得到查询结果之后，就可以利用各种迭代方式来显示查询结果。           

>强调：
- 第一点：       
HQL语言操作的是持久化类及其属性，区分大小写；         

- 第二点： 
使用HQL查询可以简化操作，避免繁琐的转换。       


#### Query接口：
Query接口是HQL查询的接口，提供了各种的查询功能。        
Query接口相当于JDBC中的Statement和PreparedStatement，同样是用来执行查询，得到查询结果。      
可以通过Session的`createQuery()`方法来创建Query的对象；          

### HQL语言的语法结构：     
```
[select/update/delete.....]
from Entity
[where....]
[group by ....]
[having ....]
[order by ....]
```

注意：          

- 第一点：     
语法结构中的各个关键字，不区分大小写，建议小写；    

- 第二点：       
类名和属性名区分大小写；               
必须和实际的类名和属性名保持一致。             

#### 实体查询：
所谓实体查询，就是要查询持久化类的完整信息，要查询它的全部属性而不是部分属性；   

最简单的实体查询：      
举例：     
```
from Student
from com.pb.hibernate.po.Student
```
这两个查询语句的作用是一样的；都是要查询学生表中的全部学生信息；       
不同点在于，第一个语句只使用了类名，而第二个语句使用了其完整的类名，指定了包名；              

通常不需要使用类的全限定名，缺省自动导入           
在通常情况下，使用HQL语言进行查询的时候，是不需要使用类的完整名称的；      
因为，在缺省情况下会自动导入类的完整名称。             
```
from Student s
from Student as s
```

这两条查询语句都为Student类指定了别名；        

可以为类名指定别名，as是可选的；        
在指定别名的时候，可以使用 "as"关键字，也可以不使用。该关键字是可选的；          

#### where子句      
同样，在HQL查询中也可以使用where子句；       
```
from Student where sid=11070130
from Student s where s.sid = 11070130
```
这两条查询语句的作用是一样的，都是要查询编号为11070130的学生信息；          

where子句：         
在where子句中，可以使用各种运算符来构造复杂的查询条件；    

|属性值 |              功能描述|
|-------|------|
|比较运算符  |=、<> 、!= <、>、<=、>=、is null、is not null|
|逻辑运算符    |or and not|
|数字运算符          | +  -  *  /|
|限定范围    |in、not in、like、not like、between A and B、not between A and b|

举例：   
```
from Student where sex='男' and sid <11070200
from Student where sid between 11070100 and 11070200

from Student where sname like '%飞'
from Student where sname like '_ik*'
```
> "_"匹配单个字符，"%"匹配任意长度字符       
> "_"不可以进行汉字的匹配，"%"可以进行汉字的匹配。 


获取返回结果：       
如何来执行HQL语句返回查询结果：     
可以使用Query的三个方法来实现；         
这三个方法分别为：       
```
list()
iterator()
uniqueResult()
```     
- list():       
比如我们要查询所有的学生信息，调用Query的`list()`方法，将直接返回包含所有学生信息的List对象，然后可以通过for循环、foreach循环、iterator迭代器等各种方式对其内容进行迭代。输出其中各个对象的内容就可以了。      
>如下代码：      
```
Query query = session.createQuery("from Student");
List <Student>list = query.list();
Iterator it = list.iterator();
while(it.hasNext()) {
   Student stu = (Student) it.next();
   System.out.println(stu.getSid()+" " +stu.gtSname());
}
```

- iterator():     
>代码如下：     
```
Query query = session.createQuery("from Student");
Iterator it = query.iterator();
while(it.hasNext()) {
   Student stu = (Student) it.next();
   System.out.println(stu.getSid()+" " +stu.gtSname());
}
```

调用Query的`iterator()`方法，将直接返回Iterator对象；直接对其进行迭代，就可以输出其中的各个学生信息；      
>list()方法和iterator()方法的区别：      
使用`iterator()`就减少了一个将List转变为Iterator的一个过程。当然，这只是最表面的区别，而不是其核心的区别。          

- uniqueResult():     
在前面两个示例中，都知道查询所有的学生信息；执行查询将返回包括多个学生内容的集合；但是如果只要查询某一个编号的学生信息，再调用以上两个方法，就不合适了；如果我们确认满足条件的对象只有一个，就可以调用`uniqueResult()`方法来实现；  
如下代码：      
```
Query query = session.createQuery("from Student where sid=11070130");
Student stu = (Student)query.uniqueResult();
System.out.println(stu.getSid() + " " + stu.getSname());
```

要查询编号为11070130的学生信息，很显然这样的信息如果有，只有一个，这个时候就可以调用Query的`uniqueResult()`方法来直接返回其对象；该方法的返回值是一个Object对象，所以还需要将其进行强制类型转换。转换为学生类型，然后输出其内容就可以了；
注意：          
调用`uniqueReuslt()`的条件是，必须确认满足条件的对象只有一个；       
当然，也可以没有，但是不能是多个；如果是多个，将会跑出错误信息。            


>list()和iterator()区别：查询机制不同；       
使用`list()`，查询一次就可以，从数据库中检索出所有符合条件的记录，包含全部字段；    

使用`iterator()`，首先从数据库检索出所有符合条件的记录，仅包含id字段；     
然后，要到缓存中去查看，是否包含所需信息；           
	如果缓存中包含全部数据，无需再查询数据库，直接引用 `(1+0)`只有一次查询
	如果缓存中不包含任何数据，需再查询数据库n次，依次查询各条记录`(1+n)`

如何选择查询方法：
大多数情况下使用`list()`进行查询，当对象包含大量属性，或者要加载的大部分数据已经存在缓存中，可使用`iterator()`；


### 属性查询
属性查询(投影查询)      
>只查询持久化类的部分属性而不是全部属性       
属性查询方式：        
- 第一种方式：   
通过直接指定要查询的属性进行属性查询       
比如，使用属性查询，来查询所有学生的学生编号和学生姓名；          
如下代码：           
```
String hql = "select sid,sname from Student";
Query query = session.createQuery(hql);
List list = query.list();
Iterator it = list.iterator();
while(it.hasNext()) {
  Object obj[] = (Object[]) it.next();
  System.out.println(obj[0]+" "+obj[1]);
}
```

List的每个元素是一个对象数组；                
构造了SQL语句为`select sid,sname from Student`；             
很显然，我们直接指明了要查询的属性名称为sid和sname；          
然后，构造Query对象，调用其`list()`进行查询就可以了；查询结果就是一个List对象；数据库表中有几条学生记录，这个List中就存在几个元素；
List集合中的每个元素不是学生类型，而是一个对象数组；这个数组的长度是几，取决于指定了几个属性；在这里指定了两个属性，所以每个元素的数组长度就是2；
第一个元素存放学生编号，第二个元素存放学生的姓名；可以使用迭代器进行迭代；调用`next()`返回值是一个对象数组，这个对象数组有两个元素；所以，分别调用`obj[0]`和`obj[1]`输出其中的学生编号和学生姓名；  
强调：       
通过直接指定属性进行属性查询，List中的每个元素是一个对象数组；        

#### 通过构造方法进行属性查询：
依然是查询所有学生的学生编号和学生姓名；      
代码如下：          
```
String hql = "select new Student(sid,sname) from Student";
Query query = session.createQuery(hql);
List list = query.list();
Iterator it = list.iterator();
while(it.hasNext()) {
  Student stu = (Student)it.next();
  System.out.println(stu.getSid()+ " " + stu.getSname());
}
```

关键是HQL语句；            
很显然，这里利用查询到的sid和sname，构造了一个学生对象；        
最终查询结果的迭代语句：      
这时，调用迭代器的`next()`将得到一个学生对象；然后，调用该对象的相应方法，就可以得到其编号和姓名；    
使用这种方法的前提：         
一定要在持久化类中，为该持久化类提供相应的构造方法；          


### 实体更新和删除：
- 实体更新：       
>要更新编号为11070130的学生信息，将其姓名修改为"赵菲"            
代码如下：            
```
String hql = "update Student set sname='赵菲' where sid=11070130";
Query query = session.createQuery(hql);
int n = query.executeUpdate();
```

HQL语句，操作的是持久化类及其属性，而不是数据库表和其字段；        
调用Query的`executeUpdate()`方法实现更新；        
返回值是一个整数n，代表更新的对象的个数；          
强调：                
实现更新操作的难点是，实现更新操作是通过调用Query的`executeUpdate()`方法进行更新的； 

- 实体删除：     
>删除编号为11070130的学生信息；     
代码如下：       
```
String hql = "delete from Student where id=11070130";
Query query = session.createQuery(hql);
int n = query.executeUpdate();
```

调用Query的`executeUpdate()`方法实现更新；         
返回值是一个整数n，代表删除的对象的个数；           


#### 参数绑定
如果不使用参数绑定功能，一般需要使用字符串拼接来构造HQL语句；        
- 字符串拼接：     
>比如，我们要查询指定姓名的学生信息，代码如下：   
```
String name = "赵菲";
String hql = "from Student where sname=' " +name+ " ' ";
Query query = session.createQuery(hql);
List<Student> list = query.list();
```

缺点：   
	性能低；        
	安全性差        
	容易拼接错误    

参数绑定：      
- 第一种方式：         
使用 `?`占位符；        
>比如，我们要查询指定姓名的学生信息，代码如下：        
```
String name = "赵菲";
String hql = "from Student where sname=?";
Query query = session.createQuery(hql);
query.setString(0,name);
List<Student> list = query.list();
```

在HQL语句中，将使用变量的地方用 `?`来代替，可以一个，也可以多个；然后，调用Query的相关方法，为各个问号进行赋值；   
如果该问号代表的是整型变量，那就调用Query的`setInt()`方法；              
如果一个问号对应的是字符串变量，就调用Query的`setString()`方法等等；       

注意： 
>设置参数时，下标从0开始；      

- 第二种方式：      
使用命名参数          
代码如下：  
```
String name = "赵菲";
String hql = "from Student where sname=：sname";
Query query = session.createQuery(hql);
query.setString("sname",name);
//query.setParameter("sname",name");
List<Student> list = query.list();
```

在HQL语句中将占用的变量位置，使用一个命名参数来替代；前面必须要加上冒号；该冒号就代表后面是一个命名参数；然后在下面，就可以通过Query的相关方法来为该命名参数赋值；命名参数同样可以指定多个；       
如果命名参数代表的是整型变量，那就调用Query的`setInt()`方法；        
如果命名参数对应的是字符串变量，就调用Query的`setString()`方法等等；            
在给命名参数进行赋值的时候，采用双引号括起来的命名参数来实现赋值；           
使用命名参数将不依赖于参数的顺序；           

强调：        
Query接口还专门提供了另外一个用于参数赋值的方法，`setParameters()`方法；
`setParameters()`绑定任意类型的参数；         
就是不管这个参数代表的是整型变量，或者是字符串变量或者其他变量，都可以通过该参数来实现参数的赋值；           

## 排序：
HQL的排序功能非常的简单；       
HQL通过`order by` 子句实现对查询结果的排序；默认升序排列；         
代码如下：    
```
String hql = "from Student order by sname";
```
该语句将查询的学生信息按照姓名进行升序排列；     
HQL语句中也可以指定升序的策略：           
```
指定排序策略 (asc升序、desc降序)
String hql = "from Student order by sname desc";
```

该语句将查询的学生信息按照姓名进行降序排列；          
在HQL语句中同样可以为`order by`子句指定多个排序条件；         
`order by`子句可指定多个排序条件：            
```
String  hql = "from Student order by sname,sid desc";
```
>在该语句中将查询的学生信息，首先按照学生姓名进行升序排列，对学生姓名相同的对象，将按照学生编号进行降序排列。


## 统计函数：
HQL中的统计函数和SQL中的统计函数相同，一共有五个，分别为：        
```
avg()  sum()  min()  max()  count()
```
分别用来实现，`求平均数、求和、求最小值、求最大值、计数`的功能；      
所不同的是：         
SQL中的统计函数用来对数据库表的字段进行操作，而HQL中的统计函数则是对持久化类的属性进行操作；    
统计函数使用举例：        
>统计学生数目：      
```
String hql = "select count(*) from Student";
long n = (Long) session.createQuery(hql).uniqueResult();
```

HQL语句和SQL语句非常的类似；       
需要注意的是       
	当调用`count(*)`来实现计数的时候，最终的返回结果不是整型，而是长整型Long；所以要进行强制类型转换的时候，要特别的注意；          



>查询所有学生的平均分、最低分、最高分；        
```
String hql = "selecct avg(score),min(score),max(score) from Student";
Object obj[] = (Object[]) session.createQuery(hql).uniqueResult();
```

需要注意的是：          
	这时，最终查询的结果是一个对象数组，数组一共有三个元素，分别存放平均分、最小值、最大值；


## 分组：
HQL的分组查询功能和SQL的分组查询功能也是非常相似的；       
同样使用`group by`子句进行分组查询；               

#### group by 用于分组查询：     
>统计各个班级的学生人数          
```
select gid,count(*) from Student group by gid
```

以班级编号进行分组，统计各个班的学生人数；     
如果要在这个查询结果的基础上更进一步，统计人数大于20的班级的学生人数；这时，可以使用having子句；     

having子句用于对分组结果添加查询条件；      
也就是说，它可以在分组查询的结果上再次进行筛选；      
>统计人数大于20的班级的学生人数    
```
select gie,count(*) from Student group by gid having count(*)>20
```
在上面分组查询语句的基础上，再添加having子句，指明其条件为        
`"count(*)>20"` 就可以了；        


## Hibernate分页
分页思路：        
- 第一种思路：             
使用非常复杂的子查询HQL语句来实现分页；       

- 第二种思路：     
使用`Hibernate Query`接口所提供的两个方法来实现分页；           
这两个方法分别为： `setFirstResult()`和`setMaxResultes()`        

>setFirstResult()            
设置一页中第一条记录的编号；       
也就是说，这一页要显示的内容从哪一条记录开始；        

>setMaxResultes()          
设置最大返回的记录条数       
也就是设置一页可以容纳的记录条数；         

### 分页实现步骤：
- 第一步：    
根据查询结果获取数据库中总的记录数；        

- 第二步：       
计算总的页数；      

- 第三步：        
使用Query接口这两个方法实现分页；         


##### 第一步：
根据查询结果获取数据库中总的记录数；      
>比如，要分页显示学生信息；
```
//Query query = session.createQuery("from Student");
//List <Student>list = query.list();
int count  = list.size();
```

首先，要查询所有的学生信息，得到List集合对象；       
调用List的`size()`方法就得到了学生总的记录数；           
##### 第二步：     
>计算总的页数；     
```
int totalpages = (count%pagesize ==0) ? (count/pageSize):
(count/pageSize +1);
```

这个时候，只要让总的记录数除以每页的记录数，就可以得到总的页数；        
注意：如果整除，就取它们的商；如果不能整除，则将其商加1；从而得到总的页数；     

##### 第三步：
使用Query接口这两个方法实现分页；     
```
query.setFirstResult((pageIndex-1)*pageSize);
query.setMaxResults(pageSize);
List result = query.list();
```
首先，调用`setFirstResult()`,指明该页起始记录的编号；这个时候，传入一个表达式；其中，pageIndex表示页号，pageSize表示每页的记录个数；             
再调用`setMaxResults()`，指明每页存储多少条记录；          
最后，调用Query的`list()`方法，就会得到指定页的学生内容；        
直接在页面上显示List的内容就可以了；             


## 子查询
同SQL类似，HQL的子查询语句中，也包括两层甚至更多层的查询；            

外部查询使用子查询的结果；        
子查询必须用圆括号包围起来且必须出现在where子句中；          

>查询成绩高于平均分的学生信息：      
```
select avg(score) from Student;
from Student where score >71;  //71为平均分；
等价于 
from Student where score > (select avg(score) from Student);
```

在该语句中，将查询平均分的语句作为子查询放到了圆括号中，并作为外部查询条件的一部分；外部查询将使用子查询的结果；          

>查询成绩高于张华成绩的学生信息      
```
from Student where score >(select score from Student where sname='张华');
```
子查询的功能就是查询出张华的成绩，然后在外部查询中要求成绩大于张华的成绩就可以了；      

>查询成绩高于张华和赵菲成绩的学生信息
```
from Student where score > all(select socre form Student where sname='张华' or sname='赵菲');
```

首先，在子查询中将通过HQL查询语句获取张华和赵菲的成绩，这就得到了两个成绩所组成的一个成绩集合；此时，在括号的前面添加一个all关键字；实质的含义就是分数要大于括号中的所有分数；         


>查询成绩高于张华或者高于赵菲成绩的学生信息      
```
from Student where score > any(select socre form Student where sname='张华' or sname='赵菲');
```
使用any关键字；      


### 连接查询
所谓连接查询，就是使用join子句来实现多个持久化对象之间的联合查询。      
一般是针对两个持久化对象进行查询。          
连接查询分类：            

|类型         |连接子句         |简写形式|&nbsp;|
|-----|------|------|----|
|内连接     |inner join         | join     |只显示匹配数据|
|左外连接   |left outer join    |left join  |显示左对象中所有数据|
|右外连接   |right outer join   |right join  |显示右对象中所有数据|
|全外连接  |full outer join     |full join   |显示对象中的所有数据|

内连接查询指的是只显示两个持久化对象中的匹配数据；    
左外连接除了显示两个对象中的所有匹配数据之外，还将显示左对象中的不匹配数据；也就是要显示左对象的全部数据； 
右外连接除了显示两个对象中的所有匹配数据之外，还将显示右对象中的不匹配数据；也就是要显示右对象的全部数据；     
全外连接，除了显示两个对象的匹配数据之外，还将显示左对象以及右对象中的其他数据；也就是说，使用全外连接将可以查询出两个对象中的所有数据；         


#### 内连接查询
内连接查询将显示两个持久化对象中所有匹配的数据；       

语法：             
```
from Entity inner join Entity.property
```

在from子句中指明了一个持久化类的名字，而在后面的inner        join子句中，指明了第一个持久化类中与第二个持久化类建立关联的属性；           


>查询班级和学生中的所有匹配数据：
```
from Grade g inner join g.students
```

等价于如下两种形式：     
```
from Grade g,Student s where g = s.grade
from Grade g,Student s where g.gid = s.grade.gid
```

重点观察其`inner join`子句后面的内容为`g.students`；         
班级类的students属性存放了所有的学生信息；而，班级类也就是通过students属性与学生类进行关联；         
在下面两种形式中，并没有使用`inner join`子句，而是使用逗号来分隔两个持久化类；关键是后面的条件：         
第一种形式的条件为班级要等于学生的班级属性，比较的是班级对象；        
第二种形式比较的是班级的ID；要求班级的ID要等于学生所属班级的ID；           
含义其实是一样的；          
执行上面这三个内连接查询语句，将产生相同的SQL语句；       
查询结果中只显示了两个表中的匹配数据；        
强调：              
	要注意HQL的连接查询中inner join子句的内容；    
	在inner join子句中不能跟上一个持久化类的名字；
错误语句：   
```
from Grade g inner join Student
```

#### 左外连接：
语法：      
```
from Entity left outer join Entity.property
``` 

与内连接的语法相比，连接子句使用了`left outer join` ；其中的outer可以省略；在连接子句中仍旧使用的是持久化类的属性，而不是使用持久化类的名称； 

查询班级和学生的所有匹配数据，并且显示所有没有学生的班级信息： 
```
from Grade g left join g.students
```

#### 右外连接
语法：     
```
from Entity right join Entity.property
```

查询班级和学生的所有匹配数据，同时还要查询不属于任何班级的学生信息：     
```
from Grade g right join g.students
```

##### 使用左外连接和右外连接的功能：             
所谓的左外连接和右外连接查询，只是相对于持久化类的位置而言的；           
我们修改HQL查询语句中持久化类的位置，就可以通过相反方向的查询得到相同的查询结果；           
比如：    
```
from Student s left join s.grade
```
在这里的from子句中不是查询班级，而是查询学生；而在左外连接子句中，使用的也不是在引用班级的students属性，而是引用了学生的grade属性；         
等同于：     
```
from Grade g right join g.students
```

Hibernate为什么要单独提供HQL语句，而不是直接使用SQL：        
HQL是操作持久化类的，而SQL语句是操作数据库表的；Hibernate要操作持久化类，所以要使用HQL语句；         

HQL相对简单了，查询结果不是结果集而是直接返回List对象，省略了对查询结果的繁琐的转换操作；    

>是HQL的执行效率高，还是SQL的执行效率高？ 
所有的HQL语句都要先转换成SQL语句，然后再执行SQL语句对数据库表执行真正的操作。多了一个转换过程。所以，理论上讲，SQL的执行效率高；但是，不一定；Hibernate做了一些优化；    
比如：`使用缓存、连接池和脏数据检查`等技术来提高速度，所以最终的执行效率不见得低；除非是数据库开发的高手，可以写出非常优化的SQL语句。          
所以，Hibernate不仅降低了开发难度，而且允许效率也不低；       

不同数据库的SQL语句可能会有细微差别，而HQL是独立于数据库的，可以根据方言的设定来转换为相应的SQL语句；       