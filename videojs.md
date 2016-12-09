#video.js 
> * 直接使用
> * 自定义控制条和样式
> * 弹幕相关
> * plugin写法

_video.js_ 是开源插件，据说甚至可以集合到gitbook上

**note**:
一个需要注意的点是下载下来的demo.html直接右键浏览器是打不开的，需要用iis或者nginx 或者beego这些服务端的玩意加载后，在前端浏览器才能看到视频页面。

参考文章:

- [HTML5视频播放器Video.Js的使用 ](http://coderlt.coding.me/2016/02/26/videojs-readme/)
- [api使用](http://docs.videojs.com/docs/api/index.html)
- brightcover 播放器是基于video.js内核封装的，所以它的文档也能参考
 - [download-video btn](http://docs.brightcove.com/en/video-cloud/brightcove-player/samples/download-video.html)
 - [customize-appearance](https://docs.brightcove.com/en/perform/brightcove-player/guides/customize-appearance.html)
 - [customize-appearance](http://docs.brightcove.com/en/video-cloud/brightcove-player/guides/customize-appearance-1x.html#specificity)
##直接使用
注意点：加载flash动画后，chrome ie firefox播放界面才能统一，否则各有各的界面
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
    <!-- 加载hls视频插件 -->
    <script src="video-js/videojs-contrib-hls.js"></script>
    <!-- 加载flash播放器 -->
    <script type="text/javascript">
        videojs.options.flash.swf = "static/video-js/video-js.swf";
    </script>
</head>
<body>

<div id="videocontainer">
    <video id="example_video" class="video-js vjs-default-skin  vjs-big-play-centered" 
           preload="auto" controls
           width="800" height="600" align="middle" poster=""
           data-setup="{ &quot;html5&quot; : { &quot;nativeTextTracks&quot; : false } }">
        <source src="你的名字.mp4" type="video/mp4"/>
	  <!-- 加载hls视频-->
        <source src="http://live.hkstv.hk.lxdns.com/live/hks/playlist.m3u8" 
         type="application/x-mpegURL">
        <!-- 加载rtmp视频-->
        <source src="rtmp://live.hkstv.hk.lxdns.com/live/hks" type="rtmp/flv"/>
    </video>

    </div>
</div>
</body>

</html>

```
##自定义样式和控制条

