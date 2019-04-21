## Node
### 1.node核心
    核心模块：EventEmitter, Stream, FS, Net和全局对象
    全局对象：process, console, Buffer和exports
    exports和module.exports区别
    exports 是 module.exports 的一个引用 module.exports 初始值为一个空对象 {}，所以 exports 初始值也是 {} require 引用模块后，返回的是 module.exports 而不是 exports
### 2.单线程优点
    Node.js依托于v8引擎，都是以单线程为基础的。单线程资源占用小。单线程避免了传统PHP那样频繁创建、切换线程的开销，使执行速度更加迅速
### 3.Node.js是如何做到I/O的异步和非阻塞的呢
    其实Node在底层访问I/O还是多线程的。Node可以借助livuv来来实现多线程。
    如果我们非要让Node.js支持多线程，还是提倡使用官方的做法，利用libuv库来实现。
    cluster可以用来让Node.js充分利用多核cpu的性能
### 4.并行与并发，进程与线程
    并发 (Concurrent) = 2 队列对应 1 咖啡机.
    并行 (Parallel) = 2 队列对应 2 咖啡机.
    线程是进程下的执行者，一个进程至少会开启一个线程（主线程），也可以开启多个线程。
### 5.谈谈Nodejs优缺点
    优点：
    1. 事件驱动，异步编程，占用内存少
    2. npm设计得好
    缺点：
    1. Debug 很困难。没有 stack trace，出了问题很难查找问题的原因；
    2 .如果设计不好，很容易让代码充满 callback，代码不优雅；
    3. 可靠性低；
    4. 单进程，单线程，只支持单核CPU，不能充分的利用多核CPU服务器。
### 1.什么是事件循环(美团)
    浏览器中, js引擎线程会循环从 任务队列 中读取事件并且执行, 这种运行机制称作 Event Loop (事件循环).
    
    每个浏览器环境，至多有一个event loop。 一个event loop可以有1个或多个task queue(任务队列)
    先执行同步的代码，然后js会跑去消息队列中执行异步的代码，异步完成后，再轮到回调函数，然后是去下个事件循环中执行setTimeout
    它从script(整体代码)开始第一次循环。之后全局上下文进入函数调用栈。直到调用栈清空(只剩全局)，然后执行所有的micro-task。当所有可执行的micro-task执行完毕之后。循环再次从macro-task开始，找到其中一个任务队列执行完毕，然后再执行所有的micro-task，这样一直循环下去。
    从规范上来讲，setTimeout有一个4ms的最短时间，也就是说不管你设定多少，反正最少都要间隔4ms才运行里面的回调。而Promise的异步没有这个问题。Promise所在的那个异步队列优先级要高一些 Promise是异步的，是指他的then()和catch()方法，Promise本身还是同步的 Promise的任务会在当前事件循环末尾中执行，而setTimeout中的任务是在下一次事件循环执行
    
```javascript
//依次输出 12354
setTimeout(function(){
  console.log(4)
  },0);
new Promise(function(resolve){
  console.log(1)
  for( var i=0 ; i<10000 ; i++ ){
    i===9999 && resolve()
  }
  console.log(2)
}).then(function(){
  console.log(5)
});
console.log(3);
```
