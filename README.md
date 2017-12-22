# JavaScript高级程序设计读书笔记

## 第一章 “JavaScript简介”

  `文档对象模型`（DOM，Document Object Model）是针对XML但是经过拓展用于HTML的应用程序编辑接口(API,Application Programming Interface)。DOM把整个页面映射为一个多层节点结构。HTML或XML页面中的每个组成部分都是某种类型的节点，这些节点又包含着不同类型的数据。
  
  JavaScript由三个不同部分组成
    
    ECMAscript，由ECMA-262定义，提供核心语言功能
    文档对象模型(DOM)。提供访问和操作网页内容的方法和接口
    浏览器对象模型(BOM)。提供与浏览器交互的方法和接口
   
 ***  
   
 ## 第二章 “在HTML中使用JavaScript”
  
  ###2.1 <script>元素
  
  HTML4.01为<script>定义了下列6个属性
  
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
   
   通过<script>元素的src属性还可以包含来自外部域的JavaScript文件。这一点让<script>元素倍显强大，又让他备受争议。在这一点上，
<script>与<img>元素非常相似，即它的src属性可以是指向当前HTML页面所在域之外的某个域中的完整URL   
   
