# React全家桶

## React的特点

> 用于构建用户界面的javascript库。属于mvc中的`'v'`视图
1. 声明式设计-使代码更具可预测性，更易于调试
2. 高效-通过虚拟DOM的，最大限度的减少DOM的交互
3. 灵活-可以与已知库或者框架很好的配合
4. 组件-通过创建组件，使代码更加容易得到复用
5. 单向数据流-单向响应的数据流，从而减少了重复代码

## 组件和元素的区别

1. 组件是由元素构成
2. 元素的结构时普通的对象，组件结构时类，或者纯函数
3. 组件首字母必须大写，元素则时小写
4. 元素使用`React.createElement()`或者`React.cloneElement()`创建，组件时使用`React.createClass()`和ES6 class 或者无状态组件



## setState过程

当你调用`setState`的时候，`react`并不会马上修改`state`。而是把这个对象放到一个更新队列，稍后才会从队列当中把新的状态提取出来合并到`state`中，然后触发更新。

## props和state的区别

1. `props`主要作用时外部组件用来传值到内部组件中，内部组件无法控制也无法修改，除非外部组件传入新的props
2. `state`主要用于组件保存，控制，修改自己的可变状态，`state`在组件内部可以初始化，可以被组件修改自身的状态，而外部不能访问也不能修改
3. `state`是让组件控制自己的状态，`props`是让外部对组件进行配置

## PureComponent

React15.3中新加的一个类`PurcComponent`，和`Component`基本一样，只不过在`render`之前会自动执行一次浅比较，来决定是否更新组件。纯组件最佳情况就时展示组件，它即没有子组件也没有依赖应用的全局状态。

1. `props`和`state`第一层的引用数据没有发生变化，`render`方法就不会触发
2. 第一层数据没变引用发生了变化，就会重新渲染
3. 第一层数据改变，但引用没变，会造成不渲染

## 展示组件和容器组件

### 展示组件

1. 主要负责组件内容如何展示
2. 从`props`接收父组件传递来的数据
3. 大多数情况可以通过函数定义组件声明

### 容器组件

1. 主要关注组件数据如何交换
2. 拥有自身的`state`，从服务器获取数据，或与redux等其他数据处理模块协作
3. 需要通过类定义组件声明，并包含生命周期函数和其他附加方法

### 这样写的好处

1. 解耦了界面和数据的逻辑
2. 更好的可复用性，比如同一个回复列表展示组件可以套用不同数据源的容器组件
3. 利于团队协作，一个负责界面结构，一个人负责数据交换

## 有状态组件和无状态组件

### 有状态组件

件能够保存，控制，修改自己的可变状态，`state`在组件内部可以初始化，可以被组件修改自身的状态的组件

### 无状态的组件

这种组件一般只接收来自其他组件的数据，在组件中能看到对`this.props`的调用。并不是所有的展示组件都是无状态组件，所有的容器组件都是有状态组件。

## 受控组件与非受控组件

### 受控组件

受控组件的值是由`props`或`state`传入，用户在元素上发生交互会引起应用的`state`的改变，在`state`改变之后重新渲染组件，我们才能在页面中看到元素变化的值。

### 非受控组件

类似与传统的DOM表单控件，用户交换不会直接引发`state`的变化，也不会直接为非受控组件传入值。

## 什么是高阶组件

高阶组件是一个函数，它接收一个组件作为参数，返回一个新的组件。这个组件会使用你传给它的组件作为子组件。

## 高阶组件的作用

高阶组件的作用是用于代码复用，可以把组件之间可复用的代码、逻辑抽离到高阶组件当中。新的组件和传入的组件通过`props`传递信息。高阶组件有助提高我们代码的灵活性，逻辑的复用性。

## 函数组件优缺点

优点

1. 组件不会被实例化，整体渲染性能得到提升
2. 减少了大量的冗余代码，增强了编写一个组件的便利
3. 组件只能访问输入的props，同样的props会得到同样的渲染效果，不会有副作用

缺点

1. 组件不能访问this对象
2. 组件无法访问生命周期的方法

## 一个组件如何返回多个元素，并且不在DOM中增加额外的节点

使用Fragments

```JavaScript
render() {
  return (
    <>
      <ChildA />
      <ChildB />
      <ChildC />
    </>
  );
}
//或者
render() {
    return (
      <React.Fragment>
        <td>Hello</td>
        <td>World</td>
      </React.Fragment>
    );
}
```

