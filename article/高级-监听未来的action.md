## 监听未来的action

我们已经使用了辅助函数 takeEvery effect 以在每个actiin来到时派生新的任务。

在现实情况中，takeEvery 只是一个在强大的低阶API之上构建的wrapper  effect。在这一节中，我们将看到新的Effect，即take。

### 一个简单的日志记录器

开始一个简单的Saga例子，这个Saga将监听所有发起到store的action，并将它们记录到控制台。

使用takeEvery('*') 我们就能捕获发起的所有类型的action。

```javascript
import { select, takeEvery } from 'redux-saga/effects'

function* watchAndLog() {
    yield takeEvery('*', function* logger(action) {
        const state = yield select()
        console.log('action', action)
        console.log('state after', state)
    })
}
```

现在使用take Effect实现上面相同功能

```javascript
import { select, take } from 'redux-saga/effects'

function* watchAndLog() {
    while(true) {
        const action = yield take('*')
        const state = yield select()
        console.log('action', action)
        console.log('state after', state)
    }
}
```

take就像我们更早之前看到的call和put。它创建另外一个命令对象，告诉middleware等待一个特定的action。正如在call Effect的情况中，middleware会暂停Generator，直到返回的Promise被resolve。在take的情况中，它将会暂停Generator直到一个匹配的action被发起。在以上的例子中，watchAndLog 处于暂停状态，直到任意的一个action被发起。

takeEvery的情况中，被调用的任务无法控制何时被调用，它们将在每次action被匹配时一遍又一遍地被调用。并且它们也无法控制何时停止监听。

而在take的情况中，控制恰恰相反。与action被推向任务处理函数不同，Saga是自己主动拉取action的。看起来就像是Saga在执行一个普通的函数调用，这个函数将在action被发起时resolve。

一个简答的例子，假设我们的Todo应用中，我们希望监听用户操作，并在用户初次创建完三条Todo信息时显示祝贺信息。

```javascript
import {take, put} from 'redux-saga/effects'

function* watchFirstThreeTodosCreating() {
    for (let i = 0; i < 3;i++) {
        const action = yield take('TODO_CREATED')
    }
    yield put({type: 'SHOW_CONGRATULATION'})
}
```

主动拉取action的另一个好处是我们可以使用熟悉的同步风格描述我们的控制流。