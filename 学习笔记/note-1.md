# jsx语法
学习react需要学习jsx的写法，言而简之就是可以在js里边书写html标签，有几点需要注意的地方：
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
  
  ## 4.自闭合便签 "/" 不能省略
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
    
    Babel主要是用来转换各种ES*代码为浏览器可识别的ES代码。就目前来说，会转换ES6和ES7语句为ES5语句，转换JSX看起来倒像是其的一个附加功能，还有其他的JSX语句转换器，如：jsxTransformer.js --> 对应script type 为 type="text/jsx"； browser.js --> 对应script type 为 type="text/babel",babel的浏览器版本为browser.js
    
    ReactDOM.render 是 React 的最基本方法，将模板转为HTML语言，并插入指定的 DOM 节点，使用方法：eactDOM.render（组件， 插入的节点）
    
    ReactDOM.render(
        <h3>welcome</h3>,
        document.querySelector('#app')
    )
    
    上面的这种写法，在实际项目开发过程中会越来越乱，react推荐使用面向对象的方法开发，主要有两种：
      1.es5构造函数模拟
      2.es6中class    推荐
      
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
      1.在添加事件的时候，需要更改一下this的指向 --> this.show.bind(this),不太明白的是后面的 this.props.a 中 this 指向的组件实例，为什么在方法执行的时候，其中的 this 就变成 undefined 了，需要通过 bind(this) 来修改 this 的指向
      2.props属性只读，不能修改，执行 this.props.a='good' 会报错（怎么才能修改，并且更新视图，类似ag和vue，在react中使用的是状态）
      
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
      
      更新状态：this.state.msg="new state"，这种方式有问题，虽然状态更新了，但是视图没有跟着更新，要想视图也跟着更新，需要使用react提供的一个方法：this.setState({key: val})
      
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
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
      
