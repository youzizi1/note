## React 16

React16又称React Fiber。

> React只兼容IE8及以上。

## 环境搭建

通过`create-react-app`脚手架搭建React项目。

* `npm run eject`：弹射配置文件
* `npm run test`：自动执行测试用例

## props

### props验证

首先需要安装`prop-types`。

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import PropTypes from 'prop-types'
class TodoApp extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
      msg: 'hello world'
    }
  }
  
  render() {
    return (
      <div>
        <TodoItem msg={this.state.msg} />
      </div>
    )
  }
}

class TodoItem extends React.Component {
  static defaultProps = {
    msg: 'hello'
  }

  static propTypes = {
    msg: PropTypes.string.isRequired
  }

  render() {
    return (
      <div>
        {this.props.msg}
      </div>
    )
  }
  
}

ReactDOM.render(<TodoApp />, document.getElementById('root'));
```

当然，你也可以在类上使用。

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import PropTypes from 'prop-types'
class TodoApp extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
      msg: 'hello world'
    }
  }
  
  render() {
    return (
      <div>
        <TodoItem msg={this.state.msg} />
      </div>
    )
  }
}

class TodoItem extends React.Component {
  render() {
    return (
      <div>
        {this.props.msg}
      </div>
    )
  }
}

TodoItem.defaultProps = {
  msg: 'hello'
}

TodoItem.propTypes = {
  msg: PropTypes.string.isRequired
}

ReactDOM.render(<TodoApp />, document.getElementById('root'));
```

可以检测的类型有：

* 数组：PropTypes.array
* 布尔类型：PropTypes.bool
* 函数类型：PropTypes.func
* 数字类型：PropTypes.number
* 对象类型：PropTypes.object
* 字符串类型：PropTypes.string
* symbol类型：PropTypes.symbol

还可以通过`arrayOf`和`objectOf`检测多重嵌套类型。

```jsx
import React from "react";
import ReactDOM from "react-dom";
import PropTypes from "prop-types";
class TodoApp extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      tasks: ["study", "sleep", "read"]
    };
  }

  render() {
    return (
      <div>
        <TodoItem tasks={this.state.tasks} />
      </div>
    );
  }
}

class TodoItem extends React.Component {
  render() {
    return (
      <div>
        {this.props.tasks.map((item, index) => {
          return <p key={index}>{item}</p>;
        })}
      </div>
    );
  }
}

TodoItem.defaultProps = {
  tasks: ["study"]
};

TodoItem.propTypes = {
  // 数组的元素类型必须全是string类型
  tasks: PropTypes.arrayOf(PropTypes.string)
};

ReactDOM.render(<TodoApp />, document.getElementById("root"));
```

当然还可以通过`shape`来检测对象的不同属性具有不同的类型。

```jsx
import React from "react";
import ReactDOM from "react-dom";
import PropTypes from "prop-types";
class TodoApp extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      userInfo: {
        name: 'ugu',
        age: 10
      }
    };
  }

  render() {
    return (
      <div>
        <TodoItem userInfo={this.state.userInfo} />
      </div>
    );
  }
}

class TodoItem extends React.Component {
  render() {
    return (
      <div>
        {this.props.userInfo.name}
      </div>
    );
  }
}

TodoItem.propTypes = {
  userInfo: PropTypes.shape({
    name: PropTypes.string,
    age: PropTypes.number
  })
};

ReactDOM.render(<TodoApp />, document.getElementById("root"));
```

## state

### 初始化

构造函数是唯一可以分配state的地方。

### 操作state

不能直接操作`this.state`，而是通过`this.setState()`方法来操作。

```jsx
class TodoApp extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
    	msg: 'hello world'
    }
    
    this.handleClick = this.handleClick.bind(this)
  }
  
  handleClick() {
  	this.setState({
    	msg: 'ugu'
    })
  }
  
  render() {
    return (
   		<div>
   		  <div class="title">{this.state.msg}</div>
        <button onClick={this.handleClick}>click</button>
   		</div>
    )
  }
}

ReactDOM.render(<TodoApp />, document.querySelector("#app"))
```

在React16中，建议使用`setState`的函数用法来改变state，而不是使用对象用法。

```jsx
class TodoApp extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
    	msg: 'hello world'
    }
    
    this.handleClick = this.handleClick.bind(this)
  }
  
  handleClick() {
  	this.setState(preState => {
        return {
            msg: 'ugu' + preState.msg
        }
    })
  }
  
  render() {
    return (
   		<div>
   		  <div class="title">{this.state.msg}</div>
        <button onClick={this.handleClick}>click</button>
   		</div>
    )
  }
}

ReactDOM.render(<TodoApp />, document.querySelector("#app"))
```

### 和props的区别

* state是组件自己管理数据，可变
* props是外部传入的数据，不可变
* 没有state的组件称为无状态组件，可以写成函数形式
* 多用props，少用state，也就是多写无状态组件

### 异步更新

React 为了优化性能，有可能会将多个 setState() 调用合并为一次更新。
 因为`this.props`和`this.state` 可能是异步更新的，你不能依赖他们的值计算下一个`state`(状态)。

```kotlin
this.setState({
  counter: this.state.counter + this.props.increment,
});
```

我们并不能通过上述代码得到想要的值，为了弥补这个问题，使用另一种 setState() 的形式，接受一个函数。这个函数将接收前一个状态作为第一个参数，应用更新时的 props 作为第二个参数。

```jsx
this.setState((prevState, props) => ({
  counter: prevState.counter + props.increment
}));
```

