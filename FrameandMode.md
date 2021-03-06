# 框架技术和设计模式；      
我们学习框架技术就是要从架构的角度，来学习成功的项目是怎么构建的，各种技术是怎么组合使用的。     
而设计模式是软件开发人员对代码设计经验的总结。      
它为我们在实际工作中遇到的那些常见问题，提供了完善的解决方案，它是一种程序设计的思想。           

##### 现有框架，比如：Hibernate。       
Hibernate是一种技术框架，我们前面学习的都是Java EE的基础知识，实际上我们学会这些东西只是学会了“写字”、“造句”，在这基础上我们还要学习前人组织程序代码的一些经验，它们已经为我们实现了很多框架，说的直白一些，就是给我们提供了可重用的公共结构的半成品，我们只需要掌握这些框架技术，就可以高效的构建性能优异并且安全的代码。                 

## 框架技术解决的问题      
我们将首先介绍框架；      
第一个问题就是：我们为什么要使用框架呢？      
我们通过一个日常生活中的例子来说明这个问题。          
大家想一下，在我们找工作的时候，一份好的简历是非常重要的。那么我们如何去写一份看上去具有很高水准的简历呢？            

一种简单的方法就是：         
我们可以使用Word为我们提供的模版功能。            
在我们新建简历文档的时候，我们可以从模版中选择一个合适的简历模版，该模版中已经给出了一个简历的架子，或者叫轮廓；           
在这里，我们只需要把必要的信息，像做填空题一样写到模版中就可以了。                 
非常的方便，并且简历非常的有水准。             

我们使用简历模版来创建简历的好处：                   
使用简历模版来编写简历，优点是显而易见的；         
比如说：             

首先：                              
使用简历模版将无需再考虑简历的布局和排版问题。大大的缩短了时间，提高了效率。           

第二点：                 
编写者将无需再考虑布局和排版问题。              
编写者可以专心的考虑简历的内容。                       
从而使简历的质量更加有保障。            

第三点：             
因为采用了简历模版，简历的结构统一，也便于人事人员更好的、更加方便的阅读和挑选简历。              

第四点：               
使用简历模版，即使是新手也可以写出很专业的高水准的简历。                  

总之，我们使用简历模版来编写简历，写出的简历，可以说，又好又快。可以又好又快的写出高水准的简历。         

与使用简历模版来编写简历一样，我们使用框架来构建项目也是基于同样的考虑。              
框架就相当于简历模版，当我们在开发软件的时候，确定了使用哪个技术框架之后，我们就已经有了一个半成品。然后，我们再在这个半成品里面加上内容，工作就完成了。                     

## 框架的技术优势：
框架的技术优势，与简历模版类似。            
也是主要表现在四个方面：                    

首先，开发人员将无需再考虑一些公共的问题。比如：事务问题、权限问题、异常处理问题等等。因为框架已经帮我们把这些事情都做好了；   

第二：开发人员就可以专心来考虑业务逻辑问题。 从而保证核心业务逻辑的开发质量。              

第三点：不同的开发者使用同一个框架来编写的代码结构统一，便于进行学习、便于开发、便于维护。

第四点：框架集成了前人的经验，新手使用框架，就相当于站在巨人的肩膀上，直接复用巨人的代码。          
从而便于开发出稳健的、性能优良的、并且结构优美的、高质量的程序。              

总之，我们使用框架来开发项目，将保证我们的项目又快又好。           


#### 什么是框架：
我们已经知道了使用简历模版来编写简历的优点，并且以此类比，了解了使用框架来构建项目的优势；那么，到底什么是框架技术呢？     
什么是框架：           
框架(Framework)是一个提供了可重用的公共结构的半成品。它为我们构建新的应用提供了极大的便利。一方面给我们提供了可以拿来就用的工具，更大意义上，给我们提供了可重用的设计。               

框架这个词最早出现在建造领域，指的是在建造房屋前期构建的建筑骨架；       
对于应用程序来说，“框架”的意义也是类似的；         
框架就是应用程序的骨架。             
开发人员可以在这个骨架的基础上加上血肉，加上自己的东西，从而快速的开发出完全符合自己需要的应用系统。                        


`Rickard Oberg`，是WebWork的作者和JBoss创始人之一。他曾经说过：         
框架的强大之处并不是源自它能让你做什么，而是在于它不能让你做什么。                
这句话强调了框架另一个层面上的含义就是：               
框架使混乱的东西变得结构化。                                    

