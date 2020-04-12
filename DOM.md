# DOM

## Node类型
然所有节点类型都继承自 Node

## Node属性

* **nodeType**

* **parentNode**

* **previousSibling**

* **nextSibling**

* **fitstChild, lastChild**  
  分别指向父节点的第一个和最后一个节点。
  
* **ownerDocument**  
  该属性指向表示整个文档的文档节点。这种关
  系表示的是任何节点都属于它所在的文档，任何节点都不能同时存在于两个或更多个文档中。通过这个
  属性，我们可以不必在节点层次中通过层层回溯到达顶端，而是可以直接访问文档节点。

## 文档信息

* **title**
`dcument.title`  获取/设置文档标题

* **URL**
`document.URL` 获取页面完整的url

* **domain**
`document.domain` 包含页面的域名  
如果 URL 中包含一个子域名，例如 p2p.wrox.com，那么可以将 domain 设置为"wrox.com"。  
浏览器对 domain 属性还有一个限制，即如果域名一开始是“松散的”（loose），那么不能将它再设置为“紧绷的”（tight）。换句话说，在将 document.domain 设置为"wrox.com"之后，就不能再将其设置回"p2p.wrox.com"，否则将会导致错误

* **referrer**
`document.referrer` 保存着链接到当前页面的那个页面的 URL, 没有来源页面, 则包含空字符串  

所有这些信息都存在于请求的 HTTP 头部，只不过是通过这些属性让我们能够在
JavaScrip 中访问它们

## 特殊集合

* `document.anchors` 包含文档中所有带 name 特性的`<a>`元素。  
* `document.forms` ，包含文档中所有的`<form>`元素，与 document.getElementsByTagName("form")得到的结果相同。  
* `document.images` 包含文档中所有的`<img>`元素，与 document.getElementsByTagName ("img")得到的结果相同。  
* `document.links` 包含文档中所有带 href 特性的`<a>`元素。

## DOM 一致性检验

* `document.implementation`  
DOM1 级只为 document.implementation 规定了一个方法，即 hasFeature()。这个方
法接受两个参数：要检测的 DOM 功能的名称及版本号。如果浏览器支持给定名称和版本的功能，则该方法返回 true，如下面的例子所示：  
`var hasXmlDom = document.implementation.hasFeature("XML", "1.0");`

## 操作节点

* **appendChild**

* **insertBefore**  
  > Function(newNode, target)  

  任意位置插入节点，接受两个参数：要插入的节点和作为参照的节点。第二个参数不传则跟appendChild一样。

* **replaceChild**  

  > Function(newNode, replaceNode)  

  替换节点，接受两个参数：要插入的节点和要替换的节点  
  
* **removeChild**  

  移除节点，要移除的节点将成为方法的返回值。  

**注：要使用这几个方法必须先取得父节点。**  

---

* **cloneNode**  
  > Function(boolean)

  接受一个布尔值参数，表示是否执行深复制。为 true
  的情况下，执行深复制，也就是复制节点及其整个子节点树；在参数为 false 的情况下，执行浅复制，
  即只复制节点本身。  
 
  复制后返回的节点副本属于文档所有，但并没有为它指定父节点。因此，这个节点
  副本就成为了一个“孤儿”，除非通过 appendChild()、insertBefore()或 replaceChild()将它
  添加到文档中。
  ```js
  var deepList = myList.cloneNode(true); 
  alert(deepList.childNodes.length); //3（IE < 9）或 7（其他浏览器）
  var shallowList = myList.cloneNode(false); 
  alert(shallowList.childNodes.length); //0
  ```
  deepList.childNodes.length 中的差异主要是因为 IE8 及更早版
  本与其他浏览器处理空白字符的方式不一样。IE9 之前的版本不会为空白符创建节点。  
  
  **注：** cloneNode()方法不会复制添加到 DOM 节点中的 JavaScript 属性，例如事件处
  理程序等。这个方法只复制特性、（在明确指定的情况下也复制）子节点，其他一切
  都不会复制。IE 在此存在一个 bug，即它会复制事件处理程序，所以我们建议在复制
  之前最好先移除事件处理程序。  

* **normalize**  
这个方法唯一的作用就是处理文档树中的文本节点。
由于解析器的实现或 DOM 操作等原因，可能会出现文本节点不包含文本，或者接连出现两个文本节点
的情况。当在某个节点上调用这个方法时，就会在该节点的后代节点中查找上述两种情况。如果找到了
空文本节点，则删除它；如果找到相邻的文本节点，则将它们合并为一个文本节点。

## Text类型

文本节点由 Text 类型表示，包含的是可以照字面解释的纯文本内容。纯文本中可以包含转义后的HTML 字符，但不能包含 HTML 代码。Text 节点具有以下特征：
* `nodeType` 的值为 3；  
* `nodeName` 的值为`"#text"`；  
* `nodeValue` 的值为节点所包含的文本；
* `parentNode` 是一个 `Element；`  不支持（没有）子节点。

可以通过`nodeValue`、`data`属性访问节点中包含的文本。  

  可以通过一下代码来访问文本子节点：  
  ```
  var textNode = div.firstChild; // 获者div.childNodes[0]
  ```

* 方法  

1.  `appendData(text)` 将 text 添加到节点的末尾。  

2.  `deleteData(offset, count)` 从 offset 指定的位置开始删除 count 个字符。  

3.  `insertData(offset, text)` 在 offset 指定的位置插入 text。  

4.  `replaceData(offset, count, text)` ：用 text 替换从 offset 指定的位置开始到 offset+ count 为止处的  文本。  

5.  `splitText(offset)` 从 offset 指定的位置将当前文本节点分成两个文本节点。  

6.  `substringData(offset, count)` 提取从 offset 指定的位置开始到 offset+count 为止处的字符串。 

* 创建文本节点  

  `document.createTextNode`   
  
  需要把新节点插到文档树中以存在的节点中，才能看到

## Comment类型

注释在 DOM 中是通过 Comment 类型来表示的。Comment 节点具有下列特征：
* `nodeType` 的值为 8；  
* `nodeName` 的值为`"#comment"`；  
* `nodeValue` 的值是注释的内容；
* `parentNode` 可能是 `Document` 或 `Element；`   
* 不支持（没有）子节点。  

Comment 类型与 Text 类型继承自相同的基类，因此它拥有除 splitText()之外的所有字符串操作方法。与 Text 类型相似，也可以通过 nodeValue 或 data 属性来取得注释的内容。  

注释节点可以通过其父节点来访问，以下面的代码为例。
```
  <div id="myDiv"><!--A comment --></div> 
```
在此，注释节点是`<div>`元素的一个子节点，因此可以通过下面的代码来访问它。
```
var div = document.getElementById("myDiv"); 
var comment = div.firstChild; 
alert(comment.data); //"A comment"
```  

**创建注释节点**  

`document.createComment`