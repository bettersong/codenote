### Vuex是什么？

- Vuex是一个专为Vue.js应用程序开发的状态管理模式
- 用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化
- Vuex背后的基本思想，借鉴了Flux，Redux
- Vuex也集成到Vue的官方调试工具devtools extension,提供了诸如零配置的time-travel调试，状态快照导入导出等高级调试功能

### 为什么要使用Vuex

- 在中大型应用中，应用的各个组件间需要进行数据的共享，使用传统方式繁琐且不可控
- vuex为所有组件提供集中式的可追踪的状态（数据）管理
- Vuex的状态是响应式的
- 使用Vuex将摒弃传统的event emmit和props的方式来进行数据的传递和更新

![1568644088336](D:\Users\Administrator\Desktop\briup\笔记\vue\images\vuex.png)

### Vuex核心概念

- state：驱动应用的数据源
- getters：为组件提供经过处理的数据源
- mutations：用来更改state的唯一标准方式
- actions：组件通过调用actions来提交mutation
- modules：每个module都有自己的state，getters，mutations，actions实现大型应用中业务模块数据的分治

vuex是为了保存组件之间共享数据诞生的，如果组件有要共享的数据，可以直接挂载到vuex中，而不必通过父子组件之间传值了，如果组件的数据不需要共享，此时，这些不需要共享的私有数据，没有必要放到vuex中；

只有共享的数据才有权利放到vuex中

组件内部的私有数据，只要放到组件的data中即可

#### props和data和vuex的区别：



#### vuex使用步骤

1.安装

```shell
npm install vuex --save
```

2.挂载，在一个模块化的打包系统中，必须显示地通过`Vue.use()`来挂载vuex

```js
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex)
```

如果是`script`标签引用Vuex时，则不需要手动挂载

3.在main.js中创建store实例，再将store挂载到vue实例上

```js
const store = new Vuex.Store({
    state: {
        //可以将state想象成组件中的data，专门用来存储数据的
        //如果在组件中，想要访问store中的数据，只能通过this.$store.state.xxx来访问
        count: 0
    },
    mutations: {
        //注意：如果操作store中的state值，只能通过调用mutations提供的方法，才能操作对应的数据，不推荐直接操作state中的数据，因为万亿导致了数据的紊乱，不能快速定位到错误的原因，因为每个组件都有可能有操作数据的方法
        
        increment(state){
            //注意：mutations方法中的函数最多只支持两个参数，第一个固定是state，第二个参数是commit提交过来的参数
            state.count++
        }
        //如果组件想要调用mutations中的方法，只能通过this.$store.commit('方法名')来调用
    },
    getters: {
        //注意：这里的getters，只负责对外提供数据，不负责修改数据，如果想要修改state中的数据，请去找mutations
        optCount: function(state){
            return '当前最新的count值是：' + state.count
        }
    }
})

const vm = new Vue({
    el: '#app',
    components: {
        App
    },
    template: '<App />',
    store: store
})
```

4.通过`store.state`来获取状态对象，以及通过`store.commit`方法触发状态变更

```js
store.commit('increment')
console.log(store.state.count)   //1
```

5.组件中使用store中的数据

```vue
//通过this.$store.属性名来访问

```

**总结**

- state中的数据，你不能直接修改，如果想要修改，必须通过mutations
- 如果组件想要直接从state上获取数据，需要通过`this.$store.state.xxxx`来获取
- 如果组件想要修改数据，必须使用mutations提供的方法，需要通过this.$store.commit('方法名',唯一的一个参数)
- 如果store中的state上的数据，在对外提供的时候需要做一层包装，那么推荐使用getters，如果需要使用getters，则用`this.$store.getters.xxx`





























































