### DOM文档对象模型

DOM是针对HTML和XML文档的一个API（应用程序编程接口）,DOM描绘了一个层次化的节点树，允许开发人员添加，移除，修改页面的某一部分。1998年10月DOM1级所有DOM对象都是以COM对象的形式实现的。这意味着IE中的DOM对象与原生JavaScript对象的行为或活动特点并不一致。DOM可以将任何HTML或XML文档描绘成一个由多层节点构成的结构。节点分为几种不同的类型，每种类型分别表示文档中不同的信息或标记。每个节点拥有各自的特点，数据和方法，另外也有与其他节点存在某种关系。节点之间的关系构成了层次，所有页面标记则表现为一个以特定节点为根节点的树形结构。

#### Node节点类型

- Document类型       document
- Element类型            element
- Text类型                    文本节点
- Comment类型          注释节点

#### Node节点的属性和方法

##### 节点关系

###### 属性

- nodeType   表示节点类型

  Document--> 9;Element -->1;TextNode -->3;Comment--> 8
  document 是Document构造函数的实例
  document.body是Element构造函数的实例
  document.body.firstChild 是Comment构造函数的实例

- nodeName  该属性取决于节点类型，如果是元素类型，值为元素的标签名

- nodeValue   该属性取决于节点类型，如果是元素类型，值有null

- childNodes  属性，保存一个NodeList对象，NodeList是一种类数组对象用来保存一组有序的节点，NodeList是基于DOM结构动态执行查询的结果，DOM结构变化可以自动反应到NodeList对象中。访问时可以通过中括号访问，也可以通过item()方法访问。可以使用slice方法将NodeList转换为数组

  ```js
  var arr = Array.prototype.slice.call(nodes,0)
  ```

- parentNode指向文档树中的父节点。包含在childNodes列表中所有的节点都具有相同的父节点，每个节点之间都是同胞/兄弟节点 。

- previousSibling 兄弟节点中的前一个节点

- nextSibling 兄弟节点中的下一个节点

- firstChild  childNodes列表中的第一个节点

- lastChild  childNodes列表中的最后一个节点

- ownerDocument指向表示整个文档的文档节点。任何节点都属于它所在的文档，任何节点都不能同时存在于两个或更多个文档中。

###### 方法

- hasChildNodes()在包含一个或多个子节点的情况下返回true

  **操作节点，以下四个方法都需要父节点对象进行调用!**

- appendChild()  向childNodes列表末尾添加一个节点。返回新增的节点。关系更新如果参数节点已经为文档的一部分，位置更新而不插入，dom树可以看做是由一系列的指针连接起来的，任何DOM节点不能同时出现在文档中的多个位置

- insertBefore() //第一个参数：要插入的节点；第二个参数：作为参照的节点；被插入的节点会变成参照节点的前一个同胞节点,同时被方法返回。如果第二个参数为null将会将该节点追加在NodeList后面

- replaceChild() //第一个参数：要插入的节点；第二个参数：要替换的节点；要替换的节点将由这个方法返回并从文档树中被移除，同时由要插入的节点占据其位置

- removeChild() //一个参数，即要移除的节点。移除的节点将作为方法的返回值。其他方法,任何节点对象都可以调用。

###### 其他方法

- cloneNode()

  用于创建调用这个方法的节点的一个完全相同的副本。有一个参数为布尔类型参数为true时，表示深复制，即复制节点以及整个子节点数。参数为false的时候，表示浅复制，只复制节点本身。该方法不会复制添加到DOM节点中的JavaScript属性，例如事件处理程序等。该方法只复制特定,子节点，其他一切都不复制。但是IE中可以复制，建议标准相同，在复制之前，移除所有事件处理程序。

- normalize()

  处理文档树中的文本节点，由于解析器的实现或DOM操作等原因，可能会出现文本节点不包含文本，或者接连出现两个文本节点，当在某个节点上调用了该方法，会删除空白节点，会找到相邻的两个文本节点，并将他们合并为一个文本节点。

#### Document类型

Javascript通过使用Document类型表示文档。在浏览器中，document对象是HTMLDocument的一个实例，表示整个HTML页面。document对象是window对象的一个属性，因此可以直接调用。HTMLDocument继承自Document 。

