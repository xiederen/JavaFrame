# 概述
`MVC`是一种流行的软件设计模式，代表了一种多层的应用程序实现方式；     
##### MVC模式将应用程序的实现分离为三道不同的层：        
- 模型层
- 视图层
- 控制层

##### 那么，MVC就是这三层的缩写，分别为：
- Model
- View
- Controller

MVC模式的目的是为了实现Web系统的职能分工。     


####  MVC发展史

`MVC`的目的是将模型和视图，也就是业务逻辑和页面展示分离，这样，就可以对业务逻辑和页面展示进行独立修改，而不会相互影响。       

### Model I 和 Model II
Java技术中Web编程技术的发展
- CGI技术
- Servlet
- JSP
- J2EE

无论是微软还是Java，它们的动态Web编程技术，都是从CGI开始发展的。             
CGI即通过网关接口，是一个用于规定Web服务器与外部程序之间通信方式的标准。     
我们只有通过CGI程序才能实现动态的Web页面；            
随着CGI技术的发展，在Java领域首先出现的是Servlet；         
但是，没过多久，由于Servlet不能很好的实现前台页面的定制排版，因此很快就出现了JSP，而随着JSP的发展才有后来的J2EE。          
那么，从整体来说，对于Java阵营的动态Web编程技术而言，可以说经历了两个时代：      
这两个时代就是所谓的 Model I 时代 和  Model II 时代 。          

#### Model I 时代
所谓 `Model I`，就是JSP大行其道的时代；          
在`Model I`模式下，整个Web应用大部分是用JSP页面来实现的；        
`Model I`模式共有两种使用JSP开发Web应用系统的情况：          

- 一种是纯JSP方式的开发；                 
另外一种是使用 `JSP + JavaBean`开发应用程序。          

- 纯JSP方式的开发        
最直观的使用JSP开发Web应用的方法，就是在JSP中直接嵌入Java代码；    
即，小脚本方式；          
所有的逻辑控制和业务处理都是以小脚本的方式实现。         
使用这种方法的优点是简单方便，适合搭建小型的Web应用；         
但使得页面显得非常混乱，并不易于后期的维护扩展。            

##### 执行流程：
客户端请求服务器，服务器执行JSP页面，访问数据库，数据库返回数据到JSP页面，服务器响应客户端，将JSP页面返回到客户端；       
这种纯JSP方式开发只适用于刚接触JSP时使用，在实际开发中不推荐使用。        

#### JSP + JavaBean开发应用程序      
那么，随着人们的开发实际，发现纯JSP开发模式的缺点越来越突出；      
在实际的开发中，就对纯JSP方式开发作出了一些改进；         
使用JavaBean封装数据和业务处理；         
使用可重用的JavaBean后，相对于纯JSP方式开发使得页面简洁，有利于代码的重用和后期的维护；           
但还是存在很多的限制；            
程序的逻辑控制还是由JSP完成，页面中依然需要嵌入大量的Java代码。      
##### 业务流程：    
客户端发送请求到服务器，服务器执行JSP页面，调用JavaBean，请求数据库，数据库响应，将数据传递给JavaBean，JSP调用JavaBean，服务器将响应传递给客户端；        

#### Model I 模式的缺点
代码重用          
使用`Model I`开发时，需要经常访问数据库进行数据验证，或读取数据。例如我们在编写程序时，进行用户登录验证的功能，处理页面中的代码；       

不利于后期维护与扩展         

页面维护问题    
当构建一个项目的时候，必须考虑到美工美化界面的问题。         


### Model II模式

使用`JSP + Servlet + JavaBean`共同开发应用程序的模式就是 Model II模式；      

