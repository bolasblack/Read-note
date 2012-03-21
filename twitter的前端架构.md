# twitter 的前端架构
首先，我知道 twitter 的网页（包括手机端）就是一个 Web App ，当然它调用的不是开放给开发者的那种有限制的 API。

## jQuery 1.7.1 
看到他们使用的是最新的 1.7.1 版本，这个倒是确实令我惊讶，颇多小公司还在吭吭哧哧的用 jQuery 1.6.4 什么的呢，人家就用了 1.7.1 了，这才是自信和魄力啊……

## MVC 结构
似乎要谈什么架构、框架什么的，似乎总是离不开 MVC 这三个字母。

我觉得 Twitter 的前端架构也还是可以套用 MVC 的说法的。

其实我想说的是，现在有 [Backbone.js](http://documentcloud.github.com/backbone/) 这个浏览器端的 MVC 库来接管这些事情，写这类的 JavaScript 代码会变得比较轻松……

### V: 模板 mustache.js
我刚刚看了一下 [Who Uses Mustache.js](https://github.com/janl/mustache.js/wiki/beard-competition) （顺带一说，我是非常喜欢这个模板语言的，因为它提倡更少的 logic ，更少的 logic 就代表更少的 magic，致使我认为这个模板语言非常的轻量而且容易扩展），发现 Twitter 确实还是在列的，同时，非常幸运的，我也发现了他们10年9月的一篇关于剖析他们所用技术的一篇[文章](http://engineering.twitter.com/2010/09/tech-behind-new-twittercom.html)；与此同时，我用 Firebug 查看了载入新页面时发出的请求，其中一个请求返回的就是 mustache 风格的模板的数据。

所以我大概确实可以说， Twitter 的网页就是靠 Mustache.js 渲染的。

与此同时，那篇文章还提到，如果浏览器不支持 JavaScript ，那么在发出请求的时候会加上一个名为 `_twitter_noscript` 值为 `1` 的参数，然后页面会由后端渲染（是的，也是用 Mustache 模板语言）后返回给浏览器。

由于目前我的 Twitter 帐号已经变成了新版的界面，同时我也没有什么“不支持 JavaScript”的浏览器，更甚者 w3m 根本无法登入这个网站，所以我暂时无法判断新版 Twitter 是否会在后台进行渲染。

### M: 封装了 API 的 JavaScript 库 @anywhere
这个库负责的就是前端与后台的数据交互。封装了所有提供给自己的 RESTful 风格的 API。

以及提供一些更方便的使用这些 API 的 "feature" （比如在调用 API 的前后都会触发新信息提醒事件等）。

同时使用了浏览器的持久化存储（如 localStorage）缓存 JSON 格式的数据，来最大化的减少请求数与数据包的大小。

### C: url 路由
这里用到的是 html5 提供的 onhashchange、onpopstate 等 API ，来实现的浏览器端的页面路由系统。

当页面被请求的时候，服务器返回的静态页永远都是 [http://twitter.com/] ，具体的页面内容需要在载入页面后由 JavaScript 根据 url hash 进行相应的渲染。

## 其他的细节
### 关于获取新的消息
我原来以为至少在 Chrome 或者 Firefox 其中一个版本里，twitter 会使用 WebSocket 来推送新信息。不过似乎并非如此，我看了下请求，发现依旧是使用轮询的方式。

后来想起来和一个朋友讨论 WebSocket 的时候谈起的，当数据量不大而出现的频率非常高时，我们一致认为轮询消耗的资源反而低于 WebSocket ，Twitter 大概就是属于这个情况。

### 关于 UI
twitter 前端团队是开源过一个叫做 [Bootstrap](http://twitter.github.com/bootstrap/) 的 UI 框架的，提供了相应的 CSS 栅格、界面 UI、以及一些 JavaScript 用于 UI 的库文件。

这个框架脱胎于上个版本的 Twitter ，最新版本 Twitter 的界面颇为类似于手机端（我的 Android 手机上的界面），在细节上和上个版本的区别还是颇多。

### 关于 tweet 字符串的处理
在看 Twitter 打包的 JavaScript 代码的时候才发现在不起眼的地方原来还有一个基于 Apache License 2.0 发布的 JS 库，名字叫做 [twitter-text-js](https://github.com/twitter/twitter-text-js) 。

看介绍这是一个专门用来预处理输入的 Tweet 的 JS 库，几乎算是和 Twitter 绑定的，但是我觉得还是可以看一下源代码的，把处理 @ 关键字和 # 关键字的部分找出来，正好我有个在打算的项目可以用到。

### 关于模块管理工具
Twitter 使用了开源的 [LABjs](http://labjs.com/)，但是我更喜欢 [seajs](http://seajs.com/)，主要是因为 seajs 是可扩展的，有 CoffeeScript 和 LESS 插件能够让我在测试代码的时候方便一点，而且这个东西是由国人（就是我之前在邮件里提到的 玉伯@alipay ）开发的，所以我觉得反馈啊什么的更方便一点Orz……好吧，这个算是扯淡了。

不过反正我就是喜欢 seajs，嗯，我用过豆瓣的 do.js ，也用过算是 fork 自 do.js 的 in.js ，也用过 require.js ，反正就是喜欢 seajs。

因为我是个插件控……我认为一切不可扩展的东西都是邪恶的……

### 最后一个有趣的东西：[Modernizr](http://www.modernizr.com/)
这是用来检测浏览器对 HTML5 和 CSS3 的支持程度的 JS 库，便于开发渐进增强的应用。

使用引入了 Modernizr 后会在 html 标签上加上若干作为标记的类名，譬如 Chromium 在访问 Modernizr 的官网时的类列表：`js no-touch postmessage history multiplebgs boxshadow opacity cssanimations csscolumns cssgradients csstransforms csstransitions fontface localstorage sessionstorage svg inlinesvg blobbuilder bloburls download formdata tk-loaded wf-active wf-proximanova1proximanova2-n4-active wf-proximanova1proximanova2-i4-active wf-proximanova1proximanova2-n7-active wf-proximanova1proximanova2-i7-active wf-proximanovacondensed1proximanovacondensed2-n6-active wf-athelas1athelas2-n4-active`。

当然，也可以通过 `Modernizr.特性名` 的方式来判断浏览器对某特性的支持情况。

## 最后
我想，这样也就差不多了吧……我也不知道有什么好写的。

自己算是投机取巧，选了个比较简单的来写。

其实有时候自己觉得，看起来似乎是 JavaScript 更为麻烦，但是到了一定的程度后，反倒开始觉得 CSS 更为复杂了。

尤其是在 CSS3 出现和像 Tumblr 之流的风格的网站日趋增多的时间里。

今天谈话时“说的模模糊糊”的情况的出现，自己反思了一下，大概是由于自己太过于注重 JavaScript 语言本身的细节，轻视了客户端上的 JavaScript （当然，这也有各个浏览器对 API 的不统一的原因）。比如自己买的关于 JavaScript 的书籍多数只与语言本身有关：《JavaScript 权威指南》（几乎只看核心 JavaScript 的部分……）、《JavaScript 高级程序设计（也不怎么看 DOM 操作部分）》……还有一本刚刚到手的《JavaScript 语言精髓与编程实践》……

好吧，下次弄本 《JavaScript DOM 编程艺术》看看……
