#### React中创建组件✨

##### 第一中创建组件的方式

使用构造函数来创建组件，如果要接收外界传递的数据，需要在构造函数的参数列表中使用`props`来接收；必须要向外return一个合法的JSX创建的虚拟DOM;

- **创建组件：**

  ```js
  // 第一中创建组件的方式
  function Hello(){
      //return null   //如果在一个组件中return一个null，则表示此组件什么度不会渲染
      return <div>我是Hello组件</div>
  }
  ```

- 为组件传递参数

  ```jsx
  // 在构造函数中使用props形参来接收外界传递过来的参数
  //组件的首字母必须大写
  function Hello(props){
      //return null   //如果在一个组件中return一个null，则表示此组件什么度不会渲染
      // 在组件中必须返回一个合法的JSX虚拟DOM元素
      console.log(props.name)
      // 结论：不论是vue还是react组件中的props永远都是只读的，不能被重新赋值
      return <div>我是Hello组件</div>
  }
  const user = {
      name:'张三'
      age:18,
      sex:'男'
  }
  //传递参数
  ReactDOM.render(<div>
                  <Hello name={user.name}></Hello>
                  </div>,document.getElementById('app'))
  // 多个参数可以直接用es6语法...对象，展开运算符
  ReactDOM.render(<div>
      123
      <Hello {...user}></Hello>
      </div>,document.getElementById('app'))
  ```

  1.父组件向子组件传递数据

  2.使用{...obj}属性扩散传递数据

  3.注意：组件的名称首字母必须是大写

  5.如何省略`.jsx`后缀名：

  ```js
  //打开webpack.config.js,并在导出的配置对象中新增如下节点
  resolve:{
          extensions:['.js','.jsx','.json']   //表示这几个后缀可以省略
      } 
  
  ```

  6.配置src别名

  ```js
  //打开webpack.config.js,并在resolve对象中新增如下节点,就是第五点的下方
  alias:{
              '@': path.join(__dirname,'./src')   //这样@就表示项目根目录中src这一层路径
          }
  ```

##### 第二种创建组件的方式

使用class关键字创建组件

````jsx
//如果要使用class定义组件，必须让自己的组件继承自React。Component
class 组件名称 extends React.Component{
    //在组件内部必须有render函数，render函数中必须返回合法的虚拟DOM
    render(){
        return <div>这是class创建的组件</div>
    }
}
````

##### 两种创建组件方式的区别

注意：使用class关键字创建的组件有自己的私有数据`this.state`和生命周期，使用function关键字创建出来的组件，只有props，没有自己的私有数据和生命周期

1.用构造函数创建出来的组件：叫做“无状态组件”[无状态组件用的不多]

2.用class关键字创建出来的组件：叫做“有状态组件”

3.什么情况下使用有状态组件？什么情况下使用无状态组件？

    - 如果一个组件需要有自己的私有数据，则推荐使用：class创建的有状态组件；
    - 如果一个组件不需要有自己的私有数据，则推荐使用：function创建的无状态组件
    - React官方说：无状态组件由于没有自己的state和生命周期函数，所以运行效率会比较高一些

**有状态组件和无状态组件之间的本质区别就是：有无state属性和有无生命周期函数**

4.组件中的`props`和`state/data`之间的区别

- props中的数据都是外界传递过来的
- state、data中的数据，都市组件私有的；（通过Ajax获取回来的数据，一般是私有数据）
- props中的数据都是只读的，不能重新赋值；
- state/data中的数据，都是可读可写的；

##### 样式

![1562771158360](D:\Users\Administrator\Desktop\ReactStudy\笔记\images\1562771158360.png)



###### 使用外部样式表

安装相应的loader用来打包处理css样式文件`npm install style-loader css-loader -D`

然后在webpack.config.js中配置

````js
module: { //所有第三方模块匹配规则
        rules:[//第三方匹配规则
          {test: /\.js|jsx$/,use: 'babel-loader',exclude: /node_modules/}, //千万别忘记添加exclude排除项
          {test: /\.css$/,use:['style-loader','css-loader']}//打包处理css样式文件 
        ]
    },
````

在React中直接导入的css样式表，默认实在全局上生效的

Vue组件中的样式表也有冲突的问题，但是可以使用<style scoped></style>来解决

// React中没有类似scoped的指令，因为React中没有指令的概念

**解决办法：**

1.webpack.config.js配置为css文件启用模块化

````js
{test: /\.css$/,use:['style-loader','css-loader?modules']}//打包处理css样式文件
        //   可以在css-loader之后通过?追加参数，其中有个固定的参数，叫做modules,表示启用模块化
````

2.然后在组件中这样使用：

````jsx
import cssObj from '../css/main.css'   //导入外部样式表并接受，这里CSSObj接受的是一个对象

<p className={cssObj.title}>style-loader,css-loader</p>
````

3.使用`localIdentName`自定义生成的类名格式，可选的参数有：

- [path]  表示样式表`相对于项目根目录`所在路径
- [name]  表示样式表文件名称
- [local]  表示样式的类名定义名称
- [hash:length]  表示32位的哈希值
- 例子：`{test: /\.css$/,use: ['style-loader','css-loader?modules&localIdentName=[path][name]-[local]-[hash:5]']}`
- 使用`:local()`和`:global`
  - `:local()`包裹的类名，是被模块化的类名，只能通过`className-{cssObj.类名}`来使用，同时，`:local()`默认可以不写，这样，默认在样式表中定义的类名，都是被模块化的类名
  - `:global()`包裹的类名，是全局生效的，不会被`css-modules`控制，定义的类名是什么，就是使用定义的类名`className="类名"`

