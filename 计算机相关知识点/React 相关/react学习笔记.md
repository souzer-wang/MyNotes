## ReactDOM.Render

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


## 创建组件

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

## React State（状态）

react 把组件看成一个状态机，通过和用户的交互，切换不同的状态，然后渲染 UI，让用户界面和数据保持一致。

即，只需更新组建的 state， 然后根据新的 state 重新渲染用户界面，而无需操作 DOM，下面这个例子添加了一个类构造函数来初始化this.state

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

# componentWillMount 和 componentDidMount的区别

componentWillMount： 
- 将要装载，在render之前调用
- 每一个组件render之前立即调用
- 可在服务器端被调用，也可在浏览器端被调用

componentDidMount： 
- 装载完成，在render之后调用
- 所有子组件都render完后才可以调用
- 只能在浏览器端被调用