基于 `Model I` 模式所存在的不足，当程序流程非常复杂的时候，想要修改一个程序带来的工作量将非常的大。        
为了克服`Model I`的缺陷，人们引入了`Model II`模式；        
在`Model I`中，JSP页面嵌入了流程控制代码和业务逻辑处理代码；          
我们将这部分代码提取出来，放入单独的类中；     
也就是使用 `JPS+Servlet+JavaBean`共同开发应用程序；   
而这样的方式也就是 `Model II` 了；           
`Model II`体现了基于`MVC(Mode-View-Control,模型-视图-控制器)`的设计模式； 
简单的说，就是将数据的显示、流程控制、业务逻辑处理相互分离；       
使之相互独立；        
对于`Model II`模式来说，它集成了JavaBean、JSP和Servlet；     
那么，JavaBean充当模型、JSP充当视图、Servlet充当控制器；       
这样做，就能满足逻辑及业务处理相对复杂的大型Web应用程序。             


## 什么是 MVC模式
MVC是 `Model-View-Controller`的缩写         
它由三部分组成，分别是**模型、视图和控制器**；      

每一种组件和技术都有各自的功能和特点；       
在编写程序时，我们应该是以他们的功能来设计他们的作用。      
在程序设计中，把采用模型、视图、控制器的设计方式称为MVC设计模式。        
MVC模式有助于将应用程序分割成若干逻辑部分，使程序设计变得更加容易。           
MVC提供一种按功能对各种对象进行分割的方法，其目的是为了将各对象间的耦合程度降到最低。          


## 什么是设计模式
设计模式是一套被反复使用、成功的代码设计经验的总结。     
模式必须是典型问题的解决方案。       
设计模式为某一类问题提供了解决方案，同时设计模式优化了代码，使代码更容易让别人理解，提高可重用性，保证代码的可靠性。           

## 什么是框架
框架是一种可重用的、半完成的应用程序。        
开发者利用它快速开发需要定制的程序。            
面向对象系统获得的最大的复用方式就是框架；             
一个大的应用系统往往可能由多个互相协作的框架组成。             
在某些方面使用成熟的框架，将相当于别人帮你完成了一些基础的工作。   
你只需要集中精力完成系统的业务逻辑设计。            
框架可以处理系统很多细节问题，例如：事务处理，安全性问题等等；         


#### 设计模式和框架之间的区别
- 设计模式是设计重用，框架是部分代码重用和部分设计重用；
- 设计模式是抽象描述，框架有具体的代码实现；
- 设计模式针对各种应用，框架针对某一特定应用；

构件通常是代码重用，例如我们使用JavaBean就是为了能够实现代码重用；       
设计模式是一种设计的重用；       
框架则介于两者之间，实现部分代码重用和部分设计重用。        
框架与设计模式虽然相似，但却有着根本的不同。           
设计模式是对某种环境中反复出现的问题以及解决该问题的方案的描述。         
它比框架更抽象，框架可以用代码表示，也能直接执行或复用；       
而对模式而言，只有实例才能用代码表示，设计模式是比框架更小的元素；     
一个框架中往往含有一个或多个设计模式，框架总是针对某一特定应用；         
但同一模式却可适用于各种应用。              
可以说，框架是软件，而设计模式是软件的知识。          


## MVC模式介绍
MVC是一种流行的软件设计模式，代表了一种多层的应用程序实现方式；    
##### MVC模式将应用程序实现，分离为三个不同的基本部分：          
- 第一个是 模型(Model) ,表示数据和业务处理。    
在MVC的三个部件中，模型拥有最多的处理任务。      
被模型返回的数据是中立的，就是说模型与数据格式无关。   
这样一个模型能为多个视图提供数据。        
由于应用于模型的代码只需写一次，就可以被多个视图重用，所以减少了代码的重复性。  对应的组件是 JavaBean。         

- 第二个部分是视图，是用户看到并与之交互的界面。        
MVC一个最大的好处是，它能为你的应用程序处理很多不同的视图。    
在视图中其实没有真正的处理发生，不管这些数据是什么；       
作为视图来讲，它只作为一种输出数据并允许用户操纵的方式。     
对应的组件是JSP或HTML文件；             

