---
layout:       post
title:        "h5打开app的风险刺探"
date:         2019-06-13
author:       "Conan"
header-img:   "img/in-post/build-personal-blog/build-personal-blog.jpg"
header-mask:  0.7
catalog:      false
multilingual: false
tags:
    - Spike
---

>最近由于业务需要，我这边负责对h5打开app进行了风险刺探(spike)

业务本身的需求实际上是在企业微信中或通过h5或直接通过app本身打开第三方app。

在进行了一些基础的了解后，得到了两个方向；1，浏览器scheme协议；2，universal links（IOS9以后）;

这里着重讲第一个方案，因为universal links配置比较复杂，所以建议自行谷歌。
那么scheme协议是怎么回事呢？首先，scheme是一种页面跳转协议，它的写法：[scheme]://[host]/[path]?[query]。我们如果需要在h5进行跳转可以写个<a href={scheme}></a>标签，当点击时进行跳转；也可以在window.onload时触发以进行自动跳转，常用的app的url scheme有比如微信：weixin:// ;支付宝：alipay:// 。一般常用的app在[捷径社区](https://sharecuts.cn)都可以找到。后续的具体处理我这里就不赘述了，大致上一般的做法是设一个定时器，当app被唤起，会触发onVisibility类似事件然后取消定时器，如果超时就带用户去app store。

那么在刺探的时候无论是自己做的小demo还是在网上找的资料，最终证实了万恶的腾讯竟然将唤起第三方app禁止了。。。好吧，其实说是安全方面考虑，我们也是可以理解的，所以很多类似于这种需要唤起第三方app的h5,最常见的做法就是先判断isInWechat,如果是，右上角提示用户用其他浏览器打开即可。

对于微信或者企业微信自身api直接唤起第三方app，我这边首先是除了腾讯系的app好像没见过这样操作的（这里多说一句，据资料说也不是所有的app都被禁止用scheme协议打开，据说腾讯是有个白名单的）。另外，企业微信开发文档有类似3rdPartLaunch的方法，但是也已经被删除找不到了。

以上就是这次spike的结果。希望能对看到这篇文章的人起到一定的帮助。