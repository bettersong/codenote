#### 文本超出产生省略号

```css
div{
         width:100px;
         height:100px;
         border:1px solid red;
         /* 内部文本不换行 */
         white-space: nowrap;
         /* 溢出内容隐藏 */
         overflow: hidden;
         /* 产生省略号 */
         text-overflow: ellipsis;
     }
```

#### 定位布局

#####  静态定位（ Static positioning）

是所有元素的默认定位方式。意味着将一个元素定位在默认文档流中。默认文档流的位置
position: static;

#####   相对定位（ Relative positioning ）

-  与静态定位相似。
- 不脱离文档流，原先位置保留
-  对于相对定位的元素我们可以通过属性top, bottom, left, right来改变元素最终的位置。元素移动的时候是相对于【当前元素所在的位置】进行移动。
  position: relative;

#####    绝对定位（ Absolute positioning ）

-  元素脱离了文档流，即不在原来的位置上，原先位置不保留。
-  不干扰其他元素在页面中的位置，显示在其他元素的上方。
-  绝对定位元素是相对于他的【包含元素】定位
-  【包含元素】默认情况下为html，通过为绝对定位元素的父元素设定"position:relative"可以将这个父元素变为当前绝对定位元素的父元素。
-  当两个绝对定位元素叠加在一起的时候，可以使用“z-index”来改变两个绝对定位元素出现的顺序（ z-index 取值无需指定单位，值大的显示在上方。 ）
  position: relative;

#####     固定定位 (Fixed positioning)
- 固定定位元素相对于浏览器视口。

- 脱离文档流，原先位置不保留。

- 不会随着浏览器的滚动而滚动
  position: fixed;

  ##### 定位元素

  使用了position属性的元素

  定位元素才可以使用这四个属性：top，right, bottom, left

  

  #### 层叠顺序（z-index）

  z-index   值越大离眼睛越近（默认值是0）

  绝对定位元素，层叠顺序



#### 弹性布局/伸缩盒布局/flex布局

默认情况下，主轴是X轴（main axis），即flex容器中的各个元素在一行中分多列显示。如果想在一列中显示多行，我们可以将主轴改为Y轴
**flex-direction: row** （列布局）
**flex-direction: column** （行布局）

![1561084588621](D:\Users\Administrator\Desktop\briup\笔记\H5+CSS3\images\1561084588621.png)

```css
.container{
        height:650px;
        border:1px solid red;
        display: flex;
        /* 设置排列方式 */
        flex-direction: row;
        /* 起点在右 */
        /* flex-direction: row-reverse; */
        /* 起点在下 */
        /* flex-direction: column-reverse */
        /* 默认 从主轴起点对齐 */
        /* justify-content: flex-start; */
        /* 项目在主轴终点对齐 */
        /* justify-content: flex-end; */
        /* 项目在主轴上居中对齐 */
        /* justify-content: center; */
        /* 项目在主轴上，项目与项目之间有空间 */
        /* justify-content: space-between; */
        /* 项目四周有空间 */
        /* justify-content: space-around; */
        /* 交叉轴上的对齐方式 */
        /* 交叉轴的终点对齐 */
        /* align-items:flex-end; */
        /* 交叉轴的起点对齐 */
        /* align-items: flex-start; */
        /* 交叉轴居中对齐 */
        /* align-items: center; */
        /* 如果项目没有设置高，默认的对齐方式,给项目高度占满父元素高度 */
        /* align-items: stretch; */
        /* 项目内部的文本对齐，项目设置高度 */
        /* align-items: baseline; */
    }
```

##### 容器的属性

- flex-direction
- flex-warp
- flex-flow
- justify-content
- align-items
- align-content

###### flex-direction属性

`flex-direction`属性决定主轴的方向（即项目的排列方向

它有4个可能值：

- row(默认值)：主轴为水平方向，起点在左端
- row-reverse：主轴为水平方向，起点在右端
- column：主轴为垂直方向，起点在上沿
- column-recerse：主轴为垂直方向，起点在下沿

###### flex-warp属性

默认情况下，项目都排在一条线上（轴线）。`flex-warp`属性定义，如果一条轴线排不下，如何换行。

它有3个可能值

（1）nowarp（默认）：不换行

![1566264452938](D:\Users\Administrator\Desktop\briup\笔记\H5+CSS3\images\1566264452938.png)

