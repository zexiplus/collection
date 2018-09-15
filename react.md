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



### Components

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
      	</div>;
  }
  
  function Sub(props) {
      return <p>{props.index}</p>
  }
  ```