## 什么是Redux

Redux是一种新型的前端架构模式。管理全局状态。web应用是一个状态机，视图与状态是一一对应的。所有的状态，保存在一个对象里面。可以在同一种地方查询状态、改变状态、传播状态的变化。

## React、VUE和angular三者区别

1. vue API设计上简单，语法简单，学习成本低
2. angular是一个整体框架，学习曲线很陡峭。
3. react和VUE都使用虚拟DOM
4. angular双向数据流，react单向数据流，vue都可以
5. 社区支持度react第一，vue其次，angular最后
6. 三者都是组件化开发

## React 与jQuery区别

1. 在React中，只有当事件发生，`state`改变，之后，React自动调用`render()`来更新UI
2. 每个事件不需要担心哪一部分`DOM`发生变化，他们只需要设置`state`就可以了
3. jQuery没有中间过渡层`state`,我们需要花费很大的精力解决他们之间相互的联系
4. React可以把各个UI组件独立出来，有利于提高UI组件的复用率同时降低各个UI组件的耦合
5. 新手操作DOM时很难写出高效而又优雅的代码，从而使得前端代码满满变的越来越难以维护

React适合用在那些DOM操作复杂的单页面应用，有利于提高代码可读性以及提高页面性能，jQuery则是个用来帮你完成一些基本操作的工具库。

## React与vue优缺点

1. vue优势
    + 模板和渲染函数的弹性选择
    + 简单语法与项目创建
    + 更快的渲染速度和更小的体积
2. react优势
    + 更适合用于大型应用和更好的测试性
    + 用时适用于web端和App
    + 更大的生态圈带来的更多支持和工具
3. 相同
    + 利用虚拟DOM快速渲染
    + 轻量级
    + 响应式组件
    + 服务端渲染
    + 易于集成路由工具，打包工具以及状态管理工具
    + 优秀的支持和社区

## MVVM理解

`Model-View-ViewModel` 是一种软件架构设计模式 是MVC的升级版。View是用户界面，Model后端给与的数据模型，ViewModel前端开发人员组织生成和维护的视图数据层。MVVM 的核心是 ViewModel 层，负责转换 Model 中的数据对象来让数据变得更容易管理和使用，该层向上与视图层进行双向数据绑定，向下与 Model 层通过接口请求进行数据交互，起呈上启下作用。

## react diff简述

> 节点比较都是同节点进行比较

1. 两个相同组件产生类似DOM结构，不同的组件产生不同的DOM结构
    + 不同节点类型的比较，都是新的节点替换旧的节点
    + 节点类型相同，但是属性不同，元素替换属性，元素的style属性则是合并新属性。组件属性不同则替换新属性。

2. 对于同一层次的一组子节点，它们可以通过唯一的id进行区分.

## react key

当react 进行diff算法，列表的节点比较就依赖key作为对比和更新依据。不适用key，会对性能造成影响。

## 什么时候选用react或者vue

react相对于vue，react灵活性更大些，处理复杂的业务时，技术方案选择性更多一些，而vuejs提供了更为丰富的api实现功能会更坚定，因为api多灵活性就受到一点限制。复杂度教高时更倾向于react，而在做一些面向用户端复杂度不是很高时，用vue会更好一些。具体选择哪一个要看，公司对于哪款框架的驾驭程度。

## vue 双向绑定

## react loadable原理

把js代码分成N个页面份数的文件，不在用户刚进入就全部引入，而是等用户挑战路由的时候，再加载对应的js文件。这样做的好处就是加速首屏渲染的速度，同时也减少了资源的浪费。

## react redux口述

说起react redux就要先从redux说起.Redux是一种架构模式。是对于全局状态的管理，因为所有共享状态的操作都是不可预料，出现问题的时候找起来就非常困难。而redux所要做的是提高数据修改的门槛，想要修改数据只能执行某些我允许操作来修改数据，而且是只能从一个地方修改保证统一。

redux重要3个点是：Actions,Reducers,Store。

1. Actions 来告诉我们的状态管理器需要做什么样的操作，action是个对象。其中type字段来声明你到底要做什么，后面存放需要处理的数据。
2. Reducers 它会对action做出不同操作的函数。不直接改变state的值，而是返回一个新的对象，保持state的唯一性，在没有任何操作的情况下，会返回初始的state。
3. Store 用来管理state的单一对象提供了下面三种方法
    + `store.getState()` 获取当前返回的state。
    + `store.dispatch(action)` 发出操作，更新state。
    + `store.subscribe(listener)` 监听变化，当state发生更新时，就可以在这个函数的回调中监听。

