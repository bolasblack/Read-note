# HTML5 高级程序设计
## 第一章 HTML5 概述
### 1.4 新的认识
HTML5 是基于各种各样的理念（在 WHATWG 规范中有描述）进行设计的，这些设计理念体现了对可能性和可行性的新认识。

* 兼容性
* 实用性
* 互通性
* 通用访问性

#### 兼容性和存在即合理
#### 效率和用户优先
HTML5引入了一种新的*基于来源*的安全模型。这个安全模型可以让我们做一些以前做不到的事情，不需要借助任何所谓聪明、有创意却不安全的hack就能跨域进行安全对话。

PS. [利用HTML5的window.postMessage实现跨域通信](http://www.36ria.com/3890) 与 [HTML5：使用postMessage实现Ajax跨域请求](http://yangzebo.com/blog/?p=208) 就提到了这种方式。

#### 化繁为简

* 以浏览器原生能力替代复杂的 JavaScript 代码
* 新的简化的 DOCTYPE
* 新的简化的字符集声明
* 简单而强大的 HTML5 API

基于多种改进过的、强大的错误处理方案，HTML5 具备了良好的错误处理机制。非常有现实意义的一点是，HTML5 提倡重大错误的平缓恢复，...

比如，如果页面中有错误的话，在以前可能会影响整个页面显示，而HTML5不会出现这种情况，取而代之的是以标准方式显示“*broken标记*”，这要归功于HTML5中精确定义的*错误恢复机制*

PS. 很可惜，我找不到有关 broken标记 的相关资料。

#### 通用访问
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
#### HTML5 标记元素的 7 个分类
HTML5 引入了很多新的标记元素，根据内容的不同，这些元素分成了 7 大块：

* 内嵌：向文档添加其他类型的内容，例如 audio, video, canvas, iframe 等
* 流：在文档和应用的 Body 中使用的元素，例如 form、hl 和 small 等
* 标题：段落标题，例如 h1、h2 和 hgroup 等
* 交互：与用户交互的内容，例如音频和视频的控件、 button 和 textarea 等
* 元数据：通常出现在页面的 head 中，设置页面其他部分的表现和行为，例如 script、style 和 title 等
* 短语：文本和文本标记元素，例如 mark、kbd、sub 和 sup 等
* 片段：用于定义页面片段元素，例如 article、aside 和 title 等

上述所有类型的元素都可以通过 CSS 来设定样式。

#### 新增的标签的语义
我们说过，HTML5 的宗旨之一就是存在即合理。Google 分析了上百万的页面，从中发现了 DIV 标签和通用 ID 名称重复量很大。例如很多开发人员喜欢使用 footer 和 header 作为标签的 ID ，所以 HTML5 引入了一组新的片段类元素，在目前驻留的浏览器中已经可以使用了。

* header：标记头部区域的内容（用于整个页面或页面中的一块区域）
* footer：标记脚部区域的内容（用于整个页面活页面中的一块区域）
* section：Web 页面中的一块区域
* atricle：独立的文章内容
* aside：相关内容或者引文
* nav：导航类辅助内容

#### Selectors API

* querySelector()：根据指定的选择规则，返回在页面中找到的第一个匹配元素
* querySelectorAll()：根据指定规则返回页面中所有相匹配的元素

Selectors API 与现在 CSS 中使用的选择规则一样，同时可以为 Selectors API 函数同时指定多个选择规则，例如：

``` JavaScript
// 选择文档中类名为 highClass 或 lowClass 的第一个元素
var x = document.querySelector(".highClass", ".lowClass");
```

#### DOM Level 3
IE9 将会支持 DOM Level 2 和 DOM Level 3的特性，包括非常重要的 addEventListener() 和 dispatchEvent()

## 第二章 Canvas API
PS. 这一部分我不打算看，我打算直接看 Canvas 的书籍

## 第三章 音频和视频
### 3.1 HTML5 Audio 和 Video 概述
#### Audio 和 Video 的限制

* 流式音频和视频。因为目前 HTML5 视频规范中还没有比特率切换标准，所以对视频的支持只限于加载全部媒体文件，但是将来一旦流媒体格式被 HTML5 支持，则肯定会有相关的设计规范。
* HTML5 的媒体受到 HTML 跨源 (cross-origin) 资源共享的限制。关于跨资源共享的更多信息，请参考第五章。
* 全屏视频无法通过脚本控制。从安全性角度来看，让脚本元素控制全屏操作是不合适的，不过，如果要让用户在全屏方式下播放视频，浏览器可以提供其他的控制手段。
* 对 Audio 元素和 Video 元素的访问尚未完全加入规范中。基于流行的字幕格式 SRT 的字幕支持规范 (WebSRT) 仍在编写中。

### 3.2 使用 HTML5 Audio 和 Video API
#### 浏览器支持性检测

``` javascript
var hasVideo = !!(document.createElement('video').canPlayType);
```

#### source 标签和 source 标签的 type 属性
最后，介绍最重要的属性：src。最简单的情况下，src 属性直接指向媒体文件就可以了。但是，万一浏览器不支持相关容器或者编解码器呢（比如 Ogg 和 Vorbis）？这就需要用到备用声明了。备用声明中可以包含多种来源，浏览器可以从这么多来源中进行选择：

``` html
<audio controls>
  <source src="... .ogg" />
  <source src="... .mp3" />
  ...
</audio>
```

容器和编解码器都可以在 type 属性声明，如果 type 属性中指定的类型与源文件不匹配，浏览器可能就会拒绝播放。。所有的支持列表见 RFC4218,RFC4218 是由 IETF(Internet Engineering Task Force, Internet 工程任务分组)维护的一套文档。以下列出常见组合：

* 在 Ogg 容器中的 Theora 视频和 Vorbis 音频： `type='video/ogg; codecs="theora, voribis"‘`
* 在 Ogg 容器中的 Vorbis 音频：`type='video/ogg; codecs="voribis"'`
* 在 MP4 容器中的 sinple bashline H.264 视频和 lowcomplexity AAC 音频： `type='video/mp4; codecs="avc1.42E01E, mp4a.40.2"'`
* 在 MP4 容器中的 MPEG-4 简单视频和简单 AAC 音频：`type='video/mp4; codecs="mp4v.20.8, mp4a.40.2"'`

#### video 元素相对 audio 的额外属性：

* poster: 在视频加载完成之前，代表视频内容的图片的 URL 地址，可读可修改
* width、height: 读取或者设置显示尺寸。如果设置的宽度与视频本身大小不匹配，可能导致居中显示，上下或者左右可能出现黑色条状区域
* videoWidth、videoHeight: 返回视频的固有或者自适应的宽度和高度，只读
* video 还有一个 audio 元素不支持的关键特性：可被 Canvas 的函数调用

**P63 起演示了一个创建视频市局查看器的过程**

## 第四章 Geolocation API
### 4.1 位置信息
位置信息主要由一对纬度和经度坐标组成，经纬度坐标可以用两种方式表示：

* 十进制格式（例如，39.172 22）
* DMS（Degree Minute Secound，角度）格式（例如， 39° 10′ 20″）

除了经纬度坐标，HTML5 Geolocation 还提供位置坐标的*准确度*。除此之外，它还可能会提供一些其他的元数据，具体情况取决于浏览器所在的硬件设备，这些元数据包括海拔、海拔准确度、行驶方向和速度等。如果这些与元数据不存在则返回 null 。

#### 位置信息的数据源
Geolocation API 不指定设备使用哪种底层技术来定位应用程序的用户。它只是用于检索位置信息的 API，而且通过该 API 检索到的数据只具有某种程度的准确性。它不能保证设备返回的实际位置是精确的。

设备可以使用下列数据源：

* IP 地址
* 三维坐标
 - GPS（Global Positioning System，全球定位系统）
 - 从 RFID、Wi-Fi 和蓝牙到 Wi-Fi 的 MAC 地址
 - GSM 或 CDMA 手机的 ID
* 用户自定义数据

#### 各个数据源的优缺点
##### IP 地址地理定位数据
优点

* 任何地方都可以用
* 在服务端处理

缺点

* 不精确（一般精确到市级）
* 运算代价大

##### GPS 地理定位数据
优点

* 很精确

缺点

* 定位时间长，用户耗电大
* 室内效果不好
* 需要额外的硬件设备

##### Wi-Fi 地理定位数据
优点

* 精确
* 可在室内使用
* 可以简单、快捷定位

缺点

* 在乡村这些无线接入点较少的地区效果不好

##### 手机地理定位数据
优点

* 相当精确
* 可在室内使用
* 可以简单、快捷定位

缺点

* 需要能够访问手机或者其 modem 的设备
* 在基站较少的偏远地区效果不好

##### 用户自定义地理定位数据
优点

* 用户可以获得比程序定位服务更准确的位置数据
* 允许地理位置服务的结果作为备用位置信息
* 用户自行输入可能比自动检测更快

缺点

* 可能很不准确，特别是当用户位置变更之后

### 4.2 HTML5 Geolocation 的浏览器支持情况
在 HTML5 的所有功能中，HTML5 Geolocation 是第一批被全部接受和实现的功能之一。

* Chrome：在带有 Gears 的第二版中被支持
* Firefox：3.5 及以上版本支持
* IE：通过 Gears 插件支持
* Opera：计划在版本 10 中支持
* Safari：在版本 4 中支持以实现在 iPhone 上可用

### 4.3 隐私
HTML5 Geolocation 规范提供了一套保护用户隐私的机制。除非得到用户明确许可，否则不可取得位置信息。

一般步骤如下：

* 用户从浏览器打开位置感应应用程序
* 应用程序 Web 页面加载，然后通过 Geolocation 函数调用请求位置坐标。浏览器拦截着一条请求，然后请求用户授权。
* 浏览器从其宿主设备中检索坐标信息。例如：IP 地址、Wi-Fi 或 GPS 坐标。这是浏览器内部功能。
* 浏览器将坐标发送给受信任的外部定位服务，它返回一个详细的位置信息，并将该位置信息发回给 HTML5 Geolocation 应用程序。

#### 隐私保护机制
如果仅仅是添加 HTML5 Geolocation 代码，而不被任何方法调用，则不会触发隐私保护机制。只要所添加的 HTML5 Geolocation 代码被执行，浏览器就会提示用户应用程序要共享他们的位置。

#### 如何处理位置信息
因为位置数据属于敏感信息，所以接收到之后，必须小心地处理、存储和重传。如果用户没有授权存储这些数据，那么应用程序应该在完成相应任务完成后立即删除它。

如果要重传位置数据，建议先对其进行加密。在手机地理定位数据时，应用程序应该着重提示用户以下内容：

* 会收集位置数据
* 为什么收集位置数据
* 位置数据将保存多久
* 曾一保证数据的安全
* 位置数据怎样共享（如果同意共享）
* 用户怎样检查和更新他们的位置数据

### 4.4 使用 HTML5 Geolocation API
#### 浏览器支持性检测
如果存在地理定位对象，`navigator.geolocation` 调用将返回该对象，否则将触发返回一个空值。

#### 位置请求 API
目前，有两种类型的位置要求：

* 单次定位请求：getCurrentPosition (successCallback, errorCallback, positionOptions)
* 重复性的位置更新请求：watchPosition (successCallback, errorCallback, positionOptions)

##### successCallback
successCallback 接受一个参数：位置对象。这个对象包含坐标（coords 属性）和一个获取位置数据时的时间戳。

坐标总是有多个属性，但是浏览器和用户的硬件设备会决定这些属性值是否有意义。latitude（纬度）、longitude（经度）、accuracy（准确度）是它的前三个属性。

坐标还有一些其他的属性，不能保证浏览器都为其提供支持，但如果不支持就会返回 null 。

* altitude: 用户位置的海拔高度，以 m 为单位
* altitudeAccuracy：海拔高度的准确度，也是以 m 为单位
* heading：行进方向，相对于正北而言
* speed：地面速度，以 "m/s" 为单位

##### errorCallback
errorCallback 接受错误对象作为参数，错误编号设置在错误对象的 code 属性中（原文中全部为大写，我这里为了看起来舒服，全都记作小写）：

* unknown_error：错误编号0,不包括在其他错误编号中的错误。需要通过 message 属性查找错误的更多详细信息。
* permission_denied：错误编号1,用户拒绝浏览器获得其位置信息
* position_unavailable：错误编号2,尝试获取用户位置，但失败了
* timeout：错误编号3,设置了可选的 timeout 值。尝试确定用户位置的过程超时

##### 另外的设置参数
如果要同时处理正常情况和错误情况，就应该把注意力集中到三个可选参数 enableHighAccuracy, timeout, maximumAge 上，将这三个可选参数传递给 HTML5 Geolocation 服务以调整数据收集方式。**这三个参数可以使用 JSON 对象传递**，这样更便于添加到 HTML5 Geolocation 请求调用中。

* enableHighAccuracy：启用这个参数则通知浏览器启用 HTML5 Geolocation 服务的高精确度模式。默认值为 false 。由于地理定位数据的种种限制（见4.1），启用这个参数可能没有任何差别，可能会导致极其花费更多的时间和资源来确定位置，所以应谨慎使用。
* timeout：可选值，单位为 ms，告诉浏览器计算所允许的最长时间，如果还没有完成计算，就调用错误处理函数，默认值为 Infinity
* maximumAge：表示浏览器重新计算位置的时间间隔。单位为 ms，默认值为零，这意味着浏览器每次请求时都必须立即重新计算位置

**请注意**API不允许我们为浏览器指定多长时间重新计算一次位置信息。这是完全由浏览器的实现所决定的。我们能做的就是告诉浏览器 maximumAge 的值是什么。

##### clearWatch
如果应用程序不再需要接受 watchPosition 的持续位置更新，则只需调用 clearWatch() 函数，如下所示（watchId 是 watchPosition 函数的返回值）：

```JavaScript
navigator.geolocation.clearWatch(watchId);
```

### 4.5 使用 HTML5 Geolocation 构建实时应用
#### 计算地球上两点距离的 Haversine 公式
距离计算使用众所周知的 Haversine 公式来实现（公式不好输出，请自行 Google；关于这个公式的原理，请查阅中学数学教材）。

这个公式能够更具经纬度来计算地球上两点间的距离。以下就是该公式的 JavaScript 实现：

```JavaScript
var toRadians = function(degree) {return degree * Math.PI / 180;};
var distance = function(latitude1, longitude1, latitude2, longitude2) {
  // R 是地球的半径，以 km 为单位
  var R = 6371;
  
  var deltaLatitude = toRadians(latitude2 - latitude1);
  var deltaLongitude = toRadians(longitude2 - longitude1);
  latitude1 = toRadians(latitude1);
  latitude2 = toRadians(latitude2);
  
  var a = Math.sin(deltaLatitude / 2) * Math.sin(deltaLatitude / 2) + 
          Math.cos(latitude1) * Math.cos(latitude2) * 
          Math.sin(deltaLongitude / 2) * Math.sin(deltaLongitude / 2);
  var c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1 - a));
  var d = R * c;
  return d;
};
```

> 作为开发人员，无法选择浏览器计算位置所使用的方法，但可以保持数据的准确度，所以推荐使用 accuracy 属性 —— Brian

## 第五章 Communication API
主要是要讲述 postMessage 和 XMLHttpRequest Level2

### 5.1 跨文档消息通信
出于安全方面的考虑，运行在同一浏览器中的框架、标签页、窗口间的通信一直受到严格限制。然而，现实中存在一些合理的让不同站点的内容能在浏览器内进行交互的需求。Mashup 就是最典型的一个例子。

为了满足上述需求，浏览器厂商和标准制定机构一致同意引入一种新功能：跨文档消息通信。跨文档消息通信可以确保 iframe、标签页、窗口间安全的进行跨源通信。

#### postMessage API
利用 postMessage 发送消息非常简单：

```JavaScript
chatFrame.contentWindow.postMessage("hello world", "http://www.example.com/");
```

接收消息：

```JavaScript
window.addEventListener("message", function(e) {
  switch(e.origin) {
    case "frend.example.com":
      processMessage(e.data);
      break;
    default:
      // ...
  }
}，true);
```

data 是发送方传递的实际消息，origin 属性是发送来源，用于忽略来自不可信源的消息。

PS.可以看看我在第一章 1.4 节提到的两篇文章。

鉴于 postMessage API 的一致性和易用性，以及其提供的 JavaScript 环境中的异步通信机制，在同源文档间通信时也推荐使用

在 JavsScript 环境的通信中始终应使用 postMessage API，例如使用 Web Workers 通信时。

#### 5.1.1 理解源安全
HTML5 通过引入源(origin)的概念对域安全进行了阐明与改进。源是网络上用来建立信任关系的地址的子集。

源由规则（scheme）、主机（host）、端口（port）组成：

> http://www.example.com:9000/path/
> -+--   -------+------- --+- --+--
>  |            |          |    |
> scheme       host      port 不考虑路径

HTML5 定义了源的序列化。源在 API 和协议中以字符串的形式出现。这对于使用 XHR 进行跨源 HTTP 请求是非常重要的，对于 WebSocket 也一样。

postMessage 的安全规则确保了消息不会传递到非预期的的源页面中。发送消息时由发送方指定接收方的源。如果发送方用来调用 postMessage 的窗口不具有特定的源（例如用户跳转到了其他站点），浏览器就不会传送消息。

接收信息的时候，发送方的源也作为消息的一部分，为避免伪造，消息源由浏览器提供。接收方可以决定处理与忽略哪些消息。

处理跨源通信的信息时，一定要验证每个消息的源，处理消息中的数据也应该谨慎，最好永远不要对来自第三方的字符串求值（用 window.JSON 或者 json.org 提供的解析器处理 JSON 数据而不是用 eval），下面是两个关于内容注入的示例：

```JavaScript
// 危险，e.data 里的内容可能会被当成标记
elem.innerHTML = e.data
// 相对安全
elem.textContent = e.data
```

#### 浏览器兼容检测

```JavaScript
if (typeof window.postMessage === undefined) {
  // 浏览器不支持 postMessage
}
```

### 5.2 XMLHttpRequest Level 2
更多关于 XMLHttpRequest 编程的知识，建议阅读 John Resing 撰写的 《Pro JavaScript Techniques》 (Apress, 2006)（中文版《精通 JavaScript》，ISBN: 9787115175403）。

作为 XHR 的改进版，XHR Level 2 在功能上有了很大的改进。这一章主要讲两个方面：跨源 XHR 与进度事件 (progress events)。

#### CORS(Cross Origin Resource Sharing)
跨源 HTTP 请求包括一个 Origin 头部，它为服务器提供 HTTP 的“源”信息。头部由浏览器保护，不能被应用程序代码修改。从本质上讲，它与跨文档消息通信中消息事件的 origin 属性作用相同。Origin 不同于早先的 Referer[src] 头部，因为后者 Referer 是一个包括了路径的完整 URL。由于路径可能饱含敏感信息，为了保护用户隐私，浏览器并不一定会发送 Referer ，而浏览器在任何必要的时候都会发送 Origin 头部。

跨域交换的 HTTP 头部示例：

请求的头部示例：
```
POST /main HTTP/1.1
Host: www.example.net
User-Agent: Mozilla/5.0 (X11; U; Linux x86_64; en-US; rv:1.9.1.3) Gecko/20090910 Ubuntu/9.04
(jaunty) Shiretoko/3.5.3
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-us,en;q=0.5
Accept-Encoding: gzip,deflate
Accept-Charset: ISO-8859-1,utf-8;q=0.7,*;q=0.7
Keep-Alive: 300
Connection: keep-alive
Referer: http://www.example.com/
Origin: http://www.example.com
Pragma: no-cache
Cache-Control: no-cache
Content-Length: 0
```

响应的头部示例：
```
HTTP/1.1 201 Created
Transfer-Encoding: chunked
Server: Kaazing Gateway
Date: Mon, 02 Nov 2009 06:55:08 GMT
Content-Type: text/plain
Access-Control-Allow-Origin: http://www.example.com
Access-Control-Allow-Credentials: true
```

#### Progress event
新版 XHR 最重要的改进之一是增加了对进度的响应。在 XHR 之前的版本中，仅有 readystatechage 一个事件能够被用来响应进度。更糟糕的是，浏览器对该事件的实现并不兼容，如在 IE 中永远不会触发 readystate 3（表示所有响应头部都收到，响应体开始接收但未完成），此外，readystate 的更改事件缺乏与上传进程通信的方法，在这样的情况下实现进度条是一件麻烦的事情，而且开牵扯到服务器端的编程开发。

XHR Level 2 用了一个有意义的名字 Progress 进度来命名进度事件，以下为新进度的名称：

* loadstart
* progress
* abort
* error
* load
* loaded

可以通过这些事件实现对事件的监听。

出于向后兼容的目的，新版本中 readyState 属性和 readystatechange 事件可以保留。

#### XHR Level 2 的浏览器支持情况检测

```JavaScript
var xhr = new XMLHttpRequest();
if (typeof xhr.withCredentials === undefined) {
  // 浏览器不支持...
}
```

### 5.3 进阶功能
早期版本的 postMessage 仅支持字符串。后来的版本支持 JavaScript 对象、canvas imageData 和文件等其他数据类型。*由于不同浏览器对规范支持程度的差异，对不同的对象类型的支持情况也不同。*

Framebusting 技术可以用来保证某些内容不被加载到 iframe 中：

```JavaScrpt
if (window !== window.top) {
  window.top.location = location;
}
```

不过，你可能会希望借助 iframe 导入一些确定的合作网站的页面来充实自身的内容，以下是一种使用 postMessage 实现 iframe 与其父页面间的握手通信：

```JavaScript
var framebustTimer, timeout = 3000;
if (window !== window.top) {
  framebustTimer = setTimeout(
    function() {
      window.top.location = location;
    }, timeout
  );
}
window.addEventListener("message", function(e) {
  switch(e.origin) {
    case trustedFramer:
      clearTimeout(framebustTimer);
      break;
  }
}, true);
```

## 第六章 WebSockets API
