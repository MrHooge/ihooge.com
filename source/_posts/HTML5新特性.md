---
title:  HTML5 新特性
date: 2017-07-13 21:25:36
tags:  HTML5
---

# 新特性
HTML5 中的一些有趣的新特性：
>用于绘画的 canvas 元素
>用于媒介回放的 video 和 audio 元素
>对本地离线存储的更好的支持
>新的特殊内容元素，比如 article、footer、header、nav、section
>新的表单控件，比如 calendar、date、time、email、url、search

## HTML 5 视频
<video src="movie.ogg" controls="controls"></video>
control 属性供添加播放、暂停和音量控件。

video 元素允许多个 source 元素。source 元素可以链接不同的视频文件。浏览器将使用第一个可识别的格式：
<video width="320" height="240" controls="controls">
  <source src="movie.ogg" type="video/ogg">
  <source src="movie.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>

<!-- more -->
标签的属性
autoplay		如果出现该属性，则视频在就绪后马上播放。
controls		如果出现该属性，则向用户显示控件，比如播放按钮。
height		设置视频播放器的高度。
loop		如果出现该属性，则当媒介文件完成播放后再次开始播放。
preload		如果出现该属性，则视频在页面加载时进行加载，并预备播放。 如果使用 "autoplay"，则忽略该属性。
src			要播放的视频的 URL。
width		设置视频播放器的宽度。

方法:play() pause()	 load()	 canPlayType

## HTML 5 音频
<audio src="song.ogg" controls="controls"></audio>

audio 元素允许多个 source 元素。source 元素可以链接不同的音频文件。浏览器将使用第一个可识别的格式：
<audio controls="controls">
  <source src="song.ogg" type="audio/ogg">
  <source src="song.mp3" type="audio/mpeg">
Your browser does not support the audio tag.
</audio>

标签的属性
autoplay		如果出现该属性，则音频在就绪后马上播放。
controls		如果出现该属性，则向用户显示控件，比如播放按钮。
loop			如果出现该属性，则每当音频结束时重新开始播放。
preload			如果出现该属性，则音频在页面加载时进行加载，并预备播放。如果使用 "autoplay"，则忽略该属性。
src				要播放的音频的 URL。


## HTML 5 拖放
拖放是一种常见的特性，即抓取对象以后拖到另一个位置。
在 HTML5 中，拖放是标准的一部分，任何元素都能够拖放。

	拖动什么 - ondragstart 和 setData()
	放到何处 - ondragover
	进行放置 - ondrop

## HTML 5 Canvas
HTML5 的 canvas 元素使用 JavaScript 在网页上绘制图像。
画布是一个矩形区域，您可以控制其每一像素。
canvas 拥有多种绘制路径、矩形、圆形、字符以及添加图像的方法。


## HTML5 内联 SVG
	SVG 指可伸缩矢量图形 (Scalable Vector Graphics)
	SVG 用于定义用于网络的基于矢量的图形
	SVG 使用 XML 格式定义图形
	SVG 图像在放大或改变尺寸的情况下其图形质量不会有损失
	SVG 是万维网联盟的标准
**SVG 的优势**
	SVG 图像可通过文本编辑器来创建和修改
	SVG 图像可被搜索、索引、脚本化或压缩
	SVG 是可伸缩的
	SVG 图像可在任何的分辨率下被高质量地打印
	SVG 可在图像质量不下降的情况下被放大

## HTML 5 Canvas vs. SVG
SVG 是一种使用 XML 描述 2D 图形的语言。
SVG 基于 XML，这意味着 SVG DOM 中的每个元素都是可用的。您可以为某个元素附加 JavaScript 事件处理器。
在 SVG 中，每个被绘制的图形均被视为对象。如果 SVG 对象的属性发生变化，那么浏览器能够自动重现图形。
Canvas 通过 JavaScript 来绘制 2D 图形。
Canvas 是逐像素进行渲染的。
在canvas中，一旦图形被绘制完成，它就不会继续得到浏览器的关注。如果其位置发生变化，那么整个场景也需要重新绘制，包括任何或许已被图形覆盖的对象。
### Canvas 与 SVG 的比较
**Canvas**
	依赖分辨率
	不支持事件处理器
	弱的文本渲染能力
	能够以 .png 或 .jpg 格式保存结果图像
	最适合图像密集型的游戏，其中的许多对象会被频繁重绘
