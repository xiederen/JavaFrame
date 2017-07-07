## 小经验：
1.通过查看 `Struts2\struts-2.3.4.1\docs\WW\tag-reference.html` 来查看帮助文档         
各标签帮助文档路径为： `struts-2.3.4.1\docs\WW\XXX.html`                      
2.查看 `struts-2.3.4.1\lib\struts2-core-2.3.4.1\META-INF\struts-tags.tld`,可以确定各标签的具体配置        


# Struts2标签简介
Struts2标签的优势：      
标签库简化了用户对标签的使用          
结合OGNL使用，对于集合、对象的访问功能非常强大           
提供可扩展的主题、模板支持，极大简化了视图页面的编写      
不依赖任何表现层的技术        
Struts2标签的分类：         
```
通用标签 (Generic Tags)
  数据标签 (Data Tags)
  控制标签 (Control Tags)
UI标签 (UI Tags)
Ajax标签 (Ajax Tags)
```

### 数据标签
- <s:property> : 输出指定值   
	- value属性 : 用来获取值的OGNL表达式     
	- default属性 : value属性返回空值时，如果仍希望输出某些内容，可使用default来指定这些内容；  
	- escapeHtml : 是否转义HTML，默认取值true；           

- <s:debug/> : 查看值栈以及Stack Context中所有能访问的值；    

- <s:date> : 格式化输出一个日期数据         
	- name : 被格式化的值，必须设置，它本身是一个OGNL表达式       
	- format : 用于指定日期显示的格式。比如： `yyyy-MM-dd HH:mm:ss`       
 
- <s:set> : 对设置的表达式求值，并将结果赋给特定作用域中的某个变量 类似于定义一个变量，并赋值
	- var : 变量名
	- value : 设置给变量的值。可以是常量，也可以是OGNL表达式
	- scope : 变量的生存周期
> 应用：比如一个对象在OGNL上的访问层次较深，可以使用该标签将其定义为一个变量，这样保证在多次引用它时更方便


示例：       
```
<s:property value="houses[0].title"/>
<s:property value="houses[0].name" default="默认的房屋信息"/>
//将OGNL表达式转换为字符串，要添加单引号
<s:property value=" '<hr/>' " />       输出 <hr/>
<s:property value=" '<hr/>' " escapeHtml="false" />    输出一条水平线

<s:data name="house[0].addDate" format="yyyy-MM-dd HH:mm:ss"/>

<s:set var="s" value="house[0].street"/>
<s:property value="#s.streetName"/>
<s:property value="#s.streetId"/>
<s:property value="#s.district.districtName"/>

<s:set var="s2" value="house[0].street" scope="session"/>
<s:property value="#session.s2.streetName"/>
<s:property value="#session.s2.streetId"/>
<s:property value="#session.s2.district.districtName"/>
```


- <s:url> : 用来生成一个url    
	- var属性 : 该url的引用名称。如果指定这个属性，则<s:url>标签不会在页面上生成字符串       
	- action属性 : 指定要访问的Action的名字
	- value属性 : 指定要访问的目标；如果action不提供，就使用value属性作为URL的地址值。         

- <s:url> 一般都是结合 <s:a>标签一起使用的。

- <s:a> : 用来生成HTML的<a>标签，可以通过 <s:url> 来设置它的url；   
	- href属性： 指定超链接的url；      

- <s:a>与<s:url>         
<s:a>与<s:url>虽然经常结合使用，但是他们之间还是不同的；      
<s:a>  :   用来直接生成一个超链接   
<s:url> :  用来生成一个字符串            


经常和 <s:url> 一起使用的还有一个数据标签 <s:param>      

- <s:param> : 用来为其它标签添加参数化设置的功能            
	- name属性 : 指定参数的名称  
	- value属性 : 指定参数的值       
一般不单独使用，而是作为其他标签的子标签，主要用来配合其他标签指定参数；         
             
比如，可以把<s:param>标签和<s:url>标签一起使用，通过它来设置url要传递的参数；       


- <s:include> : 把其他页面包含到当前的页面上，类似于 <jsp:include>
	- value属性 : 用来指定其他可以被引用的URL的名字，必须要设置


