#### css颜色值

​    关键字   red，blue

   16进制的6位字符   #ffffff

   16进制的3位字符   #fff

​    rgb(0,255,100) r-red;g-green;b-blue;取值范围0~255

​    rgba(0,255,100,0.5) 最后一个参数表示透明度，取值范围0~1

#### css单位

   px     绝对单位

   em    相对单位（不固定的，随当前元素设置的font-size变化而变化）

   rem   相对单位（固定的，1rem=16px）

   cm   厘米，  mm  毫米

​    %    百分比

#### 文本样式

-  **color** 为字体指定颜色
  Ø 字体颜色的取值：
  • 关键字
  • RGB

-  **font-family** 为文字指定特殊的字体，浏览器只会使用浏览器可以访、问到的字体

   Ø 通用字体

  -  serif  :有衬线的字体,笔画结尾有特殊的装饰线或衬线
  -  sans-serif :无衬线的字体，笔画结尾是平滑的字体
  -  monospace :等宽字体，用于代码，字体中每个字宽度相同
  -  cursive  :草书，这种字体有的有连笔，有的还有特殊的斜体效果。
  -  fantasy  :装饰字体 ，具有特殊艺术效果的字体

  Ø 常用字体

  -  Microsoft YaHei
  -  微软雅黑
  -  宋体

  Ø字体栈:
  由于我们不能保证我们的字体有效，可以一次性指定多个字体，如果第一个
  字体无效则使用下一个字体，以此类推。一般将字体栈中最后一个字体设置
  为通用字体

  ```html
  p {font-family: "Microsoft YaHei",arial,sans-serif;}
  ```

-  **font-style** 用于打开和关闭斜体文本

    Ø 取值

  -  normal : 正常字体，关闭斜体
  -  italic : 斜体
  -  oblique : 模拟斜体

-  **font-weight** 为字体设置粗细程度

  Ø 取值:

  -  normal(400), bold(700)  : 正常，加粗字体
  -  lighter, bolder : 设置当前元素字体比父元素更粗或者更细
  -  100–900  : 数值类型的粗细程度（值越大字体越粗）

-  **font-size** 为文字指定大小

-  **text-align** 文字排列方式

     Ø 取值：

  - left：内容左对齐。

  - center：内容居中对齐。

  - right：内容右对齐。

  - justify：内容两端对齐，但对于强制打断的行（被打断的这一行）及最后一行（包括仅有一行文本的情况，因为它既是第一行也是最后一行）不做处理。（CSS3）

  - start：内容对齐开始边界。（CSS3）

  - end：内容对齐结束边界。（CSS3）

  - match-parent：这个值和 inherit 表现一致，只是该值继承的 start 或 end 关键字是针对父母的 <' [direction](http://css.cuishifeng.cn/direction.html) '> 值并计算的，计算值可以是 left 和 right 。（CSS3）

  - justify-all：效果等同于 justify，但还会让最后一行也两端对齐。（CSS3）

-  **text-transform** 设置或者取消字体改变

    Ø 取值:

  -  none  :防止任何改变
  -  uppercase: 将文本转换为大写
  -  lowercase: 将文本转换为小写
  -  capitalize: 将所有单词第一个字母转换为大写
  -  full-width: 转换为类似于一个等宽字体

-  **text-decoration** 设置或者取消文本修饰

    Ø 取值:

  -  none : 取消所有文本修饰
  -  underline  : 为文本添加下划线
  -  overline  : 为文本添加上划线
  -  line-through : 为文本添加删除线

-  **text-shadows** 设置或者取消文本阴影

    Ø 语法：
  text-shadow: h-shadow v-shadow blur color;
  Ø 取值:

  - none : 取消所有阴影
  - h-shadow  : 必需。水平阴影的位置。允许负值。
  - v-shadow : 必需。垂直阴影的位置。允许负值。
  - blur : 可选。模糊的距离。
  - color : 可选。阴影的颜色。参阅 CSS 颜色值。

#### 字体样式

当用户电脑中没有安装对应字体的时候，webFont可以加载网络字体进行显示。

````html
@font-face {
font-family: 'myfont';
src: url(“/fonts/浅浅の奈雪体.woff”), url("/fonts/浅浅の奈雪体.ttf");
}
.myfont {
font-family: myfont;
}
<div class="myfont">
浅浅の奈雪体
</div>

````

#### 字体图标

在开发网站的时候需要各种各样的小图标，这些图标如果要求美工绘制可能比较麻烦，那么我们可以直接使用开源的字体图标库，这些字体图标库使用webFont原理，我们加载一个图标就好像加载一个字体一样简单，通过控制字体大小，字体颜色来控制图标的大小与颜色。
目前比较流行的开源字体图标库有：

```shell
font-awesome http://fontawesome.dashgame.com/
iconfont http://www.iconfont.cn/
```

#### 超链接默认样式

-  cursor : 调整光标悬浮到链接上的时候光标的形状
-  outline : 调整环绕链接的框
-  text-decoration : 自定义设置列表项标志
-  text-align : 文本对齐方式