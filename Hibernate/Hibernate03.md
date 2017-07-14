# 数据库表   
数据库表之间是如何建立关联关系的：      
各个数据库表之间，一般是通过外键字段来实现表与表之间的关联关系。          
数据库表之间的关联关系可以分为哪几类：        
关联关系主要分为三大类：      
```
一对一关系
一对多（多对一）关系
多对多关系
```

在Hibernate中如何表示数据库表之间的关联关系呢？           
Hibernate不是直接操作数据库表，而是操作数据库表所对应的持久化类；         
针对有关联关系的数据库表，如何实现ORM映射呢？         
分析：            
在Hibernate中，还是要编写持久化类，还是要编写持久化类和数据库表之间的映射文件。         
但是，需要在持久化类中添加与关联有关的属性和方法。(setter/getter方法)             
并且在映射文件中配置映射信息。              

### 持久化类之间是如何建立关联关系的：           
持久化类之间不是通过外键建立对象间关联关系，而是通过添加属性来建立关联关系。       
持久化类之间的关联关系可以分为哪几类：           
持久化类间关联关系的种类：       
```
一对一关系、
一对多（多对一）关系、
多对多关系。
```
类和类之间的多对多关系，并不需要借助一个中间类来实现。         

持久化类之间的关联关系，如果按照方向来分，可以分为哪几类：        
#### 持久化类间关联关系的方向：         
- 单向关联关系            
- 双向关联关系         

#### 一对多概述
日常生活中的一对多关联关系：  
	留言和回复       
	班级和学生        
	类别和商品         

数据库表中的一对多关联关系：            
通过外键建立两表之间的联系。           

##### Hibernate中持久化类的一对多对象关联：
分两步：     
- 第一步：       
在数据库表所对应的持久化类中，添加关联类的相关属性以及getter/setter方法。       

- 第二步：      
在数据库表和持久化类的映射文件中，建立该属性以及数据库表字段之间的映射信息；       

数据库表：           
班级表grade和学生表student通过gid字段建立一对多关联关系；          

按照之前学习的对象关系映射的知识，在Hibernate中建立两个表所对应的持久化类，分别为Grade和Student。         
```
Grade类所包含的属性有：gid gname  gdesc
Student类所包含的属性有：sid sname sex
```

这时两个类之间没有关联关系；        

如何在两个类之间建立关联关系，使用Hibernate获取一个班级的所有学生信息：        
分析：         
通过在Grade类中添加Set集合类型的students属性，实现和Student类之间的一对多单向关联关系。             

##### 建立持久化类之间关联关系：           
- 第一步：          
为持久化类Grade添加属性和方法        
持久化类Grade        
```
public class Grade {
.....
private Set students = new HashSet();
.....
public Set getStudents() {
  return students;
}
public void setStudents(Set students) {
 this.students = students;
}
...
}
```

	为什么要为Grade类添加一个集合类型的属性students，该属性用来做什么：  
	因为一个班级可以包括多个学生，所以建立属性students，就是用来存放该班级的所有学生信息；

- 第二步：        
在Grade类所对应的映射文件中，为集合属性students建立映射信息；    
在映射文件`Grade.hbm.xml`文件中：         
```
......
<set name="students">
  <key column="GID"/>
  <one-to-many class="com.pb.hibernate.po.Student"/>
</set>
.......
```

最外层是一个`<set>`，因为students属性就是一个Set类型的集合属性；         
`<set>`的name属性的值为students，就是属性的名字；在`<set>`标签的内部还要添加一个`<one-to-many>`标签，表示一对多关联关系；通过该标签的class属性来说明students属性这个集合属性的每一个元素是什么类型；就是`com.pb.hibernate.po`包下的Student类；在`<set>`标签的内部还要添加一个`<key>`，将column属性的值指定为GID，这里的GID指的就是学生表中的外键字段GID；通过这个GID字段，就在两个表之间建立了关联关系。            
在这个映射文件的`<class>`内部一共有三个同级别的标签，分别是`<id> <property> <set>`；那么这三个标签的作用是：         
这三个标签的名称虽然不同，但是它们的作用都是相似的，都是为持久化类的属性与数据库表的字段建立映射关系，只不过不同的是：        
id用来表示主键属性，property用来表示一般属性，set用来表示集合属性；               


