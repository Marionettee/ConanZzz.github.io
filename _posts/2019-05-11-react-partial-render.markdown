---
layout:       post
title:        "react中列表渲染的局部刷新"
date:         2019-05-11
author:       "Conan"
header-img:   "img/in-post/improve-professional-competence/professional-competence.jpg"
header-mask:  0.7
catalog:      false
multilingual: false
tags:
    - Blog
    - React
---

最近在写demo的时候遇到一个更新列表中某个的对象的某个值，最期待的结果肯定是局部刷新，但是我们往往在改变值之后会遇到全局都刷新的问题，以下为个人实验出来的一个小技巧。

首先我有以下数据需要通过react的列表方法渲染：
```js
let list=[
    {
        id:1,
        show:false
    },
    {
        id:2,
        show:false
    },
    {
        id:3,
        show:false
    }
]
```
我们通过以下react方法进行渲染：
```js
render(){
    return (
        {list.map((val)=>{
            <DemoComponent val={val}/>
        })}
    )
}
```
在这里我们需要重新写一个DemoComponent的组件：
```js
import React,{Component} from 'react';
export class DemoComponent extends Component{
    render(){
        <div>
          <div>this.props.val.id</div>
          <button onClick={()=>this.toggleDialog()}>toggle</button>  
        </div>
    }
    toggleDialog(){
        // 更改val.showDialog相关操作。
    }
    shouldComponentUpdate(nextProps){
        return JSON.stringify(nextProps) !== JSON.stringify(this.props);
    }
}
```
当我们点击第一个button，这个时候就能达到局部刷新，只刷新第一个DemoComponent组件的效果了。

但以上操作是什么原理呢？

首先，大家平时都推荐使用的PureComponent不能在这里使用,因为这个组件没有shouldComponentUpdate这个钩子函数，虽然PureComponent也有对比props和nextProps并自行判断当前组件是否需要重新渲染的功能，但是这个对比对对象是没有用的，因为{} === {}是返回false的（对这个知识点不理解的朋友可以去看看堆栈相关的知识），数组同理。

那么为了让其他列表组件没有必要多去render一次，所以我在shouldComponentUpdate中取了个巧，直接JSON.stringify，将两个对象转换成字符串进行比较，这样就方便的多。

当然这里也是自己为了不去写一个isEqual方法而偷懒的做法，这样做的好处是简单方便，也节省时间（isEqual方法必然会for-in循环，对于更复杂的情况甚至需要递归，在内存消耗和时间复杂度上肯定会比JSON.stringify严重），缺点在于可拓展性不高，如果是个数组，就比较头疼。