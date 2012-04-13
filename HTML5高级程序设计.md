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
### 6.1 HTML5 WebSockets 概述
#### WebSocket 接口

    [Constructor(in DOMSting url,in optional DOMString protocol)]
    interface WebSocket{
        readonly attribute DOMString URL;
        
        //就绪状态
        const unsigned short CONNECTING=0;
        const unsigned short OPEN=1;
        const unsigned short CLOSED=2;
        readonly attribute unsigned short readyState;
        readonly attribute unsigned long bufferedAmount;
        
        //网络
        attribute Function onopen;
        attribute Function onmessage;
        attribute Function onclose;
        boolean send(in DOMString data);
        void close();
    };
    WebSocket implements EventTarget;

WebSocket接口的使用很简单。要连接远程主机，只需要新建一个 WebSocket 实例，提供希望连接的对端URL。注意， ws:// 和 wss:// 前缀分别表示 WebSocket 连接和安全的 WebSocket 连接。

基于同一底层的 TCP/IP 连接，在客户端和服务器之间的初始握手阶段，将 HTTP 协议升级至 WebSocket 协议， WebSocket 连接就建立完成了。连接一旦建立，WebSocket 数据帧就可以以全双工模式在客户端和服务器之间进行双向传送。连接本身是通过 WebSocket 接口定义的 message 事件和 send 函数来运作的。在代码中，采用异步事件侦听器来控制连接生命周期的每一个阶段。

    myWebSocket.onopen = function(evt){
      alert("Connection open…");
    };
    myWebSocket.onmessage = function(evt){
      alert("Received Message:" + evt.data);
      myWebSocket.send("somethings...");
    };
    myWebSocket.onclose = function(evt){
      alert("Connection closed");
    };
    
### 6.3 编写简单的 Echo WebSocket 服务器
**自 P120 页起至 P126 ，书中详细介绍了如何利用 Python 编写一个简单的 Echo 服务器**

### 6.4 使用 HTML5 WebSocket API
#### 浏览器支持情况检测

```JavaScript
if (window.WebSocket == null) {
  // 浏览器不支持...
}
```
## 第七章 Forms API
### 7.1 HTML5 Forms 概述
#### XForms
XForms 是一个以 XML 为核心、功能强大却略显复杂的标准，它用于规范客户端表单的行为，而专门的 W3C 工作组研究这些行为已经近十年。XForms 充分的利用了 XML Schema ，制订了针对验证和格式化的精确准则。不过，很遗憾，在没有安装插件的情况下，主流浏览器均不支持 XForms。

#### HTML5 Forms 的核心设计理念
规范的核心是功能性动作和语义，而非外观和显示效果。

HTML5 表单规范更加注重对现有的简单 HTML 表单功能的改进，力求使之饱含更多控件类型。

但它并没有规定浏览器应该以何种方式将这些元素呈现给用户。这种做法的好处是：浏览器会竞相改善用户交互方式、分离了样式和语义、在面对专用用户输入设备时，能够灵活调整交互方式。

### 7.2 使用 HTML5 Forms API
#### HTML5 新增的 Forms 元素
在 W3C 网站上可以找到 HTML5 中所有新增和修改的元素，具体地址为： http://dev.w3.org/html5/markup/ 。

当浏览器不支持下列 type 属性的值时，标签会退回到文本输入框

* tel：电话号码
* email：电子邮件地址
* url：网页的 URL
* search：用于搜索引擎，比如在站点顶部显示搜索框
* range：特定值范围内的数值选择器，典型显示方式是滑动条
* number：只能饱含数值的文本框
* color：颜色选择器，基于调色盘或者取色板进行选择
* datetime：显示完整的日期和时间，包括时区
* datetime-local：显示日期和时间，不含时区
* time：不含时区的时间选择器和指示器
* date：日期选择器
* week：某年中的周选择器
* month：某年中的月选择器

#### list 属性和 detaillist 元素
通过组合使用 list 和 detaillist 标签，开发人员能够为某个输入型控件构造一张选值列表。使用方法如下：

* 创建一个带 id 的 detaillist 元素
* 添加若干 option 作为 detaillist 的子元素
* 将 input 元素的 list 属性值设为 detaillist.id

#### valueAsNumber 函数
valueAsNumber 函数的作用是完成控件值类型在文本和数值间的相互转换。它既是 setter 又是 getter ，作为 getter 时，将文本转换为数值，如果转换失败，则返回 NaN 。

