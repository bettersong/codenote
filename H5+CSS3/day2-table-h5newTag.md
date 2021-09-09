#### 相对路径和绝对路径

**相对路径：**相对于当前的目录结构产生的路径

**绝对路径：**相对系统盘符

.叫当前目录，..叫上级目录

#### 超链接

href属性：

​    1.ID值：锚点链接

​    2.URL：URL地址

​    3.邮箱跳转：mailto:邮箱地址

```html
<h3 id="top">我是top</h3>
    <!-- 跳转绝对路径 -->
    <a href="http://www.baidu.com" >百度</a>  
    <!-- 跳转相对路径 -->
    <a href="./note.md">笔记</a>
    <div style="height:800px">
    
    <!-- 邮箱跳转 -->
    <a href="mailto:songyao666@gmail.com">发送邮件</a>

    <!-- target 默认是_self,在当前选项卡打开-->
    <a href="./note.md" target="_self">在本选项卡跳转</a>
    <a href="./note.md" target="_blank">在新建项卡跳转</a>
    </div>

    <!-- 锚点链接 href值是一个id值-->
    <a href="#top">back</a>
```

#### 图片

src表示图片地址

alt表示替换图片的文字，当URL地址出错时，将会显示alt内容

width图片宽度

height图片高度

#### 表格

**table** 

属性：border表格边框宽度

​            cellspacing单元格与单元格之间的空隙

​            cellpadding单元格的内边距

​           align表格水平对齐方式，值：center，top,bottom left,right

<u>表头 thead</u>

<u>表头中使用th</u>

<u>表体 tbody</u>

<u>表体中使用td</u>

<u>表尾 tfoot</u>

<u>caption 表格的信息标题</u>

<u>tr 表示表格的行，可以包含td,th元素</u>

<u>td/th</u>

td表示表格的单元格 ， th常用于表头

**属性**

colspan 跨列数

rowspan 跨行数

colgroup定义一个表中一组列，只能作为table的子元素，位于caption元素之后，其他元素之前

```html
<!-- 单元格的合并 -->
    <table width="500" cellspacing="0" cellpadding="0" border="1" align="center">
        <colgroup>
           <col />
           <!-- 接下来的三列当做一列去处理 -->
           <col span="3" style="background-color: aqua"/>
        </colgroup>
        <tr>
            <!-- colspan 合并列 -->
            <td colspan="2">11</td>
            <!-- <td>12</td> -->
            <td>13</td>
            <td>14</td>
        </tr>
        <tr>
            <!-- rowspan 合并行 -->
            <td>21</td>
            <td>22</td>
            <td rowspan="2">23</td>
            <td>24</td>
        </tr>
    </table>
```

#### H5新增标签

- **header**是一种具有引导和导航作用的元素

- **nav**是一个可以用作页面导航的链接组

- **article**代表文档，页面或应用程序中独立的，完整的，可以独自被外部引用的内容

- **section**作为HTML文档独立的功能

- **aside**用来表示当前页面或文章的附属信息部分，可以包含当前页面或主要内容的相关引用，侧边栏，广告，导航条

- **footer**作为其上层父级区块或根区块的一个脚注

- **adress**用来在文档中呈现联系信息

- **figure&figcaption**figure元素用来表示网页上一块独立内容，将其从网页上移除后不会对网页上的其他内容产生任何影响，figure元素所表示的内容可以是图片，统计图或代码示例，也可以是音频插件，视频插件，
  统计表格等。figcaption元素表示figure元素的标题，它从属于figure元素，必须书写在figure元素内部。一个figure元素内最多只允许放置一个figcaption元素，但是允许放置多个其他元素

- **details**

  details元素是一种用于标识该元素内部的子元素可以被展开,收缩显示的元素。
  details元素内并不仅限于放置文字，也可以放置表单,插件或对于一个统计图提供的详细数据表格。
  open
  当该属性值为true时，该元素内部的子元素应该被展开显示；当该属性的值为false时，其内部的子元素应该被收缩起来不显示。默认值为false
  summary
  summary元素从属于details，用鼠标单击summary元素中的内容文字时，details元素中的其他所有从属元素将会展开或收缩。如果details元素内没有summary元素，浏览器会提供默认文字(详细信息)以供单击。