经过上面的两步设置之后，就建立了由班级到学生的一对多单向关联关系；现在，就可以使用Hibernate来获取一个班级的所有学生信息；         
测试类Test，代码如下：    
```java
public static void findById() {
 Session session = HibernateUtil.getSessionFactory().openSession();
 Grade g = (Grade) session.get(Grade.class,110701);
 System.out.println(g.getGid()+" "+g.getGname()+" "+g.gtGdesc());
 Set <Student>students = g.getStudents();
 for(Student stu:students) {
    System.out.println(stu.getSid()+" "+stu.getSname()+" "+stu.getSex());
 }
 session.close();
}
```

首先通过`get()`方法获取了编号为110701的班级信息；然后，输出其班级编号、班级名称和班级描述信息；通过调用班级对象的`getStudents()`方法，就得到了这个班级的所有的学生信息，它包含在一个Set集合中；然后，通过for循环遍历该Set对象，就可以在控制台显示该班级的所有学生信息；建立了对象的关联关系之后，就可以很方便的从一个对象转到另外一个对象；这就是对象的导航功能；         


#### 多对一单向关联
问题：      
>如何使用Hibernate获取一个学生的班级信息呢？        
>只要建立由学生到班级的多对一单向关联关系就可以了；        
分析：         
通过在Student类中添加Grade类型的grade属性，实现和Grade类之间的多对一单向关联关系；       
强调：            
持久化类之间的关联关系不是通过外键来实现，而是通过在持久化类中添加相应的属性来实现的；         

具体的实现步骤：       
- 第一步：      
为持久化类Student添加相应的属性和方法；          
即，为Student类添加Grade类型的属性，以及getter和setter方法；         
首先，为Student类添加Grade类型的属性，比如grade，用来存放学生的班级信息；注意，因为一个学生只能属于一个班级，所以，这个时候就不要添加一个集合属性。然后，为grade属性添加其getter/setter方法，用来获取其班级信息，或者指定其班级；    
代码如下：    
```java
public class Student {
......
private Grade grade;
....
public Grade getGrade() {
 return grade;
}
public void setGrade(Grade grade) {
  this.grade = grade;
}
.....
}
```

- 第二步：        
在学生类的映射文件`Student.hbm.xml`中，为刚刚新增的grade属性添加映射信息；代码如下：       
```
<many-to-one name="grade" class="com.pb.hibernate.po.Grade">
   <column name="GID"/>
</many-to-one>
```
外层标签是`<many-to-one>`，因为这是一个多对一的映射关系；其name属性的值就是刚刚添加的grade属性；class属性的值用来说明grade的类型，是`com.pb.Hibernate.po`包下的Grade类；在`<many-to-one>`的内部标签`<column>`，   
用来指明和grade属性所对应的数据库表中的字段，就是学生表中的外键字段GID；可以看到，`<many-to-one>`的设置和`<property>`以及`<id>`的设置都是很相似的，它们的作用也是很相似的，都是用来指明属性和字段的映射；       

强调：         
`<many-to-one>、<property>、<id>`以及`<set>`的作用都是相似的，只是名称不同而已。          

##### 如何来区分<one-to-many>和<many-to-one>:
判断的依据：     
依据就是用的这个标签所在的文件；         
如果它所有的文件是学生类的映射文件，而在学生和班级的映射中，学生很显然是多的一端，是many这一端，所以在它的映射文件中，将使用`<many-to-one>`；相反，在班级类的映射文件中，因为班级是一这一端，所以就要使用`<one-to-may>`    