#### 表单验证
在支持 HTML5 表单验证的浏览器中，可以通过表单控件来访问 ValidityState 对象：`var valCheck = document.myForm.myInput.validity;`。对象包含了对所有八种验证状态的引用，如果八个约束条件全部通过，那么 `valCheck.valid` 的值就为 true ，否则就是 false 。ValidityState 对象是一个实时更新的对象。获得某表单元素的 ValidityState 对象后，当表单元素内容发生变化时，可以通过它来获得更新后的检测结果。

具体八种约束条件可以看这里：http://www.itivy.com/html5/archive/2011/12/24/html5-form-validate.html

#### 验证反馈
只要发生表单验证（不管是在提交表单还是直接调用 checkValidity 函数），所有未通过验证的表单都会接收到一个 invalid 事件。可以通过 event 对象的 srcElement 属性得到表单元素，进而得到 validityState 对象。

#### 关闭验证
表单本身可以通过代码方式设置 noValidate 属性，所有的验证逻辑都会被放弃，只会单纯的提交表单。

更好的办法是在如表单提交按钮控件上设置 formNoValidate 属性，比如：`<input type="submit" formnovalidate />` 。

### 7.3 构建 HTML5 Forms 应用
#### 进阶功能 setCustomValidity
通过 setCustomValidity 函数可以设置 ValidityState 对象的 customError 属性。

## 第八章 Web Workers API
Web Workers 不能直接访问 Web 页面的 DOM API （应该是出于安全性的考虑）。虽然 Web Workers 不会冻结窗口，但仍然会消耗 CPU 周期，导致系统反应速度变慢。

Web Worker 适用于执行不影响 Web 页面交互性的后台的数据处理和监听由后台服务器广播的新闻信息。

### 8.2 使用 HTML5 Web Workers API
Web Workers 的使用方法非常简单，`new Worker("XXX.js")` 创建 Web Workers 对象，传入希望执行的 JavaScript 文件即可。

Web Workers 初始化时会接受一个 JavaScript 文件的**同源** URL 地址，其中包含了供 Worker 执行的代码。*这段代码会设置事件监听器，并与生成 Worker 的容器进行通信。*

Web Workers 之间、Web Workers 与页面的数据交换需要通过 postMessage 函数传递。

#### 浏览器支持性检查

``` JavaScript
if (typeof Worker !== "undefined") {
  // code ...
}
```

#### importScript
Web Workers 由于没有访问 document 的权限，所以 Worker 中必须使用另外的方法导入其他 JavaScript 文件。 `importScript("helper.js");`

导入的 JavaScript 只会在某个已有的 Workers 中加载和执行。

importScript 可以通过多个参数导入多个脚本，它们会按顺序执行。

### 8.3 编写主页
#### 终止 Worker
可以调用 Worker 的 `terminate` 函数实现终止 Web Workers，被终止的 Web Workers 不再响应任何信息或者执行任何其他的计算，也无法重新启动。但可以使用同样的 URL 创建一个新的 Worker。

#### Worker 脚本中能够访问到的 API
Worker 的 API 能够在 Web Workers 脚本中被访问到（可以用来创建子 Worker）

Worker 能够访问到属于 window 对象的定时器 API ，如 setTimeout 函数


## 第九章 Web Storage API
Web Storage 的使用和相关信息并不像上面提到的若干 API 那么少见，而且浏览器对它的支持也是比较全面的，从 IE8 开始，Chrome 3.0，Firefox 3.0，Opera 10.5，Safari 4.0 已经全线支持。不少网站已经开始投入使用。

### 9.3 使用 HTML5 Web Storage API
#### 设置和获取数据
Web Storage 主要分为 sessionStorage 和 localStorage 两个种存储模式，都可以通过 `setItem(key, value)` 、 `getItem(key)` 和 `removeItem(key)` 进行操作。`clear()` 函数会删除存储列表中的所有数据。

存储在 sessionStorage 中的数据，当浏览器或标签页关闭后就不复存在。同时，数据也只能在构建它们的窗口或者标签页内可见。

存储在 localStorage 中的数据，能够被同源的每个窗口或者标签页共享，同时，关闭窗口或者标签页也不会令数据消失。

