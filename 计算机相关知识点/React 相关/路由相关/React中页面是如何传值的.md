原文链接：[React页面传值的做法 - 简书 (jianshu.com)](https://www.jianshu.com/p/33ae1c9c992e)

### 一、props.params 方式

使用一个 <Route/> 来指定一个 path，然后携带参数到指定的path

1. 先引入路由相关的组件

```javascript
import { BrowserRouter as Router, Route } from 'react-router-dom'
```

2. 设定路由的逻辑

```jsx
function App() {
  return (
      <Router>
          <Route path="/home/user/:name" component={User}/>=
      </Router>
  );
}
```

3. 到User对应的页面取出相应的值

```jsx
export default function User(props) {
    return (
        <div>
            {props.match.params.name}
        </div>
    );
};
```





### 二、query

props.params的传值方法比较方便但是有一个缺点就是无法传递对象类型的数据，而query可以传递任意类型的数据，其使用过程如下：

1. 首先定义路由

```xml
<Route path="/home/user" component={User}/>
```

2. 然后编写跳转页面的行为

```jsx
const handleClick = () => {
    let data = {firstName: 'HF', lastName: 'R'};
    let path = {
        pathname: '/home/user',
        query: data,
    };
    props.history.push(path);
};
```

3. 跳转到User页面后需要接收数据

```xml
<div>
    {props.location.query.firstName}
</div>
```



### 三、state

state方式和query方法很像，只需要在query的基础上修改两个地方，暂时还没用到，案例见参考文章，以后用到了再补充