另外，当你调用 setState()， React 将合并你提供的对象到当前的状态中。所以当State是一个多键值的结构时，可以单独更新其中的一个，此时会进行“差分”更新，不会影响其他的属性值。 

### setState进阶

`setState()`方法通过一个队列机制实现`state`更新，当执行`setState()`的时候，会将需要更新的`state`合并之后放入状态队列，而不会立即更新`this.state`(可以和浏览器的事件队列类比)。如果我们不使用`setState`而是使用`this.state.key`来修改，将不会触发组件的`re-render`。如果将`this.state`赋值给一个新的对象引用，那么其他不在对象上的`state`将不会被放入状态队列中，当下次调用`setState()`并对状态队列进行合并时，直接造成了`state`丢失。

另外，`setState()` 不仅能够接受一个对象作为参数，还能够接受一个函数作为参数。函数的参数即为 `state` 的前一个状态以及 `props`。
 React文档中对setState的说明如下：

```tsx
void setState (
      function|object nextState,
      [function callback]
)
```

上述代码的第二个参数是一个回调函数，在`setState()` 的异步操作结束并且组件已经重新渲染的时候执行。换句话说，我们可以通过这个回调来拿到更新的`state`的值。

```js
updateData = (newData) => {
    this.setState(
        { data: newData },
        () => {
            // 最新的state值
            // 这里还可以进行DOM操作，因为此时DOM已经更新
            console.log(this.state.data);
        }
    );
}
```

注意：setState不一定是异步更新，也有可能是同步更新。简单来说，由 React 控制的事件处理过程 setState 不会同步更新 this.state。而在 React 控制之外的情况， setState 会同步更新 this.state。

### 表单绑定

当将表单值绑定成`state`的一个属性时，需要配合`onChange`事件才能修改值。

## 生命周期

生命周期函数是指在某一时刻组件会自动调用执行的函数。

注意：constructor是ES6 Class的特性，虽然也会自动执行，但是不属于生命周期函数。

![](./images/life_circle.jpg)

#### 实践

**shouldComponentUpdate**

在组件更新之前触发，必须返回一个布尔值，true表示更新，false表示不更新。

**componentWillReceiveProps**

componentWillReceiveProps在初始化render的时候不会执行，它会在Component接受到新的状态(Props)时被触发，一般用于父组件状态更新时子组件的重新渲染。 

```js
componentWillReceiveProps(nextProps) {
    // nextProps代表父组件传入的新的props
}
```

**componentDidMount**

常用来异步获取数据。

### 新特性

* React16废弃了componentWillMount、componentWillReceivePorps，componentWillUpdate，并且新增了getDerivedStateFromProps、getSnapshotBeforeUpdate来分别替代上面这三个钩子函数
* React16并没有删除这三个钩子函数，而是不能与新增的钩子函数混用，在17版本将会删除
* 新增了componentDidCatch来进行错误处理

## JSX

使用JSX语法之前一定要引入`react`库。

### 注释

JSX中的注释`{/* */}`。

### 事件绑定

* onclick：DOM事件
* onClick：JSX事件

注意：事件绑定的回调函数中this推荐在`constructor`中进行绑定。

### fragment

fragment类似于Vue的template标签。

### dangerouslySetInnerHTML

类似于Vue的`v-html`。 

### render函数

你可以通过三元运算符实现条件渲染。

```jsx
class App extends React.Component {
    // ...
    render() {
        return <div>
            {this.state.flag ? <span>hello world</span> : null}
        </div>
    }
}
```

注意：state和props改变的时候，render函数都会重新执行。

### ref

通过ref属性可以获取DOM。

```react
class Demo extends React.Component {
  constructor(props) {
      super(props)
      
   	  
  }
  handleClick() {
      
  }
    
  render() {
      return <div ref="msgBox" onClick={this.handleClick.bind(this)}>hello world</div>;
  }
}

// 第二种方式
class Demo extends React.Component {
  componentDidMount() {
    console.log(this.msgBox);		// 获取DOM
  }
  render() {
      return <div ref={msgBox => this.msgBox = msgBox}>hello world</div>;
  }
}
```

当然上面两种方式还可以通过ref属性获取组件的实例。

## 组件

### 组件分类

根据组件的定义方式可以分为：

* 函数组件：使用函数定义的组件，它只是一个返回React元素的函数，没有状态维护和生命周期，只关注UI的展示
* 类组件：使用ES6 Class定义，可以维护组件自身的状态，以及拥有完整的生命周期

在实际开发中，如果一个组件不需要管理自身状态，可以设计成函数组件，性能消耗更低。

根据组件内部是否需要维护自身state可以分为：

* 无状态组件：无状态组件建议写成函数组件
* 有状态组件

根据组件的职责可以分为：

* UI组件：只负责页面渲染。UI组件一般是无状态组件，建议写成函数组件
* 容器组件：需要处理数据和业务逻辑，一般是有状态组件

注意：在实际开发中，建议将组件抽离成UI组件和容器组件。

```jsx
// UI组件
const ComponentUI = props => {
    return jsx
}

// 容器组件
class Component extends React.Component {
    render() {
        return <ComponentUI />
    }
}
```

### 组件通信

通过`props`可以实现父传子。

```jsx
class TodoApp extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
    	msg: 'hello world'
    }
  }
  // 父传子
  render() {
    return (
   		<div>
   		  <TodoItem msg={this.state.msg} />
   		</div>
    )
  }
}

class TodoItem extends React.Component {
	constructor(props) {
    super(props)
  }
  // 子通过this.props接收
  render() {
  	return <div>{this.props.msg}</div>
  }
  
}

ReactDOM.render(<TodoApp />, document.querySelector("#app"))
```