经过了上面两步的设置之后，已经可以使用Hibernate来获取一个学生的班级信息了；        
测试类Test 代码如下：    
```java
public static void findById() {
 Session session = HibernateUtil.getSessionFactory().openSession();
 Student stu = (Student)session.get(Student.class,11070125);
 System.out.println(stu.getSid()+" "+stu.getSname()+" ");
 System.out.println(stu.getGrade().getSname() + " ");
 System.our.println(stu.getGrade().getGdesc());
 session.close();
}
```

还是使用`get()`方法来获取编号为11070125的学生信息，然后输出其学生的编号和学生姓名；通过调用刚刚添加的`getGrade()`方法，就可以由学生得到其班级信息；然后，再调用班级的`getGname()`方法和`getGdesc()`方法，就得到了这个班级的名称以及班级的描述信息；       


### 多对一双向关联关系：
如何使用Hibernate获取一个学生的班级信息，同时也可以获取一个班级的所有学生信息？         
分析：        
只要建立多对一或者一对多的双向关联关系就可以。        
```
多对一双向关联关系= 一对多单向关联关系 + 多对一单向关联关系
```

在建立了两个方向上的单向关联关系之后，自然也就建立了一个双向关联关系，就可以同时实现这两个访问的功能；     
 
实现步骤：        

首先，在班级类中添加一个Set集合类型的属性students，用其来存放这个班级的学生信息，并添加其getter/setter方法；然后，在其对应的映射文件中进行设置；这就建立了由班级到学生的一对多单向关联关系；然后，在学生类中添加Grade类型的属性grade，用来保存该学生的班级信息，并在该类中添加grade属性的getter/setter方法；然后，在其映射文件中进行设置；这样，就建立了由学生到班级的多对一单向关联关系；经过这两个步骤之后，就既可以获取一个班级的所有学生信息，同时也可以获取一个学生的班级信息；               

合理配置相关属性：      
#### 配置级联属性：cascade      

##### 问题1：        
如何在操作本对象的时候，可以级联操作其关联对象；    
这就需要设置cascade属性；        

- cascade属性的作用：  
cascade属性的作用就是可以实现对关联对象的级联操作；比如：级联保存、级联更新和级联删除等等；     
在我们学习数据库的时候，建立数据库表之间外键关联的时候，也可以进行类似的操作；同样会用到cascade属性，这是很相似的；  

cascade属性的常用属性值及功能描述：   

|属性值    |                        功能描述|
|------|------|
|none  |        不对关联对象进行任何级联操作；cascade属性的默认值；|
|save-update  | 保存和更新本对象时，级联保存和更新关联对象|
|delete  |      删除本对象时，级联删除关联对象|
|all  |             save-update   +     delete|

在实际业务中，cascade属性值的具体设置应该如何确定：    
cascade属性值的具体设置视业务需求而定；             

比如，当我们删除一个学生信息的时候，如果级联删除其班级信息可取吗？很显然，这是不合理的；因为，这个班级中可能还有其他的学生信息；反过来，当我们删除一个班级信息的时候，如果级联删除其学生信息，那在一定的业务情况下可能就是合理的。比如，学生毕业了，我们删除该班级，同时也从数据库中删除其学生信息；但是，在某些情况下可能就是不合理的，比如学校进行了班级调整；我们去掉一个班级，同时将其学生添加懂啊其它班级；这个时候，在删除班级的时候，同时删除其学生信息，就显得不太合理；
总之，cascade属性值的具体取值，要根据具体的业务需求而定，而不要随意的设置；             

在Grade类的映射文件中设置cascade属性：            
```
<set name="students" cascade="delete">
  <key column="GID"/>
  <one-to-many class="com.pb.hibernate.po.Student"/>
</set>
```

当在测试类中实现保存操作：  
```
session.save(g);
```

就会级联保存和班级类g相关的两个学生类stu1和stu2：
就会同时实现以下代码：
```
session.save(stu1);
sessoin.save(stu2);
```