- 第三部分是控制器，接受用户的输入并调用模型和视图去完成用户的请求。    
所以当用户提交一个请求时，控制器本身不输出任何东西，不做任何业务处理。     
它只是接收请求并决定调用哪个模型组件去处理请求；然后，确定用哪个视图来显示模型处理返回的数据；         
对应的组件是Servlet；          

#### 模型可以分为业务模型和数据模型；
它们代表应用程序的业务逻辑和状态。    
视图提供可交互的客户界面，向客户显示模型数据；         
控制器主要响应客户的请求，根据客户的请求来操作模型，并把模型的结果经过视图展现给客户。         

### MVC模式的优点
- 第一：        
各司其职，互不干涉        
就是说，视图和业务分离，这样就允许更改视图层代码，而不用重新编译模型和控制器代码；同样，一个应用的业务处理流程，或者数据库的改变只需要改动MVC的模型层即可。因为模型与控制器和视图相分离；       
所以，做任何改变都是很容易的；          
在MVC模式中，三个层各司其职，所以，如果哪一层的需求发生了变化，就只需要更改相应层中的代码，而不会影响到其他层。        

- 第二：   
有利于开发中的分工；       
在MVC模式中，由于按层把系统分开，就能更好地实现开发中的分工。       
网页设计人员可以专门进行视图中的页面开发，Java开发人员可以开发模型中相关业务处理的方法和控制器。          
使用MVC模式使开发时间得到相当大的缩减；       
它使Java开发人员集中精力于业务逻辑和控制；            
网页设计人员集中精力于表现形式上；      

- 第三：    
有利于组件的重用；           
MVC模式允许你使用各种不同样式的视图来访问同一个服务器端的代码。        
比如，用户可以通过电脑也可以通过手机来订购某样产品；       
虽然订购的方式不一样，但处理订购产品的方式是一样的；          
由于模型只负责返回的数据，所以同样的数据能被不同的视图使用。       
例如，很多数据可能用页面来显示，但是也有可能用文件来显示；         
而这些表示所需要的仅是改变视图层的实现方式；       
而控制层和模型层无需做任何改变。          
分层后更有利于组件的重用。       


### MVC模式的不足
- 第一：   
系统结构和实现较复杂；          
对于简单的界面，严格遵循MVC，使用模型、视图与控制器分离，会增加结构的复杂性，并可能产生过多的更新操作，降低运行效率；         
由于将一个应用程序分成了三个部分，这就意味着将要管理比以前更多的文件；      

- 第二：       
MVC模式的视图与控制器过于紧密    
视图与控制器本来是相互分离的，但又是联系紧密的部件；         
视图没有控制器的存在，其应用是很有限的；        
反之亦然；        
这样就妨碍了它们的独立重用。           
此外，视图可能需要多次调用才能获得足够的显示数据。        
对未变化数据的不必要的访问，也将降低运行效率；       

- 第三：     
MVC模式不适用于小型甚至中等规模的应用程序。       
在应用程序中使用MVC模式，需要精心的计划；         
由于MVC内部原理比较的复杂，将不得不花费相当多的时间考虑，如何设计视图、模型和控制器。           
同时将一个应用程序分成了三个部分，使开发工作量有所增加；   
对于并不是很大的应用程序，花费大量的时间使用MVC模式有些得不偿失；           


##### 开发基于MVC模式的应用程序

使用 `JSP+Servlet+JavaBean`来开发一个基于MVC模式的应用程序；       

### MVC程序编程思想
在我们使用MVC模式进行编程时，需要注意各个组件的分工与协作；        
当客户端发送请求时，服务器端Servlet接收请求数据，并根据数据，调用模型中相应的方法访问数据库，然后把执行结果返回给Servlet，Servlet根据结果转向不同的JSP或HTML页面，以响应客户端请求；      
这个过程就是开发MVC程序的思想。       
需要注意：    
在使用MVC模式开发程序时，应注意在视图中，不要进行业务逻辑和程序控制的操作；视图只是显示动态内容，不做其他操作。模型与控制器也是一样，它们有各自的工作内容；应该让它们各尽其职；        