莎士比亚也曾说过：“一千个人眼中有一千个哈姆雷特”。          
那么同样，如果没有框架的话，一千个人将写出一千种使用`JSP+Servlet+JavaBean`的代码；            
但是，一千个人只能写出一种基于Struts框架的Web应用程序；                       
这里的Struts就是一个开源的`Java Web`框架。              
应该说，框架保证了程序结构风格的统一；                 
从企业角度来讲，这有利于降低培训成本和降低软件的维护成本。            
应该说，框架在构造统一以及发挥创造力之间维持着一个得体的平衡。              

Gamma设计模式的提出者之一，也说过一句话，他说：         
框架就是一组组件，一组协调工作的组件。               
他描述的是框架的表现形式，同时也表明了框架和“组件”的区别。          
组件是什么？ ：                       
组件就是构建应用程序的“零件”。           
框架呢？ ：                                   
框架是一系列的组合在一起的“零件”。           
并且，框架还定义了“零件”之间协同工作的规则。                 


### 主流技术框架
我们已经知道了什么是框架以及框架技术的优点。      
那么，接下来我们就来看一下在Java中都有哪些主流的技术框架？              

#### Struts框架：       
Struts框架是最早的Java开源框架之一，它是MVC模式的一个优秀实现。                 
Struts框架定义了通用的Controller（控制器），通过配置文件（一般是struts-config.xml）隔离了Model（模型层）和View（视图层），以Action的概念对用户请求进行了封装，从而使代码更加的清晰易读。                    
Struts还提供了自动将请求的数据填充到对象中的数据绑定机制，还提供了自定义的页面标签，这些都可以用来简化代码的编写。                
Struts使快速的开发大型`Java Web`应用项目成为可能。                  

#### Struts 2框架
`Struts 2`以WebWork优秀的设计思想为核心，吸收了Struts框架的部分优点，                
提供了一个更加整洁的MVC设计模式实现的Web应用程序框架；                   
它引入了几个新的框架特征，比如：               
从逻辑中分离出横切关注点的拦截器、            
尽量的减少和消除配置文件、                         
使用了贯穿整个框架的强大的表达式语言                 
支持可变更和可重用的基于MVC模式的标签API；       
`Struts 2`充分的利用了从其他MVC框架中学到的经验和教训，从而使整个框架更加清晰、更加灵活。          
`Struts 2`采用了Struts的名字，但实际上它的内核却是WebWork。       
它并不是Struts的一个简单升级。                     


#### Struts框架：      
通过上面的介绍，我们已经对Struts框架有了一个大致的了解；       
下面，我们来分析一下Struts框架的优缺点；          
##### Struts的优点：               

- 第一：Struts是一种开源软件，从而使开发者更深入的了解其内部的实现机制。           
- 第二点：Struts框架使开发者在构建基于`JSP+Servlet`的Web应用时更加容易。     并且为开发者提供了一个统一的标准框架。     
- 第三：Struts框架实现了MVC模式，使结构更加的清晰，并且有丰富的标签可以使用。         
- 第四：Struts通过一个配置文件，就可以把握整个网络各部分之间的联系，并且提供异常处理机制，提供了数据库的连接池管理，还支持国际化功能。            

##### 缺点：
- 第一：Struts的Action必须是线程安全的，它仅仅允许一个实例去处理所有的请求。所以Action用到的所有资源都必须统一同步，这个就引起了线程安全问题。          
- 第二：Struts的适用范围是受限制的，它是一种基于Web的MVC解决方案，所以必须用到HTML、JSP和Servlet来实现它。           


#### Hibernate框架
Hibernate是一个非常优秀的、开源的持久化框架，负责简化对数据库的操作。           
我们都知道，Java是面向对象的，它的操作单位是对象。而目前主流的数据库并不是面向对象的，而是面向关系的，它的操作单位是数据库表；   
对象、数据库表，这是两个不同的结构，如何实现两者之间的简单的、有效的映射，或者叫转换：                
Hibernate就是来解决该问题的。                 
它通过简单配置和编码，就可以替代JDBC中繁琐的程序代码。           
Hibernate处理数据库操作的方式代表了当前的趋势。                       
Hibernate对JDBC进行了轻量级的对象封装，从而使Java程序员可以随心所欲的使用面向对象的编程思维来操作数据库。             
Hibernate可以应用在任何使用JDBC的场合；                     
既可以是Java的客户端程序、控制台程序、也可以是Web应用 程序。                 
或者说，既可以是`C/S`结构也可以是`B/S`结构的程序；                
这一点和Struts不同。Struts只能应用于`B/S`结构的应用程序中。                         

