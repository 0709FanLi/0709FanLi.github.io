事件循环

## 宏任务和微任务

1. 宏任务：script 所有代码、settimeout、setinterval、setImmediate、（只有 IE10 实现）、I/O、UI Rending
2. 微任务：Process.nextTick(Node 独有)、Promise、Object.ovserve、MuationServer([具体使用方式看这里](http://javascript.ruanyifeng.com/dom/mutationobserver.html))

宏任务 6 种，微任务 4 种，总共 10 种，一个只有 IE10 实现，一个被废弃。

## EventLoop

1. 执行 script 脚本所有代码
2. 将同步任务放到宏任务队列，将异步任务放到微任务队列
3. 执行所有的同步任务，执行完毕如果有微任务队列有微任务，执行微任务
4. 执行 UI 线程渲染任务
5. `检查是否有WebWorker任务，有则执行`(易忘)
6. 返回到第二步循环执行 ##经典题目

```
Promise.resolve().then(()=>{
  console.log('Promise1')
  setTimeout(()=>{
    console.log('setTimeout2')
  },0)
});
setTimeout(()=>{
  console.log('setTimeout1')
  Promise.resolve().then(()=>{
    console.log('Promise2')
  })
},0);
console.log('start');

// start
// Promise1
// setTimeout1
// Promise2
// setTimeout2

```
