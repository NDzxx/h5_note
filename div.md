局部刷新

```js
var selectedRoomId = null
    var selectedAuchorId = null    
    $(".c-items li").click(function () {
        delBackgroundColor();
        this.children[0].style.background = "url(static/res/home/AnchorSelect.png) no-repeat";
        selectedRoomId = this.getAttribute("roomId")
        selectedAuchorId = this.getAttribute("auchorId")
/*发送ajax只获取部分图像信息数据*/
        $.ajax({
            type: "get",
            dataType:'json', //接受数据格式
            cache:false,
            url: "/liveroom/menuinfo/" + this.getAttribute("roomId") + "/" + this.getAttribute("auchorId"),
            beforeSend: function(XMLHttpRequest){
            },
            success: function(data){
                $(".roomInfo")[0].style.display="block"
                $(".snapshot")[0].style.display="block"
                if (data.code == null || data.code != 200){
                    return
                }
                var miniVideo = document.getElementById("miniVideoImg")
                var roomTitle = document.getElementById("roomTitle")
                var onlineCount = document.getElementById("onlineCount")
                var anchorName = document.getElementById("anchorName")

                miniVideo.setAttribute("src",data.RoomImg)
                roomTitle.innerHTML = data.RoomTitle
                onlineCount.innerHTML = data.LookerCount
                anchorName.innerHTML = data.AnchorName
                var enterBtn = document.getElementById("enterBtn")
                if(data.IsOnline){
                    enterBtn.setAttribute("IsOnline","true")
                }else{
                    enterBtn.setAttribute("IsOnline","false")
                }
            },
            error: function(msg){
                //请求出错处理
                console.log(msg)
            }
        });
    })
```