当然你还可以通过自定义事件来子传父。

```jsx
class TodoApp extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
    	msg: 'hello'
    }
    
    this.handleEmitMsg = this.handleEmitMsg.bind(this)
  }
  
  handleEmitMsg(msg) {
  	this.setState(preState => {
    	return {
      	msg
      }
    })
  }
  // 监听事件
  render() {
    return (
      <div>
        {this.state.msg}
        <TodoItem emitMsg={this.handleEmitMsg} />
      </div>
    )
  }
}

class TodoItem extends React.Component {
	constructor(props) {
    super(props)
    
    this.handleClick = this.handleClick.bind(this)
  }
  
  handleClick() {
    // 发射事件
  	this.props.emitMsg('hello world')
  }
  
  render() {
    return (
      <div>
        <button onClick={this.handleClick}>click</button>
      </div>
    )
  }
}

ReactDOM.render(<TodoApp />, document.querySelector("#app"))
```

### 高阶组件

类似于高阶函数，输入一个函数，输出一个新函数。高阶组件（HOC，Higher Order Component）就是接收一个组件，输出一个新组件。它是复用组件逻辑的高级技术，不是React API的一部分。

```react
import React from "react";
import ReactDOM from "react-dom";

class MyComponent extends React.Component {
  render() {
    return <div>hello world</div>;
  }
}

const HOC = WrappedComponent => {
  // 可以不写类名
  return class extends React.Component {		
    render() {
      return <WrappedComponent />;
    }
  };
};

const NewComponent = HOC(MyComponent);

class App extends React.Component {
  render() {
    return <NewComponent />;
  }
}

ReactDOM.render(<App />, document.getElementById("root"));
```

注意：这里接收的组件可以是一个组件的实例，也可以是一个组件类，还可以是一个无状态组件。

#### 实现方式

React中有两种HOC的实现方式：Props Proxy以及Inheritance Inversion。

* 代理方式
  * 操作prop
  * 访问ref（不推荐）
  * 抽象状态
  * 包装组件
* 继承方式
  * 渲染劫持
  * 操作state

#### 代理方式

##### 操作prop

```react
import React from "react";
import ReactDOM from "react-dom";

class MyComponent extends React.Component {
  render() {
    return <div>hello world {this.props.msg}</div>;
  }
}

const HOC = WrappedComponent => {
  return class extends React.Component {		
    render() {
      return <WrappedComponent {...this.props} />;
      /*
      	等价于
      	React.createElement(WrappedComponent, this.props, null)
      */
    }
  };
};

const NewComponent = HOC(MyComponent);

class App extends React.Component {
  render() {
    return <NewComponent msg="ugu" />;
  }
}

ReactDOM.render(<App />, document.getElementById("root"));
```

##### 访问ref

通过代理方式实现HOC，还可以操作原组件实例。

```react
import React from "react";
import ReactDOM from "react-dom";

class MyComponent extends React.Component {
  sayName() {
    console.log("ugu");
  }
  render() {
    return <div>hello world</div>;
  }
}

const HOC = WrappedComponent => {
  return class extends React.Component {
    proc(instance) {
      // instance就是原组件实例，可以直接调用组件实例上的属性和方法
      instance.sayName();
      // 如果有props值，可以直接获取；对于state也是如此
      console.log(instance.props)
    }
    render() {
      return <WrappedComponent ref={this.proc} />;
    }
  };
};

const NewComponent = HOC(MyComponent);

class App extends React.Component {
  render() {
    return <NewComponent />;
  }
}

ReactDOM.render(<App />, document.getElementById("root"));

```

##### 抽象状态

```react
import React from "react";
import ReactDOM from "react-dom";

class MyInput extends React.Component {
  render() {
    return (
      <input value={this.props.value} onChange={this.props.handleChange} />
    );
  }
}

class MyTextarea extends React.Component {
  render() {
    return (
      <textarea
        value={this.props.value}
        onChange={this.props.handleChange}
      ></textarea>
    );
  }
}

const HOC = WrappedComponent => {
  return class extends React.Component {
    constructor(props) {
      super(props);

      this.state = {
        value: ""
      };

      this.handleChange = this.handleChange.bind(this);
    }

    handleChange(e) {
      this.setState({
        value: e.target.value
      });
    }

    render() {
      const newProps = {
        value: this.state.value,
        handleChange: this.handleChange
      };
      return <WrappedComponent {...this.props} {...newProps} />;
    }
  };
};

const NewInput = HOC(MyInput);
const NewTextarea = HOC(MyTextarea);

class App extends React.Component {
  render() {
    return (
      <div>
        <NewInput />
        <NewTextarea />
      </div>
    );
  }
}

ReactDOM.render(<App />, document.getElementById("root"));
```

在上述代码中，之前需要在原组件中定义的`value`和`handleChange`，被抽离出来。

##### 包装组件

```react
import React from "react";
import ReactDOM from "react-dom";

class MyComponent extends React.Component {
  render() {
    return <div>hello world</div>;
  }
}

const HOC = WrappedComponent => {
  return class extends React.Component {
    render() {
      return (
        <div style={{ color: "red" }}>			
          <WrappedComponent {...this.props} />
        </div>
      );
    }
  };
};

const NewComponent = HOC(MyComponent);

class App extends React.Component {
  render() {
    return <NewComponent />;
  }
}

ReactDOM.render(<App />, document.getElementById("root"));
```

