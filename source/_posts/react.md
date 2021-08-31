---
layout: about
title: react
date: 2021-08-31 10:07:09
tags: react react-router hook
---

# 虚拟dom

- 本质是Object类型的对象
- 虚拟DOM比较小，真实DOM比较大，因为虚拟DOM是React内部在用，无需真实DOM上那么多的属性
- 虚拟DOM最终会被React转化为真实DOM，呈现在页面上

# jsx语法规则

- 定义虚拟DOM时，不要写引号

```
const myId = 'anya'
const myData = 'HeLlo,rEaCt'

//1.创建虚拟DOM
const VDOM = (
	<div>
		<h2 className="title" id={myId.toLowerCase()}>
			<span style={{color:'white',fontSize:'29px'}}>{myData.toLowerCase()}</span>
		</h2>
		<h2 className="title" id={myId.toUpperCase()}>
			<span style={{color:'white',fontSize:'29px'}}>{myData.toLowerCase()}</span>
		</h2>
		<input type="text"/>
	</div>
)
//2.渲染虚拟DOM到页面
ReactDOM.render(VDOM,document.getElementById('test'))
```

- 标签中混入JS表达式时要用{}
- 样式的类名指定不要用class，要用className
- 内联样式，要用style={% raw %}{{key:value}}{% endraw %}的形式去写
- 只有一个根标签
- 标签必须闭合
- 标签首字母
  - 若小写字母开头，则将该标签转为html中同名元素，若html中无该标签对应的同名元素，则报错
  - 若大写字母开头，react就去渲染对应的组件，若组件没有定义，则报错

### 静态页面拆成组件

- 属性class=，需要改成className=
- 属性style=“”，需要改成style={{}}

# 组件

## 函数式组件

```
import React from 'react'
		
export default function Rfc() {
    return (
        <div>

        </div>
    )
}
```

执行ReactDOM.render(<Rfc/>...)后：

1. React解析组件标签，找到了Rfc组件
2. 发现组件是使用函数定义的，随后调用该函数，将返回的虚拟DOM转为真实DOM，随后呈现在页面中

## 类式组件

```
import React, { Component } from 'react'
		
export default class Rcc extends Component {
    render() {
        return (
            <div>

            </div>
        )
    }
}
```

> render放在哪里？
>
> - 放在组件的原型对象上，给实例使用
>
> render中的this指向？
>
> - 组件实例对象

执行ReactDOM.render(<Rcc/>...)后：

1. React解析组件标签，找到了Rcc组件
2. 发现组件是使用类定义的，随后new出来该类的实例，并通过该实例调用到原型上的render方法
3. 将render返回的虚拟DOM转为真实DOM，随后呈现在页面中

# 组件绑定监听事件传递了参数

这时函数并没有绑定这个函数，而是绑定的这个函数的返回值

```
onMouseEnter={handleMouse(true)}
```

- 绑定的处理函数返回一个函数

```
function handleMouse (flag) {
    return () => setMouse(flag)
  }
```

- 绑定时就确保绑定的一定是函数

```
onMouseEnter={() => handleMouse(true)}
```

# 组件绑定唯一key值

## 虚拟dom中key的作用

- 当state中的数据发生变化时，react会根据新数据生成新的虚拟dom，接着react进行新虚拟dom和旧虚拟dom的diff比较

比较规则：

- 旧虚拟DOM中找到了与新虚拟DOM相同的key：
  1. 若虚拟DOM中内容没变, 直接使用之前的真实DOM
  2. 若虚拟DOM中内容变了, 则生成新的真实DOM，随后替换掉页面中之前的真实DOM
- 旧虚拟DOM中未找到与新虚拟DOM相同的key，根据数据创建新的真实DOM，随后渲染到到页面

## 使用index作为key可能引发的问题

- 若对数据进行：逆序添加、逆序删除等破坏顺序操作
  - 会产生没有必要的真实DOM更新 ==> 界面效果没问题, 但效率低。

- 如果结构中还包含输入类的DOM
  - 会产生错误DOM更新 ==> 界面显示有问题