#### Spring框架             
Spring的出现改变了Java世界。                            
它的目标是使现有的`Java EE`技术更加易用和促进良好的编程习惯，而不是要替代这些技术。它是一个轻量级的框架，渗透到了Java EE技术的方方面面。               
Spring技术的核心功能是依赖注入容器和AOP。另外，还提供了声明式事务、对DAO层的支持等简化开发的功能。          
Spring就好比是粘合剂，它可以方便地与`Struts、Struts 2、Hibernate`等框架进行集成，从而组成目前流行的`SSH`架构。      

##### Spring框架的优点主要表现在以下几个方面：      
- 第一：低侵入设计，代码污染极低。      

- 第二：Spring独立于各种应用服务器，从而真正实现了`Write Once,Run Anywhere`。      

- 第三：Spring的依赖注入机制降低了业务对象替换的复杂性。      

- 第四：Spring开发并不完全依赖于Spring所有组件。         

开发者可以自由选择使用Spring框架的部分或者全部组件，          
目前流行的SSH框架就是选择了Spring的部分功能，然后与Struts和Hibernate进行整合。               


除了刚刚介绍的`Struts、Struts 2、Hibernate、Spring`四个框架之外，还有其他很多框架，比如：`DWR`框架就是一个Ajax框架，它使得Ajax开发变得非常的轻松。                     
另外，还有jQuery框架等等。                   

## 设计模式
设计模式简介：          
前面简单介绍了框架技术，下面再来分析一下设计模式；          
有些人对这两个概念容易混淆，其实它们之间的区别还是很大的；                   
设计模式是由四个人提出来的，我们一般称此四人为“四人帮”。他们在1995年合作完成了《设计模式》一书，该书成为了程序设计与开发的经典之作。设计模式是对所有面向对象设计原则，比如：单一职能原则、KB原则、里氏替换原则等的诠释和经典应用。如果真正掌握了设计模式，也就深刻的领悟了面向对象的设计原则。设计模式可以理解为我们生活中处理问题的解决方案，当我们想让程序具有某种特性的时候，就可以借鉴相应的设计模式。另外，设计模式还是一种编程思想，是智慧的结晶。我们提到的“四人帮”，当时一共总结出了23种设计模式，现在有些书籍对设计模式进行了扩展，提出了更多的设计模式，但也都是基于这23种设计模式而来的。对于这些设计模式，我们不应该滥用，而应该是合理的使用，也只有这样才能发挥它们的最大的功效。          

### 什么是设计模式：
要明白设计模式，就首先要知道什么是模式，简而言之，         
模式：          
人们在自己的环境中不断发现问题和寻找问题的解决方案的时候，发现有一些问题及其解决方案不断变换面孔重复出现，但在这些不同的面孔后面有着共同的本质，这些共同的本质就是模式。                  
那么，什么是设计模式呢？                  
设计模式：                 
设计模式是一套被反复使用、多数人知晓的、经过分类编目的、代码设计经验的总结。                 

#### 框架和设计模式的区别：
从三个方面进行比较。           

- 第一：
组件或者称之为构件，它通常是属于代码的重用。         
而设计模式，它属于设计重用；                   
框架则介于组件和设计模式之间，部分代码重用、部分设计重用。           
这是它们的第一点区别。            

- 第二点：            
设计模式是对在某种环境中反复出现的问题以及其解决方案的描述；它比框架更加抽象；        
框架可以用代码表示，也可以直接的执行或复用；                 
而对于设计模式而言，只有它的实例才能用代码来表示。           

- 第三点：          
设计模式是比框架更小的元素，一个框架中往往包含了多个设计模式；             
比如：在`Struts、Hibernate、Spring`框架的实现代码中都涉及了多个设计模式。           
框架总是针对某一个特定的应用领域，解决某一个问题，而同一个设计模式却可以适用于不同的应用。                     
可以说，框架是一个软件，而设计模式是软件知识。             