#### 更多磁盘空间配额
HTML5 规范中建议浏览器允许每组同源页面使用 5MB 的空间，达到空间配额时浏览器应提示用户分配更多空间，同时提供查看已用空间的方法。

事件的实现方式多少有些出入。有些浏览器在不告知用户的情况下允许页面使用更多空间，有些则向用户提示已经扩展了空间，而另外一些则只是抛出 QUOTA _EXCEEDED _ERR 错误。

#### 事件
添加事件监听器即可接收同源窗口的 Storage 事件：

```JavaScript
window.addEventListener("storage", displayStorageEvent, true);
```

Storage 事件的接口形式如下所示

```
interface StorageEvent:Event{
    readonly attribute DOMString key;       //更新或删除的键
    readonly attribute any oldValue;        //更向前键对应的数据，如果是新添加的数据，则为 null
    readonly attribute any newValue;        //更新后的数据
    readonly attribute DOMString url;       //指向 Storage 事件发生的源
    readonly attribute Storage storageArea; //一个引用，指向发生改变的 localStorage 或 sessionStorage
}
```

### 9.5 浏览器数据库存储展望
> 虽然 Web SQL Database 已经在 Safari、Chrome 和 Opera 中实现，但是 Firefox 中并没有实现它，而且它在 WHATWG 维基中也被列为 “停滞” 状态。HTML5 规范中定义了一个执行 SQL 命令的 API ，接收字符串形式的输入，遵从 SQLite 对 SQL 语句的解析规范。由于标准认定直接执行 SQL 不可取， Web SQL Database 已经被较新的规范——索引数据库 (Indexed Database，原为 WebSimpleDB) 所取代。索引数据库更简便，而且也不依赖于特定的 SQL 数据库版本。目前浏览器正在逐步实现对索引数据库的支持。 —— Frank

## 第十章 构建离线 Web 应用
### 10.2 使用 HTML5 离线 Web 应用 API
#### 检查浏览器支持情况

``` JavaScript
if (window.applicationCache) {
  // code ...
}
```

#### html 元素的 manifest 属性
想要为 HTML5 应用程序添加离线支持，那么需要在 html 元素中加入 manifest 属性，属性的值为缓存清单文件：

``` HTML
<!DOCTYPE html>
<html manifest="application.manifest">
...
</html>
```

缓存清单内容示例：

```
CACHE MANIFEST
example.html
example.js
example.css
example.gif
```

#### 判断是否在线
是否处于在线状态可以通过检测 window.navigator 对象的属性进行判断。 

`navigator.onLine` 是一个标明浏览器是否处于在线状态的布尔属性。它甚至可以在 IE 中使用。（TODO：IE 几？）

当然， onLine 的值为 true 并不能保证 Web 应用程序在用户机器上一定能够访问到相应服务器（是在说中国吗？）。

当其值为 false 时，不管浏览器是否真的有联网。应用程序都不会尝试进行网络连接。

当在线状态发生变化时，触发 "online" 或者 "offline" 事件。

#### manifest 文件
manifest 文件的 MIME 类型为 text/cache-manifest 。

*需要注意， manifest 文件的内容类型必须配置为 text/cache-manifest 发送到浏览器。如果文件类型不正确，即使浏览器支持应用缓存也会返回缓存错误。*

manifest 文件示例：

```
#注释以#开头
CACHE MANIFEST
#要缓存的文件
about.html
html5.css
index.html
happy-trails-rc.gif
lake-tahoe.jpg
   
#不缓存登录页面
NETWORK
signup.html
   
#提供获取不到缓存资源时的备选资源路径
FALLBACK
signup.html    offline.html
/app/ajax/     default.html
```

#### applicationCache API
applicationCache API 是一个操作应用缓存的接口。新的 window.applicationCache 对象可触发一系列与缓存状态相关的事件。

该对象有一个数值型属性 window.applicationCache.status ，代表了缓存的状态。共有 6 种：

* 0: UNCACHED（未缓存） // 没有指定缓存清单的状况
* 1: IDLE（空闲）       // 带有缓存清单的应用程序的典型状态
* 2: CHECKING（检查中）
* 3: DOWNLOADING（下载中）
* 4: UPDATEREADY（更新就绪）
* 5: OBSOLETE（过期）   // 缓存曾经有效，但现在 manifests 文件丢失

