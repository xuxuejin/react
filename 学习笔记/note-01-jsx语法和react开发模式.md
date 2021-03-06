# jsx语法
学习react需要学习jsx的写法，言而简之就是可以在js里边书写html标签，下面是语法规范：
## 1.单个标签书写格式
    let div=<div>welcome react</div>
    
## 2.多个标签,必须有一个根元素（标签）包裹着所有元素（标签）
    错误 -------> let div=<div>welcome react</div><span>span</span>      
    正确 -------> let div=<div><div>welcome react</div><span>span</span></div>
  
## 3.可以自由缩进
    let div=<div>
              <div>welcome react</div>
              <span>span</span>
            </div>
  也可在外边加一个括号
  
    let div=(<div>
              <div>welcome react</div>
              <span>span</span>
            </div>)
  
## 4.自闭合标签 "/" 不能省略
    错误 -------> <img>
    正确 -------> <img />
  
## 5.标签的class属性名要写成className
    let div=<div className="content">welcome react</div>
  
## 6.jsx语法里边写js代码，用{}包裹
      let str='welcome react';    
      let div=<div className="content">{str}</div>
  
## 7.标签写style属性，要写成json的形式，json属于js语法，因此必须遵循第6条书写js的规则
    错误 -------> let img=<img style="{width: '50px', height: '50px'}" />;
    正确 -------> let img=<img style={{width: '50px', height: '50px'}}/>;
  外边的{}是在jsx语法里边写js代码的规范，里边的{}是json语法的规范
    
## 8.标签添加事件，事件名遵循驼峰命名法
    错误 -------> let div=<div onclick={fn}></div>
    正确 -------> let div=<div onClick={fn}></div>

# react开发模式
## 1.直接引入文件
    react.js      -------> React 的核心库
    react-dom.js  -------> 提供与 DOM 相关的功能
    babel.js      -------> 将 JSX 语法转为 JavaScript 语法
    
  Babel主要是用来转换各种ES*代码为浏览器可识别的ES代码，转换JSX看起来倒像是一个附加功能。其他的JSX语句转换器有（babel的浏览器版本为browser.js）：
  
      jsxTransformer.js --> 对应script type 为 type="text/jsx"；
      browser.js        --> 对应script type 为 type="text/babel",
    
  ReactDOM.render 是 React 的最基本方法，将模板转为HTML语言，并插入指定的 DOM 节点
    
  使用方法：ReactDOM.render（组件， 插入的节点）
    
    ReactDOM.render(
        <h3>welcome</h3>,
        document.querySelector('#app')
    )
    
  上面的这种写法，在实际项目开发过程中会越来越乱，react推荐使用面向对象的方法开发，主要有两种：
  
    1.es5构造函数模拟
    2.es6中class（推荐）
      
### 定义组件
  页面中引用的react.js提供了一个父类，直接继承就行了
  
      class Title extends React.Component {
        // 必须有render方法
        render() {
          return <h3>这是一个组件</h3>
        }
      }

      ReactDOM.render(
        // 使用组件
        <Title />,
        document.querySelector('#app')
      )
      
### 传参
      class Title extends React.Component {
        show() {
          console.log(this.props.a);
        }
        render() {
          return <h3 onClick={this.show.bind(this)}>{this.props.a}</h3>
        }
      }

      ReactDOM.render(
        <Title a="well" />,
        document.querySelector('#wrap')
      )
        
  ☆注意：
  
  1.在添加事件的时候，（this有问题）需要使用 bind 修改一下 this 的指向 --> this.show.bind(this)

  疑问：this.props 中 this 指向的组件实例，render 函数中的 this 也指向了组件实例，但是在调用方法的时，show 函数中的 this 则为 undefined？
  解答：在JavaScript中，this不是在函数声明的时候定义的，而是在函数调用（即运行）的时候定义的。React组件也遵循这种特性，所以组件方法的“调用者”不同会导致this的不同（“调用者” 指的是函数执行时的当前对象）。如果不用“调用者”显式地调用一个函数，JS 的解释器就会把全局对象当作调用者。但是在严格模式下，没有显式地指定“调用者”，this 不会指向全局对象，而是 undefined。
  当使用 onClick={this.handleClick} 来绑定事件监听函数的时候，handleClick 函数实际上会作为回调函数传入 addEventListener()，但是这个回调没有显式地指定“调用者”，而在 ES6 的 class 语法中，所有在 class 中声明的方法都自动地使用严格模式，因此，React 的组件中添加事件处理函数不改变this指向会得到 undefnied 而不是全局对象或者别的。
  
  2.props属性只读，执行 this.props.a='good' 会报错（怎么才能修改并更新视图，类似ag和vue，在react中使用的是状态）
      
  ☆延伸：为什么React没有自动的把bind集成到render方法中呢?
  
  答:因为render多次调用每次都要bind会影响性能，所以官方建议你自己在constructor中手动bind达到性能优化

  ☆复习：有哪些改变 this 指向的方法？
  
      1.call(this指向谁, arg1, arg2...)   --> 在函数调用阶段改变 this 的指向
      2.apply(this指向谁, [arg1, arg2...])--> 在函数调用阶段改变 this 的指向
      3.bind(this指向谁)                  --> 只是在函数定义阶段改变 this 指向，最终会返回一个函数，需要再次调用（可传参）
      
### 状态（相当于组件类的属性，需要放到构造函数里边）
      class Title extends React.Component {
        constructor() {
          // 继承父级属性
          super()
          this.state={
            msg: 'welcome react'
          }
        }
        render() {
          return <h3>{this.state.msg}</h3>
        }
      }

      ReactDOM.render(
        <Title />,
        document.querySelector('#wrap')
      )
      
  更新状态：
  this.state.msg="" 这种方式状态更新了，但视图没有更新，想要更新视图，需要使用react提供的一个方法： this.setState({key: val})
      
### 获取元素
      class SyncInput extends React.Component {
        constructor() {
          super()
          this.state={
            msg: ''
          }
        }
        handleChange(ev) {
          //第一种获取元素方式
          let val=ev.target.value;
          //第二种获取元素方式
          let val=this.refs.inp.value;
          //第三种获取元素方式
          let val=document.querySelector('input').value;
          this.setState({
            msg:val
          })
        }
        render() {
          return <div>
                  <input ref="inp" onChange={this.handleChange.bind(this)} text="text" />
                  <strong>{this.state.msg}</strong>
                </div>
        }
      }

      ReactDOM.render(
        <SyncInput />,
        document.querySelector('#wrap')
      )
      
  获取元素有三种方式：
  
      1.ev.target 获取当前事件对象的元素（一般用来获取当前元素）
      2.获取添加 ref 属性的对象，this.refs 获取的是所有添加 ref 属性的元素（一般用来获取别的元素）
      3.原生js获取：document.querySelector
    
  ## 2.基于webpack开发
  详细内容见 note-3
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
      