#### 设计模式的介绍：

实际上，设计模式作为软件领域里一个新兴的研究方向，虽然发展时间不长，但是它给设计和开发带来了很多好处，因此已经成为研究的一个焦点。  
因为我们已经说过，设计模式不仅是某些问题的一个解决方案。更主要的来说，它是一种思想，它是一种智慧的结晶。        
通常情况下，了解设计模式本身并不困难，但是在什么情况下选择哪一个设计模式，往往成为模式发挥作用的绊脚石。         
那么，在本次课程下面，将先介绍设计模式的要素，然后介绍一些常用的设计模式；                     
最后，我们一起来学习设计模式怎么解决问题，我们如何选择设计模式，选择了设计模式之后我们又如何使用设计模式。          


### 设计模式：
一位相关领域的专家说过：实际上，我们定义的每一个模式，都必须依照一定的法则构造出来，以便能够建立环境，列出此环境里的力，以及一个能够平衡这些力的位形。          
根据该建议，描述模式需要一定的格式，描述模式的格式是很多的；但是一般来说，大家都同意模式应当包含以下这些要素：          
```
名字(Name) 问题(Problem) 效果(Consequences) 解答(solution)
```
- 名字(Name) ：            
一个模式必须有一个有意思的、简短而又准确的名字。         
一个好的名字可以使对模式的抽象讨论变得容易。             
如果一个模式同时有多个名字的话每，那么其他的名字应当作为别名列出。             

- 问题(Problem)：
每一个模式必须有一个能够描述它的用意的问题，以便能够说明此模式在给定的环境和力中要达到的目标和效果。问题描述了应该在何时使用模式。       

- 效果(Consequences)：       
它描述了模式应用的效果及使用模式时应该权衡的问题。         
尽管我们描述设计决策的时候，并不总提到模式效果，但它对于评价设计选择以及理解使用模式的代价和好处都具有重要的意义。         

- 解答(solution)：          
也就是解决方案；                  
解答就相当于一个生产产品的指令，它可能包括图片、图表及文字等，用于确定模式的结构、所涉及的角色以及角色之间的协作，解答要显示出问题是怎么得到解答的。          
解答不仅要给出静态的结构，而且还要给出动态的行为。               

以上就是设计模式包含的四个要素。                 
这四个要素是大部分人认为模式应当包含的，但并不是说设计模式只能包含这些要素，它还可以包含其他要素。                 


#### 常用设计模式介绍：
在前面学习Java面向对象的时候，我们已经专门学习过了设计模式；所以在这里，我们就不再对各种设计模式进行详细的分析了，而是要针对今后要学习的三大框架中所涉及的模式做一些系统的介绍，这将有利于我们以后框架的学习。              
Struts框架涉及到的模式有：                   

MVC模式：                                 
Struts框架采用了MVC的设计思想；         
这种设计思想把应用逻辑、处理过程和显示逻辑分成不同的组件来实现，这些组件之间可以进行交互和重用。那么什么是MVC模式呢？             
所谓的MVC模式，就是`模型-视图-控制器模式`。        
MVC模式的结构图如下：                
```
                          View


     Model   <------------------------>  Controller
```

View  Model  Controller 之间互相连通  <--------------->            

在我们前面学习Java面向对象中，学习设计模式的时候，并没有提到MVC模式啊；              
实际上，MVC模式确实常常作为一个设计模式出现在各种讨论中；               
但实际上，MVC模式是一种架构模式，而不是设计模式。              
那么，如何来理解MVC模式呢？                    
当`模型-视图-控制器`，也就是MVC被当做一个模式提出的时候，其实它在很大程度上是由我们已经有的较小的模式组成的，比如：合成模式加上策略模式，再加上观察者模式；                   
MVC实际上就是在这三种模式基础上又多了一些东西；         
换言之，MVC是一个比设计模式更大的尺度上的模式；               
因此，我们说，MVC是一种架构模式；                      

我们已经知道了MVC模式是一种架构模式而不是设计模式，那么我们在这个基础上再深入一下，什么是架构模式，什么又是设计模式？         

##### 架构模式：
一个架构模式描述软件系统里的基本的结构组织或纲要。架构模式提供一些事先定义好的子系统，并指定它们的责任，给出把它们组织在一起的法则和指南。           
有些人也称架构模式为系统模式；总之，这是一个意思。           
一个架构模式常常可以分解成多个设计模式的联合使用；                    
那么，很显然，MVC模式就属于这样的一种模式，而Struts是MVC模式的一种实现。               

