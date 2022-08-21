在创建一个react项目的过程中，我们需要安装node.js（同时会安装npm），然后我们会使用以下三种指令来创建一个项目

```shell
npx create-react-app my-app

npm init react-app my-app

yarn create react-app my-app
```

那么，什么是npm，什么是create-react-app，什么是yarn，什么是以上出现的等等等呢

### 什么是 npm

npm是node.js框架中的包管理工具

### 什么是 npx

npx 是在 npm v5.2.0 引入的一条命令，目的是提高从 npm 注册表使用软件包的体验。

**重点**： 就像npm极大提升了我们安装和管理包依赖的体验，npx在npm的基础上，让npm包中的**命令行工具和其它可执行文件在使用上变得更加简单**。它**简化了**我们之前在**使用npm时所需要的大量步骤**。

举个栗子，当我们创建react项目的时候

老方法：

```shell
npm install -g create-react-app //安装create-ract-app包
create-react-app my-app
```

新方法：

```shell
npx create-react-app my-app
```

这条命令会临时安装create-react-app包，命令完成后会把它删掉，如果下次还执行的话，会重新临时安装

### 什么是 create-react-app

一个npm发布的安装包，可以理解为一个创建react项目的脚手架工具

### 什么是 yarn

facebook发布的一款可以取代npm的包管理工具

优点：速度快，安全，可靠

常用命令见：[yarn的安装和使用_yw00yw的博客-CSDN博客_yarn安装](https://blog.csdn.net/yw00yw/article/details/81354533)

### 什么是 less

less 是一门CSS预处理语言，对 CSS 进行了扩展，更易维护和扩展

- css可以直接被浏览器识别，但是less需要先编译为css

### 什么是 webpack

webpack 是一种 **模块打包器技术**，通过分析项目结构找到JavaScript模块和一些浏览器不能直接运行的扩展语言或者资源，将其转化为合适的格式，供浏览器解析。

它的工作方式如下：

1. 从index.js入手，从这个文件开始，查找项目的所有依赖文件

2. 使用加载器（loaders）进行处理

3. 最后打包为一个或者多个可以让浏览器解析的JavaScripts脚本文件



### 什么是 JSX

JSX就是JavaScript XML的缩写，就是基于JavaScript的XML，通过下面这个例子来进行理解：

```javascript
const reactSpan = (
    <span>
        <h3>React JSX</h3>
        <p>Create React DOM by React JSX.</p>
    </span>
);
```

那么这个例子里面，完整的这段代码就是JSX代码，实现了一个虚拟DOM对象，通过const关键字定义了一个常量（reactSpan）

- 中间的四行代码符合XML格式

- 完整的六行代码符合JavaScript语法

所以这段段代码的语法就是 JSX




