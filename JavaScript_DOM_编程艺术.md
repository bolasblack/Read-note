TODO: 完成 223 行的 removeClass 函数

# JavaScript DOM 编程艺术 （第二版） 笔记
## 第三章 DOM P32
D: Document, O: Object, M: Model

### 3.2 对象的分类
对象可以分为三类：

* 用户定义对象（user-defined object）
* 内建对象（native object）：比如 Array,String,Date 等等，不论 JavaScript 所在哪个平台都有的对象
* 宿主对象（host object）：比如 window,document,htmlElement 等等，会根据所在平台不同而改变的对象

window 对象对应浏览器窗口本身，其属性和方法通常称为 BOM（browser object model）

### 3.3 什么是 文档对象模型
文档对象模型就是一颗家谱树，用 parent, child, sibling 来表明成员之间的关系

### 3.4 节点 （node）
节点有三种：

* 元素节点 ： DOM 的原子，标签的名字就是元素的名字
* 文本节点 ： 元素节点中的直系文字
* 属性节点 ： 元素节点更为具体的描述

## 第四章 案例研究：JavaScript 图片库 P46
### 4.2 JavaScript
#### 4.2.1 非 DOM 解决方案
setAttribute 和 getAttribute 是 “第一级 DOM” 的组成部分

推荐使用第一级 DOM 而不使用 element.attr 的原因是代码的兼容性和你需要记住的 API 数量

### 4.4 对这个函数进行扩展
* childNodes : 可以用来获取任何一个元素的所有子*节点*
* nodeType : 获取节点的类别：元素节点为1,属性节点为2,文本节点为3
* nodeValue : 得到和设置一个节点的值
* firstChild : 获取 childNodes 返回的列表里的第一个子节点
* lastChild : 获取 childNodes 返回的列表里的最后一个子节点

## 第五章 最佳实践 P61
### 5.2 平稳退化
#### 5.2.1 "javascript:" 伪协议
”真“协议用于在因特网上的计算机之间传输数据包，比如 HTTP 协议、FTP 协议等 
伪协议是一种非标准化的协议，"javascript:" 伪协议让我们通过一个链接来调用 JavaScript 函数

#### 5.2.3 谁关心这个
强调“平稳退化”的意义在于，只有极少数自动化程序（比如搜索机器人）能够读懂 JavaScript

做到平稳退化的办法也并不困难，让元素在没有 JavaScript 支持时保持可用即可

### 5.5 向后兼容
* 对象检测
* 浏览器嗅探：不提倡的原因是由于浏览器和浏览器版本的增多，会使得浏览器嗅探的代码变得越来越臃肿，而且一旦浏览器发生改变，代码也有可能会需要随之发生改变

对象检测代码：
``` javascript
  if (method) {
    // code...
  }
```

## 第六章 案例研究：图片库改进版 P75
### 6.3 它的 JavaScript 与 Html 标记是分离的吗
> 我们在学校里学过一种理论，叫结构化程序设计 (structed programming)，其中有这样一条原则：函数应该只有一个入口和一个出口。

> 在实际工作中，过分拘泥于这项原则往往会使代码变得非常难以阅读，如果为了避免留下多个出口而去改写那些 if 语句的话，这个函数的核心就会被掩埋在一层又一层的花括号里，就想下面这样

> 我个人认为，如果一个函数有多个出口，只要这些出口集中出现在函数的开头部分，就是可以接受的。

``` javascript
function prepareGallery() {
  if (document.getElementsByTagName) {
    if (document.getElementById) {
      if (document.getElementById("imagegallery")) {
        // statements go here ...
      }
    }
  }
}
```

### 6.6 键盘访问
**小心 onkeypress** 

这个事件处理函数很容易出问题，用户每按下一个键都会触发它，在某些浏览器里，甚至包括 Tab 键！这意味着如果绑定在 onkeypress 事件上的处理函数返回的是 false ，那些只能用键盘访问的用户将永远无法离开当前链接。

onclick 事件的名字似乎给人一种只与鼠标点击动作相关的印象，但事实并非如此：在几乎所有的浏览器里，用 Tab 移动到某个链接然后按下回车的动作也会触发 onclick 。

最好不要用 onkeypress 事件处理函数，在 onclick 事件处理函数已经能满足需要的情况下。

### 6.8 DOM Core 和 HTML-DOM
getElementById、getElementsByTagName、getAttribute、setAttribute 这些方法都是 DOM Core 的组成部分，它们不专属于 JavaScript，支持 DOM 的任何程序设计语言都可以使用它们。它们的用途也并非仅限于此，它们可以用来处理任何一种标记语言（比如 XML）编写出来的文档。

