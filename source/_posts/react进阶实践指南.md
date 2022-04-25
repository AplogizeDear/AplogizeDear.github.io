---
layout:
  - post
title: react进阶实践指南
date: 2022-04-25 11:15:58
tags:
---

#### React 进阶实践指南

##### 基础篇 认识 jsx

###### 我们写的 JSX 终将变成什么

- 了解常用的元素会被 React 处理成什么，有利于后续理解 react fiber 类型；
- 理解 jsx 的编译过程，方便操纵 children、控制 React 渲染，有利于便捷使用 React 插槽组件。

```
const toLearn = [ 'react' , 'vue' , 'webpack' , 'nodejs'  ]

const TextComponent = ()=> <div> hello , i am function component </div>

class Index extends React.Component{
    status = false /* 状态 */
    renderFoot=()=> <div> i am foot</div>
    render(){
        /* 以下都是常用的jsx元素节 */
        return <div style={{ marginTop:'100px' }}   >
            { /* element 元素类型 */ }
            <div>hello,world</div>
            { /* fragment 类型 */ }
            <React.Fragment>
                <div> 👽👽 </div>
            </React.Fragment>
            { /* text 文本类型 */ }
            my name is alien
            { /* 数组节点类型 */ }
            { toLearn.map(item=> <div key={item} >let us learn { item } </div> ) }
            { /* 组件类型 */ }
            <TextComponent/>
            { /* 三元运算 */  }
            { this.status ? <TextComponent /> : <div>三元运算</div> }
            { /* 函数执行 */ }
            { this.renderFoot() }
            <button onClick={ ()=> console.log( this.render() ) } >打印render后的内容</button>
        </div>
    }
```

###### babel 处理后的样子

```
React.createElement(
  type,  //组件类型：传入组件对应的类或函数  dom元素类型：传入div者span之类的字符串
  [props], //一个对象  在dom类型中为标签属性，在组件类型中为props
  [...children] //依次为 children，根据顺序排列
)
```

###### 老版本的 React 中，为什么写 jsx 的文件要默认引入 React?

因为 jsx 在被 babel 编译后，写的 jsx 会变成上述 React.createElement 形式，所以需要引入 React，防止找不到 React 引起报错。

##### createElement 处理后的样子

##### 深入 Props
