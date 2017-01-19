http://www.techweb.com.cn/news/2011-05-10/1031945.shtml  一款csrf测试工具
https://security.tencent.com/index.php/blog/msg/24 CSRF-Scanner——打造全自动检测CSRF漏洞利器
http://www.2cto.com/article/201304/200528.html XSS自动化检测 

http://netsecurity.51cto.com/art/201105/260823.htm CSRFTester:一款CSRF漏洞的测试工具



https://github.com/search?utf8=%E2%9C%93&q=+csrf+scanner&type=Repositories&ref=searchresults


liveadmin补充完毕

http://www.freebuf.com/articles/web/30960.html 使用Fiddler的X5S插件查找XSS漏洞 

http://www.freebuf.com/vuls/56848.html Dominator漏洞

http://blog.csdn.net/jiary5201314/article/details/51281707
```
    如何查找网站的XSS呢？像Acunetix WVS、IBM的AppScan、华为的WebCruiser、诺赛科技的JSky都提过了非常好的XSS扫描功能。但是光靠工具是不行的，有时候网站会做些过滤，那么就得手工绕过了。Rsnake的网站上提供了非常齐全的XSS Cheat Sheet http://ha.ckers.org/xss.html，介绍了使用各种方法向页面插入测试代码，以及如何绕过网站的简单过滤。不过貌似很久都没更新了，仅仅在IE6.0-7.0,以及Firefox2.0等浏览器上进行测试，然而现在出了Chrome，貌似安全性很强，IE都已经到9了，FF也到了5（可以下载，官方尚未公布），所以很多测试代码现在并不可用。
    那么什么地方可以插入XSS攻击代码呢？这里简单讨论HTML以及CSS的插入脚本的方法，实际上在XSS Cheat Sheet中已经完全列出：
    a)无任何过滤：直接在表单中输入<script>alert(/xss/)</script>，注意单引号的闭合。
    b)利用事件：插入图片<img src=”1.jpg” onerror=”javascript：　alert(/xss/)”>，很多网站存在。
    c)利用其他标记： <img src=”javascript：　alert(/xss/)”>，可惜IE7以后不再支持绝大多数。
    d)利用expression：很多网站都倒在这上面了，包括百度、校内，第一个XSS蠕虫（Samy蠕虫（http://namb.la/popular/tech.html））也是利用了expression，<img style="xss:expression(alert(/xss/))">。当然利用expression也会有个缺点，表达式中的代码会一直重复执行。
    e)利用@import：<style>@import url(http://xxx.xxx.xxx/xss.css); </style>，导入CSS代码，可以利用d)嵌入脚本

```

http://www.jikexueyuan.com/course/1388_2.html xxs视频

http://www.jikexueyuan.com/course/1500.html csrf视频

[Web漏洞扫描器使用方法](http://jingyan.baidu.com/article/37bce2be71c8ad1002f3a222.html)

http://www.janusec.com/downloads/