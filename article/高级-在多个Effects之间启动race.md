## 在多个Effects之间启动race

有时候我们同时启动多个任务，但又不想等待所有任务完成，我们只希望拿到第一个被resolve或reject的任务。race Effect提供了一种方法，在多个Effects之间触发一个race

### 例子

> 演示触发一个远程的获取请求，并且限制了一秒内响应，否则作超时处理。

```javascript
import { race, call, put } from 'redux-saga/effects'
import { delay } from 'redux-saga' 

function* fetchPostWithTimeout() {
    const {posts, timeout} = yield race({
        posts: call(fetchApi, '/posts'),
        timeout: call(delay, 1000)
    })
    
    if(posts){
        put({type: 'POSTS_RECEIVED', posts})
    }else{
        put({type: 'TIMEOUT_ERROR'})
    }
}
```

race的另外一个功能是，它会自动取消那些失败的Effects。

例如我们有两个UI按钮

- 第一个用于在后台启动一个任务，从这个任务运行到一个无限循环的while(true)中(例如每x秒针从服务器上同步一些数据)
- 一旦该后台任务启动，我们启动第二个按钮，这个按钮用于取消该任务。

```javascript
import{ race, take, call } from 'redux-saga/effects'

function* backgroundTask() {
    while(true) {
        ....
    }
}
function* watchStartBackgroundTask() {
    while(true) {
        yield take('START_BACKGROUND_TASK')
        yield race({
            task: call(backgroundTask),
            cancel： take('CANCEL_TASK')
        })
    }
}
```

在CANCEL_TASK action被发起的情况下，race Effect将自动取消backgroundTask，并在backgroundTask 中抛出一个取消错误。