#### 继承方式

##### 渲染劫持

通过渲染劫持你可以：

* 读取，添加，修改和删除任何一个将被渲染的React Element的props

  ```react
  import React from "react";
  import ReactDOM from "react-dom";
  
  class MyComponent extends React.Component {
    render() {
      return <div>hello world</div>;
    }
  }
  
  const HOC = WrappedComponent => {
    return class extends WrappedComponent {
      render() {
        // 修改props
        const elementTree = super.render();
  
        const styleProp = {
          style: { color: "red" }
        };
        const newProps = Object.assign({}, elementTree.props, styleProp);
  
        const newElementTree = React.cloneElement(
          elementTree,
          newProps
        );
  
        return newElementTree;
      }
    };
  };
  
  const NewComponent = HOC(MyComponent);
  
  class App extends React.Component {
    render() {
      return <NewComponent msg="ugu" />;
    }
  }
  
  ReactDOM.render(<App />, document.getElementById("root"));
  ```

* 在render方法中读取或修改React Element Tree

  ```react
  import React from "react";
  import ReactDOM from "react-dom";
  
  class MyComponent extends React.Component {
    render() {
      return (
        <div>
          <span>hello world</span>
        </div>
      );
    }
  }
  
  const HOC = WrappedComponent => {
    return class extends WrappedComponent {
      render() {
        const elementTree = super.render();
  
        const newProps = Object.assign({}, elementTree.props, {
          style: { color: "red" }
        });
  
        const newElementTree = React.cloneElement(elementTree, newProps);
        return newElementTree;
      }
    };
  };
  
  const NewComponent = HOC(MyComponent);
  
  class App extends React.Component {
    render() {
      return <NewComponent msg="ugu" />;
    }
  }
  
  ReactDOM.render(<App />, document.getElementById("root"));
  ```

* 根据条件不同，选择性的渲染子树

  ```react
  import React from "react";
  import ReactDOM from "react-dom";
  
  class MyComponent extends React.Component {
    render() {
      return <div>hello world</div>;
    }
  }
  
  const HOC = WrappedComponent => {
    return class extends WrappedComponent {
      render() {
        // isLogin为true才渲染，否则不渲染
        if (this.props.isLogin) {
          return super.render();
        }
        return null;
      }
    };
  };
  
  const NewComponent = HOC(MyComponent);
  
  class App extends React.Component {
    render() {
      return <NewComponent isLogin={true} />;
    }
  }
  
  ReactDOM.render(<App />, document.getElementById("root"));
  ```

##### 操作state

```react
import React from "react";
import ReactDOM from "react-dom";

class MyComponent extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      msg: "hello world"
    };
  }

  render() {
    return <div>{this.state.msg}</div>;
  }
}

const HOC = WrappedComponent => {
  return class extends WrappedComponent {
    render() {
      // 因为匿名组件没有重写构造函数，所以this指向WrappedComponent，这样就可以直接获取state
      console.log(JSON.stringify(this.state));
      return super.render();
    }
  };
};

const NewComponent = HOC(MyComponent);

class App extends React.Component {
  render() {
    return <NewComponent isLogin={true} />;
  }
}

ReactDOM.render(<App />, document.getElementById("root"));
```

### 异步组件

 通过lazy和Suspense组件我们就可以实现异步组件。

> 当然你也可以使用第三方库react-loadable来实现异步组件。

### 子组件

React.Children提供了处理this.props.children的工具，具体如下：

* React.Children.map()遍历
* React.Children.forEach()遍历
* React.Children.count()获取子组件个数
* React.Children.only()验证是否只有唯一的子组件并返回，否则报错
* React.Children.toArray()将子组件集合转换为数组

```jsx
import React from "react";
import ReactDOM from "react-dom";

const List = props => {
  // 克隆子元素，并且都增加class属性
  return (
    <div>
      {React.Children.map(props.children, child => {
        return React.cloneElement(child, {
          className: "list-item"
        });
      })}
    </div>
  );
};

const App = () => {
  return (
    <List>
      <p>study</p>
      <p>sleep</p>
    </List>
  );
};

ReactDOM.render(<App />, document.getElementById("root"));

```

## 动画

无关React，你可以通过加载不同的类来实现CSS动画。当然，你还可以使用`react-transition-group`库实现更复杂的JavaScript动画。

## CSS解决方案

**直接在组件中使用style**

```react
import React from "react";
import ReactDOM from "react-dom";

const style = {
  color: 'red',
  fontSize: '20px'
}

class App extends React.Component {
  render() {
    return (
      <div style={style}>hello world</div>
    );
  }
}

ReactDOM.render(<App />, document.getElementById("root"));
```

这种方式只作用于当前组件。

**组件中直接引入[name].css|scss文件**

```css
.msg {
  border: 1px solid red;
}
```

```js
import React from "react";
import ReactDOM from "react-dom";
import './index.css'

class App extends React.Component {
  render() {
    return (
      <div className="msg">
        <h3>ugu</h3>
        <Msg />
      </div>
    );
  }
}
// 有边框
const Msg = () => {
  return <div className="msg">hello world</div>
}

ReactDOM.render(<App />, document.getElementById("root"));
```

这种方式会作用于当前组件以及后代组件。

**在组件中引入[name].module.css|scss文件**

这种方式属于CSS modules解决方案。

```css
.msg {
  border: 1px solid red;
}
```

