## 二、队列

### 2.1如何判断循环队列满了

1 额外记录一个cnt值，这个值代表元素的数量

2 额外牺牲一个存储空间 ， 队头指针在队尾指针的下一个位置时，队满。

Q.front == (Q.read +1) % MAXSIZE,因为队头指针可能又重新从0位置开始，而此时队尾指针是MAXSIZE - 1，所以需要求余

[参考链接](https://blog.csdn.net/u010429311/article/details/51043149)

### 2.2 典型应用场景

#### 2.2.1 CPU的超线程技术

真两核 vs 虚拟四核 

一个核 有两个任务队列

 多路   一路代表一个cpu，多路就是多cpu

核心 => cpu => 多路（节点）=> 集群

#### 2.2.2 线程池的任务队列



队列通常用作缓冲区

[node线程池](https://juejin.cn/post/6844904122466992135)

[EventLoop](http://www.ruanyifeng.com/blog/2014/10/event-loop.html)

[Why js is a single thread language](https://www.geeksforgeeks.org/why-javascript-is-a-single-thread-language-that-can-be-non-blocking/)

 [javaScript执行机制](https://juejin.cn/post/6844903512845860872)



process.nextTick方法指定的回调函数，总是在当前"执行栈"的尾部触发，

Node.js文档中称，setImmediate指定的回调函数，总是排在setTimeout前面。实际上，这种情况只发生在递归调用的时候。

由于process.nextTick指定的回调函数是在本次"事件循环"触发，而setImmediate指定的是在下次"事件循环"触发，所以很显然，前者总是比后者发生得早，而且执行效率也高（因为不用检查"任务队列"）。



有说法说process.nextTick()是一个特殊的异步API，他不属于任何的Event Loop阶段。事实上Node在遇到这个API时，Event Loop根本就不会继续进行，会马上停下来执行process.nextTick()，这个执行完后才会继续Event Loop。 

参考链接：[![https://www.cnblogs.com/dennisj/p/12550996.html](data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxOCIgaGVpZ2h0PSIxOCIgdmlld0JveD0iMCAwIDE4IDE4Ij4KICAgIDxnIGZpbGw9Im5vbmUiIGZpbGwtcnVsZT0iZXZlbm9kZCIgc3Ryb2tlPSIjMDI3RkZGIiBzdHJva2UtbGluZWNhcD0icm91bmQiIHN0cm9rZS13aWR0aD0iLjgxOCI+CiAgICAgICAgPHBhdGggZD0iTTguMDMgNS4yNzZsMS41ODUtMS41ODRjMS4wOTMtMS4wOTMgMi45My0xLjAzIDQuMS4xNDEgMS4xNzIgMS4xNzIgMS4yMzYgMy4wMDguMTQyIDQuMTAyTDEyLjY3IDkuMTIzbC0uNzkyLjc5MmMtMS4wOTMgMS4wOTMtMi45MyAxLjAzLTQuMS0uMTQyIi8+CiAgICAgICAgPHBhdGggZD0iTTkuMjE5IDEyLjU3M2wtMS41ODQgMS41ODRjLTEuMDk0IDEuMDk0LTIuOTMgMS4wMy00LjEwMi0uMTQxLTEuMTcxLTEuMTcyLTEuMjM0LTMuMDA4LS4xNDEtNC4xMDFMNC41OCA4LjcyN2wuNzkyLS43OTJjMS4wOTMtMS4wOTQgMi45My0xLjAzIDQuMTAxLjE0MSIvPgogICAgPC9nPgo8L3N2Zz4K)www.cnblogs.com](https://www.cnblogs.com/dennisj/p/12550996.html) 

另外阮一峰老师的描述是： process.nextTick方法可以在当前"执行栈"的尾部----下一次Event Loop（主线程读取"任务队列"）之前----触发回调函数。也就是说，它指定的任务总是发生在所有异步任务之前。 另外，由于process.nextTick指定的回调函数是在本次"事件循环"触发，而setImmediate指定的是在下次"事件循环"触发，所以很显然，前者总是比后者发生得早，而且执行效率也高（因为不用检查"任务队列"）。 

参考链接：[![http://www.ruanyifeng.com/blog/2014/10/event-loop.html](data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxOCIgaGVpZ2h0PSIxOCIgdmlld0JveD0iMCAwIDE4IDE4Ij4KICAgIDxnIGZpbGw9Im5vbmUiIGZpbGwtcnVsZT0iZXZlbm9kZCIgc3Ryb2tlPSIjMDI3RkZGIiBzdHJva2UtbGluZWNhcD0icm91bmQiIHN0cm9rZS13aWR0aD0iLjgxOCI+CiAgICAgICAgPHBhdGggZD0iTTguMDMgNS4yNzZsMS41ODUtMS41ODRjMS4wOTMtMS4wOTMgMi45My0xLjAzIDQuMS4xNDEgMS4xNzIgMS4xNzIgMS4yMzYgMy4wMDguMTQyIDQuMTAyTDEyLjY3IDkuMTIzbC0uNzkyLjc5MmMtMS4wOTMgMS4wOTMtMi45MyAxLjAzLTQuMS0uMTQyIi8+CiAgICAgICAgPHBhdGggZD0iTTkuMjE5IDEyLjU3M2wtMS41ODQgMS41ODRjLTEuMDk0IDEuMDk0LTIuOTMgMS4wMy00LjEwMi0uMTQxLTEuMTcxLTEuMTcyLTEuMjM0LTMuMDA4LS4xNDEtNC4xMDFMNC41OCA4LjcyN2wuNzkyLS43OTJjMS4wOTMtMS4wOTQgMi45My0xLjAzIDQuMTAxLjE0MSIvPgogICAgPC9nPgo8L3N2Zz4K)www.ruanyifeng.com](http://www.ruanyifeng.com/blog/2014/10/event-loop.html) 这一段是在node.js 篇幅介绍的，所以process.nextTick我认为就是node.js独有。 都没有讲process.nextTick 是微任务， 所以我的理解更倾向于上面的两种说法，process.nextTick 和EventLoop无关，不算微任务。





![image-20200320151227013](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91c2VyLWdvbGQtY2RuLnhpdHUuaW8vMjAyMC8zLzIzLzE3MTA1NTU2NDkzMzRiYjM?x-oss-process=image/format,png)



![img](https://media.geeksforgeeks.org/wp-content/cdn-uploads/20210122115136/Untitled-Diagram3.jpg)



![image-20200322201434386](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91c2VyLWdvbGQtY2RuLnhpdHUuaW8vMjAyMC8zLzIzLzE3MTA1NTY4NzA4MjBiZDY?x-oss-process=image/format,png)



```

<script> 
 console.log('A'); 
   
 setTimeout(() => { 
    console.log('B'); 
   }, 3000); 
     
 console.log('C'); 
</script> 
```

