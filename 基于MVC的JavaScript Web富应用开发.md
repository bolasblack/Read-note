# 基于 MVC 的 JavaScript Web 富应用开发

## 第一章 MVC 和类

### 什么是 MVC

> MVC 是一种设计模式，它将应用划分为三个部分：数据（模型），展现层（视图）和用户交互层（控制器）。

其实 MVC 模式在我看来最为重要的部分是分层的思想，而设计模式的细节则并没有那么重要。毕竟总是会有新概念出现的。

### 函数调用

> JavaScript 中允许更换上下文是为了共享状态，尤其在事件回调中。

```coffeescript
proxy = (func, context) ->
  -> func.apply context, arguments

clicky =
  wasClicked: ->
  addListeners: ->
    $(".clicky").click proxy @wasClicked, this
```

### 控制“类”库的作用域

> 上文提到的 `proxy` 函数是一个非常有用的模式，在新版本中的 JavaScript —— ECMAScript 5 (ES5) 中同样加入了 `bind()` 函数以控制调用的作用域。`bind()` 函数位于函数的原型链中，例如：

```coffeescript
Button =
  click: ->
  init: (@elem) ->
    @$elem = $ @elem
    @$elem.click @click.bind this
```

在老版本的浏览器中可以引用 [es5-shim](https://github.com/kriskowal/es5-shim) 库以实现 ES5 中尽可能多的特性，比如上文的 `bind()` ，以及下文的 `Object.create()` 等。

### “类”库

> jQuery 本身不支持类，但通过插件的方式可以轻易引入类的支持，比如 [HJS](http://archive.plugins.jquery.com/project/HJS)

> jQuery 的作者 John Resig 在他的博客中写过一篇文章，专门讲解如何实现经典的类继承(http://goo.gl/09l0v)，这篇文章也值得一读，尤其是当你想挖掘 JavaScript 原型系统背后的真相的时候。

## 第二章 事件和监听

有关事件对象的方法和属性，可以参看 [jQuery 文档](http://api.jquery.com/category/events/event-object/)、[W3School.com.cn](http://www.w3school.com.cn/htmldom/dom_obj_event.asp) 等站点。

### 自定义事件和 jQuery 插件

```html
<ul id="tabs">
  <li data-tab="users">Users</li>
  <li data-tab="groups">Groups</li>
</ul>

<div id="tabsContent">
  <div data-tab="users">...</div>
  <div data-tab="groups">...</div>
</div>
```

```coffeescript
jQuery.fn.tabs = (control) ->
  $elem = $ this
  $control = $ control

  $elem.on "click", "li", ->
    tabName = $(this).attr "data-tab"
    $elem.trigger "change.tabs", tabName

  $elem.on "change.tabs", (event, tabName) ->
    $elem.find("li").removeClass "active"
    $elem.find(">[data-tab#{tabName}]").addClass "active"

    $control.find(">[data-tab]").removeClass "active"
    $control.find(">[data-tab=#{tabName}]").addClass "active"

  firstName = $elem.find("li:first").attr "data-tab"
  $elem.trigger "change.tabs", firstName

  this
```

这让插件代码更具扩展性，比如可以直接更改选项卡的状态：

```coffeescript
$("#tabs").trigger "change.tabs", "users"
```

也可以和 urlhash 作关联，这样就可以使用浏览器的后退按钮了：

```coffeescript
$("#tabs").on "change.tabs", (event, tabName) ->
  window.location.hash = tabName

$(window).on "hashchange", ->
  tabName = window.location.hash.slice 1
  $("#tabs").trigger "change.tabs", tabName
```

### DOM 无关事件

> 基于事件的编程非常强大，因为它能够让你的应用架构充分解耦，让功能变得更加内聚且具有更好的可维护性。这种模式称为 [发布/订阅](http://en.wikipedia.org/wiki/Publish/subscibe)。

> 发布/订阅 (Pub/Sub) 是一种消息模式，它有两个参与者：发布者和订阅者。发布者向某个信道 (channel) 发布一条消息，订阅者绑定这个信道，当有消息发布至信道时就会接收到一个通知。最重要的一点是，发布者和订阅者是完全解耦的，彼此并不知晓对方的存在，两者仅仅共享一个信道名称。

```coffeescript
PubSub =
  subscibe: (event, callback) ->
    @_callback or= {}
    (@_callback[event] or= []).push callback
    this

  publish: (event, args...)->
    return this unless calls = @_callback
    return this unless list = calls[event]
    call.apply this, args for call in list
```

如果正在使用 jQuery , 可以关注一下 [Ben Alman](http://benalman.com) 写的一个更容易的库：

<script src="https://gist.github.com/799721.js?file=jquery.ba-tinypubsub.js"></script>

## 第三章 模型和数据

### 构建对象关系映射 (ORM)

> 对象关系映射（ Object-relational mapper，简称 ORM ）是在除 JavaScript 以外的编程语言中常见的一种数据结构。

> 本质上讲，ORM 是一个包装了一些数据的对象层。以往 ORM 常用语抽象 SQL 数据库，但在这里 ORM 只是用于抽象 JavaScript 数据类型。这个额外的层有一个好处，我们可以通过给它添加自定义的函数和属性来增强基础数据的功能。

> 这样能够增加代码的重用率。

`Object.create()` 只有一个参数即原型对象，它返回一个新对象，这个新对象的原型就是新传入的参数。`Object.create()` 是属于 ES5 规范的特性。

### 增加 ID 支持

Robert Kieffer 写了一个简单明了的 GUID(Globally Unique Identifier) [生成器](http://goo.gl/0b0hu)，它使用 `Math.random()` 来产生一个伪随机数的 GUID

## 第四章 控制器和状态

略。

## 第五章 视图和模板

### 模板

jQuery.tmpl 是由微软开发的，是在 John Resig 的[原始工作](http://goo.gl/sFh6c)的基础上作的模板插件，该库目前仍在维护中，并在 jQuery 官网上有完整的[文档](http://api.jquery.com/jquery.tmpl)。

### 模板 Helpers

> 有时在视图内部使用“通用 helper 函数”(generic helper function) 是非常好用的，我们需要保持良好的 MVC 架构的思路，而不是直接在视图中任意添加感兴趣的函数

我们应当将他抽象出来，并用命名空间进行管理，而不是直接将函数残杂进视图中，这样才能保持逻辑和视图之间的解耦。

同时，抽象出 helper 函数还能够提高代码的重用率。

### 模板存储

可考虑的解决方案有这么几个：

* 在 JavaScript 中以行内形式存储
* 在自定义 script 中以行内形式存储
* 远程加载
* 在 HTML 中以行内形式存储

> 你可以将模板保存在 JavaScript 文件中，但并不推荐这样做，因为这需要将视图的代码放入一个控制器中，这违背了 MVC 的原则。

> 你可以根据需要通过 Ajax 动态加载模板。这种方法的好处是初始化页面的体积非常小，缺点是模板加载时 UI 的渲染会变得很慢。使用 JavaScript 来构建应用的主要原因就是速度问题...

> 你还可以把模板保存在 HTML 中，以行内的形式保存。好处是它不存在 UI 加载慢的问题，源代码也更清晰。缺点也是显而易见，它增加了页面的体积。坦白讲，这种体积增加造成的性能损失微不足道，特别是当服务器开启了压缩和缓存的情况下。

其实 [brunch](http://brunch.io) 提供了另一种思路，基于模块加载器，就是将模板作为一个模块，与其他 JavaScript 模块合并为一个文件，需要模板时直接 `request("templateName")` 。

### 绑定

从本质上讲，绑定将视图元素和 JavaScript 对象（通常是模型）挂接在一起。当 JavaScript 对象发生改变时，视图会根据新修改后的对象做适时更新。

## 第六章 依赖管理

### CommonJS

#### 模块和浏览器

> 译注3：

> 在客户端，为了处理模块依赖关系不得不将模块主逻辑包含在某个回调函数中，加载模块的过程实际是“保存回调”的过程，最后统一处理这些回调函数的执行顺序。作为模块加载器的鼻祖 YUILoader 就是遵循这种逻辑实现的，并在 YUI3 中形成了其独具特色的 `use()` 和 `add()` 模块化编程风格。为了便于理解客户端模块和加载器的基本原理，可以参照译者实现的一个小型类库 [Sandbox.js](http://github.com/jayli/sandbox) ，文档中详细讲解了模块加载器的基本原理。

### 包装模块

> 可以在服务器端将小文件合并为一个文件输出，这种做法一举两得。这样浏览器只需发起一个 HTTP 请求来抓取一个资源文件，就能将所有的模块都载入进来，显然这种做法更高效。

> 使用打包工具也是一种明智的做法，这样就不必随意、无组织地打包模块了，而是静态分析这些文件，然后递归地计算它们的依赖关系。打包工具同样会将不符合要求的模块包装成转换格式，这样就不必手动输入代码了。

> 除了在服务端合并代码，很多模块打包工具也支持代码的压缩(minify)，以进一步减小请求的体积。实际上，一些工具——比如 [rack-modulr](http://goo.gl/0YcFK) 和 [Transporter](http://goo.gl/Exkpm) ——已经整合进了 Web 服务器，当首次处理某个请求时会自动处理模块操作。

### 模块的按需加载

> 你可能不想用模块化的方式来写代码，或许是因为现有的代码和库改成用模块化的方式来管理需要做太多改动。幸运的是，有其他地带方案。

* Sprockets (http://getsprockets.org)

> Sprockets 给代码添加了同步 `require()` 支持，以 `//=` 的形式书写的注释都会被 Sprockets 进行预处理。比如，`//= require` 指令通知 Sprockets 来检查类库的加载路径，加载它以行内形式包含进来。

> 尽管 Sprockets 是一个基于命令行的工具，但也有一些工具集成了 Rack 和 Rails ，比如 [rack-sprockets](http://github.com/kelredd/reck-sprockets) ，[PHP 的实现](http://goo.gl/0HvwT) 。

> Sprockets （包括所有模块包装器）的中心思想是，所有的 JavaScript 文件都需要预处理，不管是在服务端用程序作处理还是使用命令行工具。 [1]

* LABjs (http://www.labjs.com)

> LABjs 是最简单的模块依赖管理器[2]，它不需要任何服务器支持和 CommonJS 模块支持。使用 LABjs 载入你的脚本代码，减少了页面加载过程中的资源阻塞，这是一种极其简单且极为有效的性能优化方法。

### 无交互行为内容的闪烁 (FUBC)

> 使用加载器来加载页面时，有一点需要尤为注意——用户可能会看到页面闪了一下，出现一部分没有交互行为的内容快速闪过(FUBC)，比如在 JavaScript 执行之前会有一部分无样式的页面原始内容闪烁一下。

所以那么多 SPA 都会使用菊花来作为页面初始化时的样式，其他无关紧要的可以直接在行内写上样式 `display: none` ，然后再调用 jQuery 的 `show()` 函数来显示。

## 第七章 使用文件

略。

## 第八章 实时 Web

略。

## 第九章 测试和调试

> 很多人认为 JavaScript 的测试是一个鸡肋，因此多数 JavaScript 开发者没有为他们的程序写测试代码。在我看来这种认识的主要原因是，JavaScript 的自动化测试非常困难，且不具备伸缩性。

> 首先考虑你的站点主要面向哪些人提供服务，然后决定要支持哪些浏览器，而不要太迷信统计数据。然而，根据经验来看，推荐大家主要测试这些浏览器[3]：

* IE8, 9
* Firefox 3.6 [4]
* Safari 5
* Chrome 11

当然，这在我看来也是见仁见智的事情了，如果是个人的项目，我更愿意只测试支持 Firefox, Safari, Chrome, Chromium ，对版本的支持则是上一个稳定版和当前稳定版以及开发者版。

### 单元测试

> 手工测试更像是集成测试，从更高层次上保证应用的正常运行。单元测试则是更低层次的测试。

> 单元测试的另一个优势是为自动化测试铺平道路。将很多单元测试整合起来就可以做到连续的集成测试了——每次代码有更新时都重新执行一遍所有的单元测试。

书里提到了 [QUnit](http://docs.jquery.com/Qunit) 和 [Jasmine](http://pivotal.github.com/jasmine/) 两个单元测试框架。

### 驱动

> 尽管使用测试框架可以做到一定程度的自动化测试，但在各式各样的浏览器中进行测试依然是个问题。每次测试时都要开发者手动执行刷新，这种做法显然很低效。

> 为了解决这个问题，有人开发了驱动。这里说的驱动其实是一个守护进程，它整合了不同的浏览器，可以自动运行 JavaScript 测试代码，测试不通过的时候会给出提示[5]。

当代码发生变动时，本地可以通过监测文件变动进行实时的测试，或者在集成时通过版本管理工具的 post-commit 的 hook 功能来运行测试。

#### Watir (http://watir.com)

> Watir 是一个基于 Ruby 的驱动类库，整合了 Chrome, Firefox, Safari 和 IE ，可以通过给 Watir 发送 Ruby 指令来启动浏览器，而且可以象真实用户一样完成点击链接和填写表单等行为。

由于浏览器的安装受到操作系统的限制，如果你想测试某些操作系统独有的浏览器，你的持续集成服务器就需要安装特定版本的操作系统

#### Selenium (http://seleniumhq.com)

> 这个库提供了一种特定领域语言（Domain Scripting Lanuage, 简称 DSL），用这种特定领域语言可以为多种编程语言编写测试代码，比如 C#, Java, Groovy, Perl, PHP, Python 和 Ruby 。它往往是以后台服务的形式运行于持续集成服务器中。

> Selenium 的优势在于它支持很多编程语言，同时提供了一个 Firefox 插件 [Selenium IDE](http://seleniumhq.com/projects/ide/) ，这个插件可以记录浏览器的行为并可回放。

### 无界面的测试

在服务器端编写 JavaScript 程序，需要在脱离浏览器环境的命令行中运行测试代码。

> 这么做的优势是命令行环境速度快而且易于安装，同时不涉及多浏览器及持续集成服务器环境。它的不足之处也很明显，就是测试代码无法在真实环境中运行。

所幸大多数 JavaScript 代码都是应用逻辑，不依赖浏览器的 DOM 和 Event 等内容。

#### Zombie.js (http://zombie.labnotes.org/)

Zombie.js 专门为 Node.js 设计，充分利用了它的高性能和异步特性，主要特点是速度快。

同时 Zombie.js 还利用了 V8 引擎的上下文特性，能够让测试用例彼此隔离，使不共用同一个全局变量/全局上下文；同时能够并行运行多个测试代码，使用一个异步测试框架，比如 [Vows.js](http://vowsjs.org/) ，减少整个测试单元的运行时间。

#### Ichabod (http://github.com/maccman/ichabod)

> 如果你需要一个简洁、高效的测试运行环境，强烈推荐你使用 Ichabod 。

> Ichabod 的优势还在于它使用了 Webkit 解析引擎，这也是 Safari 和 Chrome 浏览器所使用的解析引擎，但它的缺点是只能运行在 OS X 中，因为它需要 MacRuby 和 OS X WebView API 的支持。

> Ichabod 目前可以运行 Jasmine 和 QUnit 的测试代码，后续还会增加更多对测试类库的支持。

### 分布式测试

> 跨浏览器测试的一个解决方案是利用外包的专用服务器集群，这些集群都安装有不同的浏览器，这正是 [TestSwarm](http://swarm.jquery.org/) 的做法。

> 浏览器在 TestSwarm 的终端里运行，并自动执行推送给它们的测试。它们可以部署在任意机器、任意操作系统中，TestSwarm 会自动将得到的测试地址传递给一个新打开的浏览器。

> 这种方法听起来很简单，但仍需要对持续集成服务器作大量的部署工作，这个过程也非常痛苦和麻烦。

> 事实上可以将这个工作外包给更专业的社区和公司来做，你只需要告诉它们你想要的测试平台即可。

> 可以选一些公司，比如 [Sauce Labs](http://saucelabs.com) 提供的成熟的服务，这些公司大多是将浏览器运行于云端，你只需运行一个 Selenium 测试驱动，剩下的工作都交给服务器去做。

### 调试工具

#### Web Inspector

Web Inspector 是 Safari 和 Chrome 自带的调试工具，Safari 需要在 "Preferences..." / "Advanced" 面板中勾选 "Show Develop menu in menu bar"。

### 控制台

> 我们使用控制台来轻松地执行 JavaScript 代码并检查页面的全局变量。控制台的一个主要优势是你可以使用 `console.log()` 函数直接向他输出 log 。

> 同样，使用一个代码函数也可以对 log 做命名空间的管理

```coffeescript
App =
  trace: true
  log: (args...) ->
    return unless @trace
    return unless console?
    args.unshift "(App)"
    console.log.apply console, args
```

> 这里的 `App.log()` 函数给它的参数都加上了字符串 App 前缀，然后调用了 `console.log()`

同时预防了可能环境中没有定义 `console` 对象的情况。

其实 `App.trace` 还可以通过 url 的 querystring 来判断，如果 querystring 有一个 debug=1 ，那么 `App.trace` 即为 `true`

#### 控制台函数

* `$()` : document.getElementById()
* `$$()` : document.querySelectorAll()
* `$x()` : 返回匹配某个 XPath 表达式的一组元素组成的数组
* `clear()` : 清空控制台的 log
* `dir()` : 输出对象中的所有属性
* `inspect()` : 参数可以是元素、数据库或存储区域，并会自动跳转到调试工具的对应面板
* `keys()` : Object.keys.call(obj)
* `values()` : Object.values.call(obj)

### 分析网络请求

> 调试工具中的网络监控面板现实了本页面发起的所有 HTTP 请求，包括每个请求耗费的时间及何时完成。[6]

以下的内容基本上指的的 Web Inspector 的网络监控面板。

> 你可以看到出事请求的延时是用半透明的颜色表示的。当开始接收数据时，时间轴的颜色就变得不透明。

> 网络时间轴中的竖线表示了页面加载的状态。蓝色的线表示 DOMContentLoaded 事件的触发时间，红色的线表示页面的 load 事件触发的时间。[7]

### Profile 和函数运行时间

> 如果你所构建的是一个大型 JavaScript 应用，你需要特别关注性能，特别是当你的应用是运行在移动终端时。

> Profile 代码很简单，只要在你想要统计的代码段两端加上 `console.profile()` 和 `console.profileEnd()` 即可。

> 译注21：

> 这里的 Profile 指的是调试工具提供的一个功能，用来统计代码中函数执行的次数和时间，并给出报表。

> 同样，你也可以使用调试器 Profile 的 record 特性，它的功能和直接嵌入 console 语句是一样的。

> 你也可以使用 Profile 的快照 (snapshot) 功能生成页面当前的堆 (heap)[8] 的快照

快照能够显示当前使用了多少对象，占用了多少内存。

> 这是查找内存泄露的好方法，因为你可以看到哪些多想被无意间存储在内存中，应当被回收而未被回收。

> 使用控制台工具函数同样可以查看代码的执行时间，API 和 Profile 类似，只需要给要统计时间的代码前后加上 `console.time(name)` 和 `console.timeEnd(name)` 即可。

## 第十章 部署

[1]: Seajs 是一个模块加载器，在客户端实现了同步 `require()` ，使得客户端也能够遵循 CommonJS 标准规范进行编码开发。Seajs 从 1.2.0 开始能够加载未被包装过的 JavaScript 模块。

[2]: 译注10：作者这里将 LABjs 归类为“依赖管理器”有些勉强，LABjs 给自己的定位是“脚本加载器”，主要是为了解决加载脚本时的性能问题。

[3]: 译注5：雅虎最早提出了浏览器分级支持 (GBS) ，即根据功能需求的权重来将浏览器支持划分为多个级别，并且非常详细、系统的定义了浏览器测试基准和操作系统支持标准，请参照 http://yuilibrary.com/yui/docs/tutorials/gbc/

[4]: 译注6：撰写本书时的 Firefox 版本还是 3.6 ，从 Firefox 4 之后版本升级非常快。更重要的不同是 Firefox 3.6 遵从 ECMAScript 3 ，而 4 及以后的的版本则遵循 ECMAScript 5 ，因此这里更推荐使用 Firefox 4+

[5]: 译注9：前端开发自动化测试是目前比较热门的话题，国内也有很多前端团队开始了这方面的研究和尝试，可以参照来自淘宝的“前端测试实践”(http://goo.gl/koPLJ)以及来自新浪的“多浏览器集成的JavaScript单元测试”(http://goo.gl/2CnHe)

[6]: 译注18：网络面板只能监控页面发出的 HTTP 请求，如果页面中包含 Flash ，则 Flash 发起的请求是无法在网络面板中看到的，只能用更深层的抓包工具，比如 WireShark。

[7]: 译注20：还有一个非常重要的时间线“页面首次渲染时间”，通常是绿色的线，这条线在 Firebug 和 Web Inspector 中看不到，可以使用 [httpWatch](http://www.httpwatch.com/) 来查看。

[8]: 译注22：和其他编程语言一样，JavaScript 运行时的内存也划分为堆 (heap) 和栈 (stack) 。栈是用来存储局部变量的原始值和“引用”（这里可以将引用理解为一个内存地址）的，而堆则是存放“引用值”（引用指向的内容）的，这里的快照查看的是堆的内容，而不是栈的内容，主要是因为和堆相比栈的内存占用很小。更多内容请参照 http://goo.gl/lbECT