```js
import React from "react";
import ReactDOM from "react-dom";
import appCss from './index.module.css'

class App extends React.Component {
  render() {
    return (
      <div className={appCss.msg}>
        <h3>ugu</h3>
        <Msg />
      </div>
    );
  }
}
// 没有边框
const Msg = () => {
  return <div className="msg">hello world</div>
}

ReactDOM.render(<App />, document.getElementById("root"));
```

这种方式只作用于当前组件。

**使用styled-components**

这种方式属于CSS-In-JS解决方案。

```react
import React from "react";
import ReactDOM from "react-dom";
import styled from "styled-components";

class App extends React.Component {
  render() {
    return (
      <LoginComponent>
        <PasswordTitle>账号</PasswordTitle>
        <PasswordInput size="1em" />
      </LoginComponent>
    );
  }
}

const LoginComponent = styled.div`
  width: 200px;
  height: 100px;
  padding: 10px;
  border-radius: 5px;
  border: 1px solid #ccc;
`;
// 基于props做样式判断
const PasswordTitle = styled.h3`
  font-weight: normal;
  font-style: normal;
  font-size: 18px;
  color: ${props => props.color || 'blue'}
`;
// 为样式组件添加一些attr属性
const PasswordInput = styled.input.attrs(props => ({
  type: "password",
  mt: props => props.size || "0.5em"
}))`
  border: 1px solid #dedede;
  margin-top: ${props => props.mt};
`;

ReactDOM.render(<App />, document.getElementById("root"));
```

当然你还可以使用extend方法来扩展组件。

```react
const Button = styled.button`
  color: red
  border: 2px solid red;
  border-radius: 3px;
`;
// 继承并扩展
const BlueButton = Button.extend`
  color: blue;
`;

render(
  <div>
    <Button>Normal Button</Button>
    <BlueButton>Blue Button</BlueButton>
  </div>
);
```

当然在极少数情况下，你可能还需要修改样式组件的标签类型，你可以通过withComponent来实现。

```react
const Button = styled.button`
  color: red
  border: 2px solid red;
  border-radius: 3px;
`;
// 用a标签代替button标签
const Link = Button.withComponent('a')

// 继承并扩展
const BlueButton = Link.extend`
  color: blue;
`;

render(
  <div>
    <Button>Normal Button</Button>
    <BlueButton>Blue Button</BlueButton>
  </div>
);
```

另外，styled-components还提供一个keyframes来实现动画效果。

```react
import React from "react";
import ReactDOM from "react-dom";
import styled, {keyframes} from "styled-components";

class App extends React.Component {
  render() {
    return <Box>box</Box>
  }
}

const rotate360 = keyframes`
  from {
    transform: rotate(0deg);
  }

  to {
    transform: rotate(360deg);
  }
`;

const Box = styled.div`
  width: 100px;
  height: 100px;
  background-color: red;
  animation: ${rotate360} 2s linear infinite;
`

ReactDOM.render(<App />, document.getElementById("root"));
```

styled-components还暴露了一个`<ThemeProvider>`容器组件，提供了设置默认主题样式的功能。

```react
import React from "react";
import ReactDOM from "react-dom";
import styled, {ThemeProvider} from "styled-components";

// 定义一个公共的尺寸，颜色等全局配置
const theme = {
  primary: 'red'
}

class App extends React.Component {
  render() {
    return (
      <ThemeProvider theme={theme}>
        <Box>box</Box>
      </ThemeProvider>
    )
  }
}

const Box = styled.div`
  width: 100px;
  height: 100px;
  background-color: ${props => props.theme.primary};
`

ReactDOM.render(<App />, document.getElementById("root"));
```

通过`createGlobalStyle`可以注入全局样式。

```react
import React from "react";
import ReactDOM from "react-dom";
import styled, {createGlobalStyle} from "styled-components";
// 定义全局样式，并以组件的形式引入。一般定义到根组件中
const Globalstyle = createGlobalStyle`
  body {
    background-color: #eee;
  }
`

class App extends React.Component {
  render() {
    return (
      <div>
        <Globalstyle />
        <Box>box</Box>
      </div>
    )
  }
}

const Box = styled.div`
  width: 100px;
  height: 100px;
  background-color: red;
`

ReactDOM.render(<App />, document.getElementById("root"));
```

对于样式组件，你还可以通过ref来获取DOM节点。

```react
import React from "react";
import ReactDOM from "react-dom";
import styled from "styled-components";

class App extends React.Component {
  constructor(props) {
    super(props)

    this.handleClick = this.handleClick.bind(this)
  }
  render() {
    return <Box ref={box => this.box = box} onClick={this.handleClick}>box</Box>;
  }

  handleClick() {
    // 获取DOM
    console.log(this.box)
  }
}

const Box = styled.div`
  width: 100px;
  height: 100px;
  background-color: red;
`;

ReactDOM.render(<App />, document.getElementById("root"));
```

注意：使用图片要通过import来引入。

## 路由

react-router-dom是react的路由解决方案，之前的版本是react-router。

```jsx
import React from "react";
import ReactDOM from "react-dom";
import store from "./store";
import { Provider } from "react-redux";
import { BrowserRouter, Route, Link } from "react-router-dom";
import TodoList from "./todoList";
import Home from "./home";

ReactDOM.render(
  <Provider store={store}>
    <BrowserRouter>
      <header>
        <Link to="/">home</Link>
        <Link to="/todo">TodoList</Link>
        <Link to="/list">list</Link>
      </header>
      <Route path="/" exact component={Home} />
      <Route path="/todo" exact component={TodoList} />
      <Route path="/list" exact render={() => <div>list page</div>} />
    </BrowserRouter>
  </Provider>,
  document.getElementById("root")
);
```

