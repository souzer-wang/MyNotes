在使用 ES6 的 class 定义 react 组件的时候，经常看到如下代码：
```javascript
constructor(props) {
  super(props);
  ......
}
```

#### 梳理概念

1. constructor() -- 构造方法

- 这是 ES6 对类的默认方法，在创建对象实例时自动调用该方法。
- 该方法是类中必须有的，如果没有定义，则默认添加空的 constructor 方法

2. super() -- 继承

在 class 方法中，继承是使用 extends 关键字来实现的，子类必须在 constructor() 中调用 super 方法，否则新建实例时会报错。

#### 两个问题：

1. 是否必须要用 super() ？

> 答：只要存在 constructor 就要使用 super()

但并不是每个 react 组件都需要 constructor，下面的代码可以正常运行
```javascript
class App extends React.Component {
  render() {
    return (
      <h1>{this.props.text}</h1>
    );
  }
}
```

也就是说，如果不使用 constructor，就不用使用 super

2. 什么时候必须调用 super(props) ?

> 答： 只要在constructor 中访问 this.props 了就需要

从上面的代码可以看出，即便没有 constructor，也可以在 render 中使用 this.props

> 因为 react 在初始化类后，会将 props 自动设置到 this 中，所以可以在任何地方都直接访问 this.props，除了 constructor

举例：

```javascript
constructor(props) {
  super();
  console.log(this.props); --- undefined
}
```

这段代码在 constructor 中访问 this.props 时，才需要设置 `super(props)`

**综上，总结一下：**
1. 只要使用了 constructor，就要调用 super()
2. 只要想在 constructor 中使用this.props，就要调用 super(props)