同样，可以在学生类的映射文件中设置cascade属性，当实现保存学生对象的操作时，同样会实现保存与其相关的班级的操作；


##### 问题2：     
如何避免一对多关联关系中所产生的多余的更新语句；     
配置反转属性： inverse            
作用：            
将维护关联关系的任何反转，反转给关联关系的另外一方，由对方来完成；而，自己放弃维护关联关系，也就不会再产生相应的SQL语句；          

inverse常用属性值及功能描述：     

|属性值  |                        功能描述|
|-------|------|
|false  |          不放弃关联关系维护，默认值|
|true  |           放弃关联关系维护（不产生相应的数据库操作）；|

问题：         
	在一对多关联关系中，我们是是可以让任何一方来放弃关联关系的维护呢？
	或者说，我们可以给任何一方设置inverse属性？
	不是的！
	inverse属性只能用于集合属性中。不能用于普通属性中。
	"一"方反转关联关系维护给"多"方；

而在一对多的关联关系中，比如，班级和学生的关联关系中，班级这一方具有集合属性；因为在Grade类中具有Set类型的属性students，用来存放该班级的所有学生信息；所以，如果我们要使用inverse放弃数据库维护的话，只能够在班级这一端进行设置，让班级这一端来放弃维护；而，在学生这一端是无法设置inverse属性的；因为在学生这一端并没有集合属性；        

举例：           
1个老师和50个学生，这很显然是一个一对多的关联关系；它们这双方只要有一方认识另外一方，就算认识了；而不要求双方相互认识；       
那么，这个时候，是要让老师放弃认识学生好呢，还是让学生去放弃认识老师好呢；       
显然，老师认识学生，需要认识50个学生；而，学生认识老师，50个学生每个学生只需要记住1个老师的名字就可以；在这种情况下，让任务量大的老师去放弃认识所有的学生；这么做是比较合理的；即，让老师这个一方来放弃认识学生这个多方，而不是让学生这个多方来放弃认识老师这个一方；            
```
stu1.setGrade(g);
stu2.setGrade(g);
g.getStudents().add(stu1);
g.getStudents().add(stu2);
```

在学生对象中添加班级信息，在班级对象中添加学生信息；    
在班级类的映射文件中设置inverse属性：     
```
<set name="students" inverse="true">
  <key column="GID"/>
  <one-to-many class="com.pb.hibernate.po.Student"/>
</set>
```

这时，班级方放弃对数据库的维护，即不会产生多余的SQL语句；     


### 一对一关联关系：    
一对一关联关系概述：          

日常生活中的一对一关联关系：         
	人和身份证           
	学生和学生证          
	汽车和牌照               

数据库表的一对一关联关系：          
- 第一种：     
通过外键建立关联；        
对于一对一、一对多和多对多关系，都可以通过外键来建立表和表之间的关联关系；      

- 第二种：   
主键关联；       
仅用于一对一关联关系；           

#### Hibernate中一对一对象关联的实现方式：      
针对主键关联和外键关联，Hibernate要提供不同的实现方案；       
两种实现方案都需要两个步骤来完成：              
- 第一步：   
在持久化类中添加相应的属性及getter/setter方法；     

- 第二步：    
在映射文件中设置属性和数据库表字段的映射关系；      


### 数据库表中的一对一关联关系：    

主键关联：        
两个数据库表之间是如何建立主键关联的呢？     
以学生证表和学生表为例来进行讲解：           
学生表包括学生编号sid、学生姓名sname、学生性别sex三个字段组成；       
学生证表有pid代表学生证编号，和pdesc代表学生证描述信息两个字段组成；              
这两个表是如何建立关联关系的？           
在学生证表中并没有一个sid字段作为外键；            

学生表student和学生证表paper就是通过paper表的主键pid字段建立一对一关联关系；         
pid字段既是学生证表的主键，同时又作为外键来参考学生表的主键sid，所以sid和pid的值保存一致；        
这两个表完全可以合成一个表；只不过出于性能或者其他方面的考虑将其一分为二；               
如何在Hibernate中建立Student和Paper两个持久化对象之间的一对一关联关系呢？                 

