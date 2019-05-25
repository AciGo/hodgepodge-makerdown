<!-- TOC -->

- [什么是响应式设计？](#什么是响应式设计)
- [响应式设计是最佳选择吗？](#响应式设计是最佳选择吗)
- [什么是视口(viewport)和屏幕尺寸(screen.width)？](#什么是视口viewport和屏幕尺寸screenwidth)
- [视口调试工具](#视口调试工具)
    - [响应式 Web 设计工作原理](#响应式-web-设计工作原理)
    - [创建媒体查询](#创建媒体查询)
- [respond.js 和 css3-mediaqueries.js 的比较](#respondjs-和-css3-mediaqueriesjs-的比较)
    - [respond.js 介绍](#respondjs-介绍)
    - [css3-mediaqueries.js 介绍](#css3-mediaqueriesjs-介绍)
- [CSS3 媒体查询(Media Query)详细介绍](#css3-媒体查询media-query详细介绍)
    - [关于 link vs @import 题外话](#关于-link-vs-import-题外话)
- [响应式设计前提](#响应式设计前提)
- [开始动手制作一个响应式网站吧](#开始动手制作一个响应式网站吧)
    - [Meta 标签或@viewport{}](#meta-标签或viewport)
    - [为 IE8 和以下版本添加支持媒体查询的 JS，可以用 media-queries.js 或 respond.js](#为-ie8-和以下版本添加支持媒体查询的-js可以用-media-queriesjs-或-respondjs)
    - [编写页面 HTML 结构](#编写页面-html-结构)
    - [编写媒体查询语句](#编写媒体查询语句)
- [媒体查询语句写在哪最为合适？](#媒体查询语句写在哪最为合适)
- [怎么设置断点？](#怎么设置断点)
- [响应式图片](#响应式图片)
- [我们也可以给最外层#wrapper 来设置 max-width 来限制流体布局页面被拉大，如：](#我们也可以给最外层wrapper-来设置-max-width-来限制流体布局页面被拉大如)
- [推荐二种响应式图片解决方案如下：](#推荐二种响应式图片解决方案如下)
    - [Matt Wilcox 解决方案](#matt-wilcox-解决方案)
    - [Sencha.io Src解决方案：](#senchaio-src解决方案)
- [响应式视频](#响应式视频)
- [响应式表格](#响应式表格)
- [响应式设计推荐框架](#响应式设计推荐框架)
- [总结](#总结)

<!-- /TOC -->

## 什么是响应式设计？
iOS 和 Android 的发布，智能手机、平板电脑、智能家电等新设备层出不穷，极大便利了我们的生活，但面对形形色色的终端设备，千差万别的屏幕分辨率，给网页设计带来了新的挑战，我们无法估计用户的终端设备和网络状况，更不可能为每种设备都专门设计一套网站，如何实现 Web UI 在多终端中的适配呢？2010 年 Ethan Marcotte 曾经在 A List Apart 发表过一篇文章"Responsive Web Design"，响应式网页设计提供了一种设计方法，可以使同一网站在智能手机、桌面电脑，以及介于这两者之间的任意设备上完美显示。这种方法能够根据用户的屏幕尺寸，合理地为现有及将来的各种设备提最佳的浏览体验。

http://mediaqueri.es/ 这是一个响应式设计创意收集网站，可以在上面查看到很多响应式设计实例，较多的网站都应用了 Mobile First 设计思想，先针对小视口设备进行设计，然后逐步对大视口设计进行渐进增强支持。

图 1. Froont 响应式设计网站截屏

![Froont 响应式设计网站截屏](https://www.ibm.com/developerworks/cn/web/1506_zhangqun_responsiveweb/index493.png)

这也意味着设计思维的转换，不应再执着于布局、线框等的具体大小，而应该考虑如何使用流体元素。与其根据不同设备的大小来设计页面，更着重考虑如何针对内容进行设计。抛弃像素，抛弃固定宽度，先从小屏幕进行设计，然后逐步增强针对大屏幕的设计

当然也需要引导客户，网站不必在所有浏览器中表现一致，时间应该花在更有价值的地方，而不是花大量时间修复烂浏览器固有的缺陷。核心思想就是，优雅降级，内容优先。

## 响应式设计是最佳选择吗？

实际项目中，什么时候我们需要用到响应式设计呢？并非所有的页面呈现，内容设计问题都可以通过响应式设计思路解决，项目的预算、目标用户及定位决定了其实现方式。

举几个例子来说，如果一个提供促销内容的网站，我们可以通过 CSS3 媒体查询技术，根据视口大小为用户提供与之匹配的视觉效果，迎合小屏幕的同时兼容超大屏幕，不同的视觉展示方式来响应各种设计大小，提供更好的用户体验和无歧视的设计，比如为小屏幕提供更容易手指操作的设计同时，为超大屏幕提供内容也能提供额外改进，这时响应式设计是优选。如果是一个预算充足的点餐网站，定制一个真正的“手机移动版”更为合适，用户可基于 GPS 找到附近的店家点餐，或者是一个 dashboard 网站，有大量的数据内容和管理命令，此时单独的响应式设计方案远远不能支撑这种解决方案。任何工具或技术都应该只在应用程序需要它的时候使用，IE 7 和 8 不支持 HTML5、CSS3 的新属性。如果一个网站的绝大多数用户都使用 IE 7 或 8，或更低版本，做成响应式意义也不大，但不代表做不到。

总结下来，响应式设计的优点是灵活性强，适应不同分辨率的设备，缺点是兼容各种设备导致代码臃肿，加载时间加长。

选择在现有代码基础上的增强的折中性响应式设计？还是选择一个真正的“手机版”，要依赖于项目实际情况。

## 什么是视口(viewport)和屏幕尺寸(screen.width)？

视口和屏幕尺寸不是同一个概念。视口(viewport），即可视区域的大小，是指浏览器窗口内的内容区域，不包含工具栏、标签栏等。也就是网页实际显示的区域。屏幕尺寸指的是设备的物理显示区域，用户屏幕的整体大小。

## 视口调试工具

在 IE 下，可使用 MS IE developer toolbar, Chrom 自带的调试工具也能很方便的进行视口调试。Safari 用户可选择 ResizeMe 或 Resize, Firefox 用户可以使用 Firesizer.

图 2. IE Developer Toolbar 视口调试截图

![IE Developer Toolbar 视口调试截图](https://www.ibm.com/developerworks/cn/web/1506_zhangqun_responsiveweb/index1634.png)

图 3. Firesizer 视口调试工具截图

![Firesizer 视口调试工具截图](https://www.ibm.com/developerworks/cn/web/1506_zhangqun_responsiveweb/index1693.png)

### 响应式 Web 设计工作原理

应用 CSS3 的媒体查询(Media Queries)，创建一个包含适应各种设备尺寸样式的 CSS。一旦页面在特定的设备上加载，此时，会先检测设备的视口大小，然后加载特定于设备的样式。即为不同的媒体类型设定专有的样式表。

### 创建媒体查询

以下代码演示了一个最为简单的媒体查询实例，浏览此页面并不断调整浏览器窗口宽度。页面的背景颜色就会根据当前的视口尺寸而发生变化。CSS 属性用于声明如何表现页面背景颜色切换，而 Media Query 语句是判断输出设备的最大视口宽度是否满足某种条件的条件表达式。

清单 1. 基本媒体查询代码
~~~
body {
background-color: grey;
}
@media screen and (max-width: 960px) {
body {
background-color: red;
}
}
@media screen and (max-width: 768px) {
body {
background-color: orange;
}
}
@media screen and (max-width: 550px) {
body {
background-color: yellow;
}
}
@media screen and (max-width: 320px) {
body {
background-color: green;
}
}
~~~

目前 Firefox 3.6+、Safari 4+、Chrome 4+、Opera 9.5+、iOS Safari 3.2+、Opera Mobile 10+、Android 2.1，IE9+ 都支持媒体查询。

图 4. 兼容 Media Query 的浏览器汇总

![兼容 Media Query 的浏览器汇总](https://www.ibm.com/developerworks/cn/web/1506_zhangqun_responsiveweb/index2446.png)

IE8 及以下的老版本并不支持 Media Query 查询，但也可以引入 polyfill 脚本(腻子脚本 polyfill 就是一大堆五花八门技术的集合，目的就是填平旧浏览器对 HTML5、CSS3 支持上的缺陷)，基于 JavaScript 实现兼容修复, 引入 Respond.js（https://github.com/scottjehl/Respond）或 css3-mediaqueries.js（https://github.com/livingston/css3-mediaqueries-js）可以解决这个问题。

清单 2. 为 IE 老版本引入腻子脚本
~~~
<!--[if lt IE 9]> 
<script src="https://oss.maxcdn.com/libs/respond.js/1.3.0/respond.min.js"></script> <![endif]-->
 
 
<!--[if lt IE 9]>  
<script src="//css3-mediaqueries-js.googlecode.com/svn/trunk/css3-mediaqueries.js"></script>  
<![endif]-->
~~~

## respond.js 和 css3-mediaqueries.js 的比较

### respond.js 介绍

**优势：**

- 体积小，速度快。
- 可靠！bootstrap 3 也引用了respond.js。
- 不依赖其他脚本或库，并为移动端做了优化（respond.min.js 只有 1kb）

**劣势：**

- 仅支持 min-width 和 max-width。
- Respond.js 不解析通过@import 引用的 CSS 也不解析包含在 style 标签内联的 media queries，因为这些样式不能重新请求解析。

**提示：**

- 引用 respond.js 时，请放到所有 CSS 文件之后（越早运行它，IE 用户不容易看到非 media 内容的闪烁）
- respond.js 通过 Ajax 请求一个您的 CSS 的原始副本，如果您部署您的样式文件在 CDN 上（或者子域名下），您需要上传一个代理页面来实现跨域通信。
- 因为安全的限制，一些浏览器可能不允许这个脚本在 file://ruls 里起作用(因为他使用 XMLHTTPRequest)。

**总结：** 它不是唯一的 CSS3 媒体查询腻子脚本，但是它可能是最快的。

### css3-mediaqueries.js 介绍

**优势：**
- 更健壮，支持很多 media query 特性，支持 px 和 em
- 实时响应

**劣势：**
- 体积较大(15kb) ，运行较慢
- 不支持 width
- 不支持 @import
- 对&lt;link&gt;和&lt;style&gt;标签内的 media 属性无效

**总结：** 当渲染复杂的响应性设计时，它能支持很多 media query，但运行明显缓慢。

## CSS3 媒体查询(Media Query)详细介绍

HTML4 和 CSS 2 中可以通过<link>标签的 media 属性为样式表指定设备类型，有八种，screen 和 print 是两种最常见的媒体类型，举例说明，以下代码为不同的设备指定加载不同的样式文件，实际应用比如说，一个页面在屏幕上显示时使用无衬线字体，而在打印时则使用衬线字体以显示更好的可读性，或一个页面在屏幕上显示时有广告，打印时去除广告，都可通过 media 属性来分别加载匹配的 CSS 文件解决。
~~~
<link rel="stylesheet" type="text/css" media="screen" href="screen.css">
 
<link rel="stylesheet" type="text/css" media="print" href="print.css" />
~~~

CSS3 中的 Media Queries 增加了更多的媒体查询，在 CSS3 中我们可以设置不同类型的媒体条件，并根据对应的条件，给相应符合条件的媒体调用相对应的样式表。您可以把媒体查询语句想象为条件表达式，举例说明
~~~
<link rel="stylesheet" media="screen and (orientation: portrait)"  href="portraitscreen.css" />
~~~

您是一块纵向的显示屏吗？
~~~
<link rel="stylesheet" media="not screen and (orientation: portrait)"  href="portraitscreen.css" />
~~~

追加 not 颠倒该查询的逻辑
~~~
<link rel="stylesheet" media="screen and (orientation: portrait) and (min-width:
800px)" href="800wide-portrait-screen.css" />
~~~

视口宽度大于 800px 的纵向显示屏
~~~
<link rel="stylesheet" media="screen and (orientation: portrait) and (min-width:
800px), projection" href="800wide-portrait-screen.css" />
~~~

只要是 projection 就为真，全部查询都不为真，则不加载
~~~
@media screen and (max-device-width: 400px) {h1{color:red}
~~~

媒体查询也可以写到样式表中
~~~
@import url("phone.css") screen and (max-width:360px);
~~~

也有不常见的写法，是在 CSS 样式表中用@import 在当前样式表中引用其他样式表。但需要注意的是慎用 CSS 的@import，会影响加载速度。

### 关于 link vs @import 题外话

我们可以通过 link 和@import 两种方式引入 CSS 文件

清单 3. 引入 CSS 方式
~~~
<link rel='stylesheet' href='a.css'>
 
或
 
<style>
@import url('a.css');
</style>
~~~

二者之间的差别是：

Link 采用 HTML 标签将 CSS 关联，而 import 可以在一个 CSS 文件中引入其它的 CSS 文件

兼容性的差别。IE6 以下不支持@import

加载顺序的差别（详细请参见：http://www.stevesouders.com/blog/2009/04/09/dont-use-import/）

4）JS 控制 DOM 样式时的差别（link 支持使用 Javascript 控制 DOM 去改变样式；而@import 不支持）

实际项目经验是，@import 在 IE 中会导致文件下载次序被更改，例如放置在@import 后面的 script 文件会在 CSS 之前被下载执行，若此时脚本里 getElementsByClassName 时则出错。

## 响应式设计前提

响应式设计理念是基于流动布局、弹性图片、弹性表格、弹性视频和媒体查询等技术的组合。使用百分比布局创建流动的弹性界面，同时使用媒体查询来限制元素的变动范围，这两者组合到一起构成了响应式设计的核心。而固定宽度像素的布局无法实现多变的，未知的设备。

关于流动布局，它是一种适应屏幕的做法，即不固定 DIV 的宽度，而是采用百分比作为单位来确定每一块的宽度。具体采用的方式就是弃用 px，改用 em 或%为单位，早期我们此方式是为了文字缩放，确保老版本的 IE 也能调节字体大小。您可能会有疑问，既然现代浏览器很久以前就支持缩放以像素为单位的文字，那么用 em 的优越性又体现在哪里呢？最大的优势是，当我们用相对单位 em 来写布局和字体大小时，如果我们给<body>标签设置文字大小为 100%，给其他文字都使用相对单位 em，那这些文字都会受 body 上的初始声明的影响。这样您可以批量修改页面文字和布局，相对更为灵活。缺点是可能在百分比的计算上不如像素那样简单直接，将像素转为 em 的诀窍在于，Dan Cederholm 写过一篇关于流式布局里文章里提到一个公式，目标元素尺寸÷上下文元素尺寸=百分比尺寸，并不能简单的把目标元素理解成子元素，把上下文元素理解成父级元素，虽然大多数时可以这么理解，但上下文元素并不一定代表父级元素，举例说明如下：

如果一个实际 PSD 设计稿是采用固定像素设计的，940px 的 header 层被包含在 960px 的 wrapper 层里。

图 5. PSD 设计稿举例

![PSD 设计稿举例](https://www.ibm.com/developerworks/cn/web/1506_zhangqun_responsiveweb/index6082.png)

wrapper 层内包裹 header 层，按 px 写法是：

清单 4. 固定宽度设计代码示例
~~~
#wrapper {
margin-right: auto;
margin-left: auto;
width: 960px;
}
#header {
margin-right: 10px;
margin-left: 10px;
width: 940px;
}
~~~

转成百分比后，应该这么写，此时的上下文元素就是父级元素：

清单 5. 百分比宽度设计代码示例
~~~
#wrapper {
margin-right: auto;
margin-left: auto;
width: 90%; /* 控制最外层的 div，这个值可以是 100%, 90%或者其它您认为实际页面显示最佳的一个百分比*/
}
#header {
margin-right: 10px;
margin-left: 10px;
width: 97.9166667%; /* 940 ÷ 960 */
}
~~~

再举例说明，如下 HTML 结构
~~~
<h1>测试<span>上下文</span></h1>
 
CSS 样式为：
h1 {font-size: 4.3125em; } /* 69 ÷ 16 */
#content h1 span {
font-size: .550724638em; /* 38 ÷ 69 */
line-height: 1.052631579em; /* 40 ÷ 38 */
}
~~~

默认浏览器文字大小是 16px，假设设计稿里 h1 为 69px, span 为 38px，line-height 为 40px，可以看到<span>元素的文字大小（38px）是相对于其父元素的文字大小（69px）而言的，而它的行高（40px）则是相对于其本身的文字大小（38px）而言，所以我们不能一概而论认为上下文元素就是等同于父级元素。

总结；流式布局是响应式设计的基础和起点。

## 开始动手制作一个响应式网站吧

以这样一个页面布局为例，分别在最大宽度为 980px、700px 和 480px 时改变其布局：

图 6. 页面布局示例

![页面布局示例](https://www.ibm.com/developerworks/cn/web/1506_zhangqun_responsiveweb/index6975.png)

### Meta 标签或@viewport{}
~~~
<meta name="viewport" content="width=device-width, initial-scale=1.0">
~~~

Intial-scale=1.0 即阻止移动浏览器自动调整页面大小 ，表示浏览器将按照其视口的实际大小来渲染页面。

我们也可以使用@viewport 规则控制 viewport，与使用 meta 标签的效果相同，只是完全使用 CSS 来控制。zoom 描述符等同于 viewport meta 标签的 initial-sacale 属性。
~~~
@viewport {
width: device-width;
zoom: 1;
}
~~~

### 为 IE8 和以下版本添加支持媒体查询的 JS，可以用 media-queries.js 或 respond.js
~~~
<!--[if lt IE 9]>
<script src="//css3-mediaqueries-js.googlecode.com/svn/trunk/css3-mediaqueries.js"></script>
<![endif]-->
~~~

### 编写页面 HTML 结构
~~~
/* STRUCTURE */
#pagewrap {
width: 960px;
}
#header {
height: 180px;
}
#content {
width: 600px;
float: left;
}
#sidebar {
width: 300px;
float: right;
}
#footer {
clear: both;
}
~~~

### 编写媒体查询语句

当视口宽度为 980px 及以下时定义了百分比宽度，当视口宽度为 700 及以下时，去除宽度和浮动，此时 content 和 sidebar 将占满整个宽度，当视口宽度为 480 及以下时，缩小 h1 标题的字体大小，隐藏 sidebar，以更好的适应小屏幕有限空间。代码如下：
~~~
/* for 980px or less */
@media screen and (max-width: 980px) {
 
#pagewrap {
width: 95%;
}
#content {
width: 65%;
}
#sidebar {
width: 30%;
}
 
}
/* for 700px or less */
@media screen and (max-width: 700px) {
 
#content {
width: auto;
float: none;
}
#sidebar {
width: auto;
float: none;
}
 
}
 
/* for 480px or less */
@media screen and (max-width: 480px) {
 
h1 {
font-size: 0.8em;
}
#sidebar {
display: none;
}
 
}
~~~

（以上代码参考文章：http://webdesignerwall.com/tutorials/responsive-design-in-3-steps）

##  媒体查询语句写在哪最为合适？

将不同的媒体查询样式保存到独立的文件中，个人认为没有太大的好处，因为多个独立的文件会增加用于页面渲染的 HTTP 请求数量，从而导致页面加载变慢，事实上这样处理也并未更便于组织代码，而极容易遗漏部分媒体查询语句。

建议在已有的样式表中追加媒体查询样式。在主样式后面紧随媒体查询语句的一个好处是，当主样式有所更改时，媒体查询样式很有可能也需要做相应的调整，这样您不容易遗漏。如：
~~~
#header h1{}
@media screen and (max-width: 768px) {#header h1{}}方法。
 
窗体底端
~~~

## 怎么设置断点？

传统的确定断点的方案是使用一些固定的宽度进行划分，如 320px(iPhone)，768px(iPad)，960px 或 1024px(传统 PC 浏览器)，这种方案的好处是很好照顾到了当前主流的设备，但是技术发展实现是太快了，各种不同分辨率的设备层出不穷，比如一些手机尺寸接近平板，一些平板尺寸比电脑更大等等，很难保证未来能很好的支持各种设备。另外一种确定断点的方法是根据内容进行设置断点以及设置多少个断点。随着各种尺寸设备的出现，断点命名采用更为通用的方式，而不是用设备来命名更为合适。

图 7. 断点设置参考

![断点设置参考](https://www.ibm.com/developerworks/cn/web/1506_zhangqun_responsiveweb/index8883.png)

## 响应式图片

实现图片随着流动布局相应缩放非常简单。需要在 CSS 中声明：
~~~
img { max-width: 100%; }
~~~

这样就可以使图片自动缩放到与其容器 100%匹配。我们可以将同样的样式应用到其他多媒体标签上。如：
~~~
img,object,video,embed {max-width: 100%;}
~~~

图片可以随着视口的伸缩而缩放了，但是如果将视口拉大(比如 1900px），直到图片拉伸至超出其原始尺寸，那么图片很有可能也被拉宽，必须追加一句 CSS 声明：
~~~
.img{max-width: 202px;}
~~~

## 我们也可以给最外层#wrapper 来设置 max-width 来限制流体布局页面被拉大，如：

清单 6. 响应式图片代码示例
~~~
#wrapper {
margin-right: auto;
margin-left: auto;
width: 96%;
max-width: 1414px;
}
~~~

那么问题来了？这样算完成了图片的响应吗？答案是否定的。弹性图片并不等于响应式图片。我们应该为不同的屏幕尺寸提供不同的图片。如下所示，不再是一张图片满足不同的设备，而是为大屏幕准备大尺寸图片，小屏幕准备尺寸更小的清晰图片。

图 8. 一张图片应用在不同尺寸的设备上

![一张图片应用在不同尺寸的设备上](https://www.ibm.com/developerworks/cn/web/1506_zhangqun_responsiveweb/index9421.png)

图 9. 针对不同的设备应用不同的图片

![针对不同的设备应用不同的图片](https://www.ibm.com/developerworks/cn/web/1506_zhangqun_responsiveweb/index9444.png)

## 推荐二种响应式图片解决方案如下：

### Matt Wilcox 解决方案

- 把.htaccess 和 adaptive-images.php 放根目录
- 创建一个可写入的缓存文件夹
- &lt;script&gt;document.cookie='resolution='+Math.max(screen.width,screen.height)+'; path=/';&lt;/script&gt;

**注：** [为不同的屏幕尺寸提供不同的图片](https://blog.csdn.net/u014328357/article/details/52910437) 图片尺寸必须比其显示尺寸更大以保证渲染效果，否则的话图片可能看起来很糟糕。基于这个原因，图片文件的体积就永远比实际显示所需的大。

很多人都在研究这个问题，尝试为较小的屏幕尺寸提供较小的图片。可以使用 Matt Wilcox 的“自适应图片”(http://adaptive-images.com)了。Matt Wilcox 的解决方案则不需要对图片标签做修改，而且他的方案会根据标签中已经设定的全尺寸图片自动创建各种尺寸的图片。这种解决方案允许基于一组屏幕尺寸断点，根据用户需要为其提供不同的图片。

实现 Adaptive Images 解决方案需要 Apache 2、PHP 5.x 和 GD 库，也就是说需要 Web 服务器端编程。

### Sencha.io Src解决方案：
~~~
<img src="//src.sencha.io/http://mysite.com/images/my-image.jpg" />
~~~

## 响应式视频

不同与以往需要插入视频需要一段臃肿的代码，HTML5 插入视频只需要一行代码，但问题是它不是响应的。
~~~
<video src="Video.ogg"></video>
~~~

注意：Safari 只允许在<video>和<audio>元素中使用 MP4/H.264/AAC 媒体文件，而 Firefox 和 Opera 则只支持 Ogg 和 WebM，所以更为完整的写法如下：

清单 7. 响应式视频代码示例
~~~
<video width="640" height="480" controls autoplay preload="auto" loop
poster="myVideoPoster.jpg">
<source src="video/myVideo.ogv" type="video/ogg">
<source src="video/myVideo.mp4" type="video/mp4">
responsive video
</video>
~~~

如何让视频响应呢？我们需要追加 max-width:100%声明：
~~~
video { max-width: 100%; height: auto; }
~~~

问题：如果视频是被 iframe 内嵌又如何实现响应式呢？最简单的办法是使用一个名为 FitVids 的 jQuery 小插件。代码如下：

清单 8. Iframe 引入的视频实现响应式代码示例
~~~
<script src="js/fitvids.js"></script>
 
<script>
$(document).ready(function(){
$("#content").fitVids();
});
</script>
~~~

## 响应式表格

实现数据表格响应的方式有很多种，要根据表格内容，数据量来进行具体的设计。流体表格并不等于响应式表格，比如一个百分比宽度表格，在小屏幕设备上可以正常显示但内容整体被压缩得较小，可读性差，并不代表它是真正响应的。

举例说明，针对以下这个数据较多的表格，有多种设计思路可以采用。

图 10. 如何实现一个响应式的复杂表格之表格示例

![如何实现一个响应式的复杂表格之表格示例](https://www.ibm.com/developerworks/cn/web/1506_zhangqun_responsiveweb/index10637.png)

**思路1：** 为小屏幕隐藏不必要的列，精简表格。主要利用 CSS table td:nth-child(n){display:none}实现。

**思路 2：** 将横向的表头利用 CSS 改为纵向显示并固定位置，其余内容部分不变并出现横向滚动条。tbody 上应用 white-space:nowrap; tbody tr 下应用 display:inline-block;

**思路 3：** 将每一行数据对应表头显示在一块区域内，接下来每行依次类推。利用 HTML data 属性，将表头名称写到 CSS 中，td:before{content:attr(data-title);

**思路 4：** 将纯文本的表格在小屏幕上的饼图或柱状图，以一种更丰富的形式显示数据，前提是这是合适表格内容的。

**思路 5：** 用一张小缩略图显示在小屏幕上并提示用户点击查看表格，从而新开页面以获得更大的屏幕空间显示整个表格。

以上解决方案具体实现代码请参考“参考资源”链接。

## 响应式设计推荐框架

使用 Modernizr 让老版本 IE 支持 HTML5 元素，BootStrap, Semantic UI, Foundation 前端界面开发框架都可选择。

## 总结

本文简单介绍了常用的响应式设计工具，思路和技巧，除了需要掌握一定的技术实现手段，更需要有如何设计，如何布局的思考，也不能忽略对高分辨率设备的支持，了解渐进增强与优雅降级的本质区别，真正实现页面好看、好用、合理。