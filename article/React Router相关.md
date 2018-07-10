## React Router 相关

> [抄录来自阮一峰博客](http://www.ruanyifeng.com/blog/2016/05/react_router.html?20180529223514)

### 一、基本用法

安装

> $ npm install -s react-router

使用时引入

```jsx
import { Router } from 'react-router'
render(<Router/>, document.getElementBtId('app'))
```

Router组件只是一个容器，真正的路由要通过Route组件定义

```jsx
import { Router, Route, hashHistory } from 'react-router'

render((
	<Router history={hashHistory}>
        <Route path="/" component={APP} />
    </Router>
),document.getElememntById('app'))
```

Route组件定义了URL路径与组件的相应关系。你可以同时使用多个Route组件

```jsx
<Router history={hashHistory}>
    <Route path="/" component={App} />
    <Route path="/repos" component={Repos} />
    <Route path="/about" component={About}/>
</Router>
```

### 二、嵌套路由

Route组件可以嵌套

```jsx
<Router history={hashHistory}">
    <Route path="/" component={App}>
          <Route path="/repos" component={Repos} />
          <Route path="/abput" component={About} />
    </Route>
</Router>
```

App组件写成如下样子

```jsx
export default React.createClass({
    render(){
        return <div> {this.props.children} </div>
    }
})
```

上面代码中，App组件的this.props.children属性就是子组件。子路由也可以不写Router组件里面，单独传入Router组件的routes属性。

```jsx
let routes = <Route path="/" component={App}>
        <Route path="/repos" component={Repos} />
        <Route path="/about" component={About} />
    </Route>
 <Router routes={routes} history={browerHistory}>   
```

### 三、path属性

Route组件的path属性指定路由的匹配规则。这个属性是可以省略，这样的话，不关路径是否匹配，总是会加载指定组件。（可以忽略上一级的Route的path。）

### 四、通配符

path属相可以使用通配符

```jsx
<Route path="/hello/:name"> //匹配 /hello/michael
<Route path="/hello/(:name)"> //匹配 /hello 或 /hello/michael
<Route path="/file/*.*"> //匹配 /file/hello.jpg
<Route path="/file/*"> //匹配 /file/ 或 file/path 
```

规则

- :paramName

  :paramName匹配URL的一个部分，直到遇到下一个/、?、# 为止。这个路径参数可以通过this.props.params.paramName取出

- ()

  ()表示URL的这个部分是可选的

- *

  *匹配任意字符，直到模式里面的下一个字符为止。匹配方式是非贪婪模式

- **

  **匹配任意字符，直到下一个/、？、#为止，匹配方式是贪婪模式/

> 此外，URL的查询字符为/foo?bar=bzx，可以用this.props.location.query.bar获取

### 五、 IndexRoute 组件

IndexRoute显示指定一个组件为根路由的子组件，即指定默认情况下加载的子组件。你可以把IndexRoute现象成某个路径的index.html

```jsx
<Router>
    <Route path="/" component={App}>
    	<IndexRoute component={Home} />
        <Route path="accounts" component={Accounts}>
    </Route>
</Router>
```

当用户访问/路径是，加载的组件结构为

```jsx
<App>
    <Home />
</App>
```

### 六、 Redirect组件

Redirect组件用于路由的跳转，即用户访问一个路由会自动跳转到另一个路由

```jsx
<Route path="index" component={Index}>
    <Redirect from="message/:id" to="/message/:id" />
</Route>
```

现在访问/index/message/5 会自动跳转到/message/5

### 七、IndexRedirect组件

IndexRedirect组件用于访问根路由的时候，将用户重定向到某个子组件

```jsx
<Route path="/" component={App}>
    <IndexRedirect to="/welcome">
    <Route path="welcome" component={Welcome} />
    <Route path="about" component={About} />
</Route>
```

### 八、Link

link操作跟vue一样，生成一个a标签，生成一个链接。

```jsx
render() {
    return 
    <div>
        <ul>
        	<li><Link to="/about">About</Link></li>
            <li><Link to="/repos">Repos</Link></li>
        </ul>
    </div>
}
```

若希望当前的路由与其他路由有不同样式，这时可以使用Linl组件的activeStyle属性

```html
<Link to="/about" activeStyle={{color: 'red'}}>About</Link>                         
```

另外一种，使用activeClassName指定当前路由的Class

### 九、IndexLink

如果链接到根路由/，不要使用Link组件，而要使用IndexLink组件。

```html
<IndexLink to="/" actionClassName="active">Home</IndexLink>
```

上面的代码，根路由只会在精确匹配时，才具有activeClassNmae.

使用Link组件的onlyActiveOnIndex属性，也能达到同样效果

``` onlhtml
<Link to="/" activeClassName="active" onlyActiveOnIndex={true}>
Home
</Link>
```

### 十、history属性

Router组件的history属性，用来监听浏览器地址栏的变化，并将URL解析成一个地址对象，供React Router匹配

history属性

- browserHistory(需要后端配合)
- hashHistory
- createMemoryHistory

### 十一、表单处理

简单的说就是就是用js跳转路由

```javascript
import { browserHistory } from 'react-router'
function(){
    ....
    const path = `/repos/${userName}/${repo}`
    browserHistory.push(path)
    //或者使用context对象
    this.context.router.push(path)
}

```

### 十二、路由的钩子

每个路由都有Enter和Leave钩子，用户进入或离开该路由时触发

onEnter钩子代替Redirect组件

```jsx
<Route path="inbox" component={Inbox}>
  <Route
    path="messages/:id"
    onEnter={
      ({params}, replace) => replace(`/messages/${params.id}`)
    } 
  />
</Route>
```

高级应用，当用户离开一个路径时候，跳出一个提示框，要求用户确认是否是离开

```javascript
const Home = withRouter(
  React.createClass({
    componentDidMount() {
      this.props.router.setRouteLeaveHook(
        this.props.route, 
        this.routerWillLeave
      )
    },

    routerWillLeave(nextLocation) {
      // 返回 false 会继续停留当前页面，
      // 否则，返回一个字符串，会显示给用户，让其自己决定
      if (!this.state.isSaved)
        return '确认要离开？';
    },
  })
)
```

setRouteLeaveHook方法为Leave钩子指定routerWillLevae函数，该方法如果返回false，将阻止路由的切换，否则就返回一个字符串，提示用户决定是否要切换。