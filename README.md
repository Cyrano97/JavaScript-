# JavaScript高级程序设计读书笔记

## 第一章 “JavaScript简介”

  **文档对象模型**（DOM，Document Object Model）是针对XML但是经过拓展用于HTML的应用程序编辑接口(API,Application Programming Interface)。DOM把整个页面映射为一个多层节点结构。HTML或XML页面中的每个组成部分都是某种类型的节点，这些节点又包含着不同类型的数据。
  
  JavaScript由三个不同部分组成
    
    ECMAscript，由ECMA-262定义，提供核心语言功能
    文档对象模型(DOM)。提供访问和操作网页内容的方法和接口
    浏览器对象模型(BOM)。提供与浏览器交互的方法和接口
   
 ---
   
 ## 第二章 “在HTML中使用JavaScript”
  
  ### 2.1 <script>元素
  
  HTML4.01为 <**script**> 定义了下列6个属性
  
    asnyc 可选。表示应该立即下载脚本，但不应妨碍页面中的其他操作，比如下载其他资源或等待加载其他脚本。只对外部脚本文件有效
    
    charset 可选。表示通过src属性指定的代码的字符集。由于大多数脚本会忽略它的值，因此这个属性很少有人用
    
    defer 可选。表示脚本可以延迟到文档完全被解析和显示之后再执行。只对外部脚本文件有效。IE7及更高版本对嵌入脚本也支持这个属性
    
    language 已废弃。原来用于表示编写代码使用的脚本语言（如JavaScript、JavaScript1.2或VBScript）。大多数浏览器或忽略这个属性，因此也没有必要再用       
    
    src 可选，表示包含要执行代码的外部文件 
      
    type 可选。可以看成是language的替代属性:表示编写代码使用的脚本语言的内容类型（也称为MIME类型）。虽然text/JavaScript和text/ecmascript都已经不被推荐使用，但人们一直以来使用的还是text/JavaScript。实际上，服务器在传送JavaScript文件审核使用的MIME类型通常是application/x_JavaScript，但在type中设置这个值却有可能导致脚本被忽略。另外，在非IE浏览器中还可以使用以下值:application/JavaScript和application /ecmascript。考虑到约定俗成和最大限度的浏览器兼容性，目前type属性的值依旧还是text/JavaScript。不过，这个属性并不是必需的，如果没有指定这个属性，其默认值认为text/JavaScript。


  使用<script>元素的方式有两种：直接在页面中嵌入JavaScript代码和包含外部JavaScript文件
  
  在使用<script>元素嵌入JavaScript代码时，只须为<script>指定type属性。像下面这样把JavaScript代码直接放在元素内部即可
    
    <script type="text/javascript">
      function sayHi(){
          alert("Hi!");
      }
  
  包含在<script>元素内部的JavaScript的代码将被从上至下依次解释。就拿前面这个例子来说，解释器会解释一个函数的定义，然后将该定义保存在自己的环境当中。在解释器对<script>元素内部的所有代码求值完毕前，页面中的其余内容都不会被浏览器加载或显示。
  
  在使用<script>嵌入JavaScript代码时，记住不要在代码中任何地方出现"</script>"字符串。
  因为按照解析嵌入式代码规则，当浏览器遇到字符串"</script>"时，就会认为那是结束的<script>标签，而通过转义字符"\"解决这个问题。
    
    <script type="text/javascript">
      function(){
        alert("<\script>");
      }
     
   如果要通过<script>元素来包含外部JavaScript文件，那么src元素就是必须的。这个属性的值是一个指向外部JavaScript的链接，例
      
      <script type="text/javascript" src="example.js"/>
      
   但在不能在HTML文档中使用这种语法。这个语法不符合HTML规范，也得不到某些浏览器(尤其是IE)的正确解析
   
   通过<script>元素的src属性还可以包含来自外部域的JavaScript文件。这一点让<script>元素倍显强大，又让他备受争议。在这一点上，<script>与<img>元素非常相似，即它的src属性可以是指向当前HTML页面所在域之外的某个域中的完整URL   
  
   #### 2.1.1 <script>元素
  
  按传统的做法，所有<script>元素都应该放在页面的<head>元素中，例如
  
    <!Document html>
    <html>
      <head>
        <title>Example HTML Page</title>
        <script type="text/javascript" src="example1.js"></script>
        <script type="text/javascript" src="example2.js"></script>
      </head>
      <body>
        <!-- 这里放内容 -->
      </body>
    </html>
  
  这种做法的目的就是把所有外部文件(包括CSS文件和JavaScript文件)的引用都放在相同的地方，在文档的<head>元素中包含所有JavaScript文件，意味着必须等待全部JavaScript代码都被下载、解析和执行完成以后，才能开始呈现页面的内容(浏览器在遇到<body>标签时才开始呈现内容)。对应那些需要很多JavaScript代码的页面来说，这无疑会导致浏览器在呈现页面时出现明显的延迟，而延迟期间的浏览器窗口将是一片空白。为了避免这个问题，现代web应用程序一般都把全部JavaScript引用放在<body>元素中页面内容的后面
  
    <!Document html>
    <html>
      <head>
        <title>Example HTML Page</title>
      </head>
      <body>
        <!-- 这里放内容 -->
        <script type="text/javascript" scr="example1.js"></script>
        <script type="text/javascript" scr="example2.js"></script>
      </body>
    </html>
    
 这样，在解析包含的JavaScript代码之前，页面的内容将完全呈现在浏览器中，而用户也会因为浏览器窗口显示空白页面时间缩短而感到打开页面的速度加快了
 
 #### 2.1.2延迟脚本
 
 HTML4.01 为<script>标签定义了defer属性，这个属性的用途是表明脚本在执行时不会影响页面的构造。也就是说，脚本会被延迟到整个页面都解析完毕后再运行。
 因此，在<script>元素中设置defer属性，相当于告诉浏览器立即下载，但延迟执行
 
    <!Document html>
    <html>
      <head>
        <title>example</title>
        <script type="text/javascript" defer="defer" src="example1.js"></script>
        <script tyep="text/javascript" defer="defer" scr="example2.js"></script>
      </head>
      <body>
        <!-- 在这里放内容 -->
      </body>
    </html>
    
 #### 2.1.3异步脚本
 
 HTML5为<script>元素定义了async属性。这个属性与defer属性相似，都用于改变处理脚本的行为。async脚本只适用于外部脚本文件，并告诉浏览器立即下载文件。但是与defer不同的是，标记为async的脚本并不保证按照它们的先后顺序执行。例如
   
     <!Document html>
     <html>
      <head>
        <title>example</title>
        <script type="text/script" async src="example1.js"></script>
        <script type="text/script" async scr="example2.js"></script>
      </head>
        <!-- 在这里放内容 --->
      <body>
      </body>
     </html>
  
  以上代码中，第二个脚本文件可能会在第一个脚本文件之前执行。因此，确保两着之间互不依赖很重要。指定async属性的目的是不让页面等待两个脚本下载和执行，从而异步加载页面其他内容。我从，建议异步脚本不要在加载期间修改DOM
  
  异步脚本一定会在页面的load事件前执行，但可能会在DOMContentLoaded事件触发之前或之后执行。支持异步脚本的浏览器有FireFox3.6,Safari 5和Chrome
  
 ### 2.2 嵌入代码和外部文件
 
 支持使用的外部文件的人多会强调如下优点
 
 >可维护性
    
  >>可缓存
    
   >>>适应未来
   
 ### 2.4 <noscript>元素
  
  早期浏览器都面临一个特殊的问题，即当前浏览器不支持JavaScript时如何让页面平稳地退化。对这个问题的最终解决方案就是创建一个<noscript>元素。用以在不支持JavaScript的浏览器中显示替代内容。这个元素可以包含能够出现在文档<body>中的任何HTML元素——<script>元素除外。包含在<noscript>元素中的内容只有在下列情况下才会显示出来：
    
    浏览器不支持脚本
    浏览器支持脚本，但脚本被禁用
    
 ### 2.5 小结
 
 把JavaScript插入到HTML页面中要使用<script>元素。使用这个元素可以把JavaScript嵌入到HTML页面中，让脚本和标记混合到一起；也可以包含外部的JavaScript文件。而我们需要注意的地方有:
    
 * 在包含外部JavaScript文件时，必须将src属性设置为指向相应文件的URL
 * 所有的<scrtipt>元素将会按照在页面中出现的先后顺序依次被解析
 * 由于浏览器会先解析完不使用defer属性的<script>元素中的代码，然后再解析后面的内容，所以一般吧<script>元素放在页面最后面，即主要内容后面，</body>标签前面
 * 使用defer属性可以让脚本在文档完全呈现之后再执行。延迟脚本总是按照指定它们的顺序执行
 * 使用async属性可以表示当前脚本不必等待其他脚本，也不必阻塞文档实现
      
       另外。使用<script>元素可以指定在不支持脚本的浏览器中显示的替代内容。但在启动了脚本的情况下，浏览器不会显示<noscript>元素中的内容
 
 ---
 
 # 第三章 “基本概念”
 
 	ECAMScript中的所有参数传递的都是值，不可能通过引用传递参数
	
 ## 3.1 语法
 
 ### 3.1.1 区分大小写
 
  ECMAScript中的一切（变量、函数名、操作符）都区分大小写。
 
 ### 3.1.2 标识符
 
  所谓标识符，就是指变量、函数、属性的名字，或者函数的参数，标识符可以是按照下列格式规则组合起来的一个或者多个字符：
    
    * 第一个字符必须是一个字母、下划线（_）或者一个美元符号
    * 其他字符可以是字母、下划线、美元符号或数字
    
  按照惯例，ECMAScript中标识符采用驼峰大小写格式，也就是第一个字母小写，剩下的每个单词的字母都大写
 
 ### 3.1.3 注释
 
  ECMAScript使用C风格的注释，包括单行注释和块级注释
 
    //单行注释
    
    /*
    * 这是一个多行
    * （块级）注释
    */

