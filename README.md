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
  
  HTML4.01为**<script>**定义了下列6个属性
  
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
 
 把JavaScript插入到HTML页面中要使用<script>元素。使用这个元素可以吧JavaScript嵌入到HTML页面中，让脚本和标记混合到一起；也可以包含外部的JavaScript文件。而我们需要注意的地方有:
    
    * 在包含外部JavaScript文件时，必须将src属性设置为指向相应文件的URL
    