**SVG**
	不依赖分辨率
	支持事件处理器
	最适合带有大型渲染区域的应用程序（比如谷歌地图）
	复杂度高会减慢渲染速度（任何过度使用 DOM 的应用都不快）
	不适合游戏应用

## HTML5 地理定位
HTML5 Geolocation API 用于获得用户的地理位置。
鉴于该特性可能侵犯用户的隐私，除非用户同意，否则用户位置信息是不可用的。

## HTML 5 Web 存储
HTML5 提供了两种在客户端存储数据的新方法：
	localStorage - 没有时间限制的数据存储
	sessionStorage - 针对一个 session 的数据存储

## HTML 5 应用程序缓存
HTML5 引入了应用程序缓存，这意味着 web 应用可进行缓存，并可在没有因特网连接时进行访问。
应用程序缓存为应用带来三个优势：
	离线浏览 - 用户可在应用离线时使用它们
	速度 - 已缓存资源加载得更快
	减少服务器负载 - 浏览器将只从服务器下载更新过或更改过的资源。
如需启用应用程序缓存，请在文档的 <html> 标签中包含 manifest 属性：
manifest 文件可分为三个部分：
	CACHE MANIFEST - 在此标题下列出的文件将在首次下载后进行缓存
	NETWORK - 在此标题下列出的文件需要与服务器的连接，且不会被缓存
	FALLBACK - 在此标题下列出的文件规定当页面无法访问时的回退页面（比如 404 页面）
## HTML 5 Web Workers
当在 HTML 页面中执行脚本时，页面的状态是不可响应的，直到脚本已完成。
web worker 是运行在后台的 JavaScript，独立于其他脚本，不会影响页面的性能。您可以继续做任何愿意做的事情：点击、选取内容等等，而此时 web worker 在后台运行。
## HTML 5 服务器发送事件
Server-Sent 事件指的是网页自动获取来自服务器的更新。
以前也可能做到这一点，前提是网页不得不询问是否有可用的更新。通过服务器发送事件，更新能够自动到达。
例子：Facebook/Twitter 更新、估价更新、新的博文、赛事结果等。

## HTML5 新的Input 类型
```
	email
	url
	number
	range
	Date pickers (date, month, week, time, datetime, datetime-local)
	search
	color
```
## HTML5 的新的表单元素
```
	datalist
	keygen
	output
```
## HTML5 的新的表单属性
新的 form 属性：
	autocomplete
	novalidate
新的 input 属性：
```
	autocomplete
	autofocus
	form
	form overrides (formaction, formenctype, formmethod, formnovalidate, formtarget)
	height 和 width
	list
	min, max 和 step
	multiple
	pattern (regexp)
	placeholder
	required
```
**Input Attr : Placeholder**
占位符是一个文本框，当没有值时，它将文本放在较浅的阴影中。
**Input Attr : Required**
“必填字段”是在提交表单之前必须用值填充的字段。必填字段有时称为必填字段或强制字段

新的行级元素
```
<mark>
<time>
<meter>
<progress>
```
新的块级元素
```
<section>
<header>
<footer>
<nav>
<article>
<aside>
```

## HTML5 WebSocket

WebSocket是HTML5开始提供的一种在单个 TCP 连接上进行全双工通讯的协议。
在WebSocket API中，浏览器和服务器只需要做一个握手的动作，然后，浏览器和服务器之间就形成了一条快速通道。两者之间就直接可以数据互相传送。
浏览器通过 JavaScript 向服务器发出建立 WebSocket 连接的请求，连接建立以后，客户端和服务器端就可以通过 TCP 连接直接交换数据。
当你获取 Web Socket 连接后，你可以通过 send() 方法来向服务器发送数据，并通过 onmessage 事件来接收服务器返回的数据。