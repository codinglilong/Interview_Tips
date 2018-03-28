# React全家桶

## 组件和元素的区别
1. 组件是由元素构成
2. 元素的结构时普通的对象，组件结构时类，或者纯函数
3. 组件首字母必须大写，元素则时小写
4. 元素使用`React.createElement()`或者`React.cloneElement()`创建，组件时使用`React.createClass()`和ES6 class 或者无状态组件

## React生命周期
实例化：
1. getDefaultProps
2. getInitialState
3. componentWillMount()
4. componentDidMount()

存在期
1. componentWillReceiveProps(nextProps)
2. shouldComponentUpdate(nextProps,nextState) 需要返回true才能继续执行下面的代码
3. componentWillUpdate()
4. componentDidUpdate()

卸载
1. componentWillUnmount()

## 什么时jsx
jsx 其实把类似`HTML`的结构转换成`javascript`的对象结构

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
```
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

## Redux 工作流
1. 首先用户发出action  
    `store.dispatch(action)`
2. Store自动调用Reducer，并且传入两个参数。一个是当前State和收到的Action，一个是Redux会返回的新的State。 
    `let nextState = toduApp(previousState,action)`
3. State一旦有变化，Store就会调用监听函数  
    `store.subscribe(listener)`
4. listener可以通过`store.getState()`得到当前的状态。如果使用的是React,这时可以触发重新渲染View。
    ```
    function listerner(){
        let newState = store.getState();
        component.setState(newState);
    }
    ```

## React-Redux工作流
1. Provider组件接受redux的store作为props，然后通过context往下传。
2. connect函数收到Provider传出的store，然后接受三个参数mapStateToProps，mapDispatchToProps和组件，并将state和actionCreator以props传入组件，这时组件就可以调用actionCreator函数来触发reducer函数返回新的state，connect监听到state变化调用setState更新组件并将新的state传入组件。