#### 持久化对象的一对一双向关联关系：

一对一的关联关系比较简单，所以在这里就不再两个单向关系依次讲解了，而是直接的进行实现；       
步骤非常的简单，和一对多关联关系类似：      

- 第一步：      
建立学生到学生证的一对一单向关联关系；          
需要在Student类中添加一个Paper类型的属性，比如paper，然后添加其getter和setter方法，并在student的映射文件中通过`<one-to-one>`标签进行映射；        

- 第二步：     
在Paper类中添加Student类型的属性，比如student，并添加其getter和setter方法；并在Paper类的映射文件中通过`<one-to-one>`标签实现映射；  

即：          
Student类中增加Paper类的属性，在Paper类中增加Student类属性，及其setter和getter方法；     

两个映射文件中均通过`<one-to-one>`标签实现映射；              

通过主键关联而建立的一对一关联关系，比较简单并且不太常用；所以，具体在两个映射文件中如何通过`<one-to-one>`标签进行映射，就不再讲解了；       


#### 外键关联
数据库表一对一关联关系      
学生表student和学生证表paper通过paper表的sid字段建立一对一关联关系；   
sid字段不是学生证表的主键，而是作为外键字段参考学生表的主键；            

通过在学生证类中添加一个专门的字段sid，来建立两个表之间的关联关系；         
在学生证表中，pid代表学生证的编号，是学生证的主键；而sid是作为外键来参考学生表的主键，从而建立了两个表之间的关联关系；       

在Hibernate中如何建立学生和学生证两个持久化对象之间的一对一双向关联关系呢？         
分两步：         

- 第一步：        
在持久化类中添加相应的属性和方法；         
需要在学生类中添加Paper类型的属性，比如paper；并添加其getter和setter方法；然后，在Paper类中添加Student类型的属性，比如student，并添加其getter和setter方法；              

- 第二步：     
在映射文件中建立属性和数据库表字段之间的映射；           
在学生表映射文件`Student.hbm.xml`中通过`<one-to-one>`标签实现映射；      
在学生证的映射文件`Paper.hbm.xml`中通过`<many-to-one>`标签来实现映射；         
必须使用`<many-to-one>`标签，因为Hibernate是将一对一的外键关联关系，作为一种特殊的一对多关联关系来处理的；     

设置Student类          
首先，设置由学生类到学生证类的一对一单向关联关系：      

- 第一步：   
在Student类中添加Paper类型的属性paper，并添加其getter和setter方法，用来实现赋值和取值操作；        
代码如下：   
```java
private Paper paper;
public Paper getPaper() {
 return paper;
}
public void setPaper(Paper paper) {
  this.paper = paper;
}
```

- 第二步：    
在学生类的映射文件中进行相应的配置；    
需要通过`<one-to-one>`标签进行设置；        
代码如下：    
```
<one-to-one name="paper" class="com.pb.hibernate.po.Paper" cascade="all" lazy="false"/>
```

其中，配置四个属性；         

- `name属性`用来指定属性的值paper，`class属性`指定paper的类型；   
- `cascade`用来设置级联，其取值为all；在一对一的关联关系中涉及到了主动方和被动方的概念；比如在这个关联关系中，学生就是主动方，学生证就是被动方；当添加、更新或者删除学生信息的时候，应该对学生证进行相应的级联操作；学生没有了，学生证就不存在了；        
所以：     
在一对一的外键关联关系中，一般将主动方的cascade属性的值设置为all；       
- `lazy`属性用来设置懒加载或者叫延迟加载，其取值为false，表示立即加载；       
就是：    
在查询学生信息的时候，会同时立刻查询其对应的学生证信息；    
注意：       
在`<one-to-one>`标签中，并不需要指定该属性所对应的数据库表中的列；       


