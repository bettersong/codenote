#### babel 相关概念

- @babel/core：babel的核心，核心的API都包含在这里。
- @babel/cli：命令行工具，通过命令对js文件进行转换的工具。
- @babel/perset-env：指定转换的工作环境
- @babel/polyfill：相当于一个填充，因为babel本身只支持转换箭头函数，解构赋值这些语法糖类的语法，而一些新的API或者promise函数是无法转换的。@/babel/polyfill就是解决这个问题的。
- Babel-loader：webpack的加载器，用于调用@babel/core的核心API来完成编译

#### babel的处理流程

![img](https://user-gold-cdn.xitu.io/2019/10/2/16d8d0cd559c7e1e?imageslim)

#### babel的架构

##### 核心

`@babel/core`这也是上面说的微内核架构中的内核，没有它，在babel的世界里注定寸步难行。不安装`@babel/core`,无法使用babel进行编译。对于babel来说，这个内核主要干这些事情：

- 加载和处理配置config
- 加载插件
- 调用`perser`进行语法解析，生成AST
- 调用`traverser`遍历AST，并使用访问者模式应用插件对AST进行转换
- 生成代码，包括SourceMap转换和源代码生成

##### 核心周边支撑

- **Parser(`@babel/parser`)**： 将源代码解析为 AST 就靠它了。 它已经内置支持很多语法. 例如 JSX、Typescript、Flow、以及最新的ECMAScript规范。目前为了执行效率，parser是[不支持扩展的](https://babeljs.io/docs/en/babel-parser#faq)，由官方进行维护。如果你要支持自定义语法，可以 fork 它，不过这种场景非常少。
- **Traverser(`@babel/traverse`)**：  实现了`访问者模式`，对 AST 进行遍历，`转换插件`会通过它获取感兴趣的AST节点，对节点继续操作, 下文会详细介绍`访问器模式`。
- **Generator(`@babel/generator`)**： 将 AST 转换为源代码，支持 SourceMap

##### CLI命令行工具 @babel/cli

babel提供的命令行工具，主要是提供babel这个命令，适合安装在项目里。

`@babel/node`提供了`babel-node`命令，但是`@babel/node`更适合全局安装，不适合安装在项目里。

```shell
npm i @babel/core @babel/cli -S
```

将命令配置在`package.json`文件的script字段中：

```js
"script": {
  "compiler": "babel src --out-dir lib --watch"
}
```

使用`npm run compiler`来执行编译，现在我们没有配置任何插件，编译前后的代码是完全一样的。

因为babel虽然开箱即用，但什么动作也不做，如果想要babel做一些实际的工作，就需要为其添加插件

##### 插件

打开babel的源代码，会发现有好几种类型的插件

**语法插件(`@babel/plugin-syntax-\*`)**：上面说了 `@babel/parser` 已经支持了很多 JavaScript 语法特性，Parser也不支持扩展. **因此`plugin-syntax-\*`实际上只是用于开启或者配置Parser的某个功能特性**。

一般用户不需要关心这个，Transform 插件里面已经包含了相关的`plugin-syntax-*`插件了。用户也可以通过[`parserOpts`](https://babeljs.io/docs/en/options#parseropts)配置项来直接配置 Parser

**转换插件**： 用于对 AST 进行转换, 实现转换为ES5代码、压缩、功能增强等目的. Babel仓库将转换插件划分为两种(只是命名上的区别)：

- `@babel/plugin-transform-*`： 普通的转换插件
- `@babel/plugin-proposal-*`： 还在'提议阶段'(非正式)的语言特性, 目前有[这些](https://babeljs.io/docs/en/next/plugins#experimental)

**预定义集合(`@babel/presets-\*`)**： 插件集合或者分组，主要方便用户对插件进行管理和使用。比如`preset-env`含括所有的标准的最新特性; 再比如`preset-react`含括所有react相关的插件.

#### @babel/preset-env加强

`@babel/preset-env`主要作用是对我们使用的并且目标浏览器中缺失的功能进行代码转换和加载`polyfill`，在不进行任何配置的情况下，`@babel/preset-env`所包含的插件将支持所有最新的JS特性（ES2015，ES2016等，不包含stage阶段），将其转换成ES5代码。

需要说明的是，`@babel/preset-env`会根据你配置的目标环境，生成插件列表来编译。对于基于浏览器或`Electron`的项目，官方推荐使用`.browerslistrc`文件来指定目标环境。默认情况下，如果你没有在babel配置文件中设置`targets`或`ignoreBrowserlistConfig`,`@babel/preset-env`会使用`browserlist`配置源。

如果你不是要兼容所有的浏览器和环境，推荐你指定目标环境，这样你的编译代码能够保证最小

例如：

仅包括浏览器市场份额超过0.25%的用户所需的`polyfill`和代码转换（忽略没有安全更新的浏览器，如IE10和BlackBerry）

```js
// .browserslistrc
> 0.25%
not dead
```

最新的两个谷歌版本不用编译成ES5，因为它支持ES6语法

```js
// .browserslistrc
last 2 Chorme version
```

上面的引入方法是完全引入，导致包非常大，我们可以按需引入，这里又要配置@babel/preset-env,修改`babel.config.js`,usage参数可以做到按需引入，它只会引入相关的包。有一点需要注意：配置此参数的值为`usage`时，必须要同时设置`corejs`(如果不设置，会给出警告，默认使用的是"corejs":2),注意：这里仍然需要安装`@babel/polyfill`(当前`@babel/polyfill`版本默认会安装"corejs":2)

首先说一下使用 `core-js@3` 的原因，`core-js@2` 分支中已经不会再添加新特性，新特性都会添加到 `core-js@3`。例如你使用了 `Array.prototype.flat()`，如果你使用的是 `core-js@2`，那么其不包含此新特性。为了可以使用更多的新特性，建议使用 `core-js@3`。

**安装依赖**

```shell
npm install core-js@3 -S
```

```js
module.exports = {
  presets: [
    ["@babel/preset-env",
     {
      "targets": "ie>=8",
      "useBuiltIns": "usage",
       "corejs": 3
     }
    ]
  ]
}
```

##### useBuiltIns参数说明：

- false：不对polyfills做任何操作
- entry：根据target中浏览器版本的支持，将polyfills拆分引入，仅引入有浏览器不支持的polyfill
- usage(新)：检测代码中ES6/7/8等的使用情况，仅仅加载代码中用到的polyfills

#### @babel/polyfill(垫片)

默认babel智能转换一般的语法糖，不能转换新的API，所以就只能用polyfill来弥补

`babel/polyfill`模块包括`core-js`和一个自定义的`regenerator runtime`模块，可以模拟完整的ES2015+环境（不包含第四阶段前的提议）。

这意味着可以使用诸如`Promise`和`WeakMap`之类的新的内置组件，`Array.from`或`Object.assign`之类的静态方法，`Array.prototype.includes`之类的实例方法以及生成器函数（前提是使用了`@babel/plugin-transform-regenerator`插件）。为了添加这些功能，`polyfill`将添加到全局范围和类似string这样的内置原型中（会对全局环境造成污染）

```shell
npm i @babel/polyfill
```

注意：不使用`--save-dev`，因为这是一个需要再远吗之前运行的垫片。

#### @babel/plugin-transform-runtime

`@babel/plugin-transform-runtime`是一个可以重复使用`babel`注入的帮助程序，以节省代码大小的插件。

注意：诸如Array.prototype.flat()等实例方法将不起作用，因为这需要修改现有的内置函数（可以使用`@babel/polyfill`来解决这个问题）

另外，`@babel/plugin-transform-runtime` 需要和 `@babel/runtime` 配合使用。

首先安装依赖，`@babel/plugin-transform-runtime` 通常仅在开发时使用，但是运行时最终代码需要依赖 `@babel/runtime`，所以 `@babel/runtime` 必须要作为生产依赖被安装，如下 :

```shell
npm i @babel/plugin-transform-runtime -D
npm i @babel/runtime -S
```

除了前文所说的，`@babel/plugin-transform-runtime` 可以减少编译后代码的体积外，我们使用它还有一个好处，它可以为代码创建一个沙盒环境，如果使用 `@babel/polyfill` 及其提供的内置程序（例如 `Promise` ，`Set` 和 `Map` ），则它们将污染全局范围。虽然这对于应用程序或命令行工具可能是可以的，但是如果你的代码是要发布供他人使用的库，或者无法完全控制代码运行的环境，则将成为一个问题。

`@babel/plugin-transform-runtime` 会将这些内置别名作为 `core-js` 的别名，因此您可以无缝使用它们，而无需 `polyfill`。

#### 预设

通过使用或创建一个`presets`即可轻松使用一组插件

- @babel/preset-env 
- @babel/preset-flow
- @babel/preset-react
- @babel/preset-typescript

#### 插件/预设补充知识

插件的排列顺序很重要

如果两个转换插件都将处理程序的某个代码片段，则将根据转换插件或`presets`的排列顺序依次执行。

- 插件在Presets前运行
- 插件顺序从前往后排列
- Presets顺序是颠倒的（从后往前）



#### babel7的一些变化

##### preset的变更

淘汰es201x，删除stage-x，推荐env

官方建议使用env替换，淘汰并不是删除，只是不推荐使用

但stage-x是直接被删除了，也就是说在babel7中使用stage-x是会报错的

##### 包名称变化

babel7的一个重大变化，把所有`babel-*`重命名为`@babel/*`,

例如：

- babel-cli  -----> @babel/cli
- babel-preset-env   ------> @babel/preset-env

##### 低版本node不再支持

Babel7.0开始不再支持node0.10，0.12，4，5这四个版本，相当于要求node >=6