### 设计模式：
一个设计模式提供一种提炼子系统或软件系统中的组件，以及组件之间的关系的纲要设计。设计模式描述普遍存在的在相互通信的组件中重复出现的结构；这种结构解决在一定的背景中的具有一般性的设计问题。                

了解了架构模式之后，我们回过头来再看一看MVC模式；                
MVC模式分为三层，Model层，View层和Controller层；                 
也就是 模型层、视图层和控制层。                            

- 模型层: 模型层用于实现系统中的业务逻辑，通常可以使用JavaBean或EJB技术来实现。                  
- 视图层：视图层用于与用户的交互，它可以接收用户的请求，返回用户以结果。通常使用JSP来实现。                     
- 控制层：控制层位于模型层与视图层之间，是两者沟通的桥梁； 它可以分派用户的请求，并选择恰当的视图用于显示；同时，也可以解释用户的输入，并将它们映射为模型层可执行的操作； 控制层可以使用Servlet技术来实现。                     

我们再来分析Struts框架，它是MVC模式的一个实现；                       
Struts框架为模型层提供了Action和ActionForm对象；                  
所有的Action处理器对象，都是Action类派生的子类。                     
Action处理器对象封装了具体的处理逻辑，它调用业务逻辑模块，并把响应提交给合适的视图层组件以产生响应；         
Struts中的控制器功能是由ActionServlet和ActionMapping对象构成；              
核心是Servlet类的一个子类ActionServlet，它用来接收客户的请求。                      
Struts的视图层功能由JSP来实现；                        


#### Spring框架中使用的设计模式：          
在Spring的编码中使用了很多的设计模式，比如：`工厂模式、单例模式、模版模式、代理模式和策略模式`等。          
在前面，我们学习设计模式的时候，我们已经学习了工厂模式、代理模式以及策略模式。             
在这里，我们再一起回顾一下。           
在23种设计模式的创建型模式中，包含一种工厂方法模式；                  
该模式是比较常用的一个设计模式。我们在学习工厂方法模式之前，当时我们先学习了简单工厂模式，并由简单工厂模式的缺点引出了工厂方法模式；             

简单工厂模式，也叫做静态工厂方法模式，它并不在23种标准设计模式之中，但该模式是工厂方法模式的一个特殊的实现。              
简单工厂模式就是由一个工厂类来创建出所有产品类的实例；               
由一个工厂类根据传入的参数的不同，来决定到底创建出哪一种产品类的实例。             

##### 简单工厂模式：          
简单工厂模式就是由一个工厂类来创建出所有的产品类的实例。         
我们访问工厂类，工厂类会根据我们传入的参数的不同，来创建出我们想要的对象的实例；           
该模式的缺点在于，如果我们创建的产品特别多的话，工厂类就会变得非常的庞大。并且如果我们 新增加一种产品的时候，就需要修改工厂类的代码；很显然，这是不符合开闭原则的。                  
在此基础上，我们引入了工厂方法模式。从而有效的避免了简单工厂模式的这个缺点。                 

- 工厂方法模式：            
工厂方法模式是简单工厂模式的进一步抽象和推广。                  
由于使用了多态性，工厂方法模式既保持了简单工厂模式的优点，同时又克服了它的缺点。               
首先，在工厂方法模式中，核心的工厂类不再负责所有的产品的创建，而是将创建产品的任务交给子类来实现；           
而，这个核心的工厂类则摇身一变，变成了一个抽象的工厂类，仅仅负责给出具体工厂类必须实现的功能，而不再接触具体的产品实例化的细节。                   
这种进一步抽象化的结果，使这种工厂方法模式，可以用来允许系统在不修改具体工厂角色的情况下引进新的产品；我们只要再增加一个具体的工厂角色就可以。                         
这无疑增强了系统的灵活性和可维护性、可扩展性。               

