# 项目方面

#### 作者：高天阳
#### 邮箱：13683265113@163.com

```
更改历史

* 2019-08-09        高天阳     初始化文档

```

## 最近的项目是什么，用到了什么技术，有哪些功能点：

最近做的2个项目，分别是微信、PC端网站重构，使用的vue，UI框架分别是Vux和iView。功能点包括首页，平台介绍，项目列表，活动宣传，个人中心。

## 重构项目前后技术的差异有什么：

原项目使用的是JSP+jQuery

## 其中登录模块如何实现的，为什么这么实现：

微信端登录模块分为两部分，前端通过Vuex的store存储登录状态，后端通过token存储登录状态。在每一次调用接口的时候，向后端传token参数。
此token通过axios的header传送。在请求到未登录状态时，同步前端Vuex中的登录状态。使用中的小坑是页面刷新时，axios的header会被清空，
需要重新存储一份在header中。没有使用cookie直接传送的原因是，cookie中存储的数据比较多，后台在每一次获取时，需要遍历一次全部数据，
才能找到这个token，如果使用header会更加方便操作。

## 项目当中印象最深的地方是什么：

在开发中，进行了路由的配置，有hash配置到history。考虑原因是history模式的URL就像正常的URL，例如`http://yoursite.com/user/id`，
需要前后台共同配置，遇到的坑是页面资源的获取位置修改，项目地址为非根目录时的重定向，安卓版微信浏览器的404挟持。

[vue-router最佳实践](../../Vue/vue-router.md)

## 为什么选择了此技术及框架：

[vue常见前端UI库](https://www.cnblogs.com/cina33blogs/p/8984088.html)

主要原因如下：

1. 之前项目中有用到过
2. UI基于WeUi 符合微信端的整体感觉
3. 有较多的组件及较好的文档做支持
4. 在项目初时 查询过主流的UI框架 对比之后 既符合项目需求又有较多的star数 因此选择了vux

## 开发中遇到了什么问题：


## vue和jquery同时使用的弊端，在操作DOM上，谁先谁后：

项目中使用axios的原因是部分接口牵扯到跨域请求，原有项目是使用getJson实现的，在后端没修改接口的前提下，使用axios未成功调通，故而共用了。

Vue的官方是不建议直接操作 DOM 的，其优势在于视图和数据的双向绑定，而且所有DOM操作都可以用Vue实现，反而使用jQuery来操作DOM的话，
会造成不必要的麻烦，DOM未渲染完成之前事件绑定不上，当然也有解决办法，比如setTimeOut。

按照官方文档`this.$nextTick`可以在dom渲染完成之后执行操作，然而实际使用可能会由于资源是网络请求，存在延时的情况，导致获取失败。

## PC端为什么需要兼容IE：

公司原有的支付逻辑及对接的第三方支付通道，要求使用IE浏览器，故而需要兼容。

## 项目中有封装哪些组件和接口：

在PC端封装了公共头部、底部以及菜单栏。公共方法封装过数据表的增删改查通用接口，通过是否有id字段，表id字段来判断是调用哪个接口。

## 下图的功能实现思路是什么(注意性能的问题，比如回收机制，扩散问题)：


## `git push`和`git fetch`的区别

`git fetch`用来查看其他协作者的进程

## 项目管理经验

1. 项目整体的把控
2. 沟通是第一位(晨会、晚回)
3. 事情的紧急、重要层级
4. 标准的制订 前端代码的风格统一 标准统一

## 微信项目使用微信的sdk会有什么问题

如果项目使用的是hash模式url中含有"#"，需要去掉url"#"号后面的字符`location.href.split('#')[0]`