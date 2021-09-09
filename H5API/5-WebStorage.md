### web Storage的两个目标

- 提供一种在cookie之外存储会话数据的途径
- 提供一种存储大量可以跨会话存在的数据机制

### web storage种类

#### sessionStorage对象

将数据保存在session对象中。session是指用户在浏览某个网站时，从进入网站到浏览器关闭所经过的这段时间，也就是用户浏览这个网站所花费的时间。session对象可以用来保存在这段时间内所要保存的任何数据。(仅限当前选项卡)。

#### localStorage对象

将数据保存在客户端本地的硬盘设备中，即使浏览器被关闭了，该数据仍然存在，下次打开浏览器访问网站时仍然可以继续使用。

#### storage对象提供的方法

- setItem(name,value) 为指定的name设置一个对应的值
- getItem(name) 根据指定的名字name获取对应的值
- removeItem(name) 删除由name指定的键值对
- clear()  删除所有值
- key(index) 获得index位置处的值的名字
- length Storage对象中，键值对的数量

#### window对象提供的storage事件

当在【其他页面】中修改 sessionStorage 或者 localStorage 中的值时要执行的处理，可以使用 window 对象提供的 storage 事件来监听。

- event.key 被修改的数据键值
- event.oldValue 被修改前的值
- event.newValue 被修改后的值
- event.url 页面的URL地址
- event.storageArea 为变动的sessionStorage对象或localStorage对象