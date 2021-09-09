#### css3

##### 什么是css?

CSS (Cascading Style Sheets) 层叠样式表，是一个用于修饰文档（可以是标记语言HTML，也可以是XML或者SVN）的语言，可以将文档以更优雅的形式呈现给用户

#### css语法

#####       css 声明

css语言的核心功能就是为特定的属性设定特点的值，css引擎通过计算出声明的每个属性来显示元素。css的属性和值都是大小写敏感的。属性与值通过“:”分隔开。并不是所有的值都适用于同一个属性，不同的属性有一系列不同
的值。

#####       css声明块

将多个css声明写在一起，每个css声明通过“;”分隔开，最后一个css声明无需使用“;”分隔开。使用“{”“}”将多个css声明括起来，这样就构成了一个css声明块。

#####       css选择器和规则

在css声明块前添加一个选择器，用来指明将css声明应用在哪些元素上。

     #####       css可读性

l 空白（ White space ）
空白意味着实际空格、制表符和新行，可以添加空白使样式表更加可读。
l 注释（ Comments ）
/* 这里就是CSS的注释 */
l 速记写法（ Shorthand ）
类似于font，background， padding， border， margin 这些都被称为速记属性。
这些属性允许我们在一行中写多个属性值。速记属性可以节省时间,使代码整洁。
例如：
```padding: 10px 15px 15px 5px;```
等价于

```css
padding-top: 10px; padding-right: 15px; padding-bottom: 15px; padding-left: 5px;
```

#### css应用

      #####    外部样式表

将CSS规则写到一个以“.css”为后缀的文件中，然后使用HTML中的<link>标签将CSS文件应用到HTML文档中

```css
<head>
<meta charset="utf-8">
<link rel="stylesheet" href="style.css">
</head>
```

    #####    内部样式表

将CSS规则写到<style标签>中，在不能直接更改CSS文件情况下，这种方式有效，但是一般不推荐使用，修改起来不方便。

```css
<head>
<meta charset="utf-8">
<style>
h1 { color: blue; background-color: yellow; border: 1px solid black; }
p { color: red; }
</style>
</head>
```

   #####    行内样式表

将CSS规则写到元素的style属性中，只能作用与一个元素，一般不推荐使用。

```html
<body>
<h1 style="color: blue;background-color: yellow;border: 1px solid black;">
Hello World!
</h1>
<p style="color:red;">This is my first CSS example</p>
</body>
```

   #####   @import 导入外部样式

```html
<style>
@import './css/index.css'；
    /*放在style的第一行导入*/
</style>
```



#### css工作原理

    #####        css工作原理

当一个浏览器在显示文档的时候，需要将文档内容和样式内容结合到一起，以此，在处理文档的时候包含两个阶段：

1.   浏览器将HTML和CSS转换为DOM (Document Object Model)，DOM在计算机内存中表示文档,使得文档和CSS结合。

2. 浏览器显示DOM的内容

#####        DOM简介

DOM （Document Object Model）是一个树形结构，在HTML中的每个元素，
属性，甚至于文本都可以被转换为DOM中的一个节点。每个节点在节点树中都
有对应的关系节点（比如父节点，兄弟节点）

![1560753459536](D:\Users\Administrator\Desktop\briup\html_css\day04\1560753459536.png)





