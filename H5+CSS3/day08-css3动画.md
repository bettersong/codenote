#### 动画

一帧一帧的东西组成一个动画序列

先声明动画

```css
div{
            width:100px;
            height:100px;
            background-color:pink;
            animation:myani 3s infinite ease;
            /* animation-duration: initial; */
            /* 动画的执行顺序  reverse反转 默认normal*/
            /* animation-direction: reverse; */
            /* 奇数次执行，从第一帧到最后一帧，偶数次执行，从最后一帧到第一帧 */
            /* animation-direction: alternate; */
           /* 奇数次执行，从最后一帧到第一帧，偶数次执行，从第一帧到最后一帧 */
            /* animation-direction: alternate-reverse; */
            /* 动画延迟执行 */
            /* animation-delay: 2s; */
            /* 填充模式 */
            /* 在动画等待时间内，降准备播放的动画的样式填充到元素上 */
            /* animation-fill-mode: backwards; */
            /* 在动画结束时，使用结束的帧填充元素，默认是不填充的 */
            /* animation-fill-mode: forwards; */
            /* 速度曲线  动画在执行过程中的变化的速率 */
            /* animation-timing-function: ease; */
            /* animation-timing-function:ease-in; */
            animation-timing-function:ease-out;
        }
        div:hover{
            /* 停止动画 running运行，paused暂停*/
            animation-play-state: paused;
        }
        /* 声明动画 */
        @keyframes myani{
            from{
                width:100px;
            }
            to{
                width:300px;
                background-color: cyan;
            }
        }
```

#### 配置动画

  #####    动画持续时间（animation-duration）

  动画完成一个循环所需的时间长度。
  单位s 、ms ,默认值为0s 。

#####   动画迭代次数（ animation-iteration-count ）

   动画重复的次数。
 **<number>**
默认值为1，不能为负数，可以为小数，比如0.5表示播放一半
 **infinite**
无限循环。

#####  动画名字（ animation-name ）
可以与@keyframes中定义的名字绑定。

##### 动画延迟（ animation-delay ）
动画延迟，即元素在加载成功后到动画运行前的时间。
单位为s 、ms ,默认值为0s，即动画加载成功后立刻运行。
例如：
animation-delay: 3s;

##### 动画方向（ animation-direction ）
动画运行完是否交替方向或者是重置起点。

-  normal
  默认值，正常播放
-  reverse
  播放帧的顺序反转，播放的起点为帧的结束
- alternate
  播放帧的顺序交替反转，即第一次播放从帧的开始播放到帧的结束，第二次播放就从帧的结束播放到帧的开始，与此同时，速度曲线也交替反转。反转时ease-in交替为ease-out
- alternate-reverse
  与alternate类似，不过第一次播放时候需要反转。

##### 暂停/恢复（ animation-play-state ）

动画的状态，可以通过Js查询该属性以确定该动画目前是运行还是停止，或者可以通过
Js来设定动画的运行状态。
**running**
运行状态
**paused**
暂停状态

##### 填充模式（ animation-fill-mode ）
指定了CSS动画在执行前和执行后如何将样式应用到它的目标上。

- none 默认值
- forwards 目标将保留在执行期间所遇到的最后一个关键帧所设置的计算值
- backwards目标将保留在执行期间所遇到的第一个关键帧所设置的计算值

#####  动画的速度曲线（ animation-timing-function ）
- ease 默认值
- ease-in  先慢后快
- ease-out 先快后慢
- ease-in-out 先慢后快再慢
- linear 线性增长

#### 动画过渡（transition）

CSS transitions 提供了一种在更改CSS属性时控制动画速度的方法。 其可以让属性变化成为一个持续一段时间的过程，而不是立即生效的。比如，将一个元素的颜色从白色改为黑色，通常这个改变是立即生效的，使用 CSS transitions 后该元素的颜色将逐渐从白色变为黑色，按照一定的曲线速率变化。这个过程可以自定义。CSS transitions 可以决定哪些属性发生动画效果 (明确地列出这些属性)，何时开始(设置 delay），持续多久 (设置 duration) 以及如何动画 (定义timing funtion，比如匀速地或先快后慢)。

##### 语法

````html
div {
transition: <property> <duration> <timing-function> <delay>;
}
````

transition 是 transition-property， transition-duration，transition-duration，transition-delay的速写形式，分别表示过渡属性，持续时间，时间曲线，过渡延迟

#### 可以动画的 CSS 属性
一些CSS属性可以是动画的，也就是说，当它的值改变时，或者当 CSS动画或 CSS转换使用时，它可以以平滑的方式改变。

![1561118520788](D:\Users\Administrator\Desktop\briup\笔记\H5+CSS3\images\1561118520788.png)

