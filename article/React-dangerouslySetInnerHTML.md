## dangerouslySetInnerHTML 使用

> 将字符变量解析成html解析，等同于vue的v-html和angular的ng-bind的使用效果。

#### 例子

```jsx
var Demo = React.createClass({
    render: <div dangerouslySetInnerHTML={{_html: '<h1>HelloWorld</h1>'}}></div>
})
```

将解析成内容为HelloWorld的h1标签。

#### 为什么React不直接将字符串解析html

> 不合时宜的使用innerHTML可能会可能会导致XSS攻击。`dangerouslySetInnerHTML` 这个 prop 的命名是故意这么设计的，以此来警告，它的 prop 值（ 一个对象而不是字符串 ）应该被用来表明净化后的数据。