### MVC模式流程图：
在使用基于MVC模式的`Model II`开发应用程序时，作为View视图的JSP页面接收用户的输入，并且传递到作为Controller控制器的Servlet统一进行处理，然后由Model模型来处理具体的业务；经过处理后，控制器根据返回的结果选择一个视图显示请求的数据；    

##### 通过一个具体的示例来熟悉一下具体的开发过程：

基于MVC模式的用户登录功能示例：  

- 首先，视图的开发,实现用户登录功能，首先要创建作为视图的JSP页面；       
用户登录需要两个JSP页面，一个是登录页面`login.jsp`，另外一个是登录成功后显示管理功能的页面`manage.jsp`；
首先，开发`login.jsp`： 
如下代码：    
```jsp
<from action="servlet/login" method="post">
  <table align="center" bgcolor="Green" width="280">
  <caption align="left">用户登录</caption>
     <tr>
  <td><p>用户账户：<input type="text" name="username"
             style="widht:150px" /></p></td>
     </tr>
     <tr>
  <td><p>用户密码： <input type="password" name="password"
              style="width:150px" /></p></td>
     </tr>
     <tr>
         <td align="center"><input type="submit" value="立即登录" /></td>
     </tr>
  </table>
</form>
```

在这个页面，我们没有添加任何业务处理的代码，仅仅是用户账号输入框、用户密码输入框和提交按钮。       
当点击提交的时候，页面会以POST的方式提交到 `servlet/login`     这个Servlet中进行业务处理；         
`manage.jsp`  登录成功后的跳转视图       
如下代码： 
```jsp
<body>
   <h2>欢迎你，<%=session.getAttribute("login")%></h2>
</body>
```

这里是一个简单的示例，所以不用加载复杂数据，仅仅是把当前登录用户显示出来了；    


#### 模型的开发
`Model II`模式使用`JavaBean`作为模型来表示数据和业务处理；         

那么对于这个示例，仅仅是为了实现登录功能；    
那么，这里的模型就要实现登录的功能处理；      
当我们登录的时候，我们要输入用户名和密码，因此，首先要创建一个用户的实体类；  

用户实体类      
用户实体类包含两个属性 用户登录名 和 用户密码         
设置好两个属性之后，分别为它们添加get和set方法；          


当我们创建好了用户实体类之后，下一步要实现登录操作；      
因此，定义一个实现登录操作的接口；       

用户登录操作的接口：        
```java
public interface UserBiz {
 public User login(String username,String password);
}
```

这个接口的定义也比较简单，里面只包含了一个方法，就是登录方法；这个方法需要把用户登录信息当做参数传递进去，也就是用户名和密码；          

既然定义了接口，就必然要实现这个接口对象；      
接下来，来完成这个接口的实现类；    

用户登录操作接口的实现       
```java
public class UserBizImpl implements UserBiz {
 public User login(String username,String passwd) {
   User user = null;
   if(username != " " && passwd != " ") {
  user = new User();
  user.setUsername(username);
  user.setPasswd(passwd);
     }
     return user;
   }
}
```

在这个接口实现类里，用到了前面定义的实体类，当用户登录的时候填写了用户登录信息之后，我们就把这些信息进行封装，封装到用户实体类中，然后把这个实体类进行返回；   


#### 控制器开发  
`Model II`模式 使用Servlet作为Controller控制器；     
其作用是接收用户请求的数据，选择合适的Model模型来处理具体的业务；         
处理后，根据模型返回的结果选择一个视图显示请求的数据；          

