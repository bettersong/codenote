### Babel

#### 命令行转码babel-cli

​	全局环境下进行Babel转码。这意味着，如果项目要运行，全局环境必须有Babel，也就是说项目产生了对环境的依赖

##### 安装

```shell
npm install --global babel-cli
```

##### 安装预设并且添加配置文件配置.babelrc

在当前项目的根目录下创建该文件并配置以下代码

```js
{
    "presets": ["latest"]     //定义转义规则
}
```

##### 使用

转码结果输出到标准输出

`babel example.js`

转码结果写入一个文件，--out-file或-o参数指定输出文件

`babel example.js --out-file complied.js`

整个目录转码，--out-dir或-d参数指定输出目录

`babel src --out-dir lib`

#### 配置文件

​		Babel的配置文件是`.babelrc`,存放在项目的根目录下。使用Babel的第一步，就是配置这个文件。该文件用来设置转码规则和插件，基本格式如下。

```js
{"presets": [],"plugins":[]}
```

- presets字段设定转码规则，官方提供以下的规则集，可以按需安装

  - ES2015转码规则

    `npm install --save-dev babel-preset-es2015`    =>es2015

  - 最新转码规则

    `npm install --save-dev babel-preset-latest`     =>latest

  - 不会过时的转码规则

    `npm install --save-dev babel-preset-env`   		 =>env

- 然后将这些规则加入`.babelrc`

  ```js
  {"presets": ["es2015"], "plugins": [], "ignore": []}
  ```

#### ES6转ES5

1.全局安装babel-cli

```shell
npm install -g babel-cli
babel --version     //查看版本号
```

2.局部安装babel-preset-latest,安装到当前目录底下

```shell
npm install babel-preset-latest
```

3.项目根目录下创建一个配置文件.babelrc并进行配置

```js
{"presets": ["latest"]}
```

4.开始转换

babel a.js  将转换后的代码输出到终端
babel a.js --out-file b.js 将转换后的代码输出到b.js文件中
babel src --out-dir dist 将src目录中的所有的文件转换成ES5的代码，输出到dist目录中。文件名一致

#### ES6转ES5升级版

1.将当前目录初始化，成为一个Node项目， 在项目中有package.json文件，文件中可以声明该项目需要的包-模块。其他人拿到项目之后，使用cnpm install ,可以直接下载需要的依赖
`cnpm init -y 快速初始化一个项目`
2.在项目开发阶段安装babel-cli
`cnpm install --save-dev babel-cli`
3.在项目开发阶段安装babel-preset-latest
`cnpm install --save-dev babel-preset-latest`
--save-dev是在开发阶段依赖的包
--save 是在打包之后依然依赖的包，
将安装的记录，需要的依赖，记录到package.json文件中
4.配置文件.babelrc
`{'presets':['latest']}`
5.在package.json中编写脚本，执行转换
`'build':'babel src --out-dir dist'`
6.执行脚本
`cnpm run build`

#### babel-polyfill

Babel  默认只转换新的 JavaScript  句法（ syntax ），而不转换新的 API ，比如
Iterator 、 Generator 、 Set 、 Maps 、 Proxy 、 Reflect 、 Symbol 、 Promise 等全局对象，
以及一些定义在全局对象上的方法（比如 Object.assign ）都不会转码。举例来说， ES6
在 Array 对象上新增了 Array.from 方法。 Babel  就不会转码这个方法。如果想让这个方法
运行，必须使用 babel-polyfill ，为当前环境提供一个垫片

- 安装
  `$ npm install --save babel-polyfill`
- 在 js 文件中引用并且使用
  `import 'babel-polyfill'; // 或者 require('babel-polyfill');`