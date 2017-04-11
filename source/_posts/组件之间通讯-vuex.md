---
title: 组件之间通讯-vuex(基础篇（粗略）)
date: 2017-04-10 21:53:25
tags:
---

今天在用vue做pc端网站的时候，卡在非父子之间通讯：导航组件--登陆组件之间的cookie获取值时，因为在导航到登陆组件之间已经有一个通讯事件，即点击登陆按钮弹出登陆框的事件。可是在登陆后cookie却不能实时获取，只能刷新后才获取，于是就萌生了由vuex来实现这功能的想法。

## 1、什么是vuex ##
vuex官方的介绍是

> uex 是一个专为 Vue.js 应用程序开发的状态管理模式。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。Vuex 也集成到 Vue 的官方调试工具 devtools extension，提供了诸如零配置的 time-travel 调试、状态快照导入导出等高级调试功能。

而在前端群中的说法是在你不知道这地方要不要用vuex的情况下就不要用vuex。因为vuex适用于中大型项目，按我的理解是在数据不是繁杂到你实在不得不想有个地方来统一管理的时候就不要用vuex来进行你项目的数据管理

<!--more-->

## 2、vuex的个人理解 ##
关于vuex的具体api可以去官网自己看看理解。这次重点记录下我自己今天终于弄懂的关于vuex的基础。

 1. store：是你所要管理数据的默认值
 2. mutations：是你要对数据所要做的操作，是更改 Vuex 的 store 中的状态的唯一方法（提交 mutations）
 3. actions：是分发器-相当于你要来触发mutations的开关
 4. getters：是你所要传递出去的数据（本人大概理解）

## 3、vuex的使用 ##
首先我们要在node中安装vuex
``` bash
npm install vuex --save-dev
```
其次在main.js中引用
```bash
import Vuex from 'vuex'
import store from './vuex/store'
Vue.use(Vuex)
```
store要注入到main的app中，以便让全局获取

然后在src中新建一个vuex文件夹在里面建一个store.js
```bash
const state = {
    count:0
}

const mutations = {
    increment(state){
        state.count = state.count + 1;
    }
}

const actions = {
    increment({commit}) => commit('increment');   
    //Action 函数接受一个与 store 实例具有相同方法和属性的 context 对象，因此你可以调用 context.commit 提交一个 mutation，或者通过 context.state 和 context.getters 来获取 state 和 getters。
}

const getters = {
    tode: state => {
        return state.count
    }
}

export default new Vuex.store({
    state,mutations,actions,getters
})
```

在上面代码中，我们默认给state一个count:0的值，再在mutations中来进行一个为count+1的操作，然后再actions中来触发mutations的操作，之后在getters中来给页面获取数据（获取数据的一个途经），最后在下面抛出让全局获取到

## 4、页面中获取数据 ##
基于上面的配置后，就要在页面中来获取在state中获取的数据。

```bash
    import { mapState,mapMutations,mapActions,mapGetters  } from 'vuex'
    
    computed: {
        // 使用对象展开运算符将 getters 混入 computed 对象中
    ...mapGetters([
      'doneTodosCount',
      'anotherGetter',
      // ...
    ]),
    ... mapState([
      // 映射 this.count 为 store.state.count
      'count'
    ])
  },
  methods: {
    ...mapMutations([
      'increment' // 映射 this.increment() 为 this.$store.commit('increment')
    ]),
    ...mapMutations({
      add: 'increment' // 映射 this.add() 为 this.$store.commit('increment')
    }),
    ...mapActions([
      'increment' // 映射 this.increment() 为 this.$store.dispatch('increment')
    ]),
    ...mapActions({
      add: 'increment' // 映射 this.add() 为 this.$store.dispatch('increment')
    })
  }
```
然后在vue的页面中你可以用
```bash
<p>{{$store.state.count}}</p>
```
来获取state中count的值，也可以在写了mapGetters或者mapState 之后直接来用或者来循环值以便达到更新数据的目的。

而在引入mapActions后，你可以在methods的任何函数中可以用
```bash
this.increment()  //触发定义的开关
```

这是在今天（2017年4月10日）写的实际应用中所接触到的基础知识，下次有空再补齐全点。。嘿嘿

## 5、参考文章 ##
简书：http://www.jianshu.com/p/13bec8f5b17d