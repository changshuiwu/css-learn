### css加载会造成阻塞吗？

可能大家都知道，js执行会阻塞DOM树的解析和渲染，那么css加载会阻塞DOM树的解析和渲染吗？
#### css加载会阻塞DOM树解析？
css并不会阻塞DOM树的解析，加载css的过程，依旧会解析DOM树，所以在css加载完，就能直接操作DOM。
#### css加载会阻塞DOM树渲染？
当css还没有加载出来，页面显示白屏，直到css加载完成之后，才能显示出来。也就是说。内容虽然解析了，但是并没有被渲染出来。所以，css加载会

#### 这种机制的评价
这个可能是浏览器的一种优化机制。因为你加载css的时候，可能会修改下面DOM节点的样式，如果css加载不阻塞DOM数的渲染，那么当css加载完成之后，DOM树可能又得重新重绘或者回流。这样会造成没有必要的损耗。所以干脆就先把DOM树结构解析完，然后等css加载完之后，根据最终样式来渲染DOM树。这样性能会好一点。  

#### css加载会阻塞js运行吗？
位于css加载语句之后的js代码，会等到css加载完成之后，才会执行。

### 结论
1. css加载不会阻塞DOM树的解析
2. css加载会阻塞DOM树的渲染
3. css加载会阻塞后面js语句的执行

##### 解决方法
* 使用CDN(因为CDN会根据你的网络状况，替你挑选最近的一个具有缓存内容的节点为你提供资源，因此可以减少加载时间)
* 对css进行压缩。
* 合理的使用缓存(设置cache-control, expires,E-tag.不过要注意文件更新后，要避免缓存带来的影响。)
* 减少http请求数量，将多个css文件合并。



### 原理分析(浏览器渲染)
1. HTML解析文件，生成DOM Tree， 解析css文件生成 CSSOM Tree
2.将DOM Tree 和CSSOM Tree结合，生成Render Tree(也就是常说的渲染树)
3. 根据Render Tree进行绘制， 渲染到屏幕上。


#### DOMContentLoaded 和onLoad
对于浏览器来说，页面上有两个事件，
1. onload 就是等待页面上所有的资源都加载完成才会触发，包括css，js图片，视频等资源。
2. DoMContentLOaded ，当页面的内容解析完成后，就会触发。
  * 如果页面中同时存在css和js，并且js在css后面，则DOMContentLoaded事件会在css加载完后才执行。
  * 其他情况下，DOMContentLoaded都不会等待css加载，并且DOMContentLoaded事件也不会等待图片、视频等其他资源加载
