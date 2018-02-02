# vue.cli中怎么使用自定义组件

在components  目录新建组件，
在需要使用的页面导入
注入到vue的子组件的component属性上面
在template视图view中使用



# 对Vue.js 的template编译的理解

compile编译器把template编译成AST语法树（abstract syntax tree 即 源代码的抽象语法结构的树状表现形式）
（compile是createCompile的返回值，createCompile是用以创建编译器的，另外compile还负责合并option）

然后，AST会经过generate得到render函数，render的返回值是VNode，
Vnode是Vue的虚拟DOM节点


# Vue.cli项目中src目录每个文件夹和文件的用法
assets文件夹是放静态资源，component是放组件，router是定义路由相关的配置，
view是视图，app.vue是一个应用主组件，main.js是入口文件


# 对Vue生命周期的理解
总共分为8个阶段： 创建前/后、载入前/后，更新前/后，销毁前/后

1： 在beforeCreate阶段，Vue实例的挂载元素$el和数据对象都为undefined，还未初始化
    在created阶段，Vue实例的数据对象data有了，$el还没有
2：在beforeMount阶段，Vue实例的$el和data都都初始化了，但还是挂载之前为虚拟的dom节点，data.message还没未替换
   在mounted阶段，vue实例挂载完成，data.message成功渲染
3：当data变化时，会触发beforeUpdate和updated方法
4：在执行destroyed方法后，对data的改变不在触发周期函数，说明此时Vue实例已经解除了事件监听以及和dom的绑定，但是dom解构依然存在


# Vue 数据响应式原理
首先，需要利用Object.defineProperty,将要观察的对象，转化成getter/setter，以便拦截对象赋值和取值操作，称之为Observer
需要将DOM解析，取其中的指令和占位符，并赋予不同的操作，称之为Compiler
需要将Compiler的解析结果，与Observer所管擦的对象连接起来，建立关系，收通知，同时更新DOM，称之为Watcher
最后，需要一个公共入口对象，接收配置，协调上述三者，称之为Vue

# Vue-router原理分析
单页面应用的核心之一是：更新视图而不是重新请求页面
实现这一点主要是两种方式：
hash：通过改变hash值，
history：利用history对象的新特性

Hash 例如: xxx.com/#/id=5 HTTP请求不会包含后面的hash值，所以请求的仍然是 xxx.com,没有问题
History 例如:  xxx.com/id=5 这时请求的便是xxx.com/id=5，如后端没有配置对应id=XXX的路由处理，则会返回404错误。

而在vue-router中，它提供mode参数来决定采用哪一种方式，选择流程如下:
默认Hash-->如果浏览器支持History新特性改用History-->如果不在浏览器环境则使用abstract

选好mode后创建history对象

基本方法分析：
Hash：
    1.push()
    功能：设置新的路由添加历史记录并更新视图，常用情况是直接点击切换视图
    2.replace
    功能：替换当前路由并更新视图，常用情况是地址栏直接输入新地址，
    3.监听地址栏变化
    在setupListeners中监听hash变化（window.onhashchange）并调用replace

History
    1.push
    与hash模式类型，只是将window.hash改为history.hashState,
    2.replace
    与hash模式类似，只是将window.replace改为history.replaceState
    3.监听地址变化
    在HTML5History的构造函数中监听popState（window.onpopstate）


# Vue 非父子组件之间的通信
vue2中废弃了$dispatch和$broadcast广播和分发事件的方法。父子组件中可以用props和$emit()。
如何实现非父子组件间的通信，可以通过实例一个vue实例Bus作为媒介，要相互通信的兄弟组件之中，都引入Bus，
之后通过分别调用Bus事件触发和监听来实现组件之间的通信和参数传递

#Vuex

Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式。
它采用集中式存储管理应用的所有组件的状态。这里的关键在于集中式存储管理。
这意味着本来需要共享状态的更新是需要组件之间通讯的，而现在有了vuex，就组件就都和store通讯了。问题就自然解决了。

它集中储存该应用的所有数据，统一保管。便于维护。