react-redux在redux的基础上，还要关注的是Provider和connect

1. Provider就是把用redux创建的store传递到内部的组件中区，让内部通过react context让内部可以享有这个store并提供对state的更新
2. connent是个高阶函数。有2个主要方法
    + `mapStateToProps` 这个方法将store中的数据作为props绑定到组件上
    + `mapDispatchToProps` 这个方法是将dispatch绑定的函数作为props绑定到组件上

## react数据视图更新原理

1. state数据
2. JSX模板
3. 数据和模板生成虚拟dom
4. 用虚拟dom的结构构建成真实的dom
5. state发生变化
6. 重新拿到数据和模板生成新的虚拟dom
7. 比较原始的虚拟dom和新的虚拟dom的区别,进行替换或者删除
8. 然后用更新过后的虚拟dom构建成真实的dom


## React生命周期

### React 15以下的生命周期

实例化：

1. `constructor`
2. `componentWillMount`
3. `render`
4. `componentDidMount`


`props` 或者 `states`状态发生改变:

1. `componentWillReceiveProps` 如果是states状态发生改变是不会触发这个生命周期
2. `shouldComponentUpdate(nextProps,nextState)` 如果是true会有后续生命周期，如果false就没有后续了
3. `componentWillUpdate`
4. `render`
5. `componentDidUpdate`

卸载

1. `componentWillUnmount`

### React 16版本的class组件生命周期

实例化：

1. `constructor`
2. `static getDerivedStateFromProps(nextProps,prevState)`
3. `render`
4. `componentDidMount`

更新时：

1. `static getDerivedStateFromProps(nextProps,prevState)`

    一个静态函数，静态函数中不能使用this访问到class

    当父组件和自己本身状态改变时：

    `nextProps`:最新的父组件传过来的`props`
    `prevState`:当前自己组件最新的的`state`
      返回一个对象来更新 `state` 或者返回 `null` 来表示接收到的 `props` 没有变化，不需要更新 `state`

2. `shouldComponentUpdate(nextProps, nextState)`

    当父组件状态变更时：

    `nextProps`:最新的父组件传过来的`props`,当前的`this.props`是上一次的`props`

    `nextState`:当前最新的`state`,当前的`this.state`是最新的`state`

    当自己组件状态变更时：

    `nextProps`:最新的父组件传过来的`props`,当前的`this.props`最新的父组件传过来的`props`

    `nextState`:当前最新的`state`,当前的`this.state`是上一次的`state`

    如果是true会有后续生命周期，如果false就没有后续了    

3. `render`

4. `getSnapshotBeforeUpdate(prevProps, prevState)`

    当父组件状态变更时：

    `prevProps`:最新的父组件上一次传过来的`props`,当前的`this.props`是最新的`props`

    `prevState`:当前最新的`state`,当前的`this.state`是最新的`state`

    当自己组件状态变更时：

    `prevProps`:最新的父组件传过来的`props`,当前的`this.props`最新的父组件传过来的`props`

    `prevState`:是上一次的`state`,当前的`this.state`是最新`state`

    接收父组件传递过来的 props 和组件之前的状态,此方法必须有返回值，返回值作为第三个参数传递给`componentDidUpdate`，必须和 `componentDidUpdate` 一起使用，否则会报错

5. `componentDidUpdate`(prevProps, prevState, xx)

    基本上同`getSnapshotBeforeUpdate`一致，第三个可选参数是`getSnapshotBeforeUpdate`返回的值

卸载

1. `componentWillUnmount()`

## Fiber

### 一、Fiber出现解决什么问题

举个🌰：更新一个组件需要1毫秒，如果需要更新1000个组件，就会耗时1秒，在这1秒更新的过程中，主线程都在专心运行更新操作。浏览器绘制屏幕一般来说这个频率是每秒60次。也就说每16毫秒渲染1帧。如果执行js的时间过长超过16毫秒之后用户能察觉到页面卡顿，导致页面体验下降。

因为js单线程的特点，每个同步任务不能耗时太久，不然就会让程序不会其他输入做出反应。React以前更新过程中就会出现上述情况，而现在React Fiber就要改变现状。

### 二、什么是Fiber

把更新过程碎片化，把一个耗时的任务分成很多小片，执行非阻塞渲染，基于优先级应用更新以及在后台渲染内容。