登录Servlet的实现：         
为实现登录，我们创建了 `Login.java`；  
这是一个Servlet，它用于接收登录信息；并选择业务层中UserBizImpl处理登录数据，并根据处理的结果选择视图显示结果：         
```java
public void doGet(HttpServletRequest request,HttpServletResponse response)          throws ServletException,IOException {
  request.setCharacterEncoding("UTF-8");
  String name = request.getParameter("username");
  String password = request.getParameter("password");
  UserBiz ub = new UserBizImpl();
  User user = ub.login(name,password);
  if(user == null) {
     request.setAttribute("message","用户名或密码错误");
     request.getRequestDispatcher("/login.jsp").forward(request,response);
  }else{
        System.out.println(user.getUsername());
        request.getSession().setAttribute("login",user.getUsername());
        response.sendRedirect("../manage.jsp");
    }
public void doPost(HttpServletRequest request,HttpServletResponse response)   throws ServletException,IOException {
  doGet(request,response);
}
```

需要注意，在这个Servlet的doGet方法中，我们首先要设置字符集编码；否则的话可能出现中文乱码现象；      
控制器的作用就是接收用户请求数据，然后交给模型处理，最后根据模型的处理结果，再选择合适的视图进行显示；          
那么，在这段代码中，我们首先要从request对象中获取用户的请求数据；        
当我们得到请求数据，也就是得到用户名和密码之后，我们创建了一个UserBiz类型的对象，然后把我们得到的数据传进去,UserBiz对象用来实现用户登录操作；    
操作完成之后，控制器，也就是,Login Servlet还要进行判断；     
当user等于null的时候，说明用户名或密码错误，那么就返回到登录页面；      
相反，当user不等于null，说明登录是成功的，就跳转到`manage.jsp`页面；    

在`Login Servlet`中，我们同时使用了转发和重定向；  
当我们登录失败的时候，我们是转发到登录页面；         
当我们登录成功的时候，我们是重定向到`manage.jsp`页面；      

那么，回顾转发和重定向的知识：  
回去看看；    


`POST语义`：提交了数据，服务器端数据应该发生变化；         
`GET语义`：取得数据，不会对服务器造成影响；         

因此，对于POST请求，建议使用重定向而不是转发；       


#### 自定义MVC框架的实现

在前面已经讲解了MVC设计模式，通过示例和演示视频，已经进入了MVC的世界。  
接下来，将从架构的角度学习项目是怎么构建的；   
各种技术是怎么组合使用的，也就是 “框架技术”。     
在前面的课程中，我们也分析过什么是设计模式，什么是框架以及框架和设计模式的区别；那么现在，我们已经了解了MVC模式；接下来，我们要做的就是实现一个自定义MVC框架；     
通过实现自定义的MVC框架，我们可以更加深入的了解MVC设计模式；       


#### 如何构建基于MVC模式的框架

##### 构建基于MVC模式的框架      
MVC是一种设计模式。          
它的目的就是将模型(业务逻辑)和视图(页面展示)进行分离；  
使模型和视图可以独立修改，而不会影响到对方；          

##### 使用MVC模式有很多好处；
当一个通过浏览器浏览使用的系统，想要开发手机版本时，只需要重新开发视图，模型部分定义的业务逻辑不用修改即可重用。      
很多软件需要同时推出`B/S架构版本`和`C/S架构版本`；     
采用MVC模式，模型部分可完全重用，开发不同的视图即可。         
视图负责展示数据、获得用户输入的部分；       
控制器负责从视图接收用户输入，调用模型，返回数据到视图。         
控制器在MVC中起到类似 "中介"的作用；从而保证模型和视图不会直接交互，从而使一方更改时另一方无需跟着修改；       

在前面的示例中，Servlet充当控制器的角色，接收用户的请求，并调用实现业务逻辑的JavaBean； 
JavaBean就相当于模型；然后由Servlet将执行结果返回给JSP页面；由此可见，控制器在MVC模式中是至关重要的； 
那么，在通常的情况下，控制器由Servlet或Filter实现；            

