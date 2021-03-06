# react生命周期
1.componentWillMount         ----> 组件挂载前，属性、方法都可以拿到，元素拿不到(做交互)

2.componentDidMount          ----> 组件挂载后，属性、方法已经有了，元素可以拿到（做交互）

3.shouldComponentUpdate      ----> 初始化 render 时不会执行，当 props 或 state 发生变化时执行

4.componentWillUpdate        ----> 组件更新前

5.componentDidUpdate         ----> 组件挂载后

6.componentWillReceiveProps  ----> 组件 props 发生变化时执行，初始化render的时候不会执行，一般用于父组件状态更新时子组件的重新渲染。

应用场景：这个钩子函数适合父子组件的互动，比如父组件需要通过某些状态控制子组件渲染或销毁，在 componentWillReceiveProps 回调函数中，可以通过 this.props 来获取 props，而新的 props 是通过该函数的参数传入，那么就可以比较两个 props，从而对本组件的 state 作出更新或者触发子组件内的某些方法。

7.componentWillUnmount       ----> 组件卸载（销毁定时器，停掉网络请求等）
  
# react中的表单

  1.受控组件：react控制组件的值和状态，value 由 state 控制，通过 setState 修改 valuevalue
  
  2.非受控组件：组件的状态改变不受控制，不设置 value 值，通过ref来操作真实的DOM
  
    错误 -----> <input text="text" value="abc" />
    正确 -----> <input text="text" defaultValue="abc" />

    错误 -----> <input text="checkbox" checked />
    正确 -----> <input text="checkbox" defaultChecked />
    
# react中的遍历
    constructor() {
        super()
        this.state = {
            list: ['aaa', 'bbb', 'ccc']
        }
    }
    render() {
        var arrLi = [];

        this.state.list.forEach((item, index) => {
            arrLi.push(<li key={index}>{item}</li>)
        });

        return (<div>
                    <ul>{arrLi}</ul>
                </div>)
    }
    
☆注意：每个元素要添加一个唯一的 key 值

# react中的交互
  react 本身没有提供交互的库，凡是具有交互能力的库都可以使用:jquery,zepto,axios,fetch,原生ajax...
  
# 组件嵌套
    class List extends React.Component {
          render() {
              return <ul>
                      <li>列表项</li>
                  </ul>
          }
      }

      class App extends React.Component {
          render() {
              return <div>
                      <h1>这是一个列表</h1>
                      <List />
                  </div>
          }
      }
 
# 组件之间数据通信
  1.父级 ----> 子级
  
    父级通过添加属性的方式给子级传值： <Child msg={this.state.msg} />
    子级通过 this.props 获取父级传来的数据： {this.props.msg}
        
  2.子级 ----> 父级
  
    父级给子级添加一个方法： <Child fnCallBack={this.getMsg} /> 父级里的一个函数
    子级调用父级传递过来的方法： this.props.fnCallBack('传给父级的数据')
  
#### 补充：mvvm中的vm代表什么
  m  ----> Model：业务逻辑相关的数据对象
  
  v  ----> View：UI表现层，展现出来的用户界面
  
  vm ----> ViewModel：它是View与Model的连接器，View与Model通过ViewModel实现双向绑定
  
  
  
  
  
  
  
  
  
  
  
  
