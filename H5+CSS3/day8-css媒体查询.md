#### 媒体查询

CSS的媒体查询模块允许在不改变代码的前提下对显示效果进行调整，媒体查询由两部分组成，一个可选的媒体类型，以及若干个css表达式。当媒体类型判断结果为true的时候，其中的css表达式被解析。如果媒体查询应用在link中，即使判断结果为false，样式表同样会被下载，但是不会应用。

```html
<link rel= "stylesheet" media="(max-width:800px)" href="./main.css" />

<style>
    @media (max-width:600px){
        body{
            background:#ccc;
        }
    }
</style>
```

####  媒体类型
- all 适用所有的设备
- print 适用于打印用纸或打印预览视图
- screen 主要适用于电脑/手机/平板等智能设备屏幕
- speech 适用于语音合成器
- Tv 电视设备

####  逻辑运算符
 **and** 把多个媒体组合成一条媒体查询，对成链式的特征进行请求，只有当每个属性都为真时，结果才为真
@media tv and (min-width: 700px) and (orientation: landscape)
 **not** 用 来对一条媒体查询的结果进行取反
@media not screen and (color), print and (color) 等价于
@media (not (screen and (color))), print and (color)
**only** 仅在媒体查询匹配成功的情况下被用于应用一个样式，这对于防止让选中的样式
在老式浏览器中被应用到
<link rel="stylesheet" media="only screen and (color)" href="example.css" />
l , 媒体查询中使用逗号分隔效果等同于or逻辑操作符。当使用逗号分隔的媒体查询
时，如果任何一个媒体查询返回真，样式就是有效的。逗号分隔的列表中每个查询都是独
立的，一个查询中的操作符并不影响其它的媒体查询。
@media (min-width: 700px), handheld and (orientation: landscape)

#### 媒体特性

绝大多数媒体特性都可以使用“min-” or “max-” 修饰，表示最小条件和最大约束，例
如max-width: 600px 表示最大宽度为600px，也就是说当屏幕宽度<=600px的时候媒
体查询结果才为true。

-  color
  表示颜色设备，如果是非彩色设备，color为0，取值为颜色组件的比特数
  @media all and (min-color: 4) { ... }
- device-height/device-width
  描述输出设备的高度/宽度(意味着整个屏幕或页面，而不仅仅是呈现区域，比如文档窗
  口)。
  <link rel="stylesheet" media="screen and (max-device-height: 799px)" href="" />
- height/width
  高度媒体特性描述了输出设备的呈现表面的高度/宽度(例如窗口的高度或打印机的页面框)
- orientation

```shell
<768   超小屏
 992-1200 中屏
768-992  小屏
>1200    大屏

-webkit-  谷歌浏览器和Safari浏览器
-moz-   火狐浏览器
-ms-   IE浏览器
-o-    opera 浏览器
    
```

