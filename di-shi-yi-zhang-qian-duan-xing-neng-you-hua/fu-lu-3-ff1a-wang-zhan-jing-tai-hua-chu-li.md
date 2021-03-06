# 附录3：网站静态化处理

## 一、为什么要页面静态化

页面静态化，其实就是将动态生成的jsp页面，变成静态的HTML页面，让用户直接访问。有一下几方面好处：
1. 首先就是**访问速度**，不需要去访问数据库，或者缓存来获取哪些数据，浏览器直接加载渲染html页即可。所以可以大大的提高访问效率；
2. 从**网站优化**来分析，搜索引擎更喜欢静态的网页，静态网页与动态网页相比，搜索引擎更喜欢静的，更便于抓取，搜索引擎**SEO**排名更容易提高。
3. 从**安全**角度讲，静态网页不宜遭到黑客攻击，如果黑客不知道你网站的后台、网站采用程序、数据库的地址，静态网页， 更不容易受到黑客的攻击。
4. 从网站**稳定性**来讲，如果程序、数据库出了问题，会直接影响网站的访问，而静态网页就避免了如此情况，不会因为程序等，而损失网站数据，影响正常打开，损失用户体验，影响网站信任度。

当然有好处，也有不足
1. 信息不同步。只有重新生成HTML页面，才能保持信息同步。
2. 服务器存储问题。数据一直增加，静态HTML页面会不断增加，会占用大量的磁盘。需要考虑这个问题
3. 静态化算法的精密性。要良好的处理数据与网页模板，及各种文件链接路径的问题，这就要求我们在静态化的算法中考虑到方方面面。稍有细小疏忽，将导致生成的页面中存在这样或那样的错误链接，甚至存在死链。

因此，我们必须恰到好处的解决这些问题。既不能增加算法的可能性，又要照顾到方方面面。做到这一点，的确不容易。

## 二、静态化方案

### 2.1 CDN技术

由于静态网站的请求资源是不会经常发生变化的，那么这种资源其实很容易被迁移，我们都知道网络传输的效率是和距离长短有关系的，既然静态资源很容易被迁移那么我们就可以把静态资源服务器按地域分布在多个服务节点上，当用户请求网站时候根据一个路由算法将请求落地在离用户最近的节点上，这样就可以减少网络传输的距离从而提升访问的效率，这就是我们长提的大名鼎鼎的CDN技术，内容分发网络技术。

CDN技术应该由三个步骤组成
- 首先是解析DNS，找到离用户最近的CDN服务器
- 接下来CDN要做一下负载均衡，根据负载均衡策略将请求落地到最合适的一个服务器上，如果CDN服务器上就有用户所需要的静态资源，那么这个资源就会直接返回给浏览器，如果没有CDN服务器会请求远端的服务器，拉取资源再把资源返回给浏览器
- 如此同时拉取的资源也被缓存在CDN服务器上，下次访问就不需要在请求远端的服务器了，CDN存储资源的方式使用的是缓存，这个缓存的载体是和apache，nginx类似的服务器，我们一般称之为http加速器

之所以成为http加速器是为了和传统静态web服务器区别开来，传统的静态资源服务器一般都是从持久化设备上读取文件，而http加速器则是从内存里读取，不过具体存储的计算模型会根据硬件特点做优化使得资源读取的效率更高，常见的http加速器有varnish，squid。Ngnix加上缓存模块也是可以当做http加速器使用的，不管使用什么技术CDN的服务器基本都是做一个就近的缓存操作。

### 2.2 PHP静态化

PHP静态化的简单理解就是使网站生成页面以静态HTML的形式展现在访客面前，PHP静态化分纯静态化和伪静态化，两者的区别在于PHP生成静态页面的处理机制不同。
- 纯静态化：PHP生成HTML文件
- 伪静态化：把内容存放在nosql内存（memcached），然后访问页面的时候，直接从内存里面读取。

> 详细可参考：[《分享常见的几种页面静态化的方法》](http://www.jb51.net/article/59619.htm)



*******

**参考资料**
- [浅谈——页面静态化](https://www.cnblogs.com/coprince/p/5752153.html)
- [网站静态化方案（一）静态化原理](https://www.jianshu.com/p/344b171a6450)

**延伸阅读**
- [关于大型网站技术演进的思考（九）--网站静态化处理--总述（1）](https://www.cnblogs.com/sharpxiajun/p/4282789.html)
- [关于大型网站技术演进的思考（十）--网站静态化处理—动静整合方案（2）](http://www.cnblogs.com/sharpxiajun/p/4285085.html)
- [关于大型网站技术演进的思考（十一）--网站静态化处理—动静分离策略（3）](http://www.cnblogs.com/sharpxiajun/p/4287011.html)
