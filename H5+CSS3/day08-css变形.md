#### 变形 （ transform）

向元素应用2D或3D转换

现实生活中的绝大多数的动作都可以理解为变形后的结果。

![1561102918848](D:\Users\Administrator\Desktop\briup\笔记\H5+CSS3\images\1561102918848.png)

#### 语法

```css
div {
transform: xxx();
transform-origin:center;
}
```

  transform 用来指定一个变形函数对元素执行变形操作
  **transform-origin** 用于指定 一个元素变形的原点
ü 如果指定一个值
left / center / right / top / bottom
ü 如果指定两个值
其中一个必须是length,percentage，或left, center, right关键字中的一个。另一个必须是length,percentage，或top, center, bottom关键字中的一个。
ü 如果指定三个值
前两个值和只有两个值时的用法相同，第三个值必须是length。它始终代表Z轴偏移量

####  旋转 （ rotate ）
该函数定义了一种将元素围绕一个定点旋转而不变形的转换。指定的角度定义了
旋转的量度。若角度为正，则顺时针方向旋转，否则逆时针方向旋转。
**rotateX (angle)**
绕X轴旋转，例如单杠运动
**rotateY ( angle )**
绕Y轴旋转，例如钢管舞运动
**rotateZ ( angle )**
绕Z轴旋转，例如旋转盘。
**rotate ( angle )**
与rotateZ()一致。

####  倾斜 （skew）
skew(ax,ay) 函数表示对元素的剪切或者扭转，ax表示水平方向的扭转，ay表示垂直方向的扭转，也可以使用**skewX(ax)** 和**skewY(ay)**

#### 缩放 （scale）

scale() 函数能够更改元素的大小，scale变换的是通过矢量来实现，它的坐标定义了每个方向上的缩放比例。如果两个向量的坐标是相等的，缩放是均匀的

#### 移动 （translate）
translate(tx, ty) 函数能够移动元素。tx为元素在水平方向上移动的距离，ty为元素在垂直方向上移动的距离。

````css
div{
           width:100px;
           height:100px;
           display: inline-block;
           margin:10px;
           border-radius: 20px;
           background-color: cyan;
           animation: myani 4s infinite;
           animation-direction: alternate;
       }
       @keyframes myani{
           0%{
               transform: translate(0) rotateZ(0deg);
               
           }
           100%{
               transform: translate(600px) rotateZ(360deg);
               background-color: pink;
           }
       }
````

#### 矩阵（matrix）

语法：

matrix（a,b,c,d,e,f）