（2）warp：换行，第一行在上方

![1566264502871](D:\Users\Administrator\Desktop\briup\笔记\H5+CSS3\images\1566264502871.png)

（3）warp-reverse：换行，第一行在下方

![1566264551688](D:\Users\Administrator\Desktop\briup\笔记\H5+CSS3\images\1566264551688.png)

###### flex-flow属性

`flex-flow`属性是`flex-direction`属性和`flex-warp`属性的简写形式，默认值为`row nowarp`。

###### justify-content属性

`justify-content`属性定义了项目在主轴上的对齐方式。

```css
.box{
    justify:flex-start | flex-end | center | space-between | space-around
}
```

![1566264876644](D:\Users\Administrator\Desktop\briup\笔记\H5+CSS3\images\1566264876644.png)

它有5个可能值，具体对齐方式与轴的方向有关。

假设主轴从左到右

- flex-start（默认值）：左对齐
- flex-end：右对齐
- center：居中
- space-between：两端对齐，项目之间的间隔都相等
- space-around：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍

###### align-items属性

`align-items`属性定义项目在交叉轴上如何对齐

```css
.box{
    align-items:flex-start | flex-end | center | baseline | stretch
}
```

![1566265253626](D:\Users\Administrator\Desktop\briup\笔记\H5+CSS3\images\1566265253626.png)

它有5个可能值。具体的对齐方式与交叉轴方向有关，下面假设交叉轴从上到下

- flex-start：交叉轴的起点对齐
- flex-end：交叉轴的终点对齐
- center：交叉轴的中点对齐
- baseline：项目的第一行文字的基线对齐
- stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度

###### align-content属性

`align-content`属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用

```css
.box{
    align-content: flex-start | flex-end | center | space-between | space-around | stretch
}
```

![1566265781993](D:\Users\Administrator\Desktop\briup\笔记\H5+CSS3\images\1566265781993.png)

该属性有6个可能值：

- `flex-start`：与交叉轴的起点对齐。
- `flex-end`：与交叉轴的终点对齐。
- `center`：与交叉轴的中点对齐。
- `space-between`：与交叉轴两端对齐，轴线之间的间隔平均分布。
- `space-around`：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。
- `stretch`（默认值）：轴线占满整个交叉轴。

##### 项目的属性 

以下6个属性设置在项目上：

- `order`
- `flex-grow`
- `flex-shrink`
- `flex-basis`
- `flex`
- `align-self`

###### order属性

`order`属性定义项目的排列顺序。数值越小，排列越靠前，默认为0

```css
.item {
    order: <integer>;
}
```

###### flex-grow属性

`flex-grow`属性定义项目的放大比例，默认为`0`，即如果存在剩余空间，也不放大。

```css
.item {
  flex-grow: <number>; /* default 0 */
}
```

如果所有项目的`flex-grow`属性都为1，则它们将等分剩余空间（如果有的话）。如果一个项目的`flex-grow`属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍。

###### flex-shrink属性

`flex-shrink`属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。

> ```css
> .item {
>   flex-shrink: <number>; /* default 1 */
> }
> ```

如果所有项目的`flex-shrink`属性都为1，当空间不足时，都将等比例缩小。如果一个项目的`flex-shrink`属性为0，其他项目都为1，则空间不足时，前者不缩小。

负值对该属性无效。

###### flex-basis属性

`flex-basis`属性定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为`auto`，即项目的本来大小。

> ```css
> .item {
>   flex-basis: <length> | auto; /* default auto */
> }
> ```

它可以设为跟`width`或`height`属性一样的值（比如350px），则项目将占据固定空间。

###### flex属性

`flex`属性是`flex-grow`, `flex-shrink` 和 `flex-basis`的简写，默认值为`0 1 auto`。后两个属性可选。

> ```css
> .item {
>   flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
> }
> ```

该属性有两个快捷值：`auto` (`1 1 auto`) 和 none (`0 0 auto`)。

建议优先使用这个属性，而不是单独写三个分离的属性，因为浏览器会推算相关值。

###### align-self属性

`align-self`属性允许单个项目有与其他项目不一样的对齐方式，可覆盖`align-items`属性。默认值为`auto`，表示继承父元素的`align-items`属性，如果没有父元素，则等同于`stretch`。

> ```css
> .item {
>   align-self: auto | flex-start | flex-end | center | baseline | stretch;
> }
> ```











































