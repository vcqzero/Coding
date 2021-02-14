

## service worker使用

pwa是渐进式应用程序的缩写，得力于service work新功能；网页（client）可以注册一个service work服务。

service work服务运行在独立于网页JavaScript主线程之外的线程中；

### 推荐阅读

https://lavas-project.github.io/pwa-book/chapter04/4-service-worker-debug.html

### 关于 service worker 的几个基本知识点

- 它是一个可编程的网络代理，让你可以控制页面请求的处理方式。
- 它是一个 [JavaScript Worker](https://www.html5rocks.com/en/tutorials/workers/basics/)，因此它无法直接操作 DOM。但可以通过 [postMessage](https://html.spec.whatwg.org/multipage/workers.html#dom-worker-postmessage) 接口与页面通信。同时，service worker 中的代码不会阻塞页面响应。
- 它在闲置时被终止，在需要时被启动。并不是常驻内存。因此你不能在 `onfetch` 或是 `onmessage` 回调中依赖全局状态。
- 被设计成完全异步。因此在 service worker 中无法使用同步 API （例如同步 XHR，localStorage等）。接口重度依赖于 promise。
- 只能在 HTTPS 页面加载（唯一的例外：localhost/127.0.0.1，方便调试）。   



### Service worker 的作用域

一个 service worker 的默认作用域是这个 service worker 脚本所在的目录。例如 `https://example.com/sw.js` 脚本默认就是 `https://example.com` 下的所有页面。

rvice Worker 注册的默认作用域是与脚本网址相对的 `./`。这意味着如果您在 `//example.com/foo/bar.js` 注册一个 Service Worker，则它的默认作用域为 `//example.com/foo/`

你也可以在注册 service worker 时明确指定作用域：

```js
navigator.serviceWorker.register('sw.js', {
    scope: './abc'
});
```

假设以上代码在 `https://example.com` 页面里执行，则意味着该 service worker 的作用域就是 `https://example.com/abc` 下的页面。

我们把页面、workers 以及 shared workers 统称为 `clients`。你的 service worker 只能控制其作用域范围内的 clients。

你可以通过检查 `navigator.serviceWorker.controller` 属性来判断某个 client 是否受控于 service worker 之下。



### 安装

+ 首先准备sw.js文件(该文件不会也不应该被service worker缓存)；

+ 在页面中注册service worker

```javascript
if('serviceWorker' in navigator) {
    navigator.serviceWorker.register('/pwa-examples/js13kpwa/sw.js');
};
```

+ ### Service worker 的生命周期

  Service worker 的生命周期与页面的生命周期是完全独立的。

  #### 1. 注册 service worker

  在 `register()` 时传入的 service worker 脚本的路径决定了此 service worker 的作用域。

  ```js
    // 检测浏览器是否支持 service worker API
    if ('serviceWorker' in navigator) {
      navigator.serviceWorker.register('/sw.js').then(function(registration) {
        console.log('ServiceWorker registration successful with scope: ', registration.scope);
      }, function(err) {
        console.log('ServiceWorker registration failed: ', err);
      });
    }
  ```

  #### 2. `install` 事件

  在注册 service worker 之后，从 service worker 的视角来看。它收到的第一个事件就是 `install` 事件。在 `install` 事件回调函数中，你可以：

  1. 打开一组缓存  
  2. 缓存所需文件
  3. 检查所有需要的文件是否都已被缓存
  4. 如果缓存成功则service worker安装成功，否则安装失败，被浏览器忽略；

  ```js
  var CACHE_NAME = 'my-site-cache-v1';
  var urlsToCache = [
    '/',
    '/styles/main.css',
    '/script/main.js'
  ];
  // 
  self.addEventListener('install', function(event) {
    // waitUntil函数表示，函数内容仍然在install阶段，即延长生命周期
    // 当返回解决的promise表示install完成，否则install失败
    event.waitUntil(
      // caches 代表 CacheStorage对象
      caches.open(CACHE_NAME) //打开一组命名缓存  
        .then(function(cache) {
          return cache.addAll(urlsToCache);
        })
    );
  });
  ```

  #### 3. `activate` 事件

  在 `activate` 事件回调中通常要做的工作就是缓存管理。此时可以安全的清理之前版本 service worker 创建的缓存内容。

  #### 4. `fetch` 事件

  安装完成以后，当页面发起网络请求时，会触发 service worker 的 `fetch` 事件。在事件回调函数中你可以决定如何处理该请求。

  例如，优先从缓存中加载：

  ```js
  self.addEventListener('fetch', function(event) {
    event.respondWith(
      caches.match(event.request)
        .then(function(response) {
          // 命中缓存，直接把缓存的内容返回给页面
          if (response) {
            return response;
          }
          
          // 否则，请求网络
          return fetch(event.request);
        }
      )
    );
  });
  ```

  ### 更新 service worker

  以下情况下会触发更新：

  - 在浏览器中导航到一个作用域内的页面。
  - 更新 `push` 和 `sync` 等功能事件，除非在前 24 小时内已进行更新检查。
  - 代码中调用 `.register()`，*仅在* Service Worker 网址已发生变化时。

  

  更新一个 service worker 的流程大致如下：

  1. 修改 service worker 的脚本文件。当用户再次访问页面时，浏览器会尝试重新下载脚本文件。并与之前的版本比对。一旦发现文件内容不一致，就会进入更新流程。（每次用户在作用域内导航时均会触发更新判断）
  2. 新的 service worker 会被启动并触发 `install` 事件---安装新的service worker。
  3. 此时页面的控制器权还在老版 service worker 手中，而新版 service worker 进入 `waiting` 状态。
  4. 当前页面被关闭，老版 service worker 被终止。（注意：刷新页面不足以触发新老 service worker 交接）
  5. 用户再次访问页面，新版 service worker 被启动。触发 `activate` 事件。

  **注：要想在新版 service worker 安装完成后立刻接管页面而不必等到下一次加载页面。可以调用 `self.skipWaiting()` 方法跳过等待状态。**

  ## workbox

  workbox是谷歌开发的一套service worker工具库，可以方便实现页面的pwa;

  ### 预缓存资源

  #### 安装

  + 当sw在安装注册安装时，会下载、缓存所有在预缓存列表中的资源；
  + 当预缓存发生错误时，sw安装失败；

  #### 更新

  + 当sw更新时会重新预缓存资源；

  + workbox会比对新旧缓存列表，会缓存新资源（install阶段），和删除旧资源（activate阶段），比对的标准就是url或revision。如果新旧service worker中存在相同的资源，则不会重新下载。

  #### cacheName

  当使用workbox的`prefix` `suffix` 导致资源缓存的key改变时，会重新缓存所有资源；但是这时旧资源不会被删除，需要手动删除。要不然会缓存不会自动删除；

  ```javascript
  // 删除旧版本的预缓存资源
  self.addEventListener("activate", function(event) {
    event.waitUntil(
      caches.keys().then(function(keyList) {
        return Promise.all(
          keyList.map(function(key) {
            // 仅删除旧版本的预缓存资源
            // 不处理允许时缓存资源
            if (key.includes("precache") && !key.includes(cacheConfig.suffix)) {
              return caches.delete(key);
            }
          })
        );
      })
    );
  });
  ```

  ## 缓存资源

  + service worker使用CacheStorage Api进行资源的缓存;
  + CacheStorage是同源管理的，每个域名下可以有若干的具名cache;
  + 每个域下cache是有大小限制的，在未达到限制之前，浏览器不会去处理缓存，当达到缓存限制时，浏览器会尝试删除缓存，当是浏览器删除缓存可能比较粗暴，所有需要开发人员主动去设置清理缓存策略；

  ## 最佳实践

  [饿了么pwa最佳实践](https://juejin.im/entry/6844903470181384200)

  

  ### 降级方案

  ```javascript
  if ('serviceWorker' in navigator) {
    fetch(/use-service-worker/) // 查询服务端，是否降级。 该接口不可做缓存!!!
    .then(() => {
      if (降级) {
        // 注销所有已安装的 SW
        navigator.serviceWorker.ready.then(function (registration) {
          registration.unregister()
        })
      } else {
        // 注册 SW
        navigator.serviceWorker.register(swUrl, registrationOptions)
      }
    })
  }
  ```

  ### 错误监控

  可以在service worker.js中增加错误监控。

  

  ### 参考资料

  - [Service Workers: an Introduction | Web | Google Developers](https://developers.google.com/web/fundamentals/getting-started/primers/service-workers)
  - [Service Worker API - Web APIs | MDN](https://developer.mozilla.org/en/docs/Web/API/Service_Worker_API)
  - [The Service Worker Lifecycle | Web | Google Developers](https://developers.google.com/web/fundamentals/instant-and-offline/service-worker/lifecycle)