在使用 JavaScript 语言和 DOM 为 HTML 文件编写脚本时，还有许多属性可供选用，比如 onclick 。这些属性属于 HTML-DOM ，它们在 DOM Core 出现之前很久就为人所知了。

比如：

* document.getElementsByTagName("form") => document.forms
* element.getAttribute("src") => element.src

这些方法和属性可以相互替换。同样的操作既可以用 DOM Core 来实现，也可以使用 HTML-DOM 来实现。HTML-DOM 代码通常更短，但是它们只能用来处理 Web 文档（因为 XML 可以自定义标签）

## 第七章 动态创建标记 P96
### 7.1 一些传统方法
* document.write : 不推荐这个是因为它违背了“行为应该和表现分离”的原则，这样的标记既不容易阅读和编辑，也无法享受到行为与结构分离开来的好处。同时容易导致验证错误，比如在 `<script>` 后出现一个 `<p>`。而且 .MIME 类型 application/xhtml+xml 与 document.write 不兼容，浏览器在呈现这种 XHTML 的时候根本不会执行 document.write 方法。
* innerHTML : 这个属性并不是 W3C DOM 标准的一部分，但现在已经包含到了 HTML5 规范中。它始见于微软的 IE4 浏览器。在 DOM 看来，一个标签里有若干的节点，在 innerHTML 看来，一个标记里只有一个包含了所有内容的字符串。

### 7.2 DOM 方法
在 DOM 树中创建一个元素节点要分两步：

- 创建一个新的元素
- 将这个元素插入节点树

* createElement(nodeName) : 创建一个新元素节点
* appendChild(node) : 给某个现有元素节点增加一个子节点
* createTextNode(text) : 创建一个新的文字节点
* parentElement.insertBefore(newElement, targetElement) : 将元素节点 newElement 插入到父元素节点 parentElement 里，元素节点 targetElement 前面
* insertAfter(newElement, targetElement) ：DOM 没有提供这个函数，但是可以写一个

``` javascript
var insertAfter = function (newElement, targetElement) {
  var parent = targetElement.parentNode;
  if (parent.lastChild == targetElement) {
    parent.appendChild(newElement);
  else
    parent.insertBefore(newElement, targetElement.nextSibling);
}
```

### 7.4 Ajax
#### 7.4.1 XMLHttpRequest 对象
兼容性代码：

``` javascript
var getHTTPObject = function () {
  if (typeof XMLHttpRequest == "undefined") {
    XMLHttpRequest = function () {
      try { reutrn new ActiveXObject("Msxml2.XMLHTTP.6.0");}
        catch (e) {}
      try { reutrn new ActiveXObject("Msxml2.XMLHTTP.3.0");}
        catch (e) {}
      try { reutrn new ActiveXObject("Msxml2.XMLHTTP");}
        catch (e) {}
      return false;
    }
  }
  return new XMLHttpRequest();
}
```

#### 7.4.3 Hijax
Hijax 指的是“渐进增强的使用Ajax”

> Jeremy Keith 借用了 hijack （劫持）一次的发音与含义，意思是拦截用户操作触发的请求。

> Ajax 应用主要依赖后台服务器，实际上是服务器端的脚本语言完成了绝大部分工作。XMLHttpRequest 对象作为浏览器与服务器之间的“中间人”，它只是负责传递请求与响应。如果把这个中间人请开，浏览器与服务器之间的请求和响应应该继续完成（而不是中断），只不过花的时间可能会长一点。

书中的 P120 第五段起，描述一个“渐进增强使用Ajax”的引导。

## 第八章 充实文档的内容 P122
### 8.4 显示缩略语列表
#### 8.4.3 一个浏览器“地雷”
在 IE6 或者更早的 Windows 版本，那么 `<abbr>` 标签是无法被识别的。

究其原因在于当年浏览器大战时，网景公司和微软公司曾把 `<abbr>` 和 `<acronym>` 标签当作他们的武器之一。在竞争最激烈是，微软决定不在自己的浏览器中实现 abbr 元素。直到 IE7 才又开始支持这个元素。

### 8.5 显示“文献来源链接表”
``` HTML
<blockquote cite="http://www.w3.org/DOM/">
  <p>...</p>
</blockquote>
```
> 乍看起来，blockquote 元素的最后一个子节点应该是那个 p 元素，而这意味着 lastChild 属性的返回值将是一个 p 元素节点。可是，事实却并非如此。