设置Paper类           
再来设置由学生证到学生方向上的单向关联关系；          
首先，在Paper类中添加Student类型的属性，比如student，并添加其getter和setter方法； 代码如下：
```java
private Student student;
public Student getStudent() {
 return student;
}
public void setStudent(Student student) {
 this.student = student;
}
```

然后，设置`Paper.hbm.xml`                        
在Paper类的映射文件`Paper.hbm.xml`中通过`<many-to-one>`标签设置属性和数据库表字段的映射；代码如下：           
```
<many-to-one name="student" class="com.pb.hibernate.po.Student" unique="true" lazh="false">
   <column name="sid"/>
</many-to-one>
```

在`<many-to-one>`标签中，一共有四个属性；         

- `name属性`和`class属性`分别用来指明持久化类中所添加的属性的名字及其属性的类型；`lazy属性`，其取值为false；其含义表示立即加载；即我们在查询学生证信息的时候会同时查询其对应的学生信息； 
在一对一关联关系中，一般将其lazy属性设置为false；          
`unique属性`，取值为true；unique是唯一的意思，取值为true，其含义就是，把`<many-to-one>`变成了`<one-to-one>`；  
在`<many-to-one>`的内部还有一个`<column>`，该标签就指定了与student属性所对应的数据库表的字段；是外键字段sid；     
注意：
在这里不能够使用`<one-to-one>`标签，Hibernate没有提供这种支持；    


### 多对多关联关系
多对多关联关系概述：    
日常生活中的多对多关联关系：          
	学生和课程       
	用户和权限
	商品和订单

#### 数据库中的多对多关联关系：
需要一个中间表来存储关联关系；    

以学生和课程关系为例进行讲解：      
首先，要创建一个学生表student，和一个课程表course，还需要创建一个中间表sc,用来存放学生的选课记录；   
中间表sc中有两个字段；          
第一个字段sid，作为外键来参考学生表的外键sid，表示学生编号；       
中间表的第二个字段cid，它也是作为外键来参考课程表的主键，表示课程编号；      
通过这个中间表，就建立了学生表和课程表之间的多对多关联关系；          
实际上，它是将多对多的关联关系，变成了两个一对多的关联关系；           
在学生表和中间表的关系中，学生表是一，中间表是多；              
在课程表和中间表的关系中，课程表是一，中间表是多；              

##### Hibernate中多对多对象关联的实现方式：
有两种方式：       
- 第一种方式：      
不创建中间表的持久化类，        
只创建两端数据库表的持久化类，在两个映射文件中分别使用`<many-to-many>`标签，也就是多对多标签来设置映射；       
针对学生和课程的实例，就是不创建中间表sc所对应的持久化类，只创建学生表和课程表的持久化类；然后在学生类和课程类的映射文件中，使用`<many-to-many>`来设置映射；         

- 第二种方式：     
创建中间表、两端数据库表的持久化类；针对中间表的持久化类分别和两端的数据库表的持久化类创建一对多关联；      
针对该示例，就是要创建三个持久化类；学生表、中间表和课程表所对应的持久化类；然后在学生类和中间类之间创建一对多的关联关系，再在课程类和中间类之间创建一对多的关联关系；      
它实际上是将多对多的关联关系，变成了两个一对多的关联关系；       
这种方式和数据库表在数据库中的存储方式是一致的；            

在这里只讨论第一种方式：             


##### 多对多单向关联：
首先来建立多对多的单向关联关系；    

以学生和课程的关系为例：        

数据库表：           
在数据库中，学生表student和课程表course通过中间表sc建立多对多关联关系；            
问题：     
>如何使用Hibernate获取一个学生的所有课程信息？       

分析：          

	很显然，这需要来建立由学生到课程的多对多单向关联关系；
	如何来建立这个单向关联呢？
	与前面一对多、一对一的关联关系类似，需要两步来实现：

