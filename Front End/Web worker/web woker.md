# web worker

Web worker是运行在浏览器后台的JavaScript，不会影响到页面；

## 浏览器进程

浏览器是多进程的，每开启一个tab页面就会开启一个进程，每个进行有如下几个线程;

[网络文档](https://yeasy.gitbook.io/docker_practice/image/dockerfile)



### js引擎线程

在js中可以通过timer或promise来执行异步任务，但是异步任务的回调函数还是需要在主线程执行；而**当js代码执行时，页面处于阻塞状态,停止渲染，拒绝交互，禁止动画**；总之应该避免js主线程中执行复杂、耗时的任务；

容易混淆的概念是：将耗时任务放到异步中也是无法避免页面阻塞的；

### http异步请求线程

该线程和js主线程是独立关系，也就是执行http请求不会造成js线程阻塞和页面阻塞，这一点需要理解；



## web worker

[Web Worker 使用教程](http://www.ruanyifeng.com/blog/2018/07/web-worker.html)

service worker也是基于worker运行的；