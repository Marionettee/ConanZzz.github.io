---
layout:       post
title:        "webpack在开发中的优化"
date:         2019-05-19
author:       "Conan"
header-img:   "img/in-post/build-personal-blog/build-personal-blog.jpg"
header-mask:  0.7
catalog:      false
multilingual: false
tags:
    - Webpack
---

市面上流传着很多webpack的文章，其中大有可读性非常高的文章，然而看多了还是会眼花。所以趁闲暇时间对webpack在开发中的优化整理了一篇文章，仅是记录一点自己的使用心得。

>Js线程，如我们大家所知，是单线程的。在nodeJs中

1，[uglify-js](https://www.npmjs.com/package/uglify-js)
非常著名的压缩插件，在压缩的过程中为了开启多核压缩，需要设置'parallel',默认开启核数减一，当然也可设置paralle： os.cpus().length

2，SpeedMeasurePlugin
一个监控webpack各个loader运行速率的插件，如果有优化webpack的需求，这个插件是必备的。

3，开启通知面板

4，开启打包进度

5，开发面板更清晰
webpack-dashboard
、、、、、上线阶段
1，ES6不需要编译（前端尽量不要动态编译，会导致压力过大）
通过userAgent判断是否支持ES6
2，前端缓存负载（离线缓存）
mainfestJSON
3，真正的loading
4，build size
5，分析打包结果 webpack-chart
6，test exculde ，include 很重要
7，压缩js和css
8，dev tools
9，cache loader
