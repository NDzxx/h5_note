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
##自定义控制条
组件结构
```
Player
    PosterImage
    TextTrackDisplay
    LoadingSpinner
    BigPlayButton
    ControlBar
        PlayToggle
        FullscreenToggle
        CurrentTimeDisplay
        TimeDivider
        DurationDisplay
        RemainingTimeDisplay
        ProgressControl
            SeekBar
              LoadProgressBar
              PlayProgressBar
              SeekHandle
        VolumeControl
            VolumeBar
                VolumeLevel
                VolumeHandle
        MuteToggle
```
使用：以添加按钮为例子
```js
var player = videojs("example_video", {
    "controls": true,
    "autoplay": false,
    "preload": "auto",
    "loop": false,
    controlBar: {
        captionsButton: false,
        chaptersButton: false,
        playbackRateMenuButton: true,
        LiveDisplay: true,
        subtitlesButton: false,
        remainingTimeDisplay: true,

        progressControl: true,

        volumeMenuButton: {
            inline: false,
            vertical: true
        },//竖着的音量条
        fullscreenToggle: true
    }

}, function () {
    /*次处用于自定义控制条等*/
    /*
    做法1：直接append添加
    */
   $(".vjs-control-bar").append('<button class="vjs-control " id="danmu_send_opt"><u>按钮1</u></button>');
   /*
   做法2：dom添加
   */
     var controlBar,
        newElement = document.createElement('div'),
        newLink = document.createElement('a'),
        newImage = document.createElement('img');
    // Assign id and classes to div for icon
    newElement.id = 'downloadButton';
    newElement.className = 'downloadStyle vjs-control';
    // Assign properties to elements and assign to parents
    newImage.setAttribute('src','http://solutions.brightcove.com/bcls/brightcove-player/download-video/file-download.png');
    newLink.setAttribute('href','http://www.baidu.com');
    newLink.appendChild(newImage);
    newElement.appendChild(newLink);
    // Get control bar and insert before elements
    // Remember that getElementsByClassName() returns an array
    controlBar = document.getElementsByClassName('vjs-control-bar')[0];
    // Change the class name here to move the icon in the controlBar
    insertBeforeNode = document.getElementsByClassName('vjs-fullscreen-control')[0];
    // Insert the icon div in proper location
    controlBar.insertBefore(newElement,insertBeforeNode);
    //controlBar.appendChild(newElement);
    
   /*
    做法3：简化2的写法
    */
    var newbtn = document.createElement('btn');
    newbtn.innerHTML = '<button class="vjs-control" id="downloadButton"></button>';
    var controlBar = document.getElementsByClassName('vjs-control-bar')[0];
    insertBeforeNode = document.getElementsByClassName('vjs-fullscreen-control')[0];
    controlBar.insertBefore(newbtn,insertBeforeNode);
});

```
##自定义样式：

video.js 采用flex布局，所以float这种不起作用

如果想要使用float,需要修改默认的video.js.css

并且后面的按钮都要进行样式调整
```css
.vjs-has-started .vjs-control-bar {
    display: -webkit-box;
    display: -webkit-flex;
    display: -ms-flexbox;
    /*原为flex*/
    display: block;
    visibility: visible;
    opacity: 1;
    -webkit-transition: visibility 0.1s, opacity 0.1s;
    -moz-transition: visibility 0.1s, opacity 0.1s;
    -o-transition: visibility 0.1s, opacity 0.1s;
    transition: visibility 0.1s, opacity 0.1s;
}

……省略无关部分
.video-js .vjs-play-control {
    /*增加float*/
    float:left;
    cursor: pointer;
    -webkit-box-flex: none;
    -moz-box-flex: none;
    -webkit-flex: none;
    -ms-flex: none;
    flex: none;
}

.video-js .vjs-fullscreen-control {
    cursor: pointer;
    -webkit-box-flex: none;
    -moz-box-flex: none;
    -webkit-flex: none;
    -ms-flex: none;
    flex: none;
    /*增加float*/
    float: right;
}
```

##弹幕
使用开源jquery.danmu.js

