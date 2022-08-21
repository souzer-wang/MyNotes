# 1. React 路由概念

## 1.1 SPA 应用

1. 单页Web应用，single page web application 简称 SPA

2. 整个应用只有一个完整的页面

3. 点击页面中的链接不会刷新整个页面，只会对特定的部分进行更新

4. 数据都需要通过ajax请求获取，然后在前端异步显示



## 1.2 路由Route的理解

### 1.2.1 什么是路由

1. 路由可以理解为一个key value 的映射关系

2. 在路由中，key指的是路径，value指的是一个方法（function）或者组件（Component）

### 1.2.2 路由Route的分类

#### 后端路由：

1. 后端路由的value是 function，用来处理客户端提交的请求

2. 注册路由：router get(path, function(req, res))

3. 工作流程：当node收到一个请求，根据请求路径找到匹配的路由，调用路由中的函数来处理请求，返回响应数据

#### 前端路由：

1. 浏览器端路由，value 是组件 component，用于展示页面内容

2. 注册路由，略

3. 工作流程：举栗，当浏览器的path为 /login，当前的路由组件就会变成Login组件

## 1.3 react-router-dom

### 1.3.1 这是啥

1. 这是react的一个插件库

2. 专门用来实现一个SPA应用

3. 基于react的项目基本都会用

### 1.3.2 react-router-dom 相关的API

1. <BrowserRoute>

```javascript
Bx
```