> 那个 p 节点的确是 blockquote 元素的最后一个 *元素* 节点，但在 `</p>` 标签和 `</blockquote>` 标签之间还存在着一个换行符。有些浏览器会吧这个换行符解释为一个文本节点。这样一来 blockquote 元素节点的 lastChild 属性就将是一个文本节点而不是那个 p 元素节点。

> 注意：在编写 DOM 脚本时，你会想当然的认为某个节点肯定是一个元素节点，这是一中相当常见的的错误。如果没有百分之百的把握，就一定要去检查 nodeType 属性值，有很多 DOM 方法只能用于元素节点，如果用在了文本节点身上，就会出错。

lastChildElement 函数

``` javascript
var lastChildElement = function (elementList) {
  var allElement = elementList.getElementsByTagName("*");
  return allElement[allElement.length - 1];
}
```

### 8.6 显示“快捷键清单”
一些基本的快捷键都有约定俗成的设置方法，对此感兴趣的读者可以浏览 [http://www.clagunt.com/blog/193/] 。

* accesskey="1" 对应一个“返回到本网站主页”的链接
* accesskey="2" 对应一个“后退到前一页面”的链接
* accesskey="4" 对应一个“打开本网站的搜索表单/页面”的链接
* accesskey="9" 对应一个“本网站的联系方法”的链接
* accesskey="0" 对应一个“查看本网站的快捷键清单“的链接

## 第九章 CSS-DOM P148
### 9.2 style 属性
#### 9.2.1 获取样式
> element.style 对象只能返回包含在 HTML 代码里用 style 属性声明的样式。

### 9.3 何时该用 DOM 脚本设置样式
``` javascript
var getNextElement = function (node) {
  if (node.nodeType === 1) {
    return node;
  }
  if (node.nextSibling) {
    return getNextElement(node.nextSibling);
  }
  return null;
};
```

### 9.4 className 属性
``` javascript
var addClass = function (element, newClass) {
  if (!element.className) {
    element.setAttribute("class", newClass);
  } else {
    var newClassName = element.getAttribute("class");
    newClassName += " ";
    newClassName += newClass;
    element.setAttribute("class", newClassName);
  }
};
//顺带附上自己写的 removeClass
var removeClass = function (element, className) {
  var newClassName = element.getAttribute("class").replace(/\s*className/g, "");
  element.setAttribute("class", newClassName);
}
```

## 第十章 用 JavaScript 实现动画效果 P172
算是动画的入门吧，不作什么笔记了

## 第十一章 HTML5 P201
### 11.3 几个示例
#### 11.3.3 表单
HTML5 新加入的输入控件类型：

* email
* url
* date
* number
* range
* search
* tel
* color

输入控件的新属性：
* autocomplete 为 text 添加一组建议的输入项
* autofocus 让表单元素自动获取焦点
* form 用于对 `<form>` 标签外部的标点元素进行分组
* min、max、step 用在 number 和 range 中
* pattern 用于定义一个正则以便验证输入
* placeholder 用于在文本输入框显示临时性的提示文字
* required 必填

### 11.4 HTML5 还有其他特性吗

* [使用 WebSocket 与服务器端脚本进行开放的双向通信](http://dev.w3.org/html5/websockets/)
* [标准化的拖放实现](http://www.whatwg.org/specs/web-apps/current-work/multipage/dnd.html#dnd)
* [使用 Web Worker 在后台执行 JavaScript](http://www.whatwg.org/specs/web-workers/current-work/)
* [在浏览器中实现地理位置服务](http://www.w3.org/TR/geolocation-API/)

* [W3C HTML5 Working Draft](http://www.w3.org/TR/html5)
* [WHATWG HTML5 （包含开发中的下一代技术）](http://www.whatwg.org/specs/web-apps/current-work)
* [HTML5 的交互性演示](http://html5demos.com/)
* [Dive into HTML5](http://diveintohtml5.org/)

## 第十二章 综合示例 P220
### 12.3 CSS
这一节的内容里有提到将样式表导入到一个基本的样式表中：

``` CSS
@import url(layout.css)
@import url(color.css)
@import url(typography.css)
```

在诸多文章中提到的，”@import 在 IE6 中似乎有极大的性能问题“ 的论调似乎没有出现在书中。

当然可能是由于这本书讲的是 JavaScript DOM 编程而不是”CSS 性能 36 计“，不过我还是觉得有空的时候去了解一下 `@import` 的前世今生。


