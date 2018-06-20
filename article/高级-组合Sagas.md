## 组合Sagas

虽然使用yield*是提供组合Sagas的惯用方式，但这个方法也有一些局限性:

- 你可能会想要单独测试嵌套的Generator。这导致了一些重复的测试代码以及重复执行的开销。我们不希望执行一个嵌套的Generator，而仅仅是想确认它是被传入正确的参数来调用。
- 刚重要的是，yield* 只允许任务的顺序组合，所有一次你只能yield* 一个Generator。

你可以直接使用yield来并行地启动一个或多个子任务。当yield一个call至Generator，Saga将等待Generator处理结束，然后以返回的值恢复执行（或错误从子任务中传播过来，则抛出异常）

```javascript
function* fetchPosts() {
    yield put(action.requestPosts())
    const products = yield call(fetchApi, '/products')
    yield put(action.receivePosts(products))
}

function* watchFetch() {
    while(yield take(FETCH_POSTS)){
        yield call(fetchPosts)
    }
}
```

yield一个队列的嵌套的Generator，将同时启动这些子Generator，并等待它们完成。然后以所有返回结果恢复执行:

```javascript
function* mainSaga(getState) {
   const results = yield[call(task1),call(task2),...]
   yield put(showResults(results))
}
```

事实上，yield Sagas并不会比yield 其他effects不用。这意味着你可以使用effect合并器将那些Sagas和所有其他类型的Effect合并。

### 例子

> 你可能希望用户在有限的时间内完成一些任务

```javascript
function* game(getState) {
    let finished
    while(!finished) {
        const {score, timeout} = yield race({
            score: call(play, getState),
            timeout: call(delay, 6000)
        })
        if(!timeout) {
            finsihed = true
            yield put(showScore(score))
        }
    }
}
```

