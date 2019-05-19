---
layout:       post
title:        "基于redux的一些网络请求的理解（react相关）"
date:         2019-05-02
author:       "Conan"
header-img:   "img/in-post/build-blog-with-jekyll-theme/jekyll-theme.png"
header-mask:  0.7
catalog:      false
multilingual: false
tags:
    - Blog
    - React
---

由于新项目前端框架选型用到了react+redux+react-router的技术栈，最近一段时间终于重拾起了扔了两年的react，老泪纵横。。。react已经出到16+版本，据说facebook团队对react进行了大规模重构，但是对使用者没有丝毫的影响，牛啤！

*-------------------------------碎碎念结束--------------------------*

---
  实际上在react中，如果不借助redux中间件，一般的网络请求是在 componentDidMount 生命周期中触发的，对于为什么是componentDidMount中发请求呢？
**componentDidMount是官方推荐的用来发网络请求的生命周期**，它是在组件完全挂载之后才会去触发的。因为本人之前的项目有用过Angular5，给我的感觉就像是componentDidMount和angular5中的ngOnInit，那为什么不在constructor中调用呢？合理的解释是因为这个时候组件还未挂载，在这个时候进行一些数据绑定的操作可能会找不到一些相应的DOM，导致报错。

  OK，以上说明了为什么是在componentDidMount中发送网络请求，但实际上在项目中我看到很多人推荐用redux中间件去发送网络请求，以下会介绍我两种中间件。

---
  1，[redux-thunk](https://github.com/reduxjs/redux-thunk)使用方法详见readme。这个中间件的作用实际是给redux中的dispatch方法做了拓展，dispatch方法本身是接收一个对象的，通过redux-thunk，dispatch方法可以接收一个函数并调用。这个中间件的好处在于我们我们可以在action-creator中去发送请求，而不用写在生命周期函数里面，还有很重要的一点就是方便进行单元测试。但是如果在请求过后需要对response进行复杂的数据处理，写在action-creator中会显得比较臃肿。第二种中间件就是为了处理这种状况的。
  
  2，[redux-saga](https://github.com/redux-saga/redux-saga)使用方法详见readme。这个中间件实际是将所有异步请求放在一个文件中进行处理，如果有对请求数据进行操作的需求，建议放到这里，但是。。。saga的入门门槛好像有点高，因为他使用到了ES6中的generator函数（说实话我在接触saga之前完全没有用过generator函数），另外就是在error handle方面要用try-catch。以下可供参考：
  `function* getReq(){ 
let res = yield axios.get(url);
let action = initAction(res.data);
yield put(action);
}`