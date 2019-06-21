### redux的实现
![avatar](https://upload-images.jianshu.io/upload_images/6548744-df461a22f59ef7da.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/800/format/webp)
redux实现数据的统一管理，有三个概念：

##### 1.reducer 可以理解为一个数据容器，把数据搜集到 store 里边统一管理  刚开始没有动作的时候，返回的就是初始化的数据

    const initState = { user: '许仙', age: 18, sex: '男' };

    export const reducer = (state = initState, action) => {
        switch (action.type) {
            case 'USER_UPDATE':
                return { ...state, ...action.payload };
            case 'AGE_GROW':
                return { ...state, age: state.age + 1 };
            case 'SEX_UPDATE':
                return { ...state, ...action.payload };
            default:
                return state;
        }
    }
    
##### 2.redux 实现数据的定于发布功能

    export const createStore = (reducer) => {
        // 声明常量
        let state;
        const listeners = [];

        // 获取状态
        const getState = () => {
            return state;
        }

        // 添加监听对象
        const subscribe = (listener) => {
            listeners.push(listener);
        }

        // [1]执行reducer修改状态 [2]遍历执行监听对象
        const dispatch = (action) => {
            state = reducer(state, action);
            listeners.forEach(v => v());
        }

        // 初始化 state
        dispatch({ type: '' });

        // 暴露接口
        return { getState, subscribe, dispatch };
    }

##### 3.action 定义触发的动作

    export const changeUser = (user) => {
        return {
            payload: { user },
            type: 'USER_UPDATE',
        };
    }

    export const changeAge = () => {
        return { type: 'AGE_GROW' };
    }

在组件中使用的原理分为以下几个步骤：

  <div className="welcome-container">
    <p>user: {this.state.user}, age: {this.state.age}</p>
    user:<input type="text" className="input" onChange={this.onChange} />
    <button className="btn" onClick={this.onClick}>年龄增长</button>
  </div>

##### 1.生成数据容器

    const store = createStore(reducer)
    
##### 2.订阅需要监听的数据

    componentDidMount() {
        store.subscribe(this.update);
        this.update();
    }
    
    update = () => {
        this.setState(store.getState());
    }

##### 3.更新数据

    onChange = (e) => {
        store.dispatch(changeUser(e.target.value));
    }

    onClick = () => {
        
        store.dispatch(changeAge());
    }

### 基础知识
这个参考：https://www.jianshu.com/p/7a71181a7aa0

### 项目应用
这个参考：https://www.jianshu.com/p/7a71181a7aa0

