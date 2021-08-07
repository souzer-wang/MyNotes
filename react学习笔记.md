# 概念梳理

---

### 什么是DOM?

<br>
DOM: Document Object Model 文档对象模型

要理解DOM，先理解以下信息：
1. 普通文档（如 txt ）和 HTML / XML 文件的区别在于：  **后者是有组织的结构化文件**
2. 浏览器将结构化的文档 **以树的数据结构** 读入浏览器内存，树的每个子节点定义为一个 **NODE对象**
	1. 每个节点(NODE)都有自己的属性（名称、类型、内容）
	2. NODE之间有层级关系（parents，child，sibling……）
3. 第二步就完成了文档的建模操作（将文档内容以树形结构写入内存），此时再编写一些方法来操作节点（属性和位置信息），即为NODE API。

再抽象一下：
- DOM 是一种将HTML/XML文档 **组织成对象模型的建模过程**
- DOM 建模的重点在于 **如何解析HTML/XML文档** 和开发符合DOM接口规范的节点操作API接口

再抽象一下：
- DOM 负责
	- 解析文档，
	- 建模成对象模型，
	- 开发API接口

---

### 什么是react ?

用于构建用户界面的 Javascript 框架。

有了react，开发者不需要再直接和DOM接触，它包含三项核心技术：
- 响应式 UI
- 虚拟 DOM
- 组件

#### 响应式UI

你只需要编写自己想要的是什么，React会自己弄清楚怎么做。 当数据变化时，UI会相应地发生变化。

开发者无需关心DOM的更新，React会自动完成。

