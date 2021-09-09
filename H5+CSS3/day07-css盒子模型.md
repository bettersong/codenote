#### 属性

display:none/block

visibility:hidden/visible

opacity:0/1/5(0~1)

overflow(超出):hidden/auto

```html
/* 超出隐藏 */

/* overflow: hidden; */

 /* 超出产生滚动条 */

/* overflow: auto; */

/* 无论有没有溢出都产生滚动条 */
```

#### 盒子模型

    ##### 盒子属性

文档中的每个元素都可以被看作是一个矩形盒子。具有如图的一些属性。

 **width & height**
用于设置内容区的宽高，该片区域用于显示内容。盒子高度默认为内容的高度。                         
 **padding**                                      
代表内容盒子的外边界与外边   框的内边界之间的距离             
**border**                                           
设定介于padding的外边缘与margin的内边缘之间，默认值为0                   
 **margin**
代表盒子四周的区域。设值方法与padding类似。相邻元素的边框会合并（margin collapsing）

![1560994429630](D:\Users\Administrator\Desktop\briup\笔记\H5+CSS3\images\1560994429630.png)

**padding取值技巧：**(margin同理)

````html
/* 一个值：四个方向 */
/* padding:20px; */
/* 两个值：上下，左右 */
/* padding: 10px 20px */
/* 三个值： 上，左右，下 */
/* padding：10px 5px 15px; */
/* 四个值：上，右，下，左 */
/* padding：10px 5px 15px 20px */
````

##### 默认盒子模型（w3c盒子模型）

使用box-sizing属性可以改变盒子模型，取值“content-box”的盒子为默认盒子模型。
width 属性仅表示盒子内容所占的宽度
height 属性仅表示盒子内容所占的高度

**盒子宽width = width+padding-left + padding-right+border-left + border-right**

**盒子高height = height+padding-top+padding-bottom+border-top+border-bottom**

**盒子所占屏幕高 = 盒子高+margin-top+margin-bottom**

**盒子所占屏幕宽 = 盒子宽+margin-left+margin-right**

![1560995345717](D:\Users\Administrator\Desktop\briup\笔记\H5+CSS3\images\1560995345717.png)

##### 边框盒子模型（IE盒子模型）

使用box-sizing属性可以改变盒子模型，取值“border-box”的盒子为边框盒子模型。

````html
/* 盒子宽：width  100px */
          /* 盒子高： height 100px */
          /* 内容区宽：width-左右border-左右padding */
          /* 内容区高：height-上下border-上下padding */
          /* 所占屏幕宽：width+左右margin */
          /* 所占屏幕高：height+上下margin */
          /* box-sizing 用来设置盒子模型 */
          /* w3c盒子模型，内容区的宽高 */
          /* box-sizing: content-box; */
          /* ie盒子模型 盒子的宽高 */
          box-sizing: border-box;
````



![1560995472743](D:\Users\Administrator\Desktop\briup\笔记\H5+CSS3\images\1560995472743.png)

#### 盒子背景样式（background）

**背景  （ Backgrounds ）**

-  background-color 为元素设置一种颜色

-  background-image 为元素设置一个背景图片

  Ø 取值：
  • none
  • url()

-  background-clip 设定背景的覆盖范围

  Ø 取值：
  • border-box : 背景位于边框以内
  • padding-box : 背景位于内边距以内
  • content-box : 背景位于内容区

-  background-origin 设定背景的起始点

  Ø 取值：
  • border-box : 背景图片从边框的外边缘开始绘制
  • padding-box : 背景图片从内边距的外边缘开始绘制
  • content-box : 背景图片绘制在内容区

-  background-position 设置为背景图像初始位置

  Ø 取值：
  • 关键字
  • 坐标

-  background-repeat 设置背景图片的重复方式

  Ø 取值：
  • repeat  : 为了覆盖整个背景范围，背景图片尽可能多的重复，在背景
  边缘切割(clipping)最后一个图片以使用整个背景范围。
  • space  : 使用空白分隔开图片，尽可能使背景图片占满整个屏幕而不
  使用切割(clipping)
  • round  : 将图片压缩或者是扩展以适应整个背景范围，不使用切割
  • no-repeat : 不重复

-  background-attachment 设置背景图片的固定点

  Ø 取值：
  • fixed  : 背景固定在视口上，背景不随滚动条的滚动而滚动
  • scroll  : 背景固定在自身元素上（默认），随滚动条的滚动而滚动
  • local  : 背景固定在自身元素的内容上

-  background-size 设置背景的大小

  Ø 取值：
  • cover : 背景铺满整个屏幕，（尽量不要使用大图覆盖小范围）

-  background 背景设置的速记写法

  Ø 取值：

  ```html
  [<attachment>][<clip>][<color>][<image>][<position>][<repeat>]
  ```

-  background-color
  为元素设置一种颜色
  Ø 取值：
   关键字
   十六进制
   RGB
   HSL
   RGBA
   HSLA

#### 图片精灵技术

服务器

前端项目部署到服务器上，图片当然也是在服务器上

background-position



#### 边框（border）

-  **border-width** 为元素指定边框的宽度

-  **border-style** 为元素指定边框样式

  Ø 取值：
  • none  : 不设置
  • hidden : 隐藏
  • dotted : 显示一系列的点
  • dashed : 显示一系列小线段
  • solid  : 显示一条单一的实心直线
  • double : 显示两条实心直线
  • groove : 边框雕刻效果(与ridge相反)
  • ridge  : 与groove相反
  • inset  : 嵌入式边界框(与outset相反)
  • outset : 突出的边界框

-  **border-color** 为元素指定边框颜色

-  **border-radius** 为元素指定圆角边框的半径

-  **border-image** 为元素设定边框背景

  Ø 子属性：
  ü border-image-source 用于加载作为边框的图片
  • url()
  ü border-image-slice 用于切割边框图片
  • one value  : all the slices are the same size
  • Two values : top and bottom, left and right
  • Three values : Top, left and right, bottom
  • Four values : Top, right, bottom, left
  ü border-image-repeat 用于设置边框图片重复方式
  • stretch : 拉伸，不推荐；repeat : 重复切割；
  • round : 重复自适应； space : 重复自适应；

-  **border-image-source** 用于加载作为边框的图片

-  **border-image-slice** 用于切割边框图片

-  **border-image-repeat** 用于设置边框图片重复方式

-  **border** 边框相关属性的速记写法