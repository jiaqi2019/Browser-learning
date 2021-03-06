# HTTP/2

## HTTP 1.1 的缺点

虽然 http1.1 使用6个tcp持续链接，加快了请求速度，但是对**带宽的利用率却并不理想**，因为 HTTP/1.1 很难将带宽用满，

比如我们常说的 100M 带宽，实际的下载速度能达到 12.5M/S，而采用 HTTP/1.1 时，也许在加载页面资源时最大只能使用到 2.5M/S，很难将 12.5M 全部用满。



**主要原因**：

- **TCP 的慢启动**
  - 页面关键资源很小，如html，js，css,这些关键资源都是最开始请求的，所以由于慢启动，会耗费时间长一些
  - 导致首次渲染延后

- **同时开启了多条 TCP 连接，那么这些连接会竞争固定的带宽**
  - 假如使用3个CND，就要建立18条TCP链接，带宽不足时，每个链接都要减慢接受速度
  - 因为有个链接**下载关键资源**，有的是图片等其他资源
  - 但18条TCP链接之间不能协商哪些优先下载，从而影响关键资源的下载速度
- **HTTP/1.1 队头阻塞的问题**
  - 由于持续链接，共用一条tcp链接，一个tcp连接中同一时刻只能处理一个请求，在当前的请求没有结束之前，其他的请求只能处于阻塞状态



## HTTP 2.0 特性

http1.1的问题中，1，2由于tcp机制导致，3由于http1.1机制导致的。http2解决思路：

- 对于1，2：一个域名只使用一个 TCP 长连接来传输数据，这样整个页面资源的下载过程只需要一次慢启动，同时也避免了多个 TCP 连接竞争带宽所带来的问题
- 对于3：HTTP/2 需要实现资源的并行请求，也就是任何时候都可以将请求发送给服务器，而并不需要等待其他请求的完成，然后服务器也可以随时返回处理好的请求资源给浏览器。

总结：1一个域名只使用一个 TCP 长连接和消除队头阻塞问题

### 多路复用——充分利用带宽

每个请求都有一个对应的 ID,将请求分成一帧一帧的数据去传输，这样带来了一个额外的好处，就是当收到一个优先级高的请求时，比如接收到 JavaScript 或者 CSS 关键资源的请求，服务器可以暂停之前的请求来优先处理关键资源的请求。

通过二进制分帧层实现多路复用

流程：

- 浏览器准备好请求数据，带有ID编号
- 经过二进制分帧层后，被转换为一个个带有ID编号的帧，通过协议栈将这些发送给服务器
- 服务器收到帧后，将所有ID相同的帧合并为一条完整的请求信息
- 然后处理请求，将响应发送至二进制分帧层
- 二进制分帧层将响应数据转换为一个个带有请求ID编号的帧，经过协议栈发给浏览器
- 浏览器接收到响应帧之后，会根据 ID 编号将帧的数据提交给对应的请求。



<img src="https://static001.geekbang.org/resource/image/86/6a/86cdf01a3af7f4f755d28917e58aae6a.png" style="zoom:50%;" />

### 其他特性

- **可以设置请求的优先级**
  - 可以在发送请求时，标上该请求的优先级，这样服务器接收到请求之后，会优先处理优先级高的请求
- **服务器推送**
  - HTTP/2 还可以直接将数据提前推送到浏览器
  - 当用户请求一个 HTML 页面之后，服务器知道该 HTML 页面会引用几个重要的 JavaScript 文件和 CSS 文件，那么在接收到 HTML 请求之后，附带将要使用的 CSS 文件和 JavaScript 文件一并发送给浏览器，这样当浏览器解析完 HTML 文件之后，就能直接拿到需要的 CSS 文件和 JavaScript 文件，这对首次打开页面的速度起到了至关重要的作用
- **头部压缩**
  - HTTP/2 对请求头和响应头进行了压缩，你可能觉得一个 HTTP 的头文件没有多大，压不压缩可能关系不大，
  - 通常情况下页面也有 100 个左右的资源，如果将这 100 个请求头的数据压缩为原来的 20%，那么传输效率肯定能得到大幅提升。











