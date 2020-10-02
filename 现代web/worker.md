# Worker

- 专用worker, 即 Worker, (web worker)
- Shared Workers 
  - 可被不同的窗体的多个脚本运行，
  - 例如IFrames等，只要这些workers处于同一主域。
  - 共享worker 比专用 worker 稍微复杂一点 — 脚本必须通过活动端口进行通讯
- Service Workers
  - 一般作为web应用程序、浏览器和网络（如果可用）之间的代理服务。
  - 他们旨在（除开其他方面）创建有效的离线体验，拦截网络请求，以及根据网络是否可用采取合适的行动，更新驻留在服务器上的资源。
  - 还将允许访问推送通知和后台同步API。
  - 也就是用于实现PWA
- Chrome Workers



## web worker

- Web Worker 的目的是让 JavaScript 能够运行在页面主线程之外，不过由于 Web Worker 中是没有当前页面的 DOM 环境的，
- 在 Web Worker 中只能执行一些和 DOM 无关的 JavaScript 脚本，并通过 postMessage 方法将执行的结果返回给主线程。

- **Web Worker 其实就是在渲染进程中开启的一个新线程（与service worker区别），它的生命周期是和页面关联的。**
- 不过 Web Worker 是临时的，**每次 JavaScript 脚本执行完成之后都会退出**，执行**结果也不能保存下来**，如果下次还有同样的操作，就还得重新来一遍



## ServiceWorker

它的主要思想是在**页面和网络之间增加一个拦截器，用来缓存和拦截请求**。

![](https://static001.geekbang.org/resource/image/23/12/23b97b087c346cdd378b26b2d158e812.png)

- 在没有安装 Service Worker 之前，WebApp 都是直接通过网络模块来请求资源的。
- 安装了 Service Worker 模块之后，**WebApp 请求资源时，会先通过 Service Worker**，**让它判断是返回 Service Worker 缓存的资源还是重新去网络请求资源。一切的控制权都交由 Service Worker 来处理。**
- 由于 Service Worker 还需要会为多个页面提供服务，所以不能把 Service Worker 和单个页面绑定起来。
- S**ervice Worker 是运行在浏览器进程中的**(与web worker的区别)，因为浏览器进程生命周期是最长的，所以在浏览器的生命周期内，能够为所有的页面提供服务。



## SharedWorker

一种特定类型的 worker

可以从几个浏览上下文中*访问*，例如几个窗口、iframe 或其他 worker。它们实现一个不同于普通 worker 的接口，具有不同的全局作用域, [`SharedWorkerGlobalScope`](https://developer.mozilla.org/zh-CN/docs/Web/API/SharedWorkerGlobalScope) 。









