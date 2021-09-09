#### 级联

级联(The cascade)，CSS是Cascading Style Sheet 的简写，说明级联是非常重要的。从表层来看，级联表明CSS规则的顺序问题，但是级联远比这个复杂，在所有的选择器中某个选择器定义的规则是否能够胜出（即优先级）取决于三个元素：Importance，Specificity，Source order

```html
css级联规则

                权重

style属性内 :    1000

id选择器：    100

class选择器，伪类选择器，属性选择器： 10

标签选择器，伪元素选择器：  1

空格 + > ~: 0
 /*css级联，先判断特性值，如果特性值（权重）一样，就近原则去判断，使用！important修饰的最优先*/
```

#### 继承

属性值可以取值为

 inherit   继承

 initial   默认样式，不继承的

 unset   不设置，没有操作



#### 块级元素与行内元素转换 

    ##### display属性

块级元素独占一行，可设置宽高。

行内元素，与其他元素共享一行，不可设置宽高

```html
display: inline-block
将其他元素转换成行内元素同时可设置宽高的元素
display: inline
将其他元素转换为行内元素
display: block
将其他元素转换成为块级元素
```



​                 

