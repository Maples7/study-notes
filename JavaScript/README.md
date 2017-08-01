# JavaScript

## Basics

- `undefined` 转换为数字为 `NaN`，`null`  转换为数字为 `0`。
  - `null == undefined` 结果为 `true`

## Promise

- 在 `new Promise(func)` 的 `func` 中，如果 `resolve` 和 `reject` 都一直不被调用，则这个被构造的 `Promise` 将永远处于 `Pending` 状态，这意味着这个 `Promise` 链中后续被链接上的 `Promise` 都将得不到执行（程序不会因为死循环被挂起，后续的 `Promise` 链被存在了最先创建的 `Promise ` 的回调函数队列中，当状态变为 `resolved` 或 `rejected` 后再继续调用，但由于状态永远不改变，所以回调队列也永远不会被执行）。

- JavaScript 中的 Array#map 等数组方法都不是并发的（即数组中的元素并不会并发的来执行回调，但回调中是有可能存在异步非阻塞的操作的），而 Bluebird 的 Promise#map API 则是支持并发的。在多次实践中，我曾经考虑过有没有什么办法使得 Array#map 也支持并发执行回调，后来发现事情应该是这样考虑的（也可能理解并不正确）：Promise#map 虽然是并发的，但对于完全的同步操作而言，在 Node 中并发并没有实际意义（单线程），而看 Bluebird 的实现也没有任何多线程的代码，故此处 Promise 的并发应该指的是回调是会「几乎同时」触发，对于回调中的异步操作达到了并发效果，而同步操作并不并发意义并不大。

- bluebird 的 Pormise.map 接口不会改变原数组的值。
