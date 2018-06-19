## 使用Saga辅助函数

redux-saga提供了一些辅助函数，包装了一些内部方法，用来在一些特定的action被发起到Store时派生任务。

### 例子演示

第一个函数 takeEvery ，最常见的，提供了类似redux-thunk的行为

Ajax例子演示，每次点击Fetch按钮时，我们发起一个FETCH_REQUESTED的action。

我们先通过启动一个从服务器获取一些数据的任务，来处理这个action。

```javascript
import { call, put } from 'redux-saga/effects'

export function* fetchData(action) {
    try {
        const data = yield call(Api.fetchUser, action.payload.url);
        yield put({type: "FETCH_SUCCEEDED", data});
    } catch (error) {
        yield put({type: "FETCH_FAILED", error})
    }
}
```

然后在每次 EFTCH_REQUESTED action 被发起时启动上面的任务。

```javascript
import { takeEvery } from 'redux-saga'

function* watchFetchData() {
	yield* takeEvery（'FETCH_REQUESTED', fetchData)
}
```

在上面例子中，takeEvery运行多个fetchData实例同时启动。在某个特定时刻，尽管之前还有一个或多个fetchData尚未结束，我们还是可以启动一个新的fetchData任务。

如果我们只想得到最新那个请求的响应（例如，始终显示最新版本的数据）。我们可以使用takeLatest辅助函数

```javascript
import { takeLatest } from 'redux-saga'

function* watchFetchData() {
    yield* takeLatest('FETCH_REQUESTED', fetchData)
}
```

和 takeEvery不同，在任何时刻 takeLatest只允许一个fetchData任务在执行，并且这个任务是最后被启动的那个。如果已经有一个任务在执行的时候启动另一个fetchData， 那之前的这个任务会被自动取消。

如果你有多个Saga监视不同的action，你可以用内置辅助函数创建多个观察者，就像用了fork来派生他们。

例子

```javascript
import { takeEvery } from 'redux-saga/effects'

function* fetchUsers(action) {...}

function* createUser(action) {...}

//同时使用他们
export default function* rootSaga() {
    yield takeEvery('FETCH_USERS', fetchUsers)
    yield takeEvery('CREATE_USER', createUser)
}

```

