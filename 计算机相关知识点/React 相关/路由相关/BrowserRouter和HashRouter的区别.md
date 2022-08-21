# HashRouter



HashRouter使用的是URL的hash部分，即 windows.location.hash，来保持页面的ui和url的同步



> basename: string

这个代表所有位置的基本url，格式正确的基本名称都有一个前导斜杠 “/”，但没有尾部斜杠

```jsx
<HashRouter basename="/home">
<Link to="/today"/>
```



> getUserConfirmation: func

用户确认导航的功能，默认使用window.confirm

```jsx
const getConfirmation = (message, callback) => {
  const allowTransition = window.confirm(message);
  callback(allowTransition);
}

<HashRouter getUserConfirmation={getConfirmation }/>
```



> hashType: string

用于windows.location.hash 的编码类型，取值范围为

- slash: #/

- noslash: #

- hashbang: #!

默认slash



# BrowseRouter

使用HTML5的history API（pushState, replaceState和popState），让页面的UI与URL同步。

```jsx
import  { BrowserRouter }  form 'react-router-dom';
< BrowserRouter 
  basename = { optionalString } 
  forceRefresh = { optionalBool } 
  getUserConfirmation = { optionalFunc }
  keyLength = { optionalNumber } 
>
  < App />
</ BrowserRouter >
```



# 二者的区别

- **URL的表现形式不一样**  
  
  - BrowseRouter使用HTML5的history API，保证UI界面和URL同步。
  
  - HashRouter使用URL的哈希部分来保持UI和URL的同步。哈希历史记录不支持location.key和location.state。

- **HashRouter不支持location.state和location.key**
