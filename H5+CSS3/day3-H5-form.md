#### progress

**progress**表示任务的完成情况，常用于进度条

-  max 定义进度元素所要求的任务的工作量，默认值为1
-  value  定义已经完成的工作量，如果max值为1，该值必须是介于0~1之间的小数。

#### output

   **output** 表示用户动作产生的结果

- name：定义元素的名称
- for：其他元素的id列表，表明这些元素为计算提供了输入值(或其他影响) 。

#### meter
meter元素表示规定范围内的数量值。例如：磁盘使用量，某个候选者的投票人数占总投票人数的比例等

-  value :在元素中特地表示出来的实际值，该值在min与max之间，如果未指定，该值默认为1
-  min :指定规定范围时允许使用的最小值，默认为0
-  max :指定规定范围时允许使用的最大值，默认为1
-  low :规定范围的下限值必须小于或等于high属性的值
-  high :规定范围的上限值（表示较高危险的意思）
-  optimum :最佳值例如：

<p>He got a <meter low="69" high="80" max="100" value="84">B</meter> on
the exam.</p>



#### datalist

datalist表示其他控件可用的值，其值通过<option>作为datalist的子元素存在

````html
<label>
    <input list="brow" name="mybrow"/>
</label>
<datalist id="brow">
     <option value="tom">
     <option value="jack">
     <option value="lucy">
</datalist>
````

#### type

在H5中，对input的type进行了扩展，可以有更多的取值

-  date 日期控件（年，月，日，不包含时间）

-  datetime-local 日期时间控件

-  time 时间控件

-  month 日期插件（年，月）

-  week 日期插件（年，周）

-  number 数字控件（只能输入数字）

-  range 范围控件（通过控制条可以调整取值）

-  search 搜索控件，

-  tel 电话控件

-  url 地址控件

-  color 颜色控件

-  email email控件

  **以上只能被chrome,opera支持**



#### 新增表单属性

**form**
在H5中，可以将表单内的从属元素书写在页面上的任何地方，然后为该元素指定一个form属性，属性值为该表单的id。
**formaction**
一般用于提交按钮和图片按钮上，用于指定处理表单提交的后台程序，可以重写form中的action属性。
**formenctype**
一般用于提交按钮和图片按钮上，用于指定处理表单的内容类型。
**formmethod**
一般用于提交按钮和图片按钮上，用于指定表单的提交方式。
**formnovalidate**
一般用于提交按钮和图片按钮上，布尔类型，提交时表单不被验证。
**formtarget**
一般用于提交按钮和图片按钮上，用于指定表单提交后在哪里显示响应页面。

**autofocus**
当页面加载完毕的时候，默认聚焦。在页面中，只能有一个表单元素具有该属性，值为boolean类型，
**list**
取值为<datalist>元素的id，用于显示提示内容。
**max/min**
表单组件能够接受到的最大值/最小值。
**placeholder**
对用户的输入进行提示，常用于搜索框，不要出现回车换行。