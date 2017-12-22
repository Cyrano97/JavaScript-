## JavaScript高级程序设计读书笔记

### 第一章 “JavaScript简介”

  文档对象模型（DOM，Document Object Model）是针对XML但是经过拓展用于HTML的应用程序编辑接口(API,Application Programming Interface)。DOM把整个页面映射为一个多层节点结构。HTML或XML页面中的每个组成部分都是某种类型的节点，这些节点又包含着不同类型的数据。
  
  JavaScript由三个不同部分组成
    
    ECMAscript，由ECMA-262定义，提供核心语言功能
    文档对象模型(DOM)。提供访问和操作网页内容的方法和接口
    浏览器对象模型(BOM)。提供与浏览器交互的方法和接口
   
 ### 第二章 “在HTML中使用JavaScript”
  
  HTML4.01为<script>定义了下列6个属性
  
    asnyc 可选。表示应该立即下载脚本，但不应妨碍页面中的其他操作，比如下载其他资源或等待加载其他脚本。只对外部脚本文件有效
    
    charset 可选。表示通过src属性指定的代码的字符集。由于大多数脚本会忽略它的值，因此这个属性很少有人用
    
    defer 可选。表示脚本可以延迟到文档完全被解析和显示之后再执行。只对外部脚本文件有效。IE7及更高版本对嵌入脚本也支持这个属性
    
    language 已废弃。原来用于表示编写代码使用的脚本语言（如JavaScript、JavaScript1.2或VBScript）。大多数浏览器或忽略这个属性，因此也没有必要再用       
    
    src 可选，表示包含要执行代码的外部文件 
      
    type 可选。可以看成是language的替代属性:表示编写代码使用的脚本语言的内容类型（也称为MIME类型）。虽然text/JavaScript和text/ecmascript都已经不被推荐使用，但人们一直以来使用的还是text/JavaScript。实际上，服务器在传送JavaScript文件审核使用的MIME类型通常是application/x_JavaScript，但在type中设置这个值却有可能导致脚本被忽略。另外，在非IE浏览器中还可以使用以下值:application/JavaScript和application /ecmascript。考虑到约定俗成和最大限度的浏览器兼容性，目前type属性的值依旧还是text/JavaScript。不过，这个属性并不是必需的，如果没有指定这个属性，其默认值认为text/JavaScript。





