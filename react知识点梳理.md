# PART1

> 问题梳理：
> 1. 什么是DOM
> 2. 什么是react，它的优势在哪些方面
> 3. react 和 jQuery的不同之处
> 4. React的核心概念是什么
> 5. 什么是响应式UI
> 6. 组件有哪些好处

## DOM
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

## jQUery

jQuery 是一个javascript 库，它的作用是简化了（或者说方便了）开发者对DOM的操作（注意：开发者还是直接处理DOM的）

## React

有了react，开发者不需要再直接和DOM接触，它包含三项核心技术：
- 响应式 UI
- 虚拟 DOM
- 组件

### 响应式 UI

你只需要编写自己想要的是什么，React会自己弄清楚怎么做。 当数据变化时，UI会相应地发生变化。

开发者无需关心DOM的更新，React会自动完成。

DEMO： [Domo's hat (codepen.io)](https://codepen.io/focuser/pen/gROrXx)

### 虚拟 DOM

jQuery有个问题，就是运行速度

DOM处理的速度比较慢，React引入了**虚拟 DOM**，可以理解为一种草稿，也就是说，开发者在大部分的时间里操作的是虚拟DOM，而不是真实的DOM。

这样做的优势在于，React会对虚拟DOM进行处理（比如去除重复），降低开发者和 DOM 的工作量

### 组件

组件是React应用的一个重要特征，构建React应用基本上就是在和组件打交道：
- 寻找合适组件
- 融合组件
- 在现有组件上创建新组件

组件的好处是提高了重用性，且，如果某个组件更新了，使用了这个组建的地方都会自动更新

# PART2

> 问题梳理：
> 1. 什么是React Native？ 其中的Native什么意思
> 2. 它的特点
> 3. React Native 和 React 分别用来开发什么
> 4. 为什么有ReactDOM？ 它是用来干嘛的
> 5. React渲染器是用来干嘛的


## React Native

### 渲染器（renderer）和新的 React

对于Web应用来说，React 负责启用 React 范式（管理响应式 UI、组件和虚拟 DOM），以及实际更新浏览器中的DOM

但如果还考虑IOS和安卓端，就不行了。

所以React进行了拆分：
- 一部分是新的React， 只负责启用React范式
- 第二部分叫做ReactDOM，唯一的任务就是和浏览器中的DOM进行交互（因为ReactDOM负责更新DOM，而DOM又决定了浏览器渲染的内容，所以将ReactDOM称为**渲染器**）

### 一个完整的平台

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

# PART3

> - 什么是 prop
> - 什么是 state



- prop 是每个组件的固有属性，不可改变
- state 是组件的私有数据，组件创建后才可以使用它



