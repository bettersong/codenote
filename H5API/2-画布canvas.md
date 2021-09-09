### canvas API

#### 简介

canvas是H5中最受欢迎的功能，这个元素负责在页面中设定一个区域，然后就可以通过JS动态地在这个区域中绘制图形，canvas元素最早由苹果公司推出，当时主要用在Dashboard微件中，很快H5加入了这个元素，主要浏览器也迅速开始支持它。

#### 2D 上下文简介

画布本身是一个标签，没有绘图功能，有绘图功能的是图形上下文，图形上下文是一个封装了很多绘图功能的对象。当getContext()方法参数为“2d”的时候，获取到2D上下文。使用2D上下文提供的方法，可以绘制简单的2D图形，比如矩形，弧线和路径。2D上下文的坐标开始于canvas元素的左上角，原点坐标是(0,0)。

#### 绘制图形步骤

- 取得 canvas 对象
- 取得 2d 上下文 (context)
- 设定绘图样式
  fillStyle 填充的样式
  strokeStyle 图形边框的样式
- 指定线宽
  使用图形上下文对象lineWidth 属性设置图形边框的宽度
- 绘制矩形 ( 填充与绘制边框 )
  fillRect 填充矩形
  strokeRect 绘制矩形边框

#### 绘制矩形

矩形是唯一一种可以直接在2d上下文中绘制的形状

##### 方法

- fillRect(x,y,width,height); 绘制矩形
- strokeRect(x,y,width,height); 绘制矩形框
- clearRect(x,y,width,height); 清除画布中的矩形框
  这三个方法都能接受4个参数，矩形的x坐标，y坐标，宽度，高度，这些参数的单位都是像素

```js
var myCanvas = document.getElementById('myCanvas')
// 获取2d上下文对象
var context = myCanvas.getContext('2d')
//设置填充颜色
context.fillStyle = 'pink'
// 绘制矩形  起始坐标x,y,宽,高
context.fillRect(0,0,100,200)
// 设置边框线的宽度
context.lineWidth = 5
// 设置边框线样式
context.strokeStyle = 'coral'
// 绘制边框矩形
context.strokeRect(100,205,100,100)
```

#### 绘制路径-圆形

##### 步骤

1. 开始创建路径
context.beginPath();
2. 设置路径（圆形）
context.arc(x,y,radius,startAngle,endAngle,counterclockwise);
参数分别为：圆心x坐标，圆心y坐标，半径，开始角度，结束角度，绘制方向
(true为逆时针，false为顺时针)
3. 关闭路径
context.closePath();
4. 设定绘制样式，进行图形绘制
context.fillStyle = “rgba(225,0,0,0.25)”;
context.fill(); //填充图形

```js
var myCanvas = document.getElementById('myCanvas')
    // 获取2d上下文
    var context = myCanvas.getContext('2d')
    // 绘制圆
    // 先绘制圆的路径，进行填充或边框绘制
    // 打开路径
    context.beginPath()
    // 圆心坐标x,y,半径，开始角度，结束角度，是否逆时针true[默认false顺时针]
    context.arc(100,100,100,0,Math.PI)
    context.arc(200, 100, 100, 0, Math.PI/2)
    //context.closePath()   //关闭路径，将路径的终点与路径的起点连接
    // 绘制
    //context.fill()    //填充
    context.stroke()    //边框
```

#### 绘制路径-直线

##### 方法

- moveTo(x,y)
  将光标移动到指定坐标点，绘制直线的时候以这个坐标点为起点
  参数：起始点x坐标，起始点y坐标
- lineTo(x,y)
  表示直线的终点。
  参数：结束点x坐标，结束点y坐标

```js
var myCanvas = document.getElementById('myCanvas')
        var context = myCanvas.getContext('2d')
        // 路径
        context.beginPath()   //打开路径
        // 直线的起点
        context.moveTo(0,0)
        // 直线的终点
        context.lineTo(100,100)
        context.lineTo(200, 100)
        context.lineTo(200,200)
        context.lineTo(300,300)
        context.lineTo(400, 300)
        context.lineTo(400,400)
        //context.closePath()    //关闭路径
        // 绘制路径
        // context.fill()
        // 设置边框样式
        context.strokeStyle = 'pink'
        context.lineWidth = 1
        context.stroke()
```

#### 绘制路径-曲线

- bezierCurveTo(c1x,c1y,c2x,c2y,x,y)
  从上一点开始绘制一条曲线，到(x,y)为止，并且以(c1x, c1y)和(c2x, c2y)为控制点，曲线会圆滑的靠近这两个控制点。

  ![1564678573671](D:\Users\Administrator\Desktop\ReactStudy\笔记\images\曲线1.png)

- quadraticCurveTo(cx,cy,x,y)
  从上一点开始绘制一条 二次贝塞尔曲线 ，到(x,y)为止，并且以(cx,cy作为控制点)

![1564678512514](D:\Users\Administrator\Desktop\ReactStudy\笔记\images\曲线2.png)

