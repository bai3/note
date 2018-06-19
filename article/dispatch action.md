## 发送action到store

假设每次保存之后，我们想发起一些action通知Store数据获取成功。

我们可以找出一些方法来传递Store的dispatch函数Generator。然后Generator可以在接收到获取的响应之后调用它。

```javascript
function* fetchProducts(dispatch){
    const products = yield call(Api.fetch, '/products')
    dispatch({type: 'PRODUCES_RECEIVED', products})
}
```

然而，该解决方案与我们在上一节中看到的从Generator内部直接调用函数，有着相同缺点，如果我们想要测试fetchProducts接收到AJAX响应之后执行dispatch，我们还需要模拟dispatch函数、

相反，我们需要相同的声明式的解决方案。只需创建一个对象来只是middleware我们需要发起一些action，然后让middleware执行真实的dispatch。这种方式我们就可以同样的方式测试Generator的dispatch：只需检查yield后的Effect，并确保它包含正确的指令。

redux-saga为此提供了另外一个函数put，这个函数用于创建dispatch Effect。

```javascript
import { call, put } from 'redux-saga/effects'

function* fetchProducts() {
    const products = yield call(Api.fetch, '/products')
    yield put({ type: 'PRODUCTS_RECEIVED', products})
}
```

现在我们通过Generator的next方法来将假的响应对象传递到Generator。在middleware环境之外，我们可以完全控制Generator，通过简单地模拟结果并还原Generator，我们可以模拟一个真实环境。