#####  代理(Proxy)模式          
结构型模式中比较典型的代理模式。              
对于结构模型，它是用来处理对象该如何组织以及采用什么样的结构更加合理的问题。  代理模式就是一个比较典型的处理对象间关系的设计模式。        
代理模式给某一个对象提供一个代理对象，并由代理对象控制对原对象的引用。 通俗的讲，所谓的代理就是一个人代表另外一个人去完成某件事情。      
那么，代理人在完成这件事情的前后，还可以添加一些自己的行为。                     
也就是说，当一个客户不想或者不能够直接引用目标对象的时候，代理对象就可以在客户端和目标对象之间起到中介和桥梁的作用。          
代理可以分为很多的种类，比如：`远程代理、虚拟代理、保护代理、防火墙代理、同步化代理、智能引用代理`等等。                         
代理的种类虽然很多，但是它们所使用的设计模式都是相同的。也就是它们工作的原理都是相同的。                
`Spring AOP`中的JDK动态代理就是利用代理模式来实现的。             
代理模式的适用条件：                 
当对已有的方法进行使用的时候需要对该方法进行一些修改或者改进，这种情况下有两种改进选择：            

- 第一种方法:我们可以修改原有的方法来适应现在的使用方式。      

- 第二种方法：可以使用一个“第三者”方法来调用原有的方法，并对该方法产生的结果进行一定的控制。               

第一种方法明显的违背了“对扩展开放，对修改关闭”的开闭原则，因为需要修改原有的方法。而第二种方法却可以将功能划分得更加清晰，有助于后面的维护，而第二种就是代理模式，这就是它的适用条件。           

#####  策略(Strategy)模式     
在Spring框架中还用到了策略模式，策略模式属于行为型模式；         
行为型模式规定了各个对象应该具备的职责以及对象间的通信方式，它很好的规范了对象间的调用方式以及数据传递方式。         
策略模式就是一个比较具有代表性的行为型的设计模式。          
它定义了一系列的算法，并将每一个算法封装起来，使它们之间可以互相的替换。             
策略(Strategy)模式的适用条件            
策略模式适合于算法经常变化的情况；                    
比如：              

我们日常生活中的商场促销，商场可能会推出多种促销方法，比如打八折、比如买一送一、再比如周年庆等等。这些促销方法都可以设置为不同的策略。                   
我们可以定一系列的算法，把这些促销策略分别的封装起来，让它们之间可以相互的替换。算法的变化不会影响到使用算法的客户，可以独立于客户而变化，这就是策略模式的设计思想。          
在Spring中策略模式使用的地方也很多，比如：Bean对象的创建以及代理对象的创建等等。                

#####  单例模式        
在Spring框架中，还用到了单例模式。           

单例模式确保某一个类只有一个实例，而且会自行的实例化，并向整个系统提供这个实例。            
显然，单例模式的要点有三个，即，单例模式的特点。          

- 第一：单例类只有一个实例。    
- 第二：单例类必须自行创建这个唯一的实例。       
- 第三：单例类必须给所有其他对象提供这一实例。         
即，它必须自行向整个系统提供这个实例。

单例对象持有对自己的引用。             

单例模式的适用条件：            
当在一个系统中，要求一个类只有一个实例的时候，我们就可以使用单例模式。反过来说，如果一个类可以有几个实例共存，那么就没有必要使用单例类。                

##### 模板方法模式        
在Spring框架中还用到了模板方法模式；                   
所谓模板方法模式就是准备一个抽象类，将部分逻辑以具体方法形式给出。然后再声明一些抽象方法，使子类来实现剩余的逻辑；                 
不同的子类可以以不同的方式来实现这些抽象方法，从而使剩余的逻辑会有不同的实现。                  
模板方法模式就是先制定一个顶级逻辑框架，或者说先定义一个模板，而将逻辑的细节去留给具体的子类来实现。                
从模板方法模式的定义，可以看出该模式的一些优点：       
模板模式的优点：           

- 第一：  使用模板方法模式可以将代码的公共行为提取出来，从而达到复用的目的。也就是说，使用该模式，可以复用公共行为。    

- 第二：在模板方法模式中，父类的模板方法就控制了子类中的具体实现；        

从模板方法模式的思想就可以看出该模式的适用条件：            
一次性实现一个算法的不变的部分，也就是公共部分，而将可变的行为留给子类去实现。         
各个子类中公共的行为应该提取出来，并集中到一个公共父类中，这样可以避免代码重复，实际上这也是一种很好的编程习惯。         