首先，需要在学生类student类中添加一个集合类型的属性，用来存放课程信息，比如courses属性，并添加其getter和setter方法；然后，在课程类的映射文件中使用`<many-to-many>`标签进行配置；     


具体的设置：           
设置Student类：          
- 第一步：     
为Student类添加set集合类型的属性course，并创建其对象，然后，添加其getter和setter方法；代码如下：      
```java
private Set courses = new HashSet();
public Set getCourses() {
 return courses;
}
public void setCourses(Set courses) {
  this.courses = courses;
}
```

- 第二步：      
在学生类的映射文件中，为刚刚添加的属性courses设置映射；        
因为courses是Set类型的，所以使用<set>标签进行设置；        
```
<set name="courses" table="sc" cascade="save-update">
  <key column="sid"/>
  <many-to-many class="com.pb.hibernate.po.Course" column="cid"/>
</set>
```

name属性指明属性的名称为courses；         
然后，在其内部建立`<many-to-many>`标签，通过其内部的class属性，指明集合中元素的类型，为`com.pb.Hibernate.po`包下的Course类；     
在<set>标签中的cascade属性，其值为`save-update`，含义就是，在保存或者更新学生信息的时候，可以级联保存和更新其课程信息；        
设置到目前，还没有跟中间表发生联系；        
我们应该指明中间表的名称，以及中间表的关联字段；             
因此，我们需要在<set>内部通过table属性，来指明关联表的名称为sc；然后，在其内部的`<many-to-many>`内部，通过column属性来指明中间表中与课程类发生关联的字段是cid；最后，在<set>内部再添加内部标签<key>,通过其column属性指明中间表中与学生类发生关联的字段是sid；             


#### 多对多双向关联：       
经过前面的多对多单向关联的设置，已经能够获取一个学生的所有的课程信息；          
问题：        
>如何在获取一个学生的所有课程信息的同时，还可以获取选择一门课程的所有学生信息？          
分析：      
>按照单向多对多的配置规则对Course类和`Course.hbm.xml`进行配置即可；   

在配置了两个方向上的单向关系之后，其实，就创建了多对多的双向关联；      


具体的设置
设置Course类：         
- 第一步：       
在课程类中添加Set集合类型的属性students，并创建其对象，用来存放选择一门课程的所有的学生信息，并为该属性添加getter和setter方法；   
```java
private Set<Student> students = new HashSet<Student>();
public Set <Student>getStudents() {
  return students;
}
public void setStudents(Set <Student>students) {
  this.students = students;
}
```

- 第二步：       
在课程类的映射文件`Course.hbm.xml`中，为刚刚添加的students属性设置映射信息：   
```
<set name="students" table="sc" cascade = "save-updata" >
  <key column="cid"/>
  <many-to-many class="com.pb.hibernate.po.Student" column="sid"/>
</set>
```

关联双方的映射代码其结构是完全相同的；只是设置的属性不同；     
**注意**：          

	别忘了设置inverse属性；
	因为多对多关联的双方，都具有<set>；所以，可以在其中的一个<set>内部，
	将其inverse属性的值设置为true；从而将维护数据库的任务反转给对方；
	如果忘记了进行该设置，就要产生多余的更新语句了；


### 对象关联中的集合映射
在前面的示例中，不管是一对多关联映射，还是多对多关联映射；在映射文件中，设置集合属性的时候，都使用了<set>；而在Hibe rnate中配置集合属性，        
除了<set>之后，还可以使用<list>，<bag>和<idbag>；      
这四个标签的区别：        
```
<set> :    
元素没有顺序，不允许存放重复元素
<list> :
元素有顺序（索引顺序），允许存放重复元素
<bag> :
元素没有顺序，允许存放重复元素。 它是Hibernate自定义类型，在Java中并没有其对应的集合实现类；
<idbag> :
对bag进行了扩展，提供了collection-id标签来配置id字段，可以准确定位元素；
这四个标签中，使用最多的是<set>；
```