```js
var myCanvas = document.getElementById('myCanvas')
var ctx = myCanvas.getContext('2d')
ctx.beginPath()
ctx.moveTo(0,0)
// 参数：控制点坐标1，控制点坐标2，终点坐标
ctx.bezierCurveTo(0,100,200,100,200,0)
ctx.strokeStyle = 'blue'
// ctx.closePath()
ctx.stroke()
```

#### 绘制渐变

##### 线性渐变

绘制线性渐变时需要使用LinearGradient对象，通过2D上下文来创建和修改
1. 创建渐变对象
`var gradient = context.createLinearGradient(xstart,ystart,xend,yend)`
参数：起始点x坐标，起始点y坐标，结束点x坐标，结束点y坐标
2. 添加渐变颜色
  `gradient.addColorStop(offset,color);`
  参数：0-1之间的偏移量，颜色值
  由于是渐变，所以至少需要使用两次addColorStop方法以追加两个颜色(开始颜色和结束颜色)
3. 设置填充渐变
  `context.fillStyle = gradient;`
4. 使用渐变绘制矩形
  `context.fillRect(x,y,width,height);`

##### 径向渐变
径向渐变是指沿着圆形的半径方向向外进行扩展的渐变方式
`context.createRadialGradient(xstart,ystart,radiusStart,xend,yend,radiusend) `参数：
起始圆的圆心x坐标，y坐标，半径，结束圆的圆心x坐标，y坐标，半径
如果想从某个形状的中心点开始创建一个向外扩散的径向渐变的效果，就要将两个圆定义为同心圆

#### 绘制变形

绘制图形的时候，我们可能经常会想要旋转图形，或者对图形进行变形处理，使用Canvas API的坐标轴变换处理功能，可以实现这种效果。

##### 平移

`context.translate(x,y)`
参数：坐标原点在x轴方向移动的单位，坐标原点在y轴方向移动的单位

##### 扩大缩小

`context.scale(x,y)`
参数：水平方向的放大倍数，垂直方向放大的倍数
如果是缩小，将这两个参数设为0到1之间的数就可以

##### 旋转

`rotate(angle)`
参数：旋转的角度
旋转的中心点是坐标轴的原点，旋转是以顺时针方向进行的，要想逆时针旋转时，将angle设定为负数即可.

#### 绘制图像

在HTML5中，不仅可以使用Canvas API绘制图形，还可以读取磁盘或网络中的图像文件，然后使用Canvas API将该图像绘制在画布中。

##### 创建文件对象

```js
var image = new Image();
image.src = “images/a.jpg”;
```

##### 绘制图像

- `context.drawImage(image,x,y);`

  参数：图像对象，绘制位置的起始x坐标，起始y坐标

- `context.drawImage(image,x,y,w,h);`
  参数：图像对象，绘制位置的起始x坐标，起始y坐标，绘制图像的宽，高

##### 装载判断

当图像文件是一个来源于网络的比较大的图像文件时，用户就需要耐心等待图像全部装载完毕才能看到该图像，这种情况下可以使用以下方法来解决

```js
image.onload = function(){
//绘制图像的函数
}
```

##### 图像平铺

- `createPattern(image,type)`
  参数：image对象，平铺类型
  type的取值如下:
  no-repeat 不平铺
  repeat-x 横向平铺
  repeat-y 纵向平铺
  repeat  全方向平铺
  返回值：pattern对象，可以为fillStyle的值
  示例

```js
var pattern = createPattern(image,'repeat');
context.fillStyle = pattern;
context.fillRect(x,y,width,height);
```

##### 图像裁剪

Canvas API的图像裁剪功能是指，在画布内使用路径，只绘制该路径所包含区域内的图像，不绘制路径外部的图像。

- `context.clip();`
  这个方法会把Canvas的当前路径裁剪下来，一旦调用了clip方法之后，接下来向Canvas绘制图形时，之后clip()裁剪的路径覆盖的部分才会被显示出来。
  示例

```js
context.beginPath();
context.arc(200,150,80,0,Math.PI*2);
context.closePath();
context.clip();
var image = new Image();
image.src = "../images/1.jpg";
image.onload = function () {
context.drawImage(image,0,0,400,300);
}
```

#### 绘制文本

在HTML5中，可以在Canvas画布中进行文字的绘制，同时也可以指定绘制文字的 字体大小，对齐方式，文字文理填充等。

##### 2D 上下文文字修饰

- font 设置文字字体
- textAlign 文本对齐方式 start，end，left，right，center
- textBaseline 文本的基线，top，hanging，middle，alphabetic

##### 绘制填充或勾勒文本

- fillText(text,x,y,width) 使用fillStyle属性绘制填充文本

- strokeText(text,x,y,width) 使用strokeStyle属性向文本描边
  参数：

  - text  表示要绘制的文字
  - x,y  表示绘制文字的起点横坐标与纵坐标
  - maxWidth 表示显示文字时的最大宽度，防止文字溢出

  ```js
  context3.font = 'italic 30px sans-serif';
  context3.textBaseline = 'top';
  context3.textAlign = 'start';
  context3.fillText('hello world',150,60);
  ```