### 路由传参

**params**

```jsx
import React from "react";
import ReactDOM from "react-dom";
import store from "./store";
import { Provider } from "react-redux";
import { BrowserRouter, Route, Link } from "react-router-dom";
import Home from "./home";
import List from "./list";

ReactDOM.render(
  <Provider store={store}>
    <BrowserRouter>
      <header>
        <Link to="/">home</Link>
        <Link to="/list/1">list</Link>
      </header>
      <Route path="/" exact component={Home} />
      <Route path="/list/:id" exact component={List} />
    </BrowserRouter>
  </Provider>,
  document.getElementById("root")
);
```

组件中通过`props.match.params.id`来获取参数。

**query**

```jsx
import React from "react";
import ReactDOM from "react-dom";
import store from "./store";
import { Provider } from "react-redux";
import { BrowserRouter, Route, Link } from "react-router-dom";
import Home from "./home";
import List from "./list";

ReactDOM.render(
  <Provider store={store}>
    <BrowserRouter>
      <header>
        <Link to="/">home</Link>
        <Link to={{ pathname: "/list", query: { id: 1 } }}>list</Link>
      </header>
      <Route path="/" exact component={Home} />
      <Route path="/list" component={List} />
    </BrowserRouter>
  </Provider>,
  document.getElementById("root")
);
```

通过`props.location.query.id`来获取参数。

### 重定向

```jsx
// 登录鉴权
```

### withRouter

## 数据管理

### 三大原则

* 单一数据源
* State是只读的
* reducer必须是纯函数

> 纯函数就是没有副作用的函数。

### redux

具体工作流程：

![](./images/redux.png)

```jsx
// index.js
import React from "react";
import ReactDOM from "react-dom";
import store from "./store";

class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      ...store.getState()
    };

    store.subscribe(() => {
      this.setState({
        ...store.getState()
      });
    });

    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    // 建议将action统一写入actionCreator.js中
    const action = {
      type: "changeMsg",
      value: "world hello"
    };
    store.dispatch(action);
  }

  render() {
    return (
      <div>
        {this.state.msg}
        <button onClick={this.handleClick}>change msg</button>
      </div>
    );
  }
}

ReactDOM.render(<App />, document.getElementById("root"));

// store.js
import { createStore } from "redux";
import reducer from "./reducer";

const store = createStore(reducer);

export default store;

// reducer.js
const defaultState = {
  msg: "hello world"
};

export default (state = defaultState, action) => {
  const { type, value } = action;
  switch (type) {
    case "changeMsg":
      // 不能直接修改state
      const newState = JSON.parse(JSON.stringify(state));
      newState.msg = value;
      return newState
    default:
      return state;
  }
};
```

### 异步数据

你可以在`componentDidMount`去获取数据。

```jsx
// index.js
import React from "react";
import ReactDOM from "react-dom";
import store from "./store";
import axios from 'axios'

class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      ...store.getState()
    };

    store.subscribe(() => {
      this.setState({
        ...store.getState()
      });
    });
  }

  componentDidMount() {
    axios.get('https://www.easy-mock.com/mock/5d8a3c44d81756519b445e23/tasks')
      .then(res=>{
        const action = {
          type: 'initTasks',
          value: res.data.data
        }
        store.dispatch(action)
      })

    
  }

  render() {
    return (
      <div>
        {this.state.tasks.length ? this.state.tasks.map((item, index) => {
        return <span key={index}>{item}</span>
        }): <span>暂无数据</span>}
      </div>
    );
  }
}

ReactDOM.render(<App />, document.getElementById("root"));


// store.js
import { createStore } from "redux";
import reducer from "./reducer";

const store = createStore(reducer);

export default store;

// reducer.js
const defaultState = {
  tasks: []
};

export default (state = defaultState, action) => {
  const { type, value } = action;
  switch (type) {
    case "initTasks":
      const newState = JSON.parse(JSON.stringify(state));
      newState.tasks = value;
      return newState
    default:
      return state;
  }
};
```

但是更建议通过中间件来处理异步数据。

### 中间件

redux中间件本质上就是对dispatch的封装。

#### redux-thunk

通过redux-thunk中间件来将获取异步数据抽离到action中统一管理，而不是写在业务组件中。

```jsx
// index.js
import React from "react";
import ReactDOM from "react-dom";
import store from "./store";
import { getTasksAction } from "./action";

class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      ...store.getState()
    };

    store.subscribe(() => {
      this.setState({
        ...store.getState()
      });
    });
  }

  componentDidMount() {
    // redux-thunk中间不但可以dispatch一个对象，还可以dispatch一个函数
    // 并且这个函数会自动执行
    store.dispatch(getTasksAction())
  }

  render() {
    return (
      <div>
        {this.state.tasks.length ? (
          this.state.tasks.map((item, index) => {
            return <span key={index}>{item}</span>;
          })
        ) : (
          <span>暂无数据</span>
        )}
      </div>
    );
  }
}

ReactDOM.render(<App />, document.getElementById("root"));

// store.js
import { createStore, applyMiddleware } from "redux";
import reducer from "./reducer";
import thunk from 'redux-thunk'

const store = createStore(reducer,applyMiddleware(thunk));

export default store;

// action.js
import axios from 'axios'

export const initTaskAction = value => {
  return {
    type: 'initTasks',
    value
  }
}

export const getTasksAction = () => {
  return dispatch => {
    axios.get('https://www.easy-mock.com/mock/5d8a3c44d81756519b445e23/tasks')
      .then(res=>{
        const action = initTaskAction(res.data.data)
        dispatch(action)
      })
  }
}

// reducer.js
const defaultState = {
  tasks: []
};

export default (state = defaultState, action) => {
  const { type, value } = action;
  switch (type) {
    case "initTasks":
      const newState = JSON.parse(JSON.stringify(state));
      newState.tasks = value;
      return newState
    default:
      return state;
  }
};
```

