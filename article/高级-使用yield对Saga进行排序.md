## 使用yield* 对Sagas进行排序

你可以使用内置的yield* 操作符来组合多个Sagas，使得它们保持顺序。这让你可以一种简单的程序风格来排序你的宏观任务。

```javascript
function* one(){}
function* two(){}
function* three(){}
function* game(){
    const score1 = yield* one()
    yield put(showScore(score1))
    const score2 = yield* two()
    yield put(showScore(score2))
    const score3 = yield* three()
    yield put(showScore(score3))
}
```

**注意，使用yield*将导致该Javascript运行环境漫延至整个序列。由此产生的迭代器，将yield所有来自嵌套迭代器里的值。一个更强大的代替方案是使用更通用的中间件组合机制。**