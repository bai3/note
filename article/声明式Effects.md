## 声明式Effects

在 redux-saga的世界里，Sagas都是用Generator函数实现。我们从Generator里yield纯Javascript对象以表达Saga逻辑。我们称呼那些对象为Effect、Effect是一个简单的对象，这个对象包含了一些给middleware解释执行的信息。你可以把Effect看做是发送给middleware的指令以执行某些操作（调用某些异步函数，发起一个action到store）

介绍一些基础的Effect以及概念。

Sagas可以多种形式yield Effect。最简单的方式是yield一个Promise。

举个例子，假设我们有个监听 PRODUCTS_REQUESTED action 的Saga。每次匹配搭配action，它会启动一个从服务器上获取产品列表的任务。

```javascript
import { takeEvery } from 'redux-saga/effects'
import Api from './path/to/api'

function* watchFetchProducts() {
	yield takeEvery('PROFUCTS_REQUESTED', fetchProducts)
}

function* fetchProducts() {
	const products = yield Api.fetch('/products')
    console.log(products)
}
```

在上面的例子中，我们在Generator中直接调用了Api.fetch（在Generator函数中，yield右边的任何表达式都会被求值，结果会被yield给调用者。）

Api.fetch('/products') 触发了一个AJAX请求并返回一个Promise，Promise会resolve请求的响应，这个AJAX请求将立即执行。

......

实际上我们需要的只是保证 fetchProducts 任务yield 一个调用正确的函数，并且函数有着正确的参数。

相比于在Generator中直接调用异步函数，我们可以仅仅yield一个描述函数调用的信息。也就是说，我们简单地yield 一个看起来像下面这样的对象：

```json
{
    CALL: {
        fn: Api.fetch,
        args: ['./products']
    }
}
```

换句话说，Generator将会yield包含指令的文本对象。redux-saga middleware将确保执行这个指令并将指令的结果反馈给Generator。这样的话，在测试Generator时，所有我们需要做的就是，将yield后的对象作一个简单的deepEqual 来检查它是否yield了我们期望的指令。

出于这样的原因，redux-saga 提供了一个不一样的方式来执行异步调用

```javascript
import { call } from 'redux-saga/effects'

function* fetchProducts() {
    const products = yield call(Api.fetch, '/products')
}
```

我们使用了call(fn, ...args) 这个函数。与前面的例子不同的是，现在我们不立即执行异步调用，相反，call创建了一条描述结果的信息。就像在Redux里你使用action创建器，创建一个将被Store执行的、描述action的纯文本对象。call创建一个纯文本对象描述函数调用。redux-saga middleware 确保执行函数调用并在响应被resolve时恢复generator。

这让你能容易地测试Generator，就算它在Redux环境之外。因为call只是一个返回纯文本对象的函数而已

call同样支持调用对象方法，你可以使用以下形式，为调用的函数提供一个this 上下文

```javascript
yield call([obj, obj.method], args, arg2,...)
```

apply 提供另外一种调用的方式

```javascript
yield apply(obj,obj.method,[arg1,arg2,...])
```