对于上述状态，API 包含了与之对应的事件（和回调特性）。

* onchecking      CHECKING
* ondownloading   DOWNLOADING
* onupdateready   UPDATEREADY
* onobsolete      OBSOLETE
* oncached        IDLE

此外，没有可用更新或者发生错误时，还有一些表示更新状态的事件：

* onerror
* onnoupdate
* onprogress

window.applicationCache 有一个 `update()` 方法。调用 `update()` 方法会请求浏览器更新缓存。包括检查新版本的 manifest 文件并下载必要的新资源。如果没有缓存或者缓存已经过期，则会抛出错误。

## 第11章 HTML5未来展望
只能这么说，不论 HTML5 未来是否前途光明，它始终是前端攻城湿的饭碗。

### 触摸屏设备事件
Apple 在推出 iPhone 的同时，也将一系列的特殊事件引入到了其浏览器中。这些事件用来处理多点触摸输入和设备旋转。尽管还没有标准化，但这些事件已经被其他移动设备厂商选用。

#### 方向事件
在方向事件处理中，可以引用 `window.orientation` 属性。属性可选值如下所示，它们以页面首次加载时设备的方向为基准：

* 0     页面当前方向与首次加载时的原始方向一样
* -90   与原始方向比，设备顺时针旋转了 90 度（向右）
* 180   与原始方向比，设备旋转了 180 度（垂直翻转）
* 90    与原始方向比，设备逆时针旋转了 90 度（向左）

#### 手势事件
手势事件可以理解为通过多点触摸引发的缩放或者旋转。当用户有两个或多个手指同时在屏幕上挤压 (pinch) 或扭转 (twist) 时，就会触发手势事件。

扭转表示旋转，挤压 (pinch in) 和伸展 (pinch out) 分别表示缩小和放大。以下为事件处理程序：

* ongesturestart  用户将多个手指放在触摸屏上，开始滑动
* ongesturechange  用户正在使用手指动作进行缩放或者旋转操作
* ongestureend  用户移开手指，缩放或旋转操作已经完成

在用户做手势的过程中，事件处理程序或灵活检测事件的缩放或旋转属性，并对显示效果进行相应更新。以下为手势处理函数示例：

```JavaScript
function gestureChange(event) {
  // 获取用户手势生成的缩放量
  // 1.0 代表初始大小，小于 1.0 表示缩小，大于 1.0 表示放大
  // 基于尺寸大小按比例放大缩小
  var scale = event.scale;
  
  // 获取用户手势生成的旋转量
  // 旋转值介于 0 度到 360 度之间
  // 正值代表顺时针旋转，负值代表逆时针方向旋转
  var rotation = event.rotation;
  
  // 基于旋转操作更新页面显示
}
// 为文档节点添加手势变化监听程序
node.addEventListener('gesturechange', gestureChange, false);
```

#### 触摸事件
如果需要在低层次上处理设备事件，可以通过触摸事件获取所需信息。

* ontouchstart    已经在触摸设备表面放置了一个手指。当多个手指放在设备上时，会发生多点触摸事件
* ontouchmove     在拖动操作中，一个或多个手指发生了移动
* ontouchend      一个或多个手指离开设备表面
* ontouchcancel   意外停止了触摸操作

触摸事件需要考虑多点数据同时出现的情况，因此，用于处理触摸事件的 API 会相对复杂一点。

```JavaScript
function touchMove(event) {
  // touches 变量是一个列表，包含了当前每个手指的触摸点信息
  var touches = event.touches;
  
  // changedTouches 列表包含了当前触摸状态发生变化的手指的触摸点信息，如添加、移开或重放手指
  var changedTouches = event.changedTouches;
  
  // 监听器注册在哪个节点上， targetTouches 就包含哪个节点上发生的触摸操作的触摸点信息
  var targetTouches = event.targetTouches;
  
  // 在取得预定触摸点以后，可以取得能够从其他事件对象中正常获取的大部分信息
  var firstTouch = touches[0],
      firstTouchX = firstTouch.pageX,
      firstTouchY = firstTouch.pageY;
}
// 注册一个触摸事件监听器
node.addEventListener("touchmove", touchMove, false);
```

### 其他的书中提到的未来：

#### WebGL
##### 3D HTML
##### 3D 着色器

#### 设备
##### 音频数据 API
##### 视频元素改进
##### P2P 网络