上面讲解的这四种设计模式与我们将要学习的三种框架技术，联系是非常紧密的。          
除此之外，还有其它几种常用设计模式也是需要我们了解的，这就是适配器模式、命令模式和观察者模式。          
下面分别进行介绍：           
##### 适配器模式：
适配器(Adapter)把一个类的接口变换成客户端所期待的另一种接口，从而使原本因接口不匹配而无法在一起工作的两个类能够在一起工作。              
这一点很像变压器，变压器就是把一种电压转变为另一种电压；         
当把美国的电器拿回中国来使用的时候，用户就面临电压不同的问题。         
因为美国的生活用电电压是110V，而我国是220V；        
假如要在中国使用美国的电器，就必须有一个变压器负责把220V电压转换为110V；而这正像是适配器模式所做的事情，因此该模式也常常被称为变压器模式。             
适配器模式可以分为：             
类的适配器模式和对象的适配器模式两种不同的形式。              
它们之间的区别在于：            
类的适配器模式就是使用继承关系将被适配的类的API转换成目标类的API。              
而，对象的适配器模式并不是使用继承关系，而是使用了委托关系。               

##### 命令模式(Command)
命令模式(Command)属于对象的行为模式，命令模式又被称为行动模式(Action)或交易模式(Transaction)。        
命令模式把一个请求或者操作封装到一个对象中。          
命令模式允许系统使用不同的请求把客户端参数化，可以对请求进行排队或者记录请求日志，还可以提供命令的撤销和恢复功能。               
在战场上，指挥员对士兵发布各种命令。在运动场上，教练员对队员发布各种指令。这些都可以采用命令模式来实现。                 
命令模式是处理该类问题的有效解决方案。          
命令模式是对命令的封装。            
命令模式把发出命令的责任和执行命令的责任分割开来，委托给不同的对象。               
每一个命令都是一个操作；                   
请求方发出请求要求执行一个操作；而接收方收到请求，并执行该操作。            
命令模式的优点，主要表现在以下几个方面：      
- 第一：命令模式使新的命令很容易地被加入到系统中来。      

- 第二：命令模式允许接收方决定是否要否决请求。     

- 第三：能够较容易地设计一个命令队列。        
- 第四：可以容易的实现对请求的撤销和恢复。    
- 第五：在需要的情况下，能够较容易地将命令写入日志。       

##### 观察者模式(Observer)            
观察者模式是对象的行为模式，又叫做发布-订阅模式(Publish/Subscribe)、模型-视图模式(Model/View)、源-监听器模式(Source/Listener)或从属者模式(Dependents)。观察者模式定义了一种一对多的依赖关系，它让多个观察者对象同时监听某一个主题对象。当这个主题对象在状态上发生变化的时候，会通知所有的观察者对象，使它们能够自动的更新自己。            
我们来看一个使用环境，在一个软件系统里面包含了各种各样的对象，它就像一片欣欣向荣的森林里面充满了各种生物一样；在这一片森林中，各种生物彼此依赖和约束，形成了一个生物链。一种生物的状态变化就会造成其他一些生物的相应的行动，每一种生物都处于和其他生物的互动之中。 
同样，一个软件系统常常要求在某一个对象的状态发生变化的时候，某些其他的对象也能够做出相应的改变。       
做到这一点的设计方案有很多，但是为了使系统能够易于复用，就应该选择低耦合度的设计方案。       
不仅要保证对象之间关系的低耦合，同时，还要要求它们之间能够维持行动的协调一致，保证高度的协作。          
观察者模式就是满足这一要求的各种设计方案中最重要的一种实现方案。              

### 设计模式怎么解决问题：
前面我们介绍了一些在我们的框架技术中涉及到的设计模式；         
设计模式是一种设计的思路；        
在今后学习框架的时候，我们将能够更加深入的来理解这些设计模式；        
现在就来思考一下，设计模式怎么解决问题呢？         
设计模式采用多种方法解决面向对象设计者经常碰到的问题；            
使用设计模式可以帮我们完成以下工作：             
确定并不明显的抽象和描述这些抽象的对象；             
决定一个对象应该是什么；                       
定义对象的操作；         
描述对象的实现；               
最大程度的实现复用。                     

