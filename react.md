# react 参考





## 目录

[TOC]

## react

> react 使用 api 



### 脚手架

* **Create-react-app(官方)**

  > https://github.com/facebook/create-react-app

  * 使用

    ```bash
    # using npm
    npm init react-app my-app
    
    # using yarn
    yarn create react-app my-app
    ```



### API

- **ReactDOM.render**

  > 在指定dom元素上渲染react元素

  ```jsx
  const element  = <img src={img.url} />
  const dom = document.querySelector('.pic')
  ReactDOM.render(element, dom)
  ```




### JSX

* **普通表达式**

  ```jsx
  const element = <div>hello world</div>
  ```

* **嵌入表达式**

  > 使用单层括号使用变量, 函数 { variable } 

  ```jsx
  const name = 'float'
  const userinfo = (person) => {
      return `person's name is ${person.name} age is ${person.age}`
  }
  
  const element = <div>hello, {name}</div>
  const element2 = <div>hello, {userinfo({name: 'float'})}</div>
  ReactDOM.render(element, document.querySelector('body'))
  ```



### Components and props

* **Functional components**

  > 函数组件

  ```jsx
  function Welcome (props) {
      return <div>hello  {props.name}</div>
  }
  
  const welcome = <Welcome name="float"></Welcome>
        
  ReactDOM.render(welcome, document.querySelector('body'))
  ```

* **class components**

  > 类组件

  ```jsx
  class Tittle extends React.Component {
      render() {
          return <h1>this is a tittle {this.props.name}</h1>;
      }
  }
  
  // 渲染类组件
  const tittle = <Tittle name="float" />
  ```

* **composing components**

  > 嵌套组件

  ```jsx
  function Parent(props) {
      return 
          <div id={props.id}>
      		<Sub index="1"></Sub>
          	<Sub index="2"></Sub>
      	</div>;
  }
  
  function Sub(props) {
      return <p>{props.index}</p>
  }
  ```

* **Read-only props**

  > 组件的属性是只读的

  ```jsx
  // do not use like this
  function Title(props) {
      props.name += 'tail'
      return <h1>name is {}</h1>
  }
  ```



### State

* **init state**

  > 使用constructor 初始化state

  ```jsx
  class Num extends React.Component {
      constructor(props) {
          super(props)
          // state只可以在constructor中可以直接赋值
          this.state = {num: 1}
      }
      
      render() {
          return <div>num is {this.state.num}</div>
      }
  }
  ```

* **hooks**

  > 钩子函数改变state

  ```jsx
  class Clock extends React.Component {
      refreshClock() {
          this.setState({time: new Date()})
      }
      componentDidMount() {
          this.clockId = setInterval(() => this.refreshClock(), 1000)
      }
      componentWillUnmount() {
          this.clearInterval(this.clockId)
      }
  }
  ```

* **setState**

  > 改变state

  ```jsx
  // 对象作为参数
  this.setState({
      name: 'float',
      age: 24
  })
  
  // 函数作为参数, 用于引用state, props
  this.setState((state, props) => ({
      num: props.num + 1
  })
  ```

* **share state**

  > 当两个组件需要共享state时, 提升state

  ```jsx
  class ParentState extends React.Component {
    constructor(props) {
      super(props)
      this.saveNum = this.saveNum.bind(this)
      this.state = {
        num: 0
      }
    }
    saveNum(e) {
      this.setState({
        num: e.target.value
      })
    }
    render() {
      return (
        <div>
          <SubState num={this.state.num} tag='small' onHandleChange={this.saveNum} />
          <SubState num={this.state.num} tag='big' onHandleChange={this.saveNum} />
        </div>
      );
    }
  }
  
  class SubState extends React.Component {
    constructor(props) {
      super(props)
    }
    render() {
      let num = this.props.num
      if (this.props.tag === 'big') {
        num = Number(num) + 5
      }
      let changeHandler = this.props.onHandleChange
      return (
        <div>
          <input value={num} onChange={changeHandler} />
        </div>
      );
    }
  }
  ```


### event

* **event binding**

  > 事件绑定

  ```jsx
  <button onClick={postData}></button>
  ```

* **preventDefault**

  > 阻止事件默认行为

  ```jsx
  function Submit(props) {
      function postData(e) {
          e.preventDefault()
          // do some thing
      }
      return (
      	<div>
          	<a href="#" onClick={postData}></a>
          </div>
      )
  }
  ```

* **params**

  > 事件传参

  ```jsx
  <button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
  <button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
  ```



### conditional render

* **state conditional using if**

  ```jsx
  class LoginControl extends React.Component {
      constructor(props) {
          super(props)
          this.state = {
              isLogin: false
          }
      }
      
      login() {
          this.setState({ isLogin: true , username: 'float'})
      }
      logout() {
          this.setState({ isLogin: false })
      }
      
      render() {
  		const isLoginFlag = this.state.isLogin
          let template
          if (isLoginFlag) {
              template = (
              	<div>
                  	<p>welcome {this.state.username}</p>
                      <button onClick={this.logout}></button>
                  </div>
              );
          } else {
              template = (
              	<div>
                  	<p>please login</p>
                      <button onClick={this.logout}></button>
                  </div>
              );
          }
          
          return (
          	<div>
              	<p>this is login component</p>
                  {template}
              </div>
          )
      }
  }
  ```



### list

* **render list**

  > 列表的渲染, 必须有全局唯一 key 属性

  ```jsx
  function List(props) {
      return (
      	<ul>
              {
                  props.numbers.map( item => <li key={item.value.toString()}>{item.value}</li>)
              }
          </ul>
      )
  }
  ```




### Form

* 表单的渲染与事件绑定

  ```jsx
  class Form extends React.Component {
    constructor(props) {
      super(props)
      this.state = { name: '' }
      this.handleValueChange = this.handleValueChange.bind(this)
    }
    handleValueChange(e) {
      this.setState({name: e.target.value})
    }
    render() {
      return (
        <form>
          <label>type your name</label>
          <input value={this.state.name} onChange={this.handleValueChange} />
          <span>{this.state.name}</span>
        </form>
      );
    }
  }
  ```



### children

* **props.children**

  > 非命名子属性, 使用props.children 属性组合其他组件的内容

  ```jsx
  class Mavic extends React.Component {
      constructor(props) {
          super(props)
      }
      fly() {
  		console.log('I can fly')
      }
      render() {
  		return (
  			<div className={'colorful' + this.props.color}>
              	{this.props.children}
              </div>
          );
      }
      
  }
  
  class MavicPro extends React.Component {
      constructor(props) {
          super(props)
      }
      fly() {
          console.log(' I can fly 60km/h')
      }
      render() {
          <div>
          	<Mavic>
                  <span>hi I am mavic pro</span>
                  <button>click to buy me</button>
              </Mavic>
              
          </div>
      }
  }
  ```

* **props.left ...**

  > 命名子属性

  ```jsx
  function TableContent(props) {
    return (
      <div>
        <div className="left">
          {props.left}
        </div>
        <div className="right">
          {props.right}
        </div>
      </div>
    );
  }
  
  function RandL() {
    let left = <div>hello left</div>
    let right = <div>hello right</div>
    return (
      <div>
        <TableContent
          left={left}
          right={right}
        />
      </div>
    );
  }
  ```



## react router



### 初始化

* **install**

  ```shell
  npm install react-router-dom
  ```

* **using router** 

  ```jsx
  // app.js
  import { Route, Link, BrowserRouter as Router } from 'react-router-dom'
  
  const routing = (
  	<Router>
      	<div>
          	<Route exact path="/" component={App} />
              <Route path="/users" component={User} />
              <Route path="/contract" component={Contract} />
          </div>
      </Router>
  )
  
  ReactDOM.render(routing, document.querySelector('#root'))
  ```

  * exact 属性: 精确匹配, 若不加此属性, /users 和 /contract会匹配到 /

* **link**

  ```jsx
  const routing = (
  	<Router>
      	<div>
          	<ul>
              	<li><Link to="/">home</Link></li>
                  <li><Link to="/users">users</Link></li>
                  <li><Link to="/contract">contract</Link></li>
              </ul>
              <Route path="/" component={App} />
              <Route path="/users" component={Users} />
              <Route path="/contract" component={Contract} />
          </div>
      </Router>
  )
  ```

* Not found

### API