[jquery弹幕插件使用](http://www.jq22.com/jquery-info2123)

[jquery弹幕插件 github](https://github.com/chiruom/jquery.danmu.js)

video.js集成例子
```js
<!doctype html>

<html lang="en">
<head>
   <meta charset="UTF-8">
<link href="video-js/video-js.css" rel="stylesheet" type="text/css">
<script src="video-js/video.js" type="text/javascript" charset="utf-8"></script>
<script src="video-js/ie8/videojs-ie8.js"></script>
<script src="video-js/videojs-contrib-hls.js"></script>
<script src="danmu/jquery-1.11.1.min.js"></script>
<script src="../../../../../桌面/tanmu/DanmuPlayer/src/js/jquery.danmu.js"></script>
<script type="text/javascript">
	videojs.options.flash.swf = "video-js/video-js.swf";
</script>
   <title>RtmpPlayerTest</title>
</head>
<body>
	<video id="example_video" class="video-js vjs-default-skin  vjs-big-play-centered" controls preload="auto" width="1024" height="768"
      poster=""
      data-setup='{ "html5" : { "nativeTextTracks" : false } }'>>
     <source src="rtmp://live.hkstv.hk.lxdns.com/live/hks" type="rtmp/flv" />
	<!--  <source src="http://live.hkstv.hk.lxdns.com/live/hks/playlist.m3u8" type="application/x-mpegURL" /> -->
    <div id="danmu">
    </div>
  <p class="vjs-no-js">
    To view this video please enable JavaScript, and consider upgrading to a web browser
    that <a href="http://videojs.com/html5-video-support/" target="_blank">supports HTML5 video</a>
  </p>

 </video>
 <div id="danmu" >
 </div>
<div class="text-center ctr">
    <br>

    <button type="button" onclick="resumer() ">弹幕开始/继续</button>
    &nbsp;&nbsp;&nbsp;&nbsp;
    <button type="button" onclick="pauser()">弹幕暂停</button>
    &nbsp;&nbsp;&nbsp;&nbsp;
    显示弹幕:<input type='checkbox' checked='checked' id='ishide' value='is' onchange='changehide()'> &nbsp;&nbsp;&nbsp;&nbsp;
    弹幕透明度:
    <input type="range" name="op" id="op" onchange="op()" value="100"> <br>
    当前弹幕运行时间(分秒)：<span id="time"></span>&nbsp;&nbsp;
    设置当前弹幕时间(分秒)： <input type="text" id="set_time" max=20/>
    <button type="button" onclick="settime()">设置</button>

    <br>

    发弹幕:
    <select name="color" id="color">
        <option value="white">白色</option>
        <option value="red">红色</option>
        <option value="green">绿色</option>
        <option value="blue">蓝色</option>
        <option value="yellow">黄色</option>
    </select>
    <select name="size" id="text_size">
        <option value="1">大文字</option>
        <option value="0">小文字</option>
    </select>
    <select name="position" id="position">
        <option value="0">滚动</option>
        <option value="1">顶端</option>
        <option value="2">底端</option>
    </select>
    <input type="textarea" id="text" max=300/>
    <button type="button" onclick="send()">发送</button>
</div>

</body>
<script type="text/javascript">
	var player = videojs('example_video');
</script>


<script>
    (function () {
        $("#danmu").danmu({
                // left:$("#danmuarea").offset().left,
                // top:$("#danmuarea").offset().top,
                // height: 445,
                //    width: 800,
                left: 0,
                top: 0,
                height: "100%",
                width: "100%",
                zindex: 100,
                speed: 30000,
                opacity: 1,
                font_size_small: 16,
                font_size_big: 24,
                top_botton_danmu_time: 6000
            }
        );
    })(jQuery);


    query();
    timedCount();


    var first = true;

    function timedCount() {
        $("#time").text($('#danmu').data("nowtime"));

        t = setTimeout("timedCount()", 50)

    }


    function starter() {

        $('#danmu').danmu('danmu_start');

    }
    function pauser() {

        $('#danmu').danmu('danmu_pause');
    }
    function resumer() {

        $('#danmu').danmu('danmu_resume');
    }
    function stoper() {
        $('#danmu').danmu('danmu_stop');
    }

    function getime() {
        alert($('#danmu').data("nowtime"));
    }

    function getpaused() {
        alert($('#danmu').data("paused"));
    }

    function add() {
        var newd =
            {"text": "new2", "color": "green", "size": "1", "position": "0", "time": 60};

        $('#danmu').danmu("add_danmu", newd);
    }
    function insert() {
        var newd = {"text": "new2", "color": "green", "size": "1", "position": "0", "time": 50};
        str_newd = JSON.stringify(newd);
        $.post("stone.php", {danmu: str_newd}, function (data, status) {
            alert(data)
        });
    }
    function query() {
        $.get("query.php", function (data, status) {
            var danmu_from_sql = eval(data);
            for (var i = 0; i < danmu_from_sql.length; i++) {
                var danmu_ls = eval('(' + danmu_from_sql[i] + ')');
                $('#danmu').danmu("add_danmu", danmu_ls);
            }
        });
    }

    function send() {
        var text = document.getElementById('text').value;
        var color = document.getElementById('color').value;
        var position = document.getElementById('position').value;
        var time = $('#danmu').data("nowtime") + 5;
        var size = document.getElementById('text_size').value;
        var text_obj = '{ "text":"' + text + '","color":"' + color + '","size":"' + size + '","position":"' + position + '","time":' + time + '}';
        $.post("stone.php", {danmu: text_obj});
        var text_obj = '{ "text":"' + text + '","color":"' + color + '","size":"' + size + '","position":"' + position + '","time":' + time + ',"isnew":""}';
        var new_obj = eval('(' + text_obj + ')');
        $('#danmu').danmu("add_danmu", new_obj);
        document.getElementById('text').value = '';
    }

    function op() {
        var op = document.getElementById('op').value;
        $('#danmu').data("opacity", op);
    }


    function changehide() {
        var op = document.getElementById('op').value;
        op = op / 100;
        if (document.getElementById("ishide").checked) {
            jQuery('#danmu').data("opacity", op);
            jQuery(".flying").css({
                "opacity": op
            });
        } else {
            jQuery('#danmu').data("opacity", 0);
            jQuery(".flying").css({
                "opacity": 0
            });
        }
    }


    function settime() {
        var t = document.getElementById("set_time").value;
        t = parseInt(t)
        console.log(t)
        $('#danmu').data("nowtime", t);
    }
</script>
</html>


```
##plugin
[plugin sample](https://github.com/sprice/videojs-audio-tracks)



