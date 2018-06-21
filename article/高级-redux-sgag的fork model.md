## redux-saga 的 fork model

在redux-saga的世界里，你可以使用2稲Effects在后台动态地fork task

- fork 用来创建attached forks
- spawn 用来创建detached forks

### Attached forks

Attached forks通过以下规则继续附加在它们的parent

#### 结论

- Saga只会在这之后终止：
- - 它终止了自己的指令
  - 所有附加的forks本身被终止

#### 例子

```javascript
import { delay } from 'redux-saga'
import { fork, call, put } from 'redux-saga/effects'
import api from './somewhere/api'
import { receiveData } from './somewhere/action'

function* fetchAll() {
    const task1 = yield fork(fetchResource, 'users')
    const task2 = yield fork(fetchResource, 'comments')
    yield call(delay, 1000)
}
function* fetchResource(resource) {
    const {data} = yield call(api.fetch, resource)
    yield put(receiveData(data))
}

function* main() {
    yield call(fetchAll)
}
```

call(fetchAll) 将在这之后终止：

- fetchAll 本身终止了，意味着这三个effect都会被执行，由于fork effects是非阻塞的，task将被阻塞在call(delay, 1000)
- 这2个被fork的task终止，意思是在fetch所需的资源之后放入对应的receiveData action。

所以整个task将阻塞直到一个1000毫秒的delay被传送，并且task1和task2完成他们的任务。

比方说，1000毫秒的delay和这两个task还没有完成，然后fetchAll在终止整个task之前，竟一直等待直到所有被fork的task完成。

fetchAll saga 可以使用平行Effect来重写

```javascript
function* fetchAll() {
    yield call([
        call(fetchResource, 'users'),
        call(fetchResource, 'comments'),
        call(delay, 1000)
    ])
}
```

事实上，被附加的fork与平行的Effect共享相同的语义：

- 在平行情况下我们执行task
- 在所有被launch的task终止后，parent将会终止。

这也适合于其他语义，你可以简单地把它考虑作为一个动态平行的Effect，来理解附加fork的行为。

#### Error 传播

按照同样的比喻，让我们来详细的检查在平行的Effect中是如何处理错误的。

假如我们有一个Effect:

```javascript
yield call([
    call(fetchResource, 'users'),
    call(fetchResource, 'comments'),
    call(delay, 1000)
])
```



### Detached forks

被分离的fork存活在它们本身的执行上下文中。parent不会等待被分离的fork终止。从被spawn的task抛出的未捕获的错误不会冒泡到parent，而且取消一个parent不会自动取消被分离的fork。