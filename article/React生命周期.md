## React生命周期

React生命周期分为三种状态

- 初始化
- 更新
- 销毁

### 详细周期步骤

##### 初始化

- ` getDefaultProps`

  设置默认的props,也可以用defaultProps设置组件的默认属性。

  只在组件创建时调用一次并缓存返回的对象(即在React.createClass之后就会调用)

  如果使用ES6语法，可以直接定义defalutProps这个属性来代替

  ```javascript
  Counter.defaultProps = { initialCount： 0 }
  ```

- `getInitialState`

  初始化this.state的值，只能组件装载之前调用一次。

  在使用ES6的class语法是，可以直接在constructor中定义this.state

  ```javascript
  constructor(props) {
          super(props);
          this.state = {str: "hello"};
   }
  ```

- `componentWillMount`/装载组件触发

  组件初始化时调用，以后组件更新不调用，整个生命周期只调用一次，此时可以修改state。

- `render`/装载组件触发

  创建虚拟dom，进行diff算法，更新dom树。此时就不能修改state

- `componentDidMount`/装载组件触发

  组件渲染后调用，只调用一次

##### 更新

- `componentWillReceiveProps`

  组件接受新的props时调用

- `shouldComponentUpdate`、

  react性能优化非常重要的一环。组件接受新的state或者props时调用，我们可以设置在此对比前后两个props和state是否相同。如果相同则返回false并阻止更新，因为相同的属性状态一定会生成相同的dom树，这样就不需要创造新的dom和旧的dom树进行diff算法对比，节省大量性能。

- `componentWillUpdate`

  组件初始化不调用，只有在组件将要更新时才调用，此时可以修改state。

- `render`

  组件渲染

- `componentWillUnmount`

  组件更新完成后调用，此时可以获取dom节点。

##### 销毁

- `componentWillUnmount`

  组件将要卸载时调用，一些事件监听和定时器需要在此时清除。