#### 怎么使用设计模式：
使用设计模式能给设计人员带来很多方便和好处，而要得到这样的好处，就需要根据实际情况，来选择正确的设计模式。       
选择模式的方法有很多，特别是随着对设计模式研究的广泛开展，越来越多的模式被发现，人们也开始寻找自动获取模式的方法；当然这种技术目前还很不成熟。            
在目前的实际工作中，人们仍然采用传统的模式选择方法，主要是凭借对设计模式功能的理解以及自身的设计经验，这就要求设计人员对所有设计模式都有较深刻的理解和掌握。                
然而，对于模式选择方法的介绍，大多是基于单个方法的，并没有对方法之间的联系进行阐述，容易导致人们孤立地看待问题，并不便于使用。通过对已有的模式选择方法进行总结、归纳、简化，并把它们联系起来；发现在选择模式的时候基本上遵循以下的步骤和原则：         
选择设计模式的步骤：                    

- 第一：              
理解问题需求。           
需求是选择设计模式的基础；           
通过需求分析可以找到多个模式，形成模式组。          

- 第二： 
研究组内模式：          
需求分析得出的组内模式有一些共性；          
但是每种模式都有其特殊的意图、使用动机及其使用条件；            
因此，需要对组内模式再进行研究和选择。                 

- 第三：    
考虑设计模式如何解决设计问题：         
在此过程中，主要考虑设计模式在设计中所支持的可变化因素，即确定改变什么而不需要重新设计，根据这一点可以找到所需要的设计模式。此外考虑与其相关的设计模式。                      

根据以上三个步骤的筛选，就能选出符合需求的设计模式。       


#### 怎么使用设计模式
在Java编程中，尤其是今后我们学习了框架技术之后，      
在开发过程中，实际上我们全部都是在使用别人设计好的类库和框架。       

##### 什么是类库？           
比如我们之前介绍过的Java的各种API。           
比如网络API，图形用户界面API，输入输出AIP等等。           

##### 什么是框架呢？
比如我们现在已经介绍并且在以后学习的Struts、Hibernate、Spring等框架。            
我们将在工作中讨论类库与框架，将利用它们的API来编译成我们的程序，享受运用别人的代码所带来的好处。              
这些类库也罢，框架也好，长久以来，一直扮演着软件开发过程中的重要角色。我们从中挑选出所要的组件，把它放到合适的地方，最终开发出我们的程序。但是，只有类库与框架无法将我们的应用程序组成容易了解的、容易维护的、和具有弹性的架构，我们还需要设计模式。        
设计模式并不会直接进入我们的代码，而是先进入我们的大脑中；一旦在我们的脑海中装入了很多关于设计模式的知识，就能够开始在新的设计中逐渐的采用它，并最终熟练的掌握它。             


如果设计模式这么棒，为何没有人建立相关的Java类库呢？那样我们就不必自己动手了？             
设计模式比Java类库更高级。                   
设计模式告诉我们如何组织类和对象以解决实际问题。                    
而且采纳这些设计并使它们适合我们特定的应用，是我们责无旁贷的事。                 

类库和框架到底算不算设计模式？它们之间是什么关系？               
类库和框架提供了我们某些特定的实现，让我们的代码可以轻易地应用，但是这并不算是设计模式。           
有些时候，类库和框架本身会用到设计模式；这样很好；因为一旦你了解了设计模式，会更容易了解这些API是围绕着设计模式构造的。            

##### 没有所谓的设计模式的类库？
没错，以上讲解的都是一些常用的设计模式，我们可以在应用中利用这些设计模式。           

这些设计模式好像都只是面向对象设计的，我们已经学会了封装、抽象、继承、多态，真的还有必要考虑设计模式吗？              
运用面向对象，一切不都是很直接吗？这不正是学习面向对象的原因吗？我认为设计模式只对那些不懂好面向对象设计的人有用。               
这可是面向对象开发常有的谬误：         
以为知道面向对象的基础概念，就能自动设计出有弹性的，可复用的，可维护的系统。             
肯定不是这样的。               
要构造有这些特征的面向对象系统，事实上只有通过不断的艰苦实践，才能成功。这些构造面向对象系统的隐含经验于是被收集整理出来。被整理成了一群“设计模式”；                       
那么，如果知道了这些设计模式，就可以减少许多体力劳动，直接采用可行的模式吗？                 
对，在一定程度上可以这么说。                    
不过要记住，设计是一门艺术。               
总是有许多可取舍的地方。               
但是如果你能采用这些经过深思熟虑，并且经过时间考验的设计模式，你就领先了别人一步。                
