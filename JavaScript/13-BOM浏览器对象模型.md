### 浏览器对象模型

#### 间歇调用和超时调用

JavaScript是单线程语言，但是可以通过超时值和间歇时间来调度代码在特定时刻执行

- **setTimeout()**

  该方法返回一个数值ID，表示超时调用，这个超时调用ID是计划执行代码的唯一标识符通过它来取消超时调用。可以通过clearTimeout(ID);
  参数：
  1.要执行的代码
  2.以毫秒表示的时间

  ```js
  var id = setTimeout(function(){
  alert(1000);
  },1000);
  console.log(id);
  clearTimeout(id) //清 除
  ```

- **setInterval()**

  按照指定的时间间隔重复执行代码，直到间歇调用被取消或页面被卸载。调用该
  方法也会返回一个间歇调用ID，该ID可以让用户在将来某个时刻取消间歇调用
  参数：
  1.要执行的代码
  2.以毫秒表示的时间
  clearInterval(ID); //取消间歇调用

#### 系统对话框

alert(),confirm(),prompt()方法可以调用系统对话框向用户显示消息。显示这些对话框的时候代码会停止执行，关掉这些对话框后代码又会恢复执行。

- alert() 该方法接受一个字符串并将其显示给用户。该对话框会包含指定的文本和一个"OK"按钮。主要用来显示警告信息
- confirm() 确认对话框，显示包含指定的文本和一个"OK"按钮以及"Cancel"按钮。该方法返回布尔值，true表示单击了OK，false表示单击cancel或者关闭按钮
- prompt()  会话框，提示用户输入一些文本。显示包含文本，ok按钮,cancel按钮以及一个文本输入域，以供用户在其中输入内容。传入两个参数，要显示给用户的文本提示和文本输入域的默认值。如果用户单击OK按钮，该方法返回输入域的值，如果用户单击了Cancel或者关闭对话框该方法返回null

#### location对象

是最有用的BOM对象之一，提供了与当前窗口中加载的文档有关的信息，还提供一
些导航功能。location是个神奇的对象，既是window的对象也是document的对象。

```js
console.log(window.location == document.location);//true
```

##### 属性

- host 返回服务器名称和端口号
- hostname 返回不带端口号的服务器名称
- href 返回当前加载页面的完整URL
- pathname 返回URL的目录和文件名
- port 返回URL中指定的端口号
- protocol 返回页面使用的协议
- search 返回URL的查询字符串。这个字符串以问号开头

##### 方法：
- assign() 传递一个url参数，打开新url，并在浏览记录中生成一条记录。
- replace()  参数为一个url,结果会导致浏览器位置改变，但不会在历史记录中生成新记录
- reload() 重新加载当前显示的页面，参数可以为boolean类型，默认为false，表示以最有效方式重新加载，可能从缓存中直接加载。如果参数为true，强制从服务器中重新加载为location.href; window.location 设置为一个URL值，也会以该值调用assign()方法。以下三句话效果一样
  window.location="http://www.baidu.com";
  location.href="http://www.baidu.com"
  location.assign("http://www.baidu.com") ;

#### history对象

该对象保存着用户上网的历史记录。出于安全方面的考虑，开发人员无法得知用户浏览过的URL，不过借由用户访问过的页面列表，同样可以在不知道实际URL的情况下实现后退前进,注意： 没有应用于History对象的公开标准，不过所有浏览器都支持该对象。

- length  返回历史列表中的网址数
  注意：IE和Opera从0开始，而Firefox、Chrome和Safari从1开始。
- back()  加载 history 列表中的前一个 URL
- forward()  加载 history 列表中的下一个 URL
- go()  加载 history 列表中的某个具体页面
  负数表示向后跳转，正数表示向前跳转