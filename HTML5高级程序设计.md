# HTML5 高级程序设计
## 第一章 HTML5 概述
### 1.4 新的认识
HTML5 是基于各种各样的理念（在 WHATWG 规范中有描述）进行设计的，这些设计理念体现了对可能性和可行性的新认识。

* 兼容性
* 实用性
* 互通性
* 通用访问性

#### 1.4.1 兼容性和存在即合理
#### 1.4.2 效率和用户优先
HTML5引入了一种新的*基于来源*的安全模型。这个安全模型可以让我们做一些以前做不到的事情，不需要借助任何所谓聪明、有创意却不安全的hack就能跨域进行安全对话。

PS. [利用HTML5的window.postMessage实现跨域通信](http://www.36ria.com/3890) 与 [HTML5：使用postMessage实现Ajax跨域请求](http://yangzebo.com/blog/?p=208) 就提到了这种方式。


#### 1.4.3 化繁为简

* 以浏览器原生能力替代复杂的 JavaScript 代码
* 新的简化的 DOCTYPE
* 新的简化的字符集声明
* 简单而强大的 HTML5 API

基于多种改进过的、强大的错误处理方案，HTML5 具备了良好的错误处理机制。非常有现实意义的一点是，HTML5 提倡重大错误的平缓恢复，...

比如，如果页面中有错误的话，在以前可能会影响整个页面显示，而HTML5不会出现这种情况，取而代之的是以标准方式显示*“broken标记”*，这要归功于HTML5中精确定义的*错误恢复机制*

PS. 很可惜，我找不到有关 broken标记 的相关资料。

#### 1.4.4 通用访问
这个原则可以分成三个概念：

* 可访问性：出于对残障用户的考虑，HTML5 与 WAI（Web Accessibility Initiative，Web 可访问性倡议） 和 ARIA （Accessible Rich Internet Applications，可访问富 Internet 应用）做到了紧密结合，WAI-ARIA 中以屏幕阅读器为基础的元素已经被添加到 HTML 中。
* 媒体中立：如果可能的话，HTML5 的功能再所有不同的设备和平台上应该都能正常运行。
* 支持所有语种：例如，新的 <ruby> 元素支持在东亚页面排版中会用到的 Ruby 注释。

### 1.5 无插件范式
文中提到了的，属于 HTML5 的，我感兴趣的部分：

* Canvas （2D和3D）
* Channel 消息传送
* Cross-document 消息传送
* Geolocation
* MathML
* Microdata
* Server-Sent Events
* SVG
* Web Origin Concept
* XML HttpRequest Level 2
* Web SQL database
* Web Workers

www.caniuse.com 网站按照浏览器版本提供了详尽的 HTML5 功能支持情况。

同时也可以使用 Modernizer —— 一个检测浏览器对 HTML5 和 CSS3 的支持程度的 JavaScript 库。

### 1.6 HTML5 的新功能
#### 1.6.2 新元素与旧元素
HTML5 引入了很多新的标记元素，根据内容的不同，这些元素分成了 7 大块：

* 内嵌：向文档添加其他类型的内容，例如 audio, video, canvas, iframe 等
* 流：在文档和应用的 Body 中使用的元素，例如 form、hl 和 small 等
* 标题：段落标题，例如 h1、h2 和 hgroup 等
* 交互：与用户交互的内容，例如音频和视频的控件、 button 和 textarea 等
* 元数据：通常出现在页面的 head 中，设置页面其他部分的表现和行为，例如 script、style 和 title 等
* 短语：文本和文本标记元素，例如 mark、kbd、sub 和 sup 等
* 片段：用于定义页面片段元素，例如 article、aside 和 title 等

上述所有类型的元素都可以通过 CSS 来设定样式。

#### 1.6.3 语义化标记
我们说过，HTML5 的宗旨之一就是存在即合理。Google 分析了上百万的页面，从中发现了 DIV 标签和通用 ID 名称重复量很大。例如很多开发人员喜欢使用 footer 和 header 作为标签的 ID ，所以 HTML5 引入了一组新的片段类元素，在目前驻留的浏览器中已经可以使用了。

* header：标记头部区域的内容（用于整个页面或页面中的一块区域）
* footer：标记脚部区域的内容（用于整个页面活页面中的一块区域）
* section：Web 页面中的一块区域
* atricle：独立的文章内容
* aside：相关内容或者引文
* nav：导航类辅助内容

#### 1.6.4 使用 Selectors API 简化选取操作

* querySelector()：根据指定的选择规则，返回在页面中找到的第一个匹配元素
* querySelectorAll()：根据指定规则返回页面中所有相匹配的元素

Selectors API 与现在 CSS 中使用的选择规则一样，同时可以为 Selectors API 函数同时指定多个选择规则，例如：

``` JavaScript
// 选择文档中类名为 highClass 或 lowClass 的第一个元素
var x = document.querySelector(".highClass", ".lowClass");
```

#### 1.6.7 DOM Level 3
IE9 将会支持 DOM Level 2 和 DOM Level 3的特性，包括非常重要的 addEventListener() 和 dispatchEvent()

## 第二章 Canvas API
PS. 这一部分我不打算看，我打算直接看 Canvas 的书籍

## 第三章 音频和视频
### 3.1 HTML5 Audio 和 Video 概述
#### 3.1.3 HTML5 Audio 和 Video 的限制

* 流式音频和视频。因为目前 HTML5 视频规范中还没有比特率切换标准，所以对视频的支持只限于加载全部媒体文件，但是将来一旦流媒体格式被 HTML5 支持，则肯定会有相关的设计规范。
* HTML5 的媒体受到 HTML 跨源 (cross-origin) 资源共享的限制。关于跨资源共享的更多信息，请参考第五章。
* 全屏视频无法通过脚本控制。从安全性角度来看，让脚本元素控制全屏操作是不合适的，不过，如果要让用户在全屏方式下播放视频，浏览器可以提供其他的控制手段。
* 对 Audio 元素和 Video 元素的访问尚未完全加入规范中。基于流行的字幕格式 SRT 的字幕支持规范 (WebSRT) 仍在编写中。

### 3.2 使用 HTML5 Audio 和 Video API
#### 3.2.1 浏览器支持性检测
``` javascript
var hasVideo = !!(document.createElement('video').canPlayType);
```

#### 3.2.2 理解媒体元素
最后，介绍最重要的属性：src。最简单的情况下，src 属性直接指向媒体文件就可以了。但是，万一浏览器不支持相关容器或者编解码器呢（比如 Ogg 和 Vorbis）？这就需要用到备用声明了。备用声明中可以包含多种来源，浏览器可以从这么多来源中进行选择：

``` html
<audio controls>
  <source src="... .ogg" />
  <source src="... .mp3" />
  ...
</audio>
```

容器和编解码器都可以在 type 属性声明，如果 type 属性中指定的类型与源文件不匹配，浏览器可能就会拒绝播放。。所有的支持列表见 RFC4218,RFC4218 是由 IETF(Internet Engineering Task Force, Internet 工程任务分组)维护的一套文档。以下列出常见组合：

* 在 Ogg 容器中的 Theora 视频和 Vorbis 音频： type='video/ogg; codecs="theora, voribis"'
* 在 Ogg 容器中的 Vorbis 音频：type='video/ogg; codecs="voribis"'
* 在 MP4 容器中的 sinple bashline H.264 视频和 lowcomplexity AAC 音频： type='video/mp4; codecs="avc1.42E01E, mp4a.40.2"'
* 在 MP4 容器中的 MPEG-4 简单视频和简单 AAC 音频：type='video/mp4; codecs="mp4v.20.8, mp4a.40.2"'

#### 3.2.4 使用 Video 元素
video 元素相对 audio 的额外属性：

* poster: 在视频加载完成之前，代表视频内容的图片的 URL 地址，可读可修改
* width、height: 读取或者设置显示尺寸。如果设置的宽度与视频本身大小不匹配，可能导致居中显示，上下或者左右可能出现黑色条状区域
* videoWidth、videoHeight: 返回视频的固有或者自适应的宽度和高度，只读
* video 还有一个 audio 元素不支持的关键特性：可被 Canvas 的函数调用

** P63 起演示了一个创建视频市局查看器的过程 **

## 第四章 Geolocation API
