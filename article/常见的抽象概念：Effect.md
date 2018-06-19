## 一个常见的抽象概念： Effect

概括来说，从Saga内触发异步操作总是有yield一些声明式的Effect来完成（你也可以知己yield Promise，但是这会让测试变得困难）

一个Saga所做的实际上是组合那些所有的Effect，共同实现所需的控制流。最简单的例子是直接把yield一个接一个地放置来对序列化yield Effect。你也可以使用熟悉的控制流操作符（if，while）来实现更复杂的控制流。

我们已经看到，使用Effect诸如call和put，与高阶API如takeEvery相结合，让我们实现与redux-thunk同样的东西。易于测试。

但redux-saga与redux-thunk相比还一有另外一种好处。在[高级]一节，你会遇到一些更强大的Effect，让你可以表达更复杂的控制流的，仍然拥有可测试性的好处。