#### redux-saga

```jsx
// index.js
import React from "react";
import ReactDOM from "react-dom";
import store from "./store";

class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      ...store.getState()
    };

    store.subscribe(() => {
      this.setState({
        ...store.getState()
      });
    });
  }

  componentDidMount() {
    store.dispatch({type: 'initTasks'})
  }

  render() {
    return (
      <div>
        {this.state.tasks.length ? (
          this.state.tasks.map((item, index) => {
            return <span key={index}>{item}</span>;
          })
        ) : (
          <span>暂无数据</span>
        )}
      </div>
    );
  }
}

ReactDOM.render(<App />, document.getElementById("root"));

// store.js
import { createStore, applyMiddleware } from "redux";
import reducer from "./reducer";
import createSagaMiddleware from 'redux-saga'
import saga from './saga'

const sagaMiddleware = createSagaMiddleware()

const store = createStore(reducer,applyMiddleware(sagaMiddleware));

sagaMiddleware.run(saga)

export default store;

// action.js
export const getTasksAction = value => ({
  type: "getTasks",
  value
});

// reducer.js
const defaultState = {
  tasks: []
};

export default (state = defaultState, action) => {
  const { type, value } = action;
  switch (type) {
    case "getTasks":
      return {
        ...state,
        tasks: value
      }
    default:
      return state;
  }
};

// saga.js
import { takeEvery, put } from "redux-saga/effects";
import axios from "axios";
import { getTasksAction } from "./action";

function* mySaga() {
  yield takeEvery("initTasks", initTasks);
}

function* initTasks() {
  const res = yield axios.get(
    "https://easy-mock.bookset.io/mock/5dcc4d57bd94c34583b56b19/getTasks"
  );
  const action = getTasksAction(res.data.tasks);
  yield put(action);
}

export default mySaga;
```

#### Redux DevTools

单独使用Redux DevTools。

```js
import { createStore } from "redux";
import reducer from "./reducer";

const store = createStore(reducer,window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__());

export default store;
```

当然你也可以配合其它中间件使用。

```js
// redux-saga
import { createStore, applyMiddleware, compose } from "redux";
import reducer from "./reducer";
import createSagaMiddleware from "redux-saga";
import saga from "./saga";

const sagaMiddleware = createSagaMiddleware();

const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__
  ? window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__({})
  : compose;

const enhancer = composeEnhancers(applyMiddleware(sagaMiddleware));

const store = createStore(reducer, enhancer);

sagaMiddleware.run(saga);

export default store;

// redux-thunk
import { createStore, applyMiddleware, compose } from "redux";
import reducer from "./reducer";
import thunk from "redux-thunk";

const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__
  ? window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__({})
  : compose;

const enhancer = composeEnhancers(applyMiddleware(thunk));

const store = createStore(reducer, enhancer);

export default store;
```

#### 其它中间件

* redux-logger：记录redux日志信息

### react-redux

redux需要手动订阅store的状态变化，而react-redux可以自动帮我们处理。

```jsx
// index.js
import React from "react";
import ReactDOM from "react-dom";
import store from "./store";
import { Provider } from "react-redux";
import TodoList from "./TodoList";

ReactDOM.render(
  <Provider store={store}>
    <TodoList />
  </Provider>,
  document.getElementById("root")
);

// TodoList.js
import React from "react";
import { connect } from "react-redux";

class TodoList extends React.PureComponent {
  render() {
    const { props } = this;
    return (
      <div>
        <p onClick={props.handleClick}>{props.msg}</p>
      </div>
    );
  }
}

const stateToProps = state => ({
  msg: state.msg
});

const dispatchToProps = dispatch => ({
  handleClick() {
    const action = {
      type: 'changeMsg',
      value: 'world hello'
    }
    dispatch(action)
  }
});

export default connect(stateToProps, dispatchToProps)(TodoList);

// store.js
import { createStore } from "redux";
import reducer from "./reducer";

const store = createStore(reducer);

export default store;

// reducer.js
const defaultState = {
  msg: "hello world"
};

export default (state = defaultState, action) => {
  const { type, value } = action;
  switch (type) {
    case "changeMsg":
      return {
        ...state,
        msg: value
      };
    default:
      return state;
  }
};
```

当然复杂的项目你可能需要对reducer进行拆分。

