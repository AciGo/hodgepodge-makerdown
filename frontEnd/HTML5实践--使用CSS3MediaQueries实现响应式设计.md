转载请注明原创地址：http://www.cnblogs.com/softlover/archive/2012/11/21/2781388.html

 

　　现在屏幕分辨率的范围很大，从 320px (iPhone) 到 2560px (大型显示器)，甚至更大。用户也不只是使用台式电脑访问web站点了，他使用手机、笔记本电脑、平板电脑。所以传统的设置网站宽度为固定值，已经不能满足需要了。web设计需要适应这种新要求，页面布局需要能够根据访问设备的不同分辨率自动进行调整。本教程将会向你介绍，如何使用html5和CSS3 Media Queries完成跨浏览器的响应式设计。

　　demo预览地址：http://webdesignerwall.com/demo/adaptive-design/final.html

　　demo下载地址：http://www.webdesignerwall.com/file/adaptive-design-demo.zip

## 第一次运行

　　在开始之前，我们可以查看 最终demo 来查看最终效果。调整你浏览器的大小，我们可以看到页面会根据视窗的大小自动调整布局。

![https://pic002.cnblogs.com/images/2012/303151/2012112217235716.jpg](https://pic002.cnblogs.com/images/2012/303151/2012112217235716.jpg)

**_更多例子_**

你可以访问下面的地址，查看更多相关例子： [WordPress themes](https://themify.me/)。我设计了如下media queries： [Tisa](http://themify.me/themes/tisa), [Elemin](https://themify.me/themes/elemin), [Suco](https://themify.me/themes/suco), [iTheme2](https://themify.me/themes/itheme2), [Funki](http://themify.me/themes/funki), [Minblr](http://themify.me/themes/minblr), 和 [Wumblr](http://themify.me/themes/wumblr)。

## 概述

默认情况下，页面容器的宽度是980px，这个尺寸优化了大于1024px的分辨率。Media query用来检查 viewport 宽度，如果小于980px他会变为窄屏显示模式，页面布局将会以流动的宽度代替固定宽度。如果 viewport 小于650px，他会变为mobile显示模式，内容、侧边栏等内容会变为单独列布局方式，他们的宽度占满屏幕宽度。

![https://pic002.cnblogs.com/images/2012/303151/2012112217253149.jpg](https://pic002.cnblogs.com/images/2012/303151/2012112217253149.jpg)

## HTML代码

在这里，我不会介绍下面html代码中的细节。下面是布局页面的主框架，我们有一个“pagewrap”的容器包装了"header", "content", "sidebar", 和 "footer"。
~~~
<div id="pagewrap">

    <header id="header">

        <hgroup>
            <h1 id="site-logo">Demo</h1>
            <h2 id="site-description">Site Description</h2>
        </hgroup>

        <nav>
            <ul id="main-nav">
                <li><a href="#">Home</a></li>
            </ul>
        </nav>

        <form id="searchform">
            <input type="search">
        </form>

    </header>
    
    <div id="content">

        <article class="post">
            blog post
        </article>

    </div>
    
    <aside id="sidebar">

        <section class="widget">
             widget
        </section>
                        
    </aside>

    <footer id="footer">
        footer
    </footer>
    
</div>
~~~

**_HTML5.js_**

请注意，我在demo中使用了html5标签，不过IE9之前IE浏览器不支持&lt;header&gt;, &lt;article&gt;, &lt;footer&gt;, &lt;figure&gt;等html5新标签。可以在html文档中添加 html5.js 文件将解决这一问题。
~~~
<!--[if lt IE 9]>
    <script src="http://html5shim.googlecode.com/svn/trunk/html5.js"></script>
<![endif]-->
~~~

## CSS

**_设置html5元素为块状元素_**

下面的css将会把html5的元素（article, aside, figure, header, footer 等）设置为块元素。
~~~
article, aside, details, figcaption, figure, footer, header, hgroup, menu, nav, section { 
    display: block;
}
~~~

**_css主体结构_**

在这里我也不会解释css文件的细节。页面主容器“pagewrap”的宽度被设置为980px。header被设置为固定高度160px。“content”的宽度为600px，靠左浮动。“sidebar”宽度设置为280px，靠右浮动。
~~~
#pagewrap {
    width: 980px;
    margin: 0 auto;
}

#header {
    height: 160px;
}

#content {
    width: 600px;
    float: left;
}

#sidebar {
    width: 280px;
    float: right;
}

#footer {
    clear: both;
}
~~~

**_Step 1 Demo_**

我们可以通过[demo](http://webdesignerwall.com/demo/adaptive-design/demo-step1.html)查看当前效果。这时我们还没有使用 media queries，调整浏览器宽度，页面布局也不会发生变化。

## CSS3 Media Query

你可以通过《[HTML5实践 -- CSS3 Media Queries](http://www.cnblogs.com/softlover/archive/2012/11/25/2787429.html)》了解更多信息。

**_包含 Media Queries Javascript文件_**

IE8和之前的浏览器不支持CSS3 media queries，你可以在页面中添加css3-mediaqueries.js来解决这个问题。
~~~
<!--[if lt IE 9]>
    <script src="http://css3-mediaqueries-js.googlecode.com/svn/trunk/css3-mediaqueries.js"></script>
<![endif]-->
~~~

**_包含 Media Queries CSS_**

创建media query所需的css，然后在页面中添加引用。
~~~
<link href="media-queries.css" rel="stylesheet" type="text/css">
~~~

**_Viewport小于 980px(流动布局)_**

当viewport小于980px的时候，将会采用下面的规则：

- pagewrap = 宽度设置为 95%
- content = 宽度设置为 60%
- sidebar = 宽度设置为 30%

　　tips：使用百分比（%）可以使容器变为不固定的。
~~~
@media screen and (max-width: 980px) {

    #pagewrap {
        width: 95%;
    }

    #content {
        width: 60%;
        padding: 3% 4%;
    }

    #sidebar {
        width: 30%;
    }
    #sidebar .widget {
        padding: 8% 7%;
        margin-bottom: 10px;
    }

}
~~~

