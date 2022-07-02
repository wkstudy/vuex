## 记录
基于vuex3, 分支3.x

### Question
~~1. 没看懂为啥要做context？~~

store为整个store实例，context是当前module

~~2. 为啥vuex要做成响应式的？~~

因为响应式的数据才能和Vue之前建立发布-订阅的关系，数据变了才能引起页面的重新渲染

### 笔记


在vue中使用vuex3.0一般方法是：
```
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const store = new Vuex.store(
  state: {
    ...
  },
  modules: {
    ...
  }
)

new Vue({
  store,
})

```
## `Vue.use(Vuex)`
调用`install`方法,主要是将下方的`store`赋值给`this.$store`,让用户可以直接访问`store`
```
 this.$store = typeof options.store === 'function' ? options.store() : options.store
```
### `new Vuex.store`
主要流程是下面三行
1. `this._modules = new ModuleCollection(options)`
把整个store进行初始化，放在store._modules上，各个module都变成了一个Module类，通过_children属性记录module的层级
2. `installModule(this, state, [], this._modules.root)`
将各个层级的module(state、get、commit、action)都放到根目录上(state、_wrappedGetters、_mutations、_actions)，因为整个store为对象，key不能重复，所以各个module的key都会做一次处理，比如从 `a` 变为`${namespace}/a`
3. `resetStoreVM(this, state)` 
把state变为响应式数据，（就是new Vue()了一下）



