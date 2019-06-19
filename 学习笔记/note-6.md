# Router的使用
#### 1.安装
    npm i react-router-dom
#### 2.引用
    import { BrowserRouter as Router, Route, Link } from 'react-router-dom'

    <Router>
        <Route path="/" component={Home}/>
        <Route path="/news" component={News}/>
    </Router>

    <Link to="/">主页</Link>

这里有个问题，就是嵌套关系比较乱的时候，路由加上了，可能会报一个错：You should not use ‘Link’ outside a ‘Router’

这是因为 Router 是一个容器组件，不管是 Route、Link 还是其他的 'react-router-dom' 模块，都需要依赖 Router， 比如 Link 写在其他的地方，就需要再嵌套一层：

    <Header />
    <Router>
        <Sider />
    </Router>
    <Container />

其中 Sider 是个侧边栏导航，Container 是个右侧的容器，用来加载不同的组件。在 Container 组件中，通过路由匹配加载不同的组件：

    <Router>
        <Route path="/" component={Home}/>
    </Router>

#### 3.使用中会遇到的一些问题
##### 1.本地服务访问没问题，打包之后，刷新或者手动输入路由地址时，提示:Cannot GET /detail
具体解决方案参考的是：https://blog.csdn.net/zwkkkk1/article/details/83411071
