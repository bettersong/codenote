#### 标签选择器

标签选择器又叫元素选择器，换句话说，文档的元素就是最基本的选择器，使用元素名称直接选中元素即可。

例如：

```html
<style>
p{
height:100px;
border:1px solid red;
}
</style>
```

#### 类选择器

类选择以点"."开头，后面紧跟一个类名。类名不允许有空格，与元素中class属性的值保持一致。一个元素可以有多个class的值，每个值通过空格分割开。类名相同的元素属于一类元素。

```html
<style>
.first{font-weight: bold;}
.done {text-decoration: line-through;}
</style>
<ul>
<li class="first done">Create an HTML document</li>
<li class="second done">Create a CSS style sheet</li>
<li class="third done">Link them all together</li>
</ul>
```

#### ID选择器

ID选择器以"#"开头，后面紧跟一个ID名，在一个文档中，ID值不能重复，因此在选择文档中唯一元素的时候该选择器比较有用。

```html
<style>
#polite { font-family: cursive;}
#rude { font-family: monospace; text-transform: uppercase;}
</style>
<p id="polite"> — "Good morning."</p>
<p id="rude"> — "Go away!"</p>
```

#### 普遍选择器

使用"*”来表示普遍选择器，表示选择所有元素，通常用在组合选择器中。

```html
<style>
.left-nav > * { width:200px; background-color:#fafafa}
</style>
<article class="left-nav">
<dl>
<dt>推荐</dt>
<dd class="current"><i class="icon-music"></i>发现音乐</dd>
</dl>
<dl>
<dt>我的音乐</dt>
<dd><i class="icon-cloud-download"></i>下载的音</dd>
</dl>
</article>
```

#### 层次选择器

    #####    后代选择器

使用 “ ” 隔开两个选择器。例如 “ul li”表示选择ul的后代元素li，li可以为ul的直接子元素，也可以为ul的孙子元素。

#####   子代选择器

 使用 “>” 隔开两个选择器。例如 "ul>li"表示选择ul的直接子代元素li,ul的孙子元素li无法被选择到

#####   相邻同胞选择器

 使用 “+” 隔开两个选择器。例如 ".one+*"表示选择class为"one"元素的下一个兄弟元素。

```html
#p2+*{
   border:1px dotted #ccc
}
```



#####   一般同胞选择器

 使用 “~” 隔开两个选择器。例如 ".one~*"表示选择class为"one"元素的所有兄弟元素。。

#### 多选择器

多个选择器并列使用，使用“,”分割。
例如 "div,.one,#tt" 表示选择div元素，class为one的元素以及id为tt的元素

#### 组合选择器

多个选择器组合使用。例如 "div.one" 表示class为one的div元素。

#### 属性选择器

-  **[attr]**  选择具有attr属性的元素、无论该属性的值为什么
-  **[attr=val]**  选择具有attr属性的、并且attr的值为val元素
- **[attr~=val]** 选择具有attr属性的、并且attr的值之一为val的元素
- **[attr^=val]**  选择具有attr属性的、并且attr的值以val开头的元素
- **[attr$=val]**  选择具有attr属性的、并且attr的值以val结尾的元素
- **[attr*=val]** 选择具有attr属性的、并且attr的值包含val的元素

#### 伪类选择器

伪类以 ":" 开头，用在选择器后，用于指明元素在某种特殊的状态下才能被选中

**表示子元素的**

-  :only-child  独生子元素

-  :first-child  第一个孩子

-  :last-child   最后一个孩子

-  :nth-child(n) 、: nth-last-child(n)

  ​      • n可以为元素的序号，也可以为特殊的字符，比如“odd”，“even

-  :first-of-type、:last-of-type 、

-  :nth-of-type(n)、:nth-last-of-type(n)   每种类型的第几个，倒数第几个
        • n可以为元素的序号，也可以为特殊的字符，比如“odd”，“even

**元素状态相关**

-  :hover、 :active、 :focus
-  :enabled可用的、 :disabled禁用的；:checked用户选中的、 :default默认选中的
-  :invalid未通过验证的、 :valid通过验证的、 :required必填项、 :optional选填项、 :in-range在范围内 、:out-of-range在范围外

#### 伪元素选择器

伪元素以“::”开头，用在选择器后，用于选择指定元素

-  ::after  选中之后不存在的节点，可配合content来使用
-  ::before 选中之后不存在的节点，可配合content来使用
-  ::first-letter 选中第一个字符
-  ::first-line  选中第一行文本
-  ::selection  用户选中的文本

```html
section>p::before{
        content:'*';
        color:red;
    }
/* 选中第一个字符 */
    .words::first-letter{
        font-size: 30px;
        color: coral;
    }
    /* 选中第一行文本 */
    .words::first-line{
        color: navy;
    }
    /* 用户选中的文本 */
    .words::selection{
        background-color: cyan;
        color: #fff;
    }
```



