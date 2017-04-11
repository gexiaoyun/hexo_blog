---
title: vue2.0组件使用记录
date: 2017-04-05 20:35:56
tags:
---
组件的官方介绍：
> 组件（Component）是 Vue.js 最强大的功能之一。组件可以扩展 HTML 元素，封装可重用的代码。在较高层面上，组件是自定义元素， Vue.js 的编译器为它添加特殊功能。在有些情况下，组件也可以是原生 HTML 元素的形式，以 js 特性扩展。

## 1、组件的编写 ##

组件编写其实就是把页面中的一块功能单独拿出来，比如登陆注册框，头部，底部和页面中的图片滚动之类，这些都可以单独拿出来写成一个组件。也就是说组件的编写跟页面正常编写一样，也就html+css+js。
<!--more-->

## 2、组件的引用 ##
组件的引用就官方案例来说：

1、首先在script标签中引入所要的组件

``` bash
import test from 'test'
```
 
2、其次在components中申明你所import的对象
 
``` bash
 components: {
    test 
  }
```
3、在页面中使用你所申明的标签
``` bash
  <test >...</test >
```
    
## 3、父传值给子 ##
父子之间的传值可以用vuex 也可以直接传值，vuex因为项目上暂时没用到就先不说，先记录下直接传值的方法。
父子组件间的传值可以通过props来传值，比如：
``` bash
  <test :val="111" :num="222" >...</test >
```
在这里的：val 和：num 就是你要传给子级的数据，在子级中你可以通过props来获取：
``` bash
  props: ['val','num'],
```
当然如果要传递特地的值过来，可以这样写：
``` bash
  props: {
    val:{  
        type: Object,
        default: function () {
          return { message: 'hello' }
        }
     },
     num:{  
        type: Number,
        default:'111'
     }
}
```

在这里，定义val传递的值为function，然后给val定义一个没数据传参过来的默认值，num定义为数字，并且默认值为'111'.
这个方法可以来编写可扩展的组件。

## 4、子传值给父 ##
在vue的由于把的传值方法改变了。在2中子级想传值给父级，我们只能触发父级传递过来的函数，再把值传递过去，比如再伏击中可以这样写 ：
``` bash
 <test v-on:test="testAlert" >...</test >

methods:{
 testAlert(index){
   alert(index)
 }
}
```

而在子级中，我们定义一个button：
``` bash
 <button type="button" value="测试触发" @click="trigger">

methods:{
 trigger(){
   this.$emit('test',1)   //这里的test为父级v-on后面的参数， 1为传值给父级的参数
 }
}
```
