#### 列表样式

-  列表的默认样式
-  list-style-type : 设置列表项标志类型
-  list-style-position : 设置列表项标志出现的位置
-  list-style-image  : 自定义设置列表项标志
-  list-style : 列表样式的速记写法

**默认样式**

-  ul,ol 元素的margin-top，margin-bottom均为16px(1em) ， padding-left为40px(2.5em)
-  li 元素没有设置默认的空白（行间距）
-  dl 元素的margin-top，margin-bottom均为16px(1em),但是没有内边距
-  dd 元素的margin-left为40px(2.5em)
-  p 元素的margin-top，margin-bottom为16px(1em)

#### list-style-type
设置列表项标志类

Ø 取值:

-  none : No item marker is shown.  去除样式
-  disc : A filled circle (default value)  小黑圆点
-  circle : A hollow circle  空心圆
-  square : A filled square  方形
-  decimal : Decimal numbers，Beginning with 1  数字编号 1,2,3,4,5...
-  lower-roman : Lowercase roman numerals，E.g. i, ii, iii, iv, v…  小写罗马数字
-  upper-roman : Uppercase roman numerals，E.g. I, II, III, IV, V… 大写罗马数字
- upper-alpha: 大写英文字母
-  decimal-leading-zero : Decimal numbers，Padded by initial zeros，E.g.01, 02, 03, … 98, 99
-  ...

####  list-style-position
设置列表项标志出现的位置
Ø 取值:

-  outside : 列表项标志出现在主块框的外部
-  inside : 列表项标志出现在主块框的内部

####  list-style-image
自定义设置列表项标志
Ø 取值:
 url() : 指定图标位置

#### list-style
列表样式的速记写法
Ø 取值:

```html
[<type>][<image>][<position>]
    /*如果图片能用优先显示图片，图片不能用是则显示type*/
```

####  line-height
设置列表的行高
Ø 取值:

-  绝对单位
-  相对单位