示例：     
```

<s:url value="house"/>     在页面上显示字符串 house
<s:url value="house" var="t"/>         不在页面上显示字符串house

<s:url value="house" var="t"/>
<s:a href="#t"/>超链接1</s:a>      这时不能生成指向house的Action的超链接，因为 #t是字符串
查看生成页面的源代码为：
<a href="#t">超链接1</a>

//将字符串转换为 OGNL表达式，使用 %{}
<s:url value="house" var="t"/>
<s:a href="%{#t}"/>超链接1</s:a>
这时就能生成指向house的Action的超链接，查看生成页面的源代码为：
<a href="house">超链接1</a>

//<s:param>的使用
//<s:param>的value属性为OGNL表达式
//若要传递字符串，使用单引号包含
<s:url value="house" vat="t">
  <s:param name="str" value="accp"></s:param>
</s:url>
<s:a href="%{#t}">超链接1</s:a>
生成页面的源文件：
<s:a href="house">超链接1</a>


<s:url value="house" vat="t">
  <s:param name="str" value=" 'accp' "></s:param>
</s:url>
<s:a href="%{#t}">超链接1</s:a>
生成页面的源文件：
<s:a href="house" str="accp">超链接1</a>
```

#### 小结：
                 
一些标签的属性为OGNL表达式，当我们需要把它转换为字符串时，可以通过 单引号 的形式；       
OGNL 变为 字符串 ：       
单引号；      
如：       
```
<s:property value=" '<hr/>' " escapeHtml="false"/>
```

如果我们拿到的为字符串，希望标签属性将它作为OGNL表达式处理，则可以通过 \%\{\} 的形式：       
字符串 变为 OGNL
```
%{}
```
如：
```
<s:a href="%{#houseUrl}">超链接</s:a>
```

问题：        
OGNL部分，之前用过 `#`号，现在又有了 \%\{\}，该什么时候使用这些符号呢？         

OGNL中 \{\}  \# 以及 \$ 的使用场景：

\%\{\} : 设置动态的值，告诉标签的处理类，该字符串需要按照OGNL表达式来处理；      


问题：         
有些标签属性本身就是OGNL表达式，不加 `%{}` 也可以，比如： 
```
<s:property value="house.title" />
```
这个标签属性不加 `%{}`，标签处理类已经将其作为OGNL表达式动态处理了；     
这时 取得是 值栈中的值；              

问题：          
如何知道哪些标签属性需要加 `%{}` 哪些不需要加？  
方法一：      
当不确定是否需要加 `%{} `时，可以给每个属性都加 `%{}`；
因为加上以后，对于标签处理类来说，普通字符串会作为OGNL表达式处理，本身已经作为OGNL表达式处理的属性不受影响；直接忽略了 `%{}`         

方法二：      
根据实际的运行效果来调整标签属性写法     
根据实际运行效果来看，发现错误时再及时调整标签属性的写法就可以了；         

OGNL加 `#`号的情况： 
取 `Stack Context` 中的值 的时候；比如： 
```
<s:property value="#request.name"/>
```

`$`号的使用：       
用于在XML文件中获取动态值；      
如： `struts.xml `中设置动态结果；    
```
<result name="success" type="redirectAction">${nextDispose}</result>
```


控制标签
```

<s:if> <s:elseif> <s:else> 表达分支判断
```

test属性 ： 用来表示是否符合条件，本身是一个OGNL表达式，运算结果为一个boolean值；


- <s:iterator> : 用来处理循环，可以用它遍历数组、Set和List等集合对象      
	- value属性 : 用来指明到底循环的是谁，这个属性的值是一个OGNL表达式；
	- var属性 : 指定变量名称，用来引用被循环的对象
	- Status属性 : 返回当前循环的各种信息；
比如：
```
count : 集合含有多少个对象
index : 正在循环的这一项的索引
even :  当前遍历到的对象是不是处于列表的偶数索引位置
odd  :  当前遍历到的对象是不是处于列表的奇数索引位置
```


学习方法：            
各标签的属性不用死记硬背，必须以帮助文档为指导，用到哪个标签就去查找哪个标签的属性和范例；