那么，结合框架的概念，如何来构建基于MVC模式的框架呢？               


分析：             
框架是一个可重用的，半完成的应用程序，那么我们就可以知道框架的特性；                      

可重用        
          
它是一个半成品；      
 
它又同时是一个应用程序；       

那么，我们就结合框架来分析前面学习的MVC模式：        
首先，我们可以将所有的请求发送到控制器；                            
并且，系统中最好只有一个控制器负责接收请求并调用模型；            
然后，我们还需要定义一个Action接口；                                  
这个接口用于表示用户的请求；比如：登录请求；           
当不同的业务实现这个通用的Action接口后，控制器根据请求的路径，可以判断哪个Action执行操作；           
最后，Action调用模型，完成业务操作并获取操作结果；最后，返回给视图；                     

这个样子就是下面将要实现的自定义MVC框架；                  

对于MVC框架来说，我们的实现核心是控制器。                   
那么如何才能做到所有请求都发送到一个控制器呢？               
如下：                
配置Servlet访问路径          
```xml
<servlet-mapping>
   <servlet-name>ActionServlet</servlet-name>
   <url-pattern>*.action</url-pattern>
</servlet-mapping>
```

控制器ActionServlet类基于`Servlet API`实现，在配置Servlet访问路径时可以配置成`*.action`，表示只要是以 `.action`结束的请求，就会派发到ActionServlet。           

##### 接下来，以登录为例，一起来完成自定义MVC框架：            
MVC分为三层，那么首先来看模型层：         

- Model设计         
通常情况下编写处理请求的代码前，我们要将相关的业务层以及数据访问层的代码编写好；        
作为Model的业务层、数据访问层代码与之前我们开发的业务层、数据访问层代码并没有区别。       
所以在这里，以登录为例，我们的模型层代码的开发重用原来的就可以了。          

- 自定义MVC框架          
接下来，我们来看自定义MVC框架中Controller的设计；       

- Controller设计：       
对于MVC框架，其核心是控制器，因此这部分设计是我们需要重点关注的；          
在前面我们已经提到，并且我们已经制定了一个把所有请求都发送到一个控制器的策略，如下代码：      
```xml
<servlet-mapping>
   <servlet-name>ActionServlet</servlet-name>
   <url-pattern>*.action</url-pattern>
</servlet-mapping>
```

也就是我们配置了一个公有的Servlet访问路径；那么，所有以`.action`结束的请求就会派发到ActionServlet；那么，接下来，我们就来实现这个ActionServlet；                  

我们要实现ActionServlet，首先我们先来定义一个Action接口；       
定义Action接口：         
```java
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
/**
* Action接口
*/
public interface Action {
  public String execute(HttpServletRequest request,
      HttpServletResponse response) throws Exception;
}
```

Action接口中只定义一个execute方法；         
传入两个参数，即request和response；              
有一个字符串类型的返回值，表示执行完这个操作转发到哪个页面；              


当我们定义好Action接口之后，接下来我们来创建一个这个接口的实现类，这个实现类就用来实现登录功能；         
实现登录Action         
```java
public class LoginAction implements Action {
  public String execute(HttpServletRequest request,
                    HttpServletResponse response) throws Exception {
 request.setCharacterEncoding("UTF-8");
 String name = request.getParameter("username");
 String password = request.getParameter("passwd");
 //业务处理
 UserBiz ub = new UserBizImpl();
 User user = ub.login(name,password);
 //判断登录是否成功
 if(user == null)
      request.setAttribute("message","用户名或密码错误");
      return "/login.jsp";
    }else{
       request.getSession().setAttribute("login",user.getUsername());
       return "/manage.jsp";
     }
  }
}
```

