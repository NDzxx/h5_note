#布局

* h5只管控制语义，样式由css来定
* 给div加一堆用图片构成的边框
```css
#videocontainer {
    width: 590px;
    height: 375px;
    background: url("../res/home/videoBoder/video-left-top-boder.png") no-repeat scroll 0px 0px/16px 16px content-box padding-box,
    url("../res/home/videoBoder/video-left-boder.png") no-repeat scroll 0px 16px/16px 343px content-box padding-box,
    url("../res/home/videoBoder/video-left-bottom-boder.png") no-repeat scroll 0px 359px/16px 16px content-box padding-box,
    url("../res/home/videoBoder/video-bottom-boder.png") no-repeat scroll 16px 359px/558px 16px content-box padding-box,
    url("../res/home/videoBoder/video-right-bottom-boder.png") no-repeat scroll 574px 359px/16px 16px content-box padding-box,
    url("../res/home/videoBoder/video-right-boder.png") no-repeat scroll 574px 16px/16px 343px content-box padding-box,
    url("../res/home/videoBoder/video-right-top-boder.png") no-repeat scroll 574px 0px/16px 16px content-box padding-box,
    url("../res/home/videoBoder/video-top-boder.png") no-repeat scroll 16px 0px/558px 16px content-box padding-box,
    url("../res/home/videoBoder/video-middle-boder.png") no-repeat scroll 16px 16px/558px 343px content-box padding-box;
    z-index:0;
}
```
z-index 属性设置元素的堆叠顺序。拥有更高堆叠顺序的元素总是会处于堆叠顺序较低的元素的前面。  
  
 参考:[CSS z-index 属性](http://www.w3school.com.cn/cssref/pr_pos_z-index.asp)
* 图片圆角
```css
.thlistItem .portrait .portraitImg {
    float: left;
    width: 32px;
    height: 32px;
    margin-left: 1px;
    margin-top: 1px;
    border-radius: 30px; /*这里是切割部分*/
}
```
* 图片等比缩放 只设置宽度（高度）而不是同时设置高宽

```html
<div class="portrait">
    <div>
        <img class="portraitImg" src="static/res/temp/1.jpg" width="50" />
    </div>
</div>

```
* flex布局 设为Flex布局以后，子元素的**float**、clear和vertical-align属性将失效。

[CSS flex 属性](http://www.runoob.com/cssref/css3-pr-flex.html)   

[flex布局教程](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html?utm_source=tuicool)






