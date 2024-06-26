---
title: 面经-React
date: 2022-08-01
categories: 面经
tags: [面经,React]

---

## 说说Real DOM和虚拟DOM的区别及优缺点

Real DOM即文档对象模型，是结构化文本的抽象

虚拟DOM是以JS对象形式对DOM的描述，主要目的是为了更好地将虚拟的节点渲染到页面视图中。

区别：

- 虚拟DOM不会进行回流与重绘操作，而真实DOM会频繁回流与重绘
- 虚拟DOM的总损耗是虚拟DOM增删改+真实DOM差异增删改+回流与重绘，真实DOM的总损耗是真实DOM完全增删改+回流与重绘

使用虚拟DOM带来的优点：

- 简单方便，如果手动操作真实DOM来完成页面，繁琐又容易出错，在大规模应用下维护起来也很困难
- 性能方面，使用虚拟DOM，能够有效避免真实DOM的频繁更新，减少多次引起回流与重绘，特高性能
- 跨平台，React借助虚拟DOM，带来了跨平台能力，一套代码可以在多端运行

缺点：

- 在性能要求极高的应用中，虚拟DOM无法针对性的极致优化
- 首次渲染时，由于多了一层虚拟DOM的计算，速度比正常稍慢

## React生命周期

React的生命周期可以分为三个阶段：

- 创建阶段
- 更新阶段
- 卸载阶段

创建阶段的生命周期方法有：

- constructor，实例内部自动调用的方法，在方法内部通过super关键字获取来自父组件的props，该方法中通常的操作为初始化state状态或者在this上挂载方法。
- getDerivedStateFromProps，在组件创建和更新阶段会调用，在render之前，第一个参数为即将更新的props，第二个参数为上一状态的state，可以比较props和state加一些限制条件，防止无用更新
- render 必须实现的方法，用于渲染DOM结构，可以访问组件state和props属性
- componentDidMount 组件挂在到真是DOM节点后执行执行，执行时机在render之后，用于执行一些数据获取，事件监听等操作

更新阶段的方法主要有：

- getDerivedStateFromProps 同上
- shouldComponentUpdate 用于告知组件本身基于当前的props和state是否需要重新渲染组件，默认返回true
- render 同上
- componentDidUpdate 执行时机在组件更新结束后触发，该方法中，可以根据前后的props和state的变化做相应的操作，如获取数据，修改DOM样式等

卸载阶段的方法有：

- componentWillUnmount 执行时机在组件卸载前，用于清理一些注册的监听事件，或者取消订阅的网络请求等

## React的事件机制

React基于浏览器的事件机制自身实现了一套事件机制，包括事件注册、事件合成、事件冒泡、事件派发等，React中的事件机制被称为合成事件

合成事件并不会把事件代理函数直接绑定到真实节点上，而是把所有的事件绑定到结构的最外层(document)，使用一个统一的事件去监听。这个事件监听器上维持了一个映射来保存组件内部所有组件的事件监听函数。当组件挂载或卸载时，只在这个统一的事件监听器上插入或删除一些对象。事件发生时，首先被这个统一的事件监听器处理，然后在映射里找到真正的事件处理函数并调用。这样简化了事件处理与回收机制。

假设元素结构为docment-父元素-子元素，那么执行的时机为

docment捕获阶段->父元素捕获阶段->处在子元素阶段->父元素冒泡阶段->子元素合成事件冒泡阶段->父元素合成事件冒泡阶段->docment冒泡阶段

## 类式组件与函数式组件的区别

- 状态管理，类式组件以成员的方式保存状态，函数式组件使用hooks函数(useState)来保存状态
- 生命周期，类式组件的生命周期函数见上，函数式组件的生命周期函数可以使用hooks函数useEffect实现
- 调用方法，函数式组件直接执行函数即可，类式组件则需要先生成实例，再调用实例的render方法

## 高阶组件

高阶组件接收一个或者多个组件作为参数并且返回一个组件，其本质是一个函数，而不是一个组件。其作用为为输入组件进行加工，返回加工后的组件，本质上是装饰者设计模式

## React Hooks

可以在不编写class的情况下使用state以及其他的React特性。

常见的hooks有useState、useEffect、useRef等

- useState：const [count,setCount]=setState(0) 实现了类式组件的状态管理
- useEffect：useEffect(()=>{},[count])  实现了componentDidMount、componentDidUpdate、componentWillUnmount功能
- useRef：const ref=useRef()
- useCallback和useMemo：当依赖项发生改变时重新计算新结果，否则直接使用之前缓存的结果。区别在于useMemo用于缓存值，而useCallback用于缓存函数

## React的diff算法

React的diff算法分为了三个层级策略

tree层级策略指的是DOM节点的跨层级移动操作特别少可以忽略不计

component层级策略指的是相同类的两个组件会生成类似的树形结构，不同类的两个组件将会生成不同的树形结构

element层级策略是指同一层级下，同一位置的节点可以直接复用，不需要重新创建