DEMO： [Domo's hat (codepen.io)](https://codepen.io/focuser/pen/gROrXx)

#### 虚拟 DOM

jQuery有个问题，就是运行速度

DOM处理的速度比较慢，React引入了**虚拟 DOM**，可以理解为一种草稿，也就是说，开发者在大部分的时间里操作的是虚拟DOM，而不是真实的DOM。

这样做的优势在于，React会对虚拟DOM进行处理（比如去除重复），降低开发者和 DOM 的工作量

#### 组件

组件是React应用的一个重要特征，构建React应用基本上就是在和组件打交道：
- 寻找合适组件
- 融合组件
- 在现有组件上创建新组件

组件的好处是提高了重用性，且，如果某个组件更新了，使用了这个组建的地方都会自动更新

#### react 和 jQuery的不同之处

jQuery 是一个javascript 库，它的作用是简化了（或者说方便了）开发者对DOM的操作（注意：开发者还是直接处理DOM的，这就是区别）

---

### 什么是 React Native ？

> 问题梳理：
> 1. 什么是React Native？ 其中的Native什么意思
> 2. 它的特点
> 3. React Native 和 React 分别用来开发什么
> 4. 为什么有ReactDOM？ 它是用来干嘛的
> 5. React渲染器是用来干嘛的

#### 渲染器（renderer）和新的 React

对于Web应用来说，React 负责启用 React 范式（管理响应式 UI、组件和虚拟 DOM），以及实际更新浏览器中的DOM

但如果还考虑IOS和安卓端，就不行了。

所以React进行了拆分：
- 一部分是新的React， 只负责启用React范式
- 第二部分叫做ReactDOM，唯一的任务就是和浏览器中的DOM进行交互（因为ReactDOM负责更新DOM，而DOM又决定了浏览器渲染的内容，所以将ReactDOM称为**渲染器**）

#### 一个完整的平台

React的官网定义：用来开发用户界面的 JavaScript 库

换言之，它只负责开发UI

Web 开发环境下，React 能自然地适应 Web 环境下的其他部件。

but，对于移动端来说就比较困难，需要支持多种语言和技术，为了解决这个问题，产生了**React Native**

那么相对于Web上的 React， React Native 包括更多的东西：
- 新的React作为核心库
- iOS 和安卓的渲染器
- 将代码转换成可安装应用的工具
- 原生 UI 组件 (状态栏、列表等等)和动画
- UI 的样式和布局工具箱 (flexbox)
- 构建大多数应用的基础部分 (比如网络)
- 提供原生功能的部分，比如粘贴板、加速计和存储

为什么叫React Native呢， 因为它的**内置UI是由原生UI组件组成的**

---

### 什么是 prop， 什么是 state

- prop 是每个组件的固有属性，不可改变
- state 是组件的私有数据，组件创建后才可以使用

---

### ReactDOM.Render

ReactDOM.render 是 react 的最基本的方法，它将模板转化为html语言，并插入指定的 DOM 节点，它包含两个参数：
1. 模板的渲染内容，即虚拟DOM对象（HTML 形式）
2. 需要插入的 DOM 节点
3. 渲染后的回调（一般用不到）

举个最简单的例子：
```javascript
//1. 导入 react
import React from 'react'
import ReactDOM from 'react-dom'

//2. 创建 虚拟DOM
//参数1：元素名称  参数2：元素属性对象（null 表示无） 参数3：当前元素的子元素 string||createElement 的返回值
const divVD = React.createElement('div',{title: 'hello react'}, 'Hello React!')

//3. 渲染
//参数1：虚拟DOM对象 参数2. dom对象表示渲染到哪个元素内 参数3. 回调函数
ReactDOM.render(divVD, document.getElementById('app'))
```


### 创建组件

react 中创建组件的方式有两种：
1. 无状态函数式
2. class 类

```javascript
//1. 函数式
function HelloMessage(props) {
	return <h1>Hello World!</h1>;
}

//2. class 方式
class Welcome extends React.Component {
	render() {
		return <h1>Hello World!</h1>;
	}
}
```

还有一种是用户自定义组件，比如：

const element = <HelloMessage />

结合以上，举例：
```javascript
function HelloMessage(props) {
	return <h1>Hello {props.name}!</h1>;	
}

const element = <HelloMessage name = "Joe"/>

ReactDOM.render(
	element,
	document.getElementById('example')
);
```

得到结果： Hello Joe

### React State（状态）

**react 把组件看成一个状态机，通过和用户的交互，切换不同的状态，然后渲染 UI，让用户界面和数据保持一致。**

即，只需更新组件的 state， 然后根据新的 state 重新渲染用户界面，而无需操作 DOM，下面这个例子添加了一个类构造函数来初始化 this.state

[实例](https://www.runoob.com/try/try.php?filename=try_react_state)
```javascript
class Clock extends React.Component {
	constructor(props) {
		super(props);
		this.state = {date: new Date()};
	}
	
	render() {
		return (
			<div>
				<h1>Hello,World!</h1>
				<h2>现在是 {this.state.date.toLocalTimeString()}.</h2>
			</div>
		);
	}
}

ReactDOM.render(
	<Clock />,
	document.getElementById('example')
);
```

这样每次启动后，每次运行将显示当前时间，如果需要显示实时时间，则需要将生命周期方法加入到类中

### 将生命周期方法添加到类中

**在具有许多组件的应用程序中，在销毁时释放组件所占用的资源非常重要**

每当 Clock 组件第一次加载到 DOM 中时，我们想生成定时器，在React中称为 **挂载**

在 Clock 生成的这个 DOM 被移除的时候，我们也想消除定时器，在React 中称为 **卸载**

[实例](https://www.runoob.com/try/try.php?filename=try_react_state2)：
```javascript
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }
 
  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }
 
  componentWillUnmount() {
    clearInterval(this.timerID);
  }
 
  tick() {
    this.setState({
      date: new Date()
    });
  }
 
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>现在是 {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
 
ReactDOM.render(
  <Clock />,
  document.getElementById('example')
);
```

其中：
- componentDidMount() 与 componentWillUnmount() 方法被称作 **生命周期钩子**
- 在组件输出到 DOM 后会执行 componentDidMount() 钩子，我们可以在这个钩子上设置一个定时器
- this.timeID 为定时器的 ID， 我们可以在 componentWillUnmount() 钩子中卸载定时器

代码执行的顺序：
1. **调用构造函数**
> 当 <Clock /> 被传递到 ReactDOM.render() 时，React 调用 Clock 组建的构造函数，即 用包含当前时间的对象来初始化 this.state
2. **调用render方法**
> 然后 React 调用 Clock 组件的 render() 方法。 这是 React 在了解屏幕上应该显示什么内容，然后 React 更新 DOM 以匹配 Clock 的渲染输出
3. **componentDidMount()**
> 在 Clock 的输出插入到 DOM 中时，React 调用 componentDidMount() 生命周期钩子。本例子中 Clock 组件要求浏览器设置一个定时器，每秒钟调用一次 tick()
4. **每秒钟调用 tick() **

> 浏览器每秒调用tick()，其中，`Clock` 组件通过使用包含当前时间的对象调用 `setState()` 来调度UI更新。
> 
> 通过调用 `setState()` ，React 知道状态已经改变，并再次调用 `render()` 方法来确定屏幕上应当显示什么。
> 
> 也就是说 `render()` 方法中的 `this.state.date` 将不同，所以渲染输出将包含更新的时间，并相应地更新 DOM

5. **清除定时器**
> 一旦 `Clock` 组件被从 DOM 中移除，React 会调用 `componentWillUnmount()` 这个钩子函数，定时器也就会被清除。

## render 方法(基础)

一个组件类必须要实现一个 render 方法

### render 方法必须返回一个 JSX 元素

即，如果要返回多个JSX元素，就在外面用一个JSX元素包裹起来

**错误示例**
```javascript
render () {
	return (
		<div>第一个</div>
		<div>第二个</div>
	)
}
```

**正确用法**
```javascript
render () {
	return (
		<div>
			<div>第一个</div>
			<div>第二个</div>
		</div>
	)
}
```

### 表达式插入

在 JSX 当中可以插入 JavaScript 的表达式，表达式返回的结果会相应地渲染到页面上（表达式用{ }包裹）

```javascript
render () {
	const word = 'is good'
	return (
		<div>
			<h1>React 小书 {word}</h1>
		</div>
	)
}
```

也可以用函数表达式来返回

```javascript
render () {
	return (
		<div>
			<h1>React 小书 {(function (){
					 return 'is good'})()}</h1>
		</div>
	)
}
```

## componentWillMount 和 componentDidMount的区别

componentWillMount： 
- 将要装载，在render之前调用
- 每一个组件render之前立即调用
- 可在服务器端被调用，也可在浏览器端被调用

componentDidMount： 
- 装载完成，在render之后调用
- 所有子组件都render完后才可以调用
- 只能在浏览器端被调用

> 关于接口请求数据是放在 didMount 好还是放在 willMount 好，还是看情况
> 
> 对于同步的状态改变，可以放在 componentWillMount 里面；对于异步的，最好放在 componentDidMount。