那么，对于每个Action类都表示用户请求的一个操作。          
首先从request中获得页面输入的数据，然后调用业务方法获得结果。             
再将结果保存到request对象中。        
`request.getParameter()`和`request.setAttribute()`是Action和JSP页面交互的主要方式。       
Action类是模型和视图交互的中枢。               
那么在这里，我们首先通过 `request.getParameter()` 方法获得用户请求的用户名和密码，然后调用模型层的处理类UserBiz；   
对于模型层的UserBiz接口和其实现类UserBizImpl，在前面已详细介绍过它们的创建方法；这里还是重用原来的代码；            
当我们的模型层完成业务处理后，接下来我们就来进行判断，判断登录是否成功；      
如果不成功，返回一个字符串表示要跳转到登录页面；            
相反，如果登录成功，同样返回一个字符串，但是表示要跳转到登录成功的页面；                  

当我们完成Action接口的定义及实现之后，接下来我们来实现我们的Controller类；也就是前面我们配置的ActionServlet；   
ActionServlet类基于Servlet技术实现；           
在处理每次请求时，首先根据请求路径找到将要被执行的Action，然后调用Action的`execute()`，根据`execute()`返回的转发路径转发到对应的JSP页面；                                
如下代码：          
实现Controller类：           
```java
public class ActionServlet extends HttpServlet {
 public void doGet(HttpServletRequest request,
   HttpServletResponse response) throws ServletException,IOException {
  //获得Action
  Action action = this.getAction(request);
  //调用Action execute方法
  String resultView = null;
  try {
        resultView = action.execute(request,response);
  }catch(Exception e) {
       e.printStackTrace();
  }
  //页面跳转
  if(null != resultView) {
     request.getRequestDispatcher(resultView).forward(request,response);
   }
 }

 public void doPost(HttpServletRequest request,
   HttpServletResponse response) throws ServletException,IOException {
           this.doGet(request,response);
 }

 public Action getAction(HttpServletRequest request) {
  //获得请求的uri
  String uri = request.getRequestURI();
  String contextPath = request.getContextPath();
  //截取上下文路径以后的部分
  String actionPath = uri.substring(contextPath.length());
  //获取Action名称
  String actionName = 
    actionPath.substring(1,actionPath.lastIndexOf('.')).trim();
  Action action = null;
  //添加新功能时在这里添加
  if("login".equals(actionName)) {
      action = new LoginAction();
  }
  return action;
  }
 }
}
```

在ActionServlet中，主要有三个方法；           
第一个是`doGet()`，第二个是`doPost()`，最后一个是`getAction()`；        
`getAction()`也就是获得用户请求的方法；                           
接下来，看一下`doGet()`；          
在`doGet()`中，我们首先通过`getAction()`来获得Action，也就是用户请求；    
然后，我们调用Action中的`execute()`来获取转发路径；        
接下来，我们会判断如果转发路径不为空，那么我们就会跳转到相应的页面；          
这就是ActionServlet中`doGet()`主要实现的功能；         

完成ActionServlet代码后，还需要在`web.xml`中配置；        
如下代码：          
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app version=2.5"
  xmlns="http://java.sun.con/xml/ns/javaee"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
  http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd">
<servlet>
     <servlet-name>ActionServlet</servlet-name>
     <servlet-class>*.*.ActionServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>ActionServlet</servlet-name>
    <url-pattern>*.action</url-pattern>
</servlet-mapping>
</web-app>
```
按照这里给出的配置代码进行配置后，所有以 `.action` 结尾的请求将要全部派发到ActionServlet处理；        


那么到这里，Action接口和ActionServlet类就构成了自定义框架的MVC控制器部分；          
当我们自定义MVC框架的Controller设计好之后，最后就是视图的设计了；              

对于MVC框架来说，视图负责展示数据、获得用户输入。            
可以有多种技术来实现MVC中的视图功能；              
在这里，我们开发的自定义MVC框架中，使用JSP页面作为视图部分；           
在前面的登录示例中，我们已经开发了`login.jsp`和`manage.jsp`；             
那么现在，我们只要重用这两个页面来作为我们的视图就可以了；              