```jsx
/*
	src/
		store/
			index.js
		todoList/
			store/
				index.js
				reducer.js
			todoList.js
		index.js
*/

// index.js
import React from "react";
import ReactDOM from "react-dom";
import store from "./store";
import { Provider } from "react-redux";
import TodoList from "./todoList/TodoList";

ReactDOM.render(
  <Provider store={store}>
    <TodoList />
  </Provider>,
  document.getElementById("root")
);

// todoList/store/index.js
// 可以暴露出reducer，action以及action-type
import reducer from "./reducer";

export default reducer;

// todoList/store/reducer.js
const defaultState = {
  msg: "hello world"
};

export default (state = defaultState, action) => {
  const { type, value } = action;
  switch (type) {
    case "changeMsg":
      return {
        ...state,
        msg: value
      };
    default:
      return state;
  }
};

// todoList/todoList.js 
import React from "react";
import { connect } from "react-redux";

class TodoList extends React.PureComponent {
  render() {
    const { props } = this;
    return (
      <div>
        <p onClick={props.handleClick}>{props.msg}</p>
      </div>
    );
  }
}

const stateToProps = state => ({
  msg: state.todoList.msg
});

const dispatchToProps = dispatch => ({
  handleClick() {
    const action = {
      type: 'changeMsg',
      value: 'world hello'
    }
    dispatch(action)
  }
});

export default connect(stateToProps, dispatchToProps)(TodoList);

// store/index.js
import { createStore, combineReducers } from "redux";
import todoListReducer from "../todoList/store";

const reducer = combineReducers({
  todoList: todoListReducer
});

const store = createStore(reducer);

export default store;
```

### immutable.js

通过immutable.js来确保reducer返回的是一个新的state。

```jsx
// index.js
import React from "react";
import ReactDOM from "react-dom";
import store from "./store";
import { Provider } from "react-redux";
import TodoList from "./todoList/TodoList";

ReactDOM.render(
  <Provider store={store}>
    <TodoList />
  </Provider>,
  document.getElementById("root")
);

// todoList/store/index.js
// 可以暴露出reducer，action以及action-type
import reducer from "./reducer";

export default reducer;

// todoList/store/reducer.js
import {fromJS} from 'immutable'

const defaultState = fromJS({
  msg: "hello world"		// 转换成不可变对象
})

export default (state = defaultState, action) => {
  const { type, value } = action;
  switch (type) {
    case "changeMsg":
      // 修改state，可以链式调用修改多个
      // 或者通过state.merge({msg: value})同时修改多个
      return state.set('msg', value)		
    default:
      return state;
  }
};

// todoList/todoList.js 
import React from "react";
import { connect } from "react-redux";

class TodoList extends React.PureComponent {
  render() {
    const { props } = this;
    return (
      <div>
        <p onClick={props.handleClick}>{props.msg}</p>
      </div>
    );
  }
}

const stateToProps = state => ({
  // state是不可变对象，获取属性需要使用get方法
  msg: state.get('todoList').get('msg')
  /*
  	等价于：
  	msg: state.getIn(['todoList', 'msg'])
  */
});

const dispatchToProps = dispatch => ({
  handleClick() {
    const action = {
      type: 'changeMsg',
      value: 'world hello'
    }
    dispatch(action)
  }
});

export default connect(stateToProps, dispatchToProps)(TodoList);

// store/index.js
import { createStore } from "redux";
import { combineReducers } from 'redux-immutable'	// 保证组件的中state是不可变对象
import todoListReducer from "../todoList/store";

const reducer = combineReducers({
  todoList: todoListReducer
});

const store = createStore(reducer);

export default store;
```

注意：如果使用redux-thunk或redux-sage来处理异步逻辑，那么也需要通过`fromJS`来将获取到数据转换为不可变对象。

另外：

* immutable数组可以直接map循环，但是不能通过下标获取对应元素，需要通过`toJS()`方法来转换JavaScript数组

## 虚拟DOM

在React中，模板+数据=真实DOM，其中模板是JSX，数据是state。每当数据改变，DOM就会重新渲染，性能很差。

实际上，你并不需要全局更新DOM，于是便有了第二种方案，比较数据变化前后的真实DOM，然后替换需要更新的DOM。这种方式虽然有性能提升，但是并不是很明显，因为比对DOM也有性能损耗。

于是出现了虚拟DOM，首次渲染真实DOM的时候，会生成一个与之对应的虚拟DOM。当数据再发生变化时，你只需要比对数据变化前后的虚拟DOM即可，然后将变化映射到真实DOM上。这种方式也是React性能优异的主要原因之一，当然虚拟DOM也使得跨端——例如RN得以实现。

注意：我们写的JSX语法，会先编译成`React.createElement`的形式，然后在渲染真实DOM之前会生成与之对应的虚拟DOM。之所以不直接写`createElement`是因为JSX写起来简单方便。

### Diff算法

当数据发生变化的时候，也就是调用`setState()`的时候，此时就会通过Diff算法来比较前后差异。

具体算法是：先进行DOM树同层比较，如果同一层节点不同，下层的所有节点就不会再进行比较。虽然会带来DOM更新上的开销，但是算法相对简单，复杂度会低很多。

注意：比对的过程中会根据key值比较，所以为了保证比对准确性，不建议使用index作为key值，因为index容易变化。例如：通过splice方法操作数组，这样index和元素就无法进行前后关联，从而带来额外的性能消耗。

## 调试

### react devtools

* 复现操作

## 性能优化

### Perf

你可以通过`shouldComponentUpdate()`来优化你的DOM diff算法，而Perf工具可以告诉你哪里需要设置`shouldComponentUpdate`。

```js
import Perf from 'react-addons-perf'

window.Perf = Perf
Perf.start()
```

Perf只是用来分析和监测页面性能，所以要用在开发环境。

### 具体优化

* 使用immutable.js
* 使用shouldComponentUpdate，当然你可以直接使用PureComponent，它内置了shouldComponentUpdate的实现。另外，使用PureComponent建议配合immutable来使用，否则会有一些意外的问题

## 生态

* dva：阿里开源的状态管理解决方案
* umi：react企业级脚手架工具
* antd：阿里开源的React UI组件库
* recharts：第三方React图表库