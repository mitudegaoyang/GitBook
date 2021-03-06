# Vuex使用

#### 作者：高天阳
#### 邮箱：13683265113@163.com

```
更改历史

* 2018-11-07        高天阳     初始化文档

```

## 1 简介

### 1.1 Vuex 是什么？

Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式。它采用集中式存储管理应用的所有组件的状态，
并以相应的规则保证状态以一种可预测的方式发生变化。Vuex 也集成到 Vue 的官方调试工具 [devtools extension](https://github.com/vuejs/vue-devtools)，
提供了诸如零配置的 time-travel 调试、状态快照导入导出等高级调试功能。

### 1.2 什么是“状态管理模式”？

让我们从一个简单的 Vue 计数应用开始：

```
new Vue({
  // state
  data () {
    return {
      count: 0
    }
  },
  // view
  template: `
    <div>{{ count }}</div>
  `,
  // actions
  methods: {
    increment () {
      this.count++
    }
  }
})
```

这个状态自管理应用包含以下几个部分：

* state，驱动应用的数据源；
* view，以声明方式将 state 映射到视图；
* actions，响应在 view 上的用户输入导致的状态变化。

以下是一个表示“单向数据流”理念的极简示意：

![](../assets/vuex/flow.png)

但是，当我们的应用遇到**多个组件共享状态**时，单向数据流的简洁性很容易被破坏：

* 多个视图依赖于同一状态。
* 来自不同视图的行为需要变更同一状态。

对于问题一，传参的方法对于多层嵌套的组件将会非常繁琐，并且对于兄弟组件间的状态传递无能为力。对于问题二，
我们经常会采用父子组件直接引用或者通过事件来变更和同步状态的多份拷贝。以上的这些模式非常脆弱，
通常会导致无法维护的代码。

因此，我们为什么不把组件的共享状态抽取出来，以一个全局单例模式管理呢？在这种模式下，
我们的组件树构成了一个巨大的“视图”，不管在树的哪个位置，任何组件都能获取状态或者触发行为！

另外，通过定义和隔离状态管理中的各种概念并强制遵守一定的规则，我们的代码将会变得更结构化且易维护。

这就是 Vuex 背后的基本思想，借鉴了 [Flux](https://facebook.github.io/flux/docs/overview.html)、
[Redux](http://redux.js.org/)、和 [The Elm Architecture](https://guide.elm-lang.org/architecture/)。
与其他模式不同的是，Vuex 是专门为 Vue.js 设计的状态管理库，以利用 Vue.js 的细粒度数据响应机制来进行高效的状态更新。

![](../assets/vuex/vuex.png)

### 1.3 什么情况下我应该使用 Vuex？

虽然 Vuex 可以帮助我们管理共享状态，但也附带了更多的概念和框架。这需要对短期和长期效益进行权衡。

如果您不打算开发大型单页应用，使用 Vuex 可能是繁琐冗余的。确实是如此——如果您的应用够简单，您最好不要使用 Vuex。一个简单的
[store 模式](https://cn.vuejs.org/v2/guide/state-management.html#%E7%AE%80%E5%8D%95%E7%8A%B6%E6%80%81%E7%AE%A1%E7%90%86%E8%B5%B7%E6%AD%A5%E4%BD%BF%E7%94%A8)
就足够您所需了。但是，如果您需要构建一个中大型单页应用，您很可能会考虑如何更好地在组件外部管理状态，Vuex 将会成为自然而然的选择。
引用 Redux 的作者 Dan Abramov 的话说就是：

> Flux 架构就像眼镜：您自会知道什么时候需要它。

## 2 安装与使用

### 2.1 安装

### 2.1.1 直接下载 / CDN 引用

[https://unpkg.com/vuex](https://unpkg.com/vuex@3.0.1/dist/vuex.js)

[Unpkg.com](https://unpkg.com/#/) 提供了基于 NPM 的 CDN 链接。以上的链接会一直指向 NPM 上发布的最新版本。
您也可以通过 `https://unpkg.com/vuex@2.0.0` 这样的方式指定特定的版本。

在 Vue 之后引入 `vuex` 会进行自动安装：

```
<script src="/path/to/vue.js"></script>
<script src="/path/to/vuex.js"></script>
```

### 2.1.2 NPM

```
npm install vuex --save
```

### 2.1.3 Yarn

```
yarn add vuex
```

在一个模块化的打包系统中，您必须显式地通过 `Vue.use()` 来安装 Vuex：

```
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)
```

当使用全局 script 标签引用 Vuex 时，不需要以上安装过程。

### 2.1.4 Promise

Vuex 依赖 [Promise](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Using_promises)。
如果你支持的浏览器并没有实现 Promise (比如 IE)，那么你可以使用一个 polyfill 的库，
例如 [es6-promise](https://github.com/stefanpenner/es6-promise)。

你可以通过 CDN 将其引入：

```
<script src="https://cdn.jsdelivr.net/npm/es6-promise@4/dist/es6-promise.auto.js"></script>
```

然后 `window.Promise` 会自动可用。

如果你喜欢使用诸如 npm 或 Yarn 等包管理器，可以按照下列方式执行安装：

```
npm install es6-promise --save # npm
yarn add es6-promise # Yarn
```

或者更进一步，将下列代码添加到你使用 Vuex 之前的一个地方：

```
import 'es6-promise/auto'
```

### 2.1.5 自己构建

如果需要使用 dev 分支下的最新版本，您可以直接从 GitHub 上克隆代码并自己构建。

```
git clone https://github.com/vuejs/vuex.git node_modules/vuex
cd node_modules/vuex
npm install
npm run build
```

### 2.2 开始

每一个 Vuex 应用的核心就是 store（仓库）。“store”基本上就是一个容器，它包含着你的应用中大部分的**状态 (state)**。
Vuex 和单纯的全局对象有以下两点不同：

1. Vuex 的状态存储是响应式的。当 Vue 组件从 store 中读取状态的时候，若 store 中的状态发生变化，那么相应的组件也会相应地得到高效更新。

1. 你不能直接改变 store 中的状态。改变 store 中的状态的唯一途径就是显式地**提交 (commit) mutation**。
这样使得我们可以方便地跟踪每一个状态的变化，从而让我们能够实现一些工具帮助我们更好地了解我们的应用。

### 2.3 最简单的 Store

> 提示： 我们将在后续的文档示例代码中使用 ES2015 语法。如果你还没能掌握 ES2015，[你得抓紧了](Babel.md)！

安装 Vuex 之后，让我们来创建一个 store。创建过程直截了当——仅需要提供一个初始 state 对象和一些 mutation：

```
// 如果在模块化构建系统中，请确保在开头调用了 Vue.use(Vuex)

const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  }
})
```

现在，你可以通过 `store.state` 来获取状态对象，以及通过 `store.commit` 方法触发状态变更：

```
store.commit('increment')

console.log(store.state.count) // -> 1
```

再次强调，我们通过提交 mutation 的方式，而非直接改变 store.state.count，
是因为我们想要更明确地追踪到状态的变化。这个简单的约定能够让你的意图更加明显，
这样你在阅读代码的时候能更容易地解读应用内部的状态改变。此外，这样也让我们有机会去实现一些能记录每次状态改变，
保存状态快照的调试工具。有了它，我们甚至可以实现如时间穿梭般的调试体验。

由于 store 中的状态是响应式的，在组件中调用 store 中的状态简单到仅需要在计算属性中返回即可。
触发变化也仅仅是在组件的 methods 中提交 mutation。

这是一个[最基本的 Vuex 记数应用](https://jsfiddle.net/n9jmu5v7/1269/)示例。

接下来，我们将会更深入地探讨一些核心概念。让我们先从 State 概念开始。

## 3 示例

> Vue登录注册，并保持登录状态

关于vue登录注册，并保持登录状态，是vue玩家必经之路，网上也有很多的解决方法，
但是有一些太过于复杂，新手可能会看的一脸懵逼，现在给大家介绍一种我自己写项目在用而且并不难理解的一种方法。

项目中有一些路由是**登录与否**均可以进入的，比如首页，活动页等等
有一些路由是**需要登录**才可以进入的，比如个人中心，业务详情等等
有一些路由是**不需要登录**就可以进入，比如登录页，注册页，忘记密码等等
那如何判断路由是否需要登录呢？就要在路由JS里面做文章

**在router.js中添加meta区分**
  
比如登录注册页面，尚未登录才可进入，那么我们把meta中的isLogin标志设置为**false**

> yumaomoney_WeChat/src/router/index.js

```
// 登录
{
  path: 'login',
  name: 'login',
  component: Login,
  meta: {
    isLogin: false
  }
},
{
  path: 'registered',
  component: Container,
  children: [
    // 注册
    {
      path: '/',
      component: Registered,
      meta: {
        isLogin: false
      }
    },
    // 确认注册
    {
      path: 'registeredDetail',
      name: 'registeredDetail',
      component: RegisteredDetail,
      meta: {
        isLogin: false
      }
    }
  ]
},
```

而在个人中心我们需要登录才能进入，那么我们把meta中的isLogin标志设置为true

```
{
  //首页
  path: '/user',
  component: User,
  meta: {
    isLogin: true
  },
}
```

而在首页我们登录、未登录均能进入，那么我们不设置meta

```
{
  //首页
  path: '/home',
  component: Home
}
```

这样我们就为进入各个路由是否需要登录做了区分。

接下来我们在login.vue中修改登录后状态

我们使用axios向后台发起登录请求

> yumaomoney_WeChat/src/components/start/login/Login.vue

```
this.$axios.post("/xxx/login", {user:name,password:pwd})
  .then(data => {
    if (res.data === 1) {
      // 登录成功
      self.$store.dispatch('setUser', true)
      // Vuex在用户刷新的时候userLogin会回到默认值false，所以我们需要用到HTML5储存
      // 我们设置一个名为Flag，值为isLogin的字段，作用是如果Flag有值且为isLogin的时候，证明用户已经登录了。
      localStorage.setItem('Flag', 'isLogin')
      // 登录成功后跳转到指定页面
      self.$router.push('/home')
    } else if (res.data === 2) {
      // 登录失败,先不讨论
      self.data.toastMsg = '验证码不正确!'
      self.data.toastType = true
    }
 })
```

Vuex里面我是这样写的（如果项目不需要Vuex，那么直接使用HTML5储存就可以了）：

> yumaomoney_WeChat/src/store/index.js

```
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex)

export const store = new Vuex.Store({
  // 设置属性
  state: {
    isLogin: false
  },

  // 获取属性的状态
  getters: {
    // 获取登录状态
    isLogin: state => state.isLogin
  },

  // 设置属性状态
  mutations: {
    // 保存登录状态
    userStatus (state, flag) {
      state.isLogin = flag
    }
  },

  // 应用mutations
  actions: {
    // 获取登录状态
    setUser ({commit}, flag) {
      commit('userStatus', flag)
    }
  }
})
module.exports = store
```

重点来了~，在mian.js里

> yumaomoney_WeChat/src/main.js

```
import Vuex from 'vuex'
import store from './store/index'
Vue.use(Vuex)

/* eslint-disable no-new */
new Vue({
  router,
  store,
  render: h => h(App)
}).$mount('#app-box')

router.beforeEach((to, from, next) => {
  // 获取用户登录成功后储存的登录标志
  let getFlag = localStorage.getItem('Flag')
  // 如果登录标志存在且为isLogin，即用户已登录
  if (getFlag === 'isLogin') {
    // 设置vuex登录状态为已登录
    store.state.isLogin = true
    next()
    // 如果已登录，还想想进入登录注册界面，则定向回首页
    if (to.meta && (to.meta.isLogin === false)) {
      next({
        path: '/home'
      })
    }
    // 如果登录标志不存在，即未登录
  } else {
    // 用户想进入需要登录的页面，则定向回登录界面
    if (to.meta.isLogin) {
      next({
        path: '/start/login'
      })
      // 用户进入无需登录的界面，则跳转继续
    } else {
      next()
    }
  }
})

router.afterEach(route => {
  window.scroll(0, 0)
})
```

这样就已经完成了Vue的登录注册，当用户关闭浏览器或者第二天再次进入网站，用户依旧可以保持着登录的状态直到用户手动退出登录。

用户退出只需要`localStorage.removeItem("Flag")`即可

> yumaomoney_WeChat/src/components/user/center/Center.vue

```
  logout () {
    var self = this
    window.localStorage.removeItem("Flag")
    self.$store.dispatch('setUser', false)
    self.$router.push('/home')
  }
```

## 4 最佳实践

## 5 同类技术比较


## 参考资料

* [vux官网](https://vuex.vuejs.org/zh/)
* [vux登录注册并保持登录状态](https://blog.csdn.net/sinat_17775997/article/details/83450620)
* [使用Vue.js和Vuex实现购物车场景](https://segmentfault.com/a/1190000005780326)
* [报错处理：router is not defined](https://segmentfault.com/q/1010000014461113)
