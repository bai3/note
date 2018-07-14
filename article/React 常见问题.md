## 调用setState之后发生了什么

在代码中调用setState函数之后，React会将传入的参数对象与组件当前的转台合并，然后触发所谓的调和过程。经过调和过程，React会以相对高效的方式根据新的状态构建React元素树并且重新渲染整个UI界面。然后diff算法后，按需更新问不是全部重新渲染、

## React中Element 与 Component的区别

React Element是描述屏幕上所见内容的数据结构，是对于UI的对象表述。典型的React Element就是利用JSX构建的声明式代码片段然后被转化为createElement的调用组合。而React Component则是可以接收参数输入并且返回某个React Element的函数或者类。

## 在什么情况下有限选择Class Component而不是Functional Component

在组件需要包含内部状态或者使用到生命周期函数的时候使用Class Component，否则使用函数式组件

### React创建组件的三种方式

- 函数式组件(无组件状态)
- es5原生方式 React.createClass
- es6 extends React.Component

## 无状态组件的特点

- 组件不会被实例化，整体渲染性能得到提升

- 组件不能访问this对象

  无状态组件由于没有实例化过程，所以无法访问组件this中的对象。例如 this.ref 和 this.state

- 组件无法访问生命周期的方法

- 无状态组件只能访问输入的props，同样的props会得到相同的渲染结果，不会有副作用。

## React的refs的作用是什么

Refs是React提供给我们的安全访问DOM元素或者某个组件实例的句柄。我们可以为元素提那家ref属性然后在回调函数中接受该元素的DOM树中的句柄,该值会作为回调函数的第一个参数返回。

## React中的keys的作用是什么？

Keys是React用于追踪那些列表中元素被修改、被添加或者被移除的辅助标识。

在React Diff算法中React会借助元素的Key值来判断该元素是新建的还是移动的元素，从而减少不必要的元素重渲染。

## 在生命周期中的哪一步发起AJAX请求。

我们应当将AJAX请求放到componentDidMount函数中执行

- 如果我们将AJAX请求放置在生命周期中的其他函数，不能保证请求在组件挂载完毕后才会要求相应。

## shouldComponentUpdate 的作用

shouldComponentIUpdate允许我们手动地判断是否要进行组件更新，根据组件的应用场景设置函数的合理返回值能够帮我们避免不必要的更新。

## 传入 setState 函数的第二个参数的作用是什么？

该函数会在setState函数调用完成并且组件开始重渲染的时候被调用，我们可以用该函数来监听渲染是否完成、