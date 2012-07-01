This free book is the book I would have wanted when I started working with single page apps. It's not an API reference on a particular framework, rather, the focus is on discussing patterns, implementation choices and decent practices.

这是一本 free book ，我写他是因为当我刚开始做单页面应用的时候我就希望有这么一本书（当然那个时候没有）。这本书不是某个框架的接口文档，相反，它着重于讨论制作单页面应用的模式，选择和实践。

I'm taking a "code and concepts" approach to the topic - the best way to learn how to use something is to understand how it is implemented. My ambition here is to decompose the problem of writing a web app, take a fresh look at it and hopefully make better decisions the next time you make one.

我给每个主题增加了一个叫做 “代码与概念” 的部分——了解一个东西怎么用的最好方法就是知道它是怎么工作的。我的目的是把写一个 Web App 时会遇到的问题分解开，这样你下次需要做一个单页面应用时就能够针对这些问题做一个更好的解决方案啦。

I'm releasing this a bit early, since I'm heading to Nodeconf. In my writing, I may sound slightly opinionated, but I am happy to be proven wrong. Your feedback is much appreciated!

这本书在我去过 Nodeconf 后我就发布了，在我写这本书的时候，我可能会有点自以为是，but i am happy to be proven wrong，你的反馈就是最好的赞赏了。

Introduction 大纲

  * [Modern single page apps - an overview](http://singlepageappbook.com/goal.html)
  * 现代单页面应用程序 - 一个总览

Writing maintainable code 编写可维护的代码

  * [Maintainability depends on modularity: Stop using namespaces!](http://singlepageappbook.com/maintainability1.html)
  * 可维护性依赖于模块化：别再用命名空间啦
  * [Getting to maintainable](http://singlepageappbook.com/maintainability2.html)
  * 让代码开始变得可维护起来吧
  * [Testing explained](http://singlepageappbook.com/maintainability3.html)
  * 有关测试的解释

Implementation alternatives: a look at the options 实现原理二选一：看看选项

  * [The view layer](http://singlepageappbook.com/detail1.html)
  * 视图层
  * [The model layer](http://singlepageappbook.com/detail2.html)
  * 模型层

Meditations on Models & Collections 关于模型和集合的思考

  * [Implementing a data source](http://singlepageappbook.com/collections1.html)
  * 实现一个一个数据源
  * [Implementing a model](http://singlepageappbook.com/collections2.html)
  * 实现一个模型
  * [Implementing a collection](http://singlepageappbook.com/collections3.html)
  * 实现一个集合
  * [Implementing a data cache](http://singlepageappbook.com/collections4.html)
  * 实现一个数据缓存
  * [Implementing associations](http://singlepageappbook.com/collections5.html)
  * 实现关联

Views - templating, behavior and event consumption 视图 - 模板，行为和事件消费

  * [Templating: from data to HTML](http://singlepageappbook.com/views1.html)
  * 模板：从数据到 HTML
  * [Behavior: binding DOM events to HTML and responding to events](http://singlepageappbook.com/views2.html)
  * 行为：在 HTML 上绑定 DOM 事件以及反馈给事件
  * [Consuming events from the model layer: communication between views and re-rendering views in response to model data changes](http://singlepageappbook.com/views3.html)
