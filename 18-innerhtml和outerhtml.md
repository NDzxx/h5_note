#innerhtml outerhtml

##一、区别：

1）innerHTML:

　　从对象的起始位置到终止位置的全部内容,不包括Html标签。

2）outerHTML:

　　除了包含innerHTML的全部内容外, 还包含对象标签本身。


##二、例子：
```
<div id="test"> 
   <span style="color:red">test1</span> test2 
</div>

1）innerHTML的值是“<span style="color:red">test1</span> test2 ”

2）outerHTML的值是<div id="test"><span style="color:red">test1</span> test2</div>
```

需要注意的是outerHTML属性只有IE浏览器才有，其它浏览器是不支持的