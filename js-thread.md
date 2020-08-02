# js运行机制
[参考文献](https://juejin.im/post/5e22b391f265da3e204d8c14)
## 1. 宏任务
* 主代码块
* setTimeout & setInterval
* setImmediate()
* requestAnimationFrame()

## 2. 微任务
* promise.nextTick()
* promise.then()
* catch
* finally
* Object.observe
* MutationObserver

## 3. 执行过程
* 同步任务进入主线程
* 异步任务分为微任务和宏任务
* 宏任务和微任务分别进入对应的event table中，（定时、请求获得数据）处理好了则进入event queue中，定时则哪个时间短，哪个先进入event queue
* 宏任务执行完了，（先微后宏）则先询问微任务，有则先执行微任务，然后在下一个宏任务执行前进行渲染