**_Viewport小于 650px(单列布局)_**

当viewport小于650px的时候，将会采用下面的规则：

- header = 设置高度为 auto
- searchform = 重新设置 searchform 的位置 5px top
- main-nav = 设置位置为 static
- site-logo = 设置位置为 static
- site-description = 设置位置为 static
- content = 设置宽度为 auto (这会使容器宽度变为fullwidth) ，并且摆脱浮动
- sidebar = 设置宽度为 100%，并且摆脱浮动
~~~

@media screen and (max-width: 650px) {

    #header {
        height: auto;
    }

    #searchform {
        position: absolute;
        top: 5px;
        right: 0;
    }

    #main-nav {
        position: static;
    }

    #site-logo {
        margin: 15px 100px 5px 0;
        position: static;
    }

    #site-description {
        margin: 0 0 15px;
        position: static;
    }

    #content {
        width: auto;
        float: none;
        margin: 20px 0;
    }

    #sidebar {
        width: 100%;
        float: none;
        margin: 0;
    }

}
~~~

**_Viewport小于 480px_**

下面得css是为了应对小于480px屏幕的情况，iphone横屏的时候就是这个宽度。

- html = 禁用文字大小调整。 默认情况，iphone增大了字体大小，这样更便于阅读。你可以使用 -webkit-text-size-adjust: none; 来取消这种设置。
- main-nav = 字体大小设置为 90%
~~~
@media screen and (max-width: 480px) {

    html {
        -webkit-text-size-adjust: none;
    }

    #main-nav a {
        font-size: 90%;
        padding: 10px 8px;
    }

}
~~~

## 弹性的图片

为了让图片尺寸变得更为弹性，可以简单的添加 max-width:100% 和 height:auto。这种方式在IE7中正常工作，不能在IE8中工作，需要使用 width:auto\9 来解决这个问题。
~~~
img {
    max-width: 100%;
    height: auto;
    width: auto\9; /* ie8 */
}
~~~

## 弹性的嵌入视频

为了使嵌入视频也变得更加弹性，也可以使用上面的方法。但是不知道什么原因，max-width:100% 在safari浏览器中不能正常的在嵌入资源中工作。我们需要使用width:100% 来代替他。
~~~
.video embed,
.video object,
.video iframe {
    width: 100%;
    height: auto;
}
~~~

## initial-scale Meta 标签 (iPhone)
默认情况下，iphone的safari浏览器会收缩页面，以适应他的屏幕。下面的语句告诉iphone的safari浏览器，使用设备宽度作为viewport的宽度，并且禁用initial-scale。
~~~
<meta name="viewport" content="width=device-width; initial-scale=1.0">
~~~

## 最终效果

查看最终的demo，调整浏览器的大小，查看media query 的行为。你也可以使用iPhone, iPad, 新版Blackberry, 和 Android 来查看modile版的效果。

![https://pic002.cnblogs.com/images/2012/303151/2012112217235716.jpg](https://pic002.cnblogs.com/images/2012/303151/2012112217235716.jpg)

## 总结

**_可靠的Media Queries Javascript_**

　　可以使用css3-mediaqueries.js来解决浏览器不支持media queries的问题
~~~
<!--[if lt IE 9]>
    <script src="http://css3-mediaqueries-js.googlecode.com/svn/trunk/css3-mediaqueries.js"></script>
<![endif]-->
~~~

**_CSS Media Queries_**

　　这一技巧可以创建自适应的设计，可以根据 viewport 的宽度重写布局的css。
~~~
@media screen and (max-width: 560px) {

    #content {
        width: auto;
        float: none;
    }

    #sidebar {
        width: 100%;
        float: none;
    }

}
~~~

**_弹性的图片_**

　　使用max-width:100% 和 height:auto，可以让图片变得更加弹性。
~~~
img {
    max-width: 100%;
    height: auto;
    width: auto\9; /* ie8 */
}
~~~

**_弹性的内嵌视频_**

　　使用width:100% 和 height:auto，可以让内嵌视频变得更加弹性。
~~~
.video embed,
.video object,
.video iframe {
    width: 100%;
    height: auto;
}
~~~
 

**_Webkit字体大小调整_**

　　使用-webkit-text-size-adjust:none，在iphone上禁用字体大小调整。
~~~
html {
    -webkit-text-size-adjust: none;
}
~~~
 

**_设置iPhone Viewport 和 Initial Scale_**

　　下面的语句实现了在iphone中，使用meta标签设置viewport 和 inital scale。
~~~
<meta name="viewport" content="width=device-width; initial-scale=1.0">
~~~

　　好了，今天的教程到此为止。

原文地址：http://webdesignerwall.com/tutorials/responsive-design-with-css3-media-queries
