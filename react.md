# react

> react 使用指南

### 目录



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



### API

* **ReactDOM.render**

  > 在指定dom元素上渲染react元素

  ```jsx
  const element  = <img src={img.url} />
  const dom = document.querySelector('.pic')
  ReactDOM.render(element, dom)
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


