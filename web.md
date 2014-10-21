web.md

tcp连接。

- Q: [浏览器允许的并发请求资源数是什么意思？](http://www.zhihu.com/question/20474326)
 A: 浏览器的并发请求数目限制是针对同一域名的。同一时间针对同一域名下的请求有一定数量限制。超过限制数目的请求会被阻塞，这就是为什么会有zhimg.com, http://twimg.com 之类域名的原因。
（这是其中一个原因，另一个主要原因是，向 http://zhihu.com 请求资源会把 http://zhihu.com 下本地的所有 cookie 发送过去，这是请求图片，js等资源不需要的，会造成很大的浪费。

  ![](http://pic4.zhimg.com/ea606d016e8ab77db9d8a8dfa5243a1b_r.jpg)


- [Yahoo前端优化十四条军规](http://yuehu.io/padding-me/419)

  1. 第一条、尽可能的减少 HTTP 的请求数 （Make Fewer HTTP Requests ）

    合并 css，js（将一个页面中的 css 和 js 文件分别合并）以及 Image maps 和 css sprites 等。大型的网站直接把css,js写在首页里。

    <http://www.csssprites.com/>可自动将你上传的图片合并并给出对应的 `background-position` 坐标。并将结果以 png 和 gif 的格式输出。

  2.第二条、使用 CDN（内容分发网络）: Use a Content Delivery Network

   > CDN:通过在现有的 Internet 中增加一层新的网络架构，将网站的内容发布到最接近用户的 cache 服务器内，通过 DNS 负载均衡的技术，判断用户来源就近访问 cache 服务器取得所需的内容，杭州的用户访问近杭州服务器上的内容，北京的访问 近北京服务器上的内容。这样可以有效减少数据在网络上传输的时间，提高速度。

   > 内容分发网络是一个经策略性部署的整体系统，包括分布式存储、负载均衡、网络请求的重定向和内容管理4个要求，而内容管理和全局的网络流量管理是CDN的核心所在。通过用户就近性和服务器负载的判断，CDN确保内容以一种极为高效的方式为用户的请求提供服务。

  3. 第三条、 添加 Expire/Cache-Control 头：Add an Expires Header

   Expire 其实就是通过 header 报文来指定特定类型的文件在览器中的缓存时间。在服务器端添加。

  4. 第四条、启用 Gzip 压缩：Gzip Components

   Gzip 的思想就是把文件先在服务器端进行压缩，然后再传输。这样可以显著减少文件传输的大小。传输完毕后浏览器会 重新对压缩过的内容进行解压缩，并执行。

  5. 第五条、将 css 放在页面最上面 （ Put Stylesheets at the Top）

    如 IE，把样式表放在页面的底部的问题在于它禁止了网页内容的顺序显示。浏览器阻止显示以免重画页面元素，那用户只能看到空白页了。Firefox 不会阻止显示，但这意味着当样式表下载后，有些页面元素可能需要重画，这导致闪烁问题。所以我们应该尽快让 css 加载完毕。(FOCUS)

  6. 第六条、将 script 放在页面最下面 （Put Scripts at the Bottom ）

    * 因为防止 script 脚本的执行阻塞页面的下载。在页面 loading 的过程中，当浏览器读到 js 执行语句的时候一定会把它全部解释完毕后在会接下来读下 面的内容。
    * 脚本引起的第二个问题是它阻塞并行下载数量。HTTP/1.1 规范建议浏览器每个主机的并行下载数不超过 2 个（IE 只能为 2 个，其他浏览器如 ff 等都是默认设置为 2 个，不过新出的 ie8 可以达 6 个）。

  7. 第七条、避免在 CSS 中使用 Expressions （Avoid CSS Expressions ）

  8. 第八条、把 javascript 和 css 都放到外部文件中 （Make JavaScript and CSS External ）

   不仅从性能优化上会这么做，用代码易于维护的角度看也应该这么做。把 css 和 js 写在页面内容可以减少 2 次请求，但也增 大了页面的大小。如果已经对 css 和 js 做了缓存，那也就没有 2 次多余的 http 请求了。

  9. 第九条、减少 DNS 查询 (Reduce DNS Lookups)

   ![](http://photo.blog.sina.com.cn/showpic.html#blogid=735f29100101dbgj&url=http://album.sina.com.cn/pic/0026ZEdigy6EoC5LveE20)

   一次 DNS 的解析过程会消耗 20-120 毫秒的 时间, 在 dns 查询结束之前，浏览器不会下载该域名下的任何东西。所以减少 dns 查询的时间可以加快页面的加载速度。yahoo 的建议一个页面所包含的域 名数尽量控制在 2-4 个。

  10. 第十条、压缩 JavaScript 和 CSS (Minify JavaScript )

  11. 第十一条、避免重定向 (Avoid Redirects )

   发生重定向的原因还有很多，但是不变的是每增加一次重定向就会增加一次 web 请求，所以因该尽量减少。

  12. 第十二条、移除重复的脚本 (Remove Duplicate Scripts )

  13. 第十三条、配置实体标签（ETags） (Configure ETags )

   不懂。[使用ETags减少Web应用带宽和负载](http://www.infoq.com/cn/articles/etags)

  14. 第十四条、使 AJAX 缓存 (Make Ajax Cacheable )

   ajax 还要去缓存？做 ajax 请求的时候往往还要增加一个时间戳去避免他缓存。It’s important to remember that “asynchronous” does not imply “instantaneous”.（记住“异步”不是“瞬间”这一点很重要）。记住，即使 AJAX 是动态产生的而且只对一个用户起作用，他们依然可以被缓存。

- [前端工程精粹（一）：静态资源版本更新与缓存](http://www.infoq.com/cn/articles/front-end-engineering-and-performance-optimization-part1)

|优化方向|优化手段|
|---|---|
|请求数量|合并脚本和样式表，CSS Sprites，拆分初始化负载，划分主域|
|请求带宽|开启GZip，精简JavaScript，移除重复脚本，图像优化|
|缓存利用|使用CDN，使用外部JavaScript和CSS，添加Expires头，减少DNS查找，配置ETag，使AjaX可缓存|
|页面结构|将样式表放在顶部，将脚本放在底部，尽早刷新文档的输出|
|代码校验|避免CSS表达式，避免重定向|