##### 文档子节点

可以继承Node中所有的属性和方法
属性：

- documentElement 始终指向HTML页面中的<html>元素。
- body 直接指向<body>元素
- doctype 访问<!DOCTYPE>, 浏览器支持不一致，很少使用
- title 获取文档的标题
- URL 取得完整的URL
- domain 取得域名，并且可以进行设置，在跨域访问中经常会用到。
- referrer 取得链接到当前页面的那个页面的URL，即来源页面的URL。
- images 获取所有的img对象，返回HTMLCollection类数组对象
- forms 获取所有的form对象，返回HTMLCollection类数组对象
- links 获取文档中所有带href属性的<a>元素

##### 查找元素

- **getElementById()**

  参数为要取得元素的ID，如果找到返回该元素，否则返回null如果页面中多个元素的ID值相同，只返回文档中第一次出现的元素。如果某个表单元素的name值等于指定的ID，该元素也会被匹配。

- **getElementsByTagName()**

  参数为要取得元素的标签名，返回包含另一个或者多个元素的NodeList，在HTML文档中该方法返回的是HTMLCollection对象，与NodeList非常类似。可以通过[index/name],item(),namedItem(name)访问

- **getElementsByName()**

  参数为元素的name,返回符合条件的HTMLCollection

- **getElementsByClassName()**

  参数为一个字符串，可以由多个空格隔开的标识符组成。当元素的class属性值包含所有指定的标识符时才匹配。HTML元素的class属性值是一个以空格隔开的列表，可以为空或包含多个标识符。

#### Element类型

##### HTML元素

所有的HTML元素都由HTMLElement类型表示，或者其子类型表示。每个HTML元素都应具有如下一些属性以及html元素特有的属性 。
`id` 元素在文档中的唯一标识符
`title` 有关元素的附加说明信息
`className` 与元素class特性对应
`src` img元素具有的属性
`alt`img元素具有的属性
`lang` 元素内容的语言代码，很少使用！
`dir` 语言方向，ltr,rtl 左到右，右到左、
每个元素都有一个或者多个特性，这些特性的用途是给出相应元素或内容的附加信息。可以通过属性访问到该属性对应的值,特性的名称是不区分大小写的，即"id""ID"表示相同的特性，另外需要注意的是，根据HTML5规范，自定义特性应该加上data-前缀，以便验证。

##### 取得自定义属性

getAttribute() 参数为实际元素的属性名，class,name,id,title,lang,dir一般只有在取得自定义特性值的情况下，才会用该方法。大多数直接使用属性进行访问，比如style,onclick

##### 设置属性

```js
dom.className = "one"
dom.setAttribute("class","one");
```

setAttribute() ：两个参数，第一个参数为要设置的特性名，第二个参数为对应的值。如果该值存在，替换

##### 移除属性

removeAttribute() 移除指定的特性

#####  attributes 属性

其中包含了一个NamedNodeMap,与NodeList类似
getNamedItem(name) 返回nodeName属性等于name的节点
removeNamedItem(name)  从列表中删除nodeName属性等于name的值
setNamedItem(node) 向列表中添加一个节点
item(pos) 返回位于数字pos位置处的节点

##### 创建元素

document.createElement()：一个参数，要创建元素的标签名。该标签名在HTML中不区分大小写，但是在XML中区分大小写

##### 元素内容
innerHTML 返回元素内容
textContent 元素内部的文本，不去除空格和回车
innerText 元素内部的文本，去除回车和换行

#### Text文本类型

文本节点。包含的是可以按照字面解释的纯文本内容。
length //文本长度
appendData(text) //追加文本
deleteData(beginIndex,count) //删除文本
insertData(beginIndex,text) //插入文本
replaceData(beginIndex,count,text) //替换文本
splitText(beiginIndex) //从beginIndex位置将当前文本节点分成两个文本节点
document.createTextNode() //创建文本节点，参数为要插入节点中的文本
substringData(beiginIndex,count)  //从beginIndex开始提取count个子字符串

#### Comment注释类型

```html
<div id = "myDiv"><!--a comment--></div>

<!--a comment--> Comment类型
```



