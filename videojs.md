#video.js 
> * 直接使用
> * 自定义控制条和样式
> * 弹幕相关
> * plugin写法

_video.js_ 是开源插件，据说甚至可以集合到gitbook上

**note**:
一个需要注意的点是下载下来的demo.html直接右键浏览器是打不开的，需要用iis或者nginx 或者beego这些服务端的玩意加载后，在前端浏览器才能看到视频页面。

##直接使用

```html
<!doctype html>
<html>
<head>
    <meta charset="utf-8">
    <title>无标题文档</title>
    <link href="video-js/video-js.css" rel="stylesheet" type="text/css"/>
    <script src="js/jquery.js" type="text/javascript" charset="utf-8"></script>
    <script src="video-js/video.js" type="text/javascript" charset="utf-8"></script>
    <script src="video-js/ie8/videojs-ie8.js"></script>
    <script type="text/javascript">
        videojs.options.flash.swf = "static/video-js/video-js.swf";
    </script>

</head>
<body>

<div id="videocontainer">
    <video id="example_video" class="video-js vjs-default-skin  vjs-big-play-centered" preload="auto" controls
           width="800" height="600" align="middle" poster=""
           data-setup="{ &quot;html5&quot; : { &quot;nativeTextTracks&quot; : false } }">
        <source src="你的名字.mp4" type="video/mp4"/>
		<!--
       // <source src="http://live.hkstv.hk.lxdns.com/live/hks/playlist.m3u8" type='video/mp4'>-->
    </video>

    <div id="btnCtrl" class="oriCtrl">
        <div id="idBtnPlay" type="button" title="播放" onclick="onPlay()"></div>
        <div id="idBtnFs" type="button" title="全屏" onclick="onFullScreen()"></div>
        <!--
        <button id="idBtnPlay" type="button" onclick="onPlay()">播放</button>
        <button id="idBtnStop" type="button" onclick="onPause()">暂停</button>
        <button id="idBtnFull" type="button" onclick="onFullscreen()">全屏</button>
        -->
    </div>
</div>
</body>
<!--<script src="js/btnctrl.js" type="text/javascript" charset="utf-8"></script>-->
</html>


```