### 3.1.4 严格模式

 ECMAScript5引入了严格模式(strict mode)的概念。严格模式是为JavaScript定义了一种不同解析和执行模型。
在严格模式下，ECMAScript的一些不确定的行为将得到处理，而对某些不安全的操作也会抛出错误，要在整个脚本中启用严格模式，可以在顶部添加如下代码

    "use strict"
 这是一个编译指令，用于告诉支持的JavaScript引擎切换到严格模式。

 在函数内部的上方包含这条编译指令，也可以指定函数在严格模式下执行：
    
    function doSomething(){
      "use strict";
      // 函数体
    }
    
### 3.1.5 语句

 EMCAScript中的语句以一个分号结尾

    var sum=a+b    // 即使，没有分号也是有效的语句——不推荐
    var diff=a-b;  // 有效的语句——推荐
    
 可以使用C风格的语法把多条语句组合到一个代码块中。虽然条件控制语句(如if语句)只在执行多条语句的情况下才要求使用代码块，但最佳实践是始终在控制语句中使用代码块——即使代码块中只有一句语句，例如

    if(test)
     alert(test); // 有效但容易出错，不要使用

    if(test){
     alert(test); // 推荐使用
    }
    
## 3.2 关键字和保留字

见书p21~22

## 3.3 变量

  ECMAScript的变量是松散类型的（弱类型语言），所谓松散类型即使可以用来保存任何类型的数据。换句话说，每个变量仅仅是一个用于保存值的占位符而已。定义变量时要使用var操作符（注意var是一个关键字），后跟变量名（即一个标识符）。
 
     var message;
     
  这行代码定义了一个名为message的变量，该变量可以用来保存任何值(像注意未经过初始化的变量，会保留一个特殊的值——undefined)
  
  有一点必须注意，即用var操作符定义的变量将成为定义该变量的作用域中的局部变量，也就是如果在函数中使用var定义一个变量，那么这个变量在函数退出后就会被销毁。
    
    funciton test(){
     var message="hi"; // 局部变量
    }
    test();
    alert(message); // 错误!
    
  这里，变量message是在函数中使用var定义的，当函数被调用时，就会创建该变量并为其赋值。而在此之后，这个变量又会被立即销毁，因此例子中的下一行代码就会导致错误。不过，可以像下面这样忽略var操作符，从而创建一个全局变量
  
    function test(){
     message="hi"; // 全局变量
    }
    test();
    alert(message); // "hi"
    
  这个例子忽略了var操作符，然而message就成了全局变量。这样，只要调过一次test()函数，这个变量就有了定义，就可以在函数外部的任何地方被访问到。
  
    虽然忽略var操作符可以定义全局变量，但这也不是我们推荐的做法。因为在局部作用域中定义的全局变量很难维护
    
  可以用一条语句定义了多个变量，只要像下面这样把每个变量（初始化或不初始化均可）用逗号分隔开即可：
    
    var message="hi",
        found=false,
        age=29;
        
 ## 3.4 数据类型
   
   ECMAScript中有五种简单数据类型（也称为基本数据类型）：undefined、Null、Boolean、Number和String，还有一种复杂数据类型——Object,Object本质是一组无序的名值对组成的。ECMAScript下不支持任何创建自定义类型的机制，而所有值最终都是上述6种数据类型之一。乍一看，好像只有6种数据类型不足以表示所有数据：但是，由于ECMAScript数据类型具有动态性，因此的确没有再定义其他数据类型的必要了。
   
 ### 3.4.1 typeof 操作符
 
   鉴于ECMAScript是松散类型的，因此需要有一种手段来检测给定变量的数据类型——typeof就是负责提供这方面信息的操作符。对一个值使用typeof操作符可能返回下列某个字符串：
   
    * “undefinde”——如果这个值未定义;
    * “Boolean”——如果这个值是布尔值；
    * “string”——如果这个值是字符串;
    * “number”——如果这个值是数值;
    * “object”——如果这个值是对象或null;
    * “function”——如果这个值是函数;
   
 typeof操作符的操作数可以是变量,也可以是数值字面量。
    
    typeof是一个操作符而不是函数
  
 从技术角度将
 
    函数在ECMAScript中是对象，不是一种数据类型。然而，函数也确实有些特殊的属性，因此通过typeof操作符来区分函数和其他对象是有必要的
    
  ### 3.4.2 undefined 类型
    
   undefined类型只有一个值，即特殊的undefined。在使用var声明变量但未对其加以初始化时，这个变量的值就是undefined。例如
   
     var message;
     alert(message==undefinde); // true
     
  ### 3.4.3 Null类型
    
   null类似是第二个只有一个值的数据类型，这个特殊的值是null。从逻辑角度来看，null值表示应该空对象指针，而这个也是正是用typeof操作符检测null值返回object的原因。如下面例子：
     
     var car=null;
     alert(typeof car);  // "object"
   
   如果定义的变量准备在将来用于保存对象，那么最好将该变量初始化为null，而不是其他值，这样一来，只要直接检查null值就可以指定相应的变量是否已经保存一个对象的引用，如下列所示：
   
    if（car!=null）{
      // 对car对象执行某些操作
    }
   
   实际上，undefined值也是派生自null值，因此ECMA-262规定对它们的相等性测试要返回true
   
    alert(null==undefined); // true
    
   这里，位于null和undefined之间的相等操作符（==）总是返回true，不过要注意的是，这个操作符出于比较的目的会转换其操作数。
   
   尽管null和undefined有这样的关系，但它们的用途完全不同。
   
    只要意在保存对象的变量还没真正保存对象，就应该明确地让该变量保存null值。这样做不仅可以体现null作为空对象指针的惯例，而且也有助于进一步区分null和undefined。
    
    
  ### 3.4.4 Boolean类型
  
  Boolean类型是ECMAScript中使用最多的一种类型，该类型只有两个字面值：true和false。这两个值与数字值不是同一回事，因此true不一定等于1，而false也不一定等于0。
  
    var found=true;
    var lost=false;
    
  虽然Boolean类型的字面值只有两个，但ECMAScript中所有类型的值都有这两个Boolean等价的值。要将一个值转换为其对应的Boolean值，可以调用转型函数Boolean(）,例子
  
    var message="Hello World!":
    var messageAsBoolean=Boolean(message):
  在这个例子中，字符串message被转换成一个Boolean值，该值被保存自messageAsBoolean变量中。可以对任何数据类型的值调用Boolean（）函数，而且总会返回一个Boolean类型的值。至于这个值是true还是false，取决于要转换值的数据类型及其实际值。
  
  *表见书p26~27*
  
  ### 3.4.5 Number类型
   
  最基本的数值字面格式是十进制整数，十进制整数可以像下面这样直接在代码中输入：
   
    var intNum=55; // 整数
    
  除了十进制表示外，整数还可以通过八进制（以8为基数）或十六进制（以16为基数）的字面值来表示。其中，八进制的字面值第一位必须是零（0），然后是八进制数字序列（0~7）。如果字面值中的数值超出了范围，那么前面的零将被忽略，后面的数值将被当做十进制数值解析。请看下面的例子
  
    var octalNum1=070; // 八进制的56
    var octalNum2=079; // 无效的八进制数值——解析为79
    var octalNum3=08;  // 无效的八进制数值——解析为8
  
  八进制字面量在严格模式下是无效的。会导致支持该模式的JavaScript引擎抛出错误
  
  十六进制字面值的前两位必须是0X，后跟任何十六进制数字（0~9及A~F）。其中，A~F可以大写，也可以小写。
  
    var hexNum1=0xA;  // 十六进制的10
    var henNum2=0x1f; // 十六进制的31
    
  在进行算术计算时，所有以八进制和十六进制表示的数值最终都将被转换成十进制数值。
  
  1、浮点数值 *见书p28*
  
  2、数值范围 *见书p29*
  
  3、NaN     *见书p30*

  4、数值转换 *见书p30~32*
  
  ### 3.4.6 String类型
  
  用双引号""和单引号''表示的字符串完全相同
  
  1、字符串字面量
  
  String数据类型包含一些特殊的字符字面量，也叫转义序列，用于表示非打印字符，可见表
  
  *表见书p33*
  
  任何字符串的长度都可以通过访问其length属性取得，例如
  
     var text="This is the lette sigma:\u03a3.";
     alert(text.length);   //输出28
     
  这个属性返回的字符数包括16位字符的数目。如果字符串中包含双字节字符，那么length属性可能不会精准地返回字符串中的字符数目。
  
  2、字符串的特点
  
  EMACScript中字符串是不可变的，也就是说，字符串一旦创建，它们的值就不能改变。要改变某个变量保存的字符串，首先要销毁原来的字符串，然后再用另一个包含新值的字符串填充该变量。例如：
  
    var lang="Java";
    lang=lang+"Script";
  
  以上示例中的变量lang开始时包含字符串“Java”。而第二行代码把lang的值重新定义为"Java"与"Script"的组合，即"JavaScript"。
  实现这个操作的过程如下：
     
     1、首先创建一个能容纳10个字符的字符串
     2、然后在这个字符串中填充"Java"和"Script"
     3、最后一步是销毁原来的字符串"Java"和字符串"Script"，因为这两个字符串语句没用了。
     
  这个过程是在后台发生的，这也就是在某些旧版本的浏览器(例如版本低于1.0的Firefox、IE6等)中拼接字符串时速度很慢的原因所在。
  
  3、转化为字符串
 
  要把一个值转化为字符串有两种方法。第一种是使用几乎每个值都会有的toString()方法，这个方法的唯一要做的就是返回相应值的字符串表现。例子：
  
   	var age=11;
				ageAsString=age.toString(); // 字符串"11"
				var found=true;
				foundAsString=found.toString(); // 字符串"true"
    
  数值、布尔值、对象和字符串值(没错，每个字符串也都有一个toString方法，该方法返回字符串的一个副本)都有toString方法。但null和undefined值没有这个方法。
  
  多数情况下，调用toString()方法不必传递参数。但是，在调用数值的toString（）方法时，可以传递一个参数：输出数值的基数。默认情况下，toString()方法十进制格式返回数值的字符串表示。而通过传递基数，toString（）可以输出二进制、八进制、十六进制，乃至其他任意有效进制格式表示的字符串值。例子:
  
     var num=10;
     alert(num.toString());   // "10"
     alert(num.toString(2));  // 二进制表示 "1010"
     alert(num.toString(8));  // 八进制表示 "12"
     alert(num.toString(10)); // 十进制表示 "10"
     alert(num.toString(16)); // 十六进制表示 "a"
     
  通过这个例子可以看出，通过指定基数，toString（）方法会改变输出的值。而数值10根据基数不同，可以在输出时候被转换为不同的数值格式。注意，默认的(没有参数的）输出值与指定基数10的输出值相同。
  
  在不知道要转换的值是不是null或undefined的情况下，还可以使用转型函数String（），这个函数能够将任何类型的值转换为字符串。String（）函数遵循下列转换规则
  
    * 如果值有toString()方法，则调用该方法(没有参数)并返回相应的结果
    * 如果值是null，则返回"null"
    * 如果值是undefined，则返回"undefined"
  
  下面是结果例子：
  
    var value1=10；
    var value2=true;
    var value3=null;
    var value4;
    
    alert("value1="+String(value1));  // "10"
    alert("value2="+String(value2));  // "true"
    alert("value3="+String(value3));  // "null"
    alert("value4="+String(value4));  // "undefined"
    
    这里先后转换了4个值：数值、布尔值、null和undefined。数值和布尔值的转换结果与调用toString（）方法得到的结果相同。因为null和undefined没有toString（）方法，使用String（）返回了两个值的字面量。
    
  如果要把某个值转换为字符串，可以使用加号操作符把它和一个字符串（""）加到一起
     
  ### 3.4.7 Object类型
  
  ECMAScript中的对象其实就是一组数据和功能的集合。对象可以通过执行new操作符后跟要创建的对象类型的名称来创建。而创建Object的实例并为其添加属性和（或）方法，就可以创建自定义对象。如下：
  
    var o=new Object();
  
  这个语法与Java中创建对象的语法相似；但在ECMAScript中，如果不给构造函数传递参数，则可以忽略后面的那一对圆括号。也就是说，在像前面这个示例一样不传递参数的情况下，完全可以忽略那对圆括号（但这不是推荐的做法）:
  
    var 0=new Object; //有效，但不推荐省略圆括号
    
  在ECMAScript中，（就像Java中的java.lang.Object一样）Object类型是所有它的实例的基础。换句话说，Object类型所具有的任何属性和方法也同样存在于更具体的对象中。  
  	
    * constructor: 保存着用于创建当前对象的目录。对前面的例子而言，构造函数(constructor)就是object(）。
    * hasOwnProperty(propertyName): 用于检查给的属性在当前对象实例中（而不是在实例原型中）是否存在。其中，作为参数的属性名（propertyName）必须以字符串形式指定(例如：o.hasOwnProperty("name"))。
    * isPrototepyOf(propertyName):用于检查传入的对象是否是当前对象的原型
    * propertyIsEnumerable(propertyName):用于检查给定的属性能否能够作为for-in语句来枚举。与hasOwnerProperty方法一样，作为参数的属性名必须以字符串形式指定
    * toLoactionString(): 返回对象的字符串表示，该字符串与执行环境的地区对应
    * toString():返回对象的字符串表示
    * valueOf():返回对象的字符串、数值或者布尔值表示，通常与toString（）方法的返回值相同
    
  
  ## 3.5 操作符
  
  *见书p36~p54*
  
  ## 3.6 语句
  
  *见书p54~p62*
  
  ## 3.7 函数
  严格模式对函数有一些限制：
  	
	* 不能把函数命名为eva1或arguments；
	* 不能把参数命名为eva1或arguments； 
	* 不能出现两个命名参数同名的情况
  
  ### 3.7.1 理解参数
  
  ECMAScript函数不介意传递来多少个参数，也不在乎传来的参数是什么数据类型。
  原因是ECMAScript中的参数在内部是用一个数组来表示的。函数接收到的始终是这个数组，也不关心数组中包含哪些参数（如果有参数的话）。
  实际上，在函数体内可以通过arguments对象来访问这个参数数组，从而获取传递给函数的没一个参数
  
  其实，arguments对象只是和数组相似（它并不是Array的实例），因为可以使用方括号来访问它的没一个元素（即第一个元素是arguments[0]）,第二个元素是arguments[1]，以此类推
  
  ECMAScript的一个重要特点：
  	
	命名的参数只提供便利，但不是必需的
  
  另外，在命名参数方面，其他语言可能需事先创建一个函数签名，而将来的调用必须与该签名一致。但在ECMAScript中，没有这些条条框框，解析器将不会验证命名函数
  
  由于num1的值与arguments[0]的值相同，因此它们可以互换使用（num2与arguments[1]也是如此）
 	
	关于arguments的值还有一点比较意思，那就是它的值永远与对应命名参数的值保持同步
  
  ### 3.7.2 没有重载
  
  ECMAScript函数不能像传统意义上那样实现重载。而在其他语言（如Java）中，可以为一个函数编写两个定义，只要这两个定义的签名（接受的参数的类型和数量）不同即可。如前所述，ECMAScript函数没有签名，因为其参数是由包含零或多个值的数组来表示的。而没有函数签名，真正的重载是不可能做到的。
  	
	如果在ECMAScript中定义了两个名字相同的函数，则名字只属于后定义的函数。
	
  ## 3.8 小结
  
  *见书p67*
  ---
  # 第四章 “变量、作用域和内存问题”
  
  ECMAScript变量可能包含两种不同数据类型的值：*基本类型*和*引用类型*。
 
 	基本类型值指的是简单的数据段，而引用类型值指那些可能由多个值构成的对象。
 
  ## 4.1 基本类型和引用类型的值
  
  将一个值赋给变量时，解析器必须确定这个值是基本类型值还是引用类型值。五种基本数据类型是按值访问的，因为可以操作保存在变量中的实际的值。
  
  引用类型的值是保存在内存中的对象。于其他语言不同，JavaScript不允许直接访问内存的位置，也就是说不能直接操作对象的内存空间。在操作对象时，实际上是在操作对象的引用而不是实际的对象。为此，引用类型的值是按引用访问的。*：此说法存在争议，见PDF86页*
  
  ### 4.1.1 动态属性
  
  定义基本类型值和引用类型值的方式是类似的，创建一个变量并为该变量赋值。
  
  ### 4.1.2 复制变量值
  *复制基本类型值VS复制复杂类型值* 区别见书p69~70
  
  传递基本类型的时候，副本是一个值
  传递复杂类型的时候，副本是一个指针
  
  ！JavaScript-/img/JavaScriptNote1.png 
  
  ！JavaScript-/img/JavaScriptNote2.png 
  		
  ### 4.1.3 传递参数
  ### 4.1.4 检测类型
  
  ## 4.2 执行环境及作用域
  
  ### 4.2.1 延长作用域链
  ### 4.2.2 没有块级作用域
  
  ## 4.3 垃圾收集
  
  ### 4.3.1 标记清除
  ### 4.3.2 引用计数
  ### 4.3.3 性能问题
  ### 4.3.4 管理内存
  